# Normalized Frontend State

Normalize frontend state when API responses contain nested or repeated entities
that are cached and updated on the client. The goal is the same as in a small
database: keep one source of truth for each entity and reference it by ID from
other records.

## The Problem

Nested API responses are convenient to render at first, but they become painful
when the app needs to update one record.

```js
const blogPosts = [
  {
    id: "post-1",
    author: { id: "user-1", username: "user1" },
    body: "...",
    comments: [
      {
        id: "comment-1",
        author: { id: "user-2", username: "user2" },
        body: "...",
      },
      {
        id: "comment-2",
        author: { id: "user-3", username: "user3" },
        body: "...",
      },
    ],
  },
];
```

Problems with this shape:

- the same user can appear in many places;
- updates require walking deep object graphs;
- immutable updates become noisy because every parent object on the path must be
  copied;
- list rendering can re-render more than necessary when parent arrays or objects
  get new references.

## Normalized Shape

Split each entity type into a lookup table and an ID list. Older Redux examples
often use `byId` and `allIds`; Redux Toolkit uses the equivalent `entities` and
`ids` naming.

```js
const state = {
  posts: {
    byId: {
      "post-1": {
        id: "post-1",
        author: "user-1",
        body: "...",
        comments: ["comment-1", "comment-2"],
      },
    },
    allIds: ["post-1"],
  },
  comments: {
    byId: {
      "comment-1": {
        id: "comment-1",
        author: "user-2",
        body: "...",
      },
      "comment-2": {
        id: "comment-2",
        author: "user-3",
        body: "...",
      },
    },
    allIds: ["comment-1", "comment-2"],
  },
  users: {
    byId: {
      "user-1": { id: "user-1", username: "user1" },
      "user-2": { id: "user-2", username: "user2" },
      "user-3": { id: "user-3", username: "user3" },
    },
    allIds: ["user-1", "user-2", "user-3"],
  },
};
```

Rules:

- Store each entity type separately.
- Store each entity once.
- Use IDs to represent relationships.
- Use an ID array when order matters.
- Rebuild view-specific nested data with selectors instead of storing duplicate
  nested copies.

## Updating Relationships

Adding a comment touches the comment table and the post's comment ID list.

```js
function addComment(state, postId, comment) {
  const commentId = comment.id;

  state.comments.byId[commentId] = comment;
  state.comments.allIds = [...state.comments.allIds, commentId];

  state.posts.byId[postId].comments = [
    ...state.posts.byId[postId].comments,
    commentId,
  ];
}
```

In Redux Toolkit reducers this can be written with mutating syntax because Immer
produces the immutable update. In Zustand or plain React state, still make sure
the store update produces the new references your selectors depend on.

## Reading Denormalized View Data

Use selectors to join entities for the UI.

```js
function selectCommentsForPost(state, postId) {
  const post = state.posts.byId[postId];

  return post.comments.map((commentId) => {
    const comment = state.comments.byId[commentId];
    const author = state.users.byId[comment.author];

    return {
      ...comment,
      author,
    };
  });
}
```

Memoize selectors when they perform expensive joins or return arrays/objects
that are passed into React components frequently.

## Benefits

- Updates are localized to the relevant entity and relationship lists.
- Duplicate records do not drift out of sync.
- Lookups by ID are direct.
- Reducer/store logic is easier to reason about than deep nested updates.
- React lists can render by IDs and let child components select their own
  entity data.

## Tradeoffs

- Selectors become responsible for reconstructing view-specific shapes.
- Many-to-many relationships may need join tables or additional ID maps.
- Small, static, one-off response shapes may not be worth normalizing.
- Normalized state is not a replacement for a server cache strategy such as RTK
  Query, TanStack Query, or framework data loaders.

## Decision Rules

- Normalize long-lived cached entities.
- Normalize data that has relationships across entity types.
- Normalize when the same entity appears in more than one response or UI branch.
- Keep simple component state nested if it is local, short-lived, and never
  updated independently.
- Prefer generated entity helpers, such as Redux Toolkit `createEntityAdapter`,
  when using Redux.

## References

- [Redux: Normalizing State Shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
- [Redux Essentials: Performance, Normalizing Data, and Reactive Logic](https://redux.js.org/tutorials/essentials/part-6-performance-normalization)
