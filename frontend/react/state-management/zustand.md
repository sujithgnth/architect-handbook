# Zustand Micro-State Management

Zustand is useful when React state needs to be shared across multiple components
or feature modules, but Redux-style structure would be more process than the
problem needs. Treat it as a way to create small, feature-owned stores with
explicit actions and selector-based reads.

## Core Idea

Micro-state management means avoiding one large frontend state monolith. Keep
state close to the feature or module that owns it, and expose only the store API
that other components need.

Use a Zustand store when:

- multiple components need the same client state;
- a feature needs actions that should live with the state;
- prop drilling or context plumbing is becoming noisy;
- render control matters and selectors can keep subscriptions narrow.

Do not move state to Zustand just because it exists. Component-local state is
still the default for state that only one component owns.

## Basic Store

`create` returns a React hook. The hook also has store utilities such as
`getState`, `setState`, and `subscribe` attached.

```js
import { create } from "zustand";

export const useCounterStore = create((set) => ({
  count: 0,
  message: "Hello World",
  increaseCount: (value) =>
    set((state) => ({ count: state.count + value })),
  setMessage: (message) => set({ message }),
}));
```

Read the store inside a component with a selector:

```js
import { useCounterStore } from "./store";

function Counter() {
  const count = useCounterStore((state) => state.count);
  const increaseCount = useCounterStore((state) => state.increaseCount);

  return (
    <button onClick={() => increaseCount(1)}>
      Count: {count}
    </button>
  );
}
```

## Immutable Updates

Zustand expects state changes to go through `set` or `setState`. Do not mutate
the object returned by `getState` and then pass it back as if the mutation were
the update.

```js
const state = useCounterStore.getState();

state.count = 2; // Avoid: direct mutation.
useCounterStore.setState(state);
```

Create a new partial state instead:

```js
useCounterStore.setState({ count: 2 });

useCounterStore.setState((state) => ({
  count: state.count + 1,
}));
```

By default, object updates are shallowly merged into the existing state. That
means `set({ count: 1 })` can update `count` without re-stating `message`.

## Selectors And Render Control

The main render optimization tool is a selector. A component should subscribe to
the smallest piece of state it actually uses.

Prefer this:

```js
const count = useCounterStore((state) => state.count);
const setMessage = useCounterStore((state) => state.setMessage);
```

Avoid selecting the whole store by default:

```js
const { count, message, setMessage } = useCounterStore();
```

Selecting the whole store means the component can re-render when any selected
store value changes, even if the component only displays `count`.

Be careful with selectors that create a new object or array:

```js
const values = useCounterStore((state) => ({
  count: state.count,
  setMessage: state.setMessage,
}));
```

That selector returns a new object reference. If you need to select multiple
values together, either split selectors or use `useShallow`.

```js
import { useShallow } from "zustand/react/shallow";

const { count, setMessage } = useCounterStore(
  useShallow((state) => ({
    count: state.count,
    setMessage: state.setMessage,
  })),
);
```

## Module State

A store exported from a module behaves like shared module state. If two
components read the same store and one action updates it, both components can
observe the change if their selectors are affected.

Use this deliberately:

- colocate feature stores near the feature when ownership is clear;
- name actions around domain events, not UI events;
- keep persisted state small and intentional;
- avoid turning one Zustand store into the new global monolith.

## Middleware

Useful middleware:

- `devtools`: connects store updates to Redux DevTools for debugging.
- `persist`: saves selected state to storage and rehydrates it later.
- `subscribeWithSelector`: subscribes outside React to a selected state slice.

Persist only state that should survive reloads. Avoid persisting short-lived UI
state, secrets, tokens, or data that can become stale without a migration plan.

## Decision Rules

- Local component state first.
- React context for dependency injection or low-frequency shared values.
- Zustand for feature-owned shared client state with explicit actions.
- Redux or a stricter event/state architecture when the app needs stronger
  conventions, replayability, cross-team governance, or complex async workflows.

## References

- [Zustand create API](https://zustand.docs.pmnd.rs/reference/apis/create)
- [Zustand reference](https://zustand.docs.pmnd.rs/reference/index)
- [Zustand useShallow](https://zustand.docs.pmnd.rs/reference/hooks/use-shallow)
