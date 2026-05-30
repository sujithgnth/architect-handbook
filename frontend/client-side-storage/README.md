# Client-Side Storage

Client-side storage is any browser-side mechanism that keeps data near the user
instead of requiring every read to come from the server. It is useful for
preferences, temporary UI state, caching, offline workflows, and some session
coordination.

The important distinction: most browser storage is client-only. Cookies are the
main storage mechanism that the browser automatically sends back to the server
with matching HTTP requests.

## Quick Rules

- Use cookies when the server must receive a small value on requests, especially
  a session identifier.
- Use `sessionStorage` for per-tab workflow state that can disappear when the
  tab closes.
- Use `localStorage` for small, non-sensitive preferences that can persist until
  the user or app clears them.
- Use IndexedDB for larger structured data, offline-first data, files, blobs, or
  data that should be queried by indexes.
- Use the Cache API for HTTP request and response caching, usually with a
  Service Worker.
- Do not store passwords, raw session IDs, access tokens, refresh tokens, or
  sensitive personal data in Web Storage or IndexedDB unless you have a strong
  threat model and key-management design.

## Comparison

| Mechanism | Lifetime | Shape | Sent with requests | Best fit | Main risk |
| --- | --- | --- | --- | --- | --- |
| Cookies | Session cookie or configured expiry | String key-value pairs | Yes, automatically for matching requests | Session IDs, small server-readable state, feature flags | Request overhead, tracking/privacy issues, CSRF if misconfigured |
| `sessionStorage` | Current tab/page session | String key-value pairs | No | Multi-step forms, per-tab UI state, one workflow instance | Lost on tab close; readable and writable by JavaScript from the same origin |
| `localStorage` | Persistent until cleared | String key-value pairs | No | Theme, language, lightweight preferences | Synchronous API, XSS exposure, no built-in expiry |
| IndexedDB | Persistent by default, subject to browser quota/eviction rules | Structured objects, indexes, files/blobs | No | Offline-first apps, large datasets, searchable client data | More complex API, XSS exposure, quota/eviction handling |
| Cache API | Persistent by default, subject to browser quota/eviction rules | HTTP request/response pairs | No | Offline assets and API response caching | Can serve stale or sensitive responses if cache rules are wrong |

## Cookies

Cookies are small pieces of state stored by the browser and sent back to the
server on matching requests. They are useful when the server needs the value on
every request, but they are a poor place for large app data because they add
bytes to HTTP traffic.

Use server-set cookies for authentication/session identifiers:

```http
Set-Cookie: __Host-session=<opaque-id>; Path=/; HttpOnly; Secure; SameSite=Lax; Max-Age=3600
```

Notes:

- `HttpOnly` prevents JavaScript from reading the cookie. It must be set by the
  server using `Set-Cookie`; JavaScript cannot create an `HttpOnly` cookie.
- `Secure` sends the cookie only over HTTPS, except for localhost behavior in
  modern browsers.
- `SameSite=Lax` is a practical default for many session cookies. Use
  `SameSite=Strict` for tighter same-site-only behavior when cross-site top-level
  navigation does not need the cookie. Use `SameSite=None; Secure` only when the
  cookie must be sent cross-site.
- The `__Host-` prefix requires `Secure`, `Path=/`, and no `Domain` attribute in
  supporting browsers. This keeps the cookie host-bound.
- Cookie values are user-controlled input from the server's perspective. Always
  validate them server-side.

JavaScript-set cookies should be reserved for non-sensitive values:

```js
document.cookie = "theme=dark; Max-Age=2592000; Path=/; SameSite=Lax; Secure";
```

Delete a cookie by setting the same name/path/domain with `Max-Age=0`:

```http
Set-Cookie: theme=; Path=/; Max-Age=0
```

## Sessions

A session is a server-side state-management pattern, not a client-side storage
API. The browser usually stores only an opaque session identifier, commonly in a
cookie. The actual session data should live on the server in memory, Redis, a
database, or another session store.

For Express, keep the session cookie small and store the data server-side:

```js
const session = require("express-session");

app.use(
  session({
    name: "__Host-session",
    secret: process.env.SESSION_SECRET,
    resave: false,
    saveUninitialized: false,
    cookie: {
      httpOnly: true,
      secure: true,
      sameSite: "lax",
      path: "/",
      maxAge: 60 * 60 * 1000,
    },
  })
);
```

For production, configure a real session store. The default in-memory store in
`express-session` is for development and does not scale across processes or
survive restarts.

Regenerate the session ID after login and other privilege changes to reduce
session fixation risk.

## Web Storage

`localStorage` and `sessionStorage` expose the same simple `Storage` API:

```js
localStorage.setItem("theme", "dark");
const theme = localStorage.getItem("theme");
localStorage.removeItem("theme");
localStorage.clear();
```

The API stores strings only. Use JSON for objects and handle corrupted or
missing data:

```js
const key = "draft-profile";

function saveDraft(draft) {
  try {
    localStorage.setItem(key, JSON.stringify(draft));
  } catch (error) {
    if (error.name === "QuotaExceededError") {
      // Fall back, evict old data, or ask the user to clear space.
      return;
    }

    throw error;
  }
}

function readDraft() {
  const value = localStorage.getItem(key);
  if (!value) return null;

  try {
    return JSON.parse(value);
  } catch {
    localStorage.removeItem(key);
    return null;
  }
}
```

Important constraints:

- Web Storage is synchronous. Large or frequent reads/writes can block the main
  thread.
- `localStorage` is shared by same-origin pages and survives browser restarts.
- `sessionStorage` is scoped to the origin and tab/page session, so separate tabs
  get separate storage.
- In private/incognito modes, data may be cleared when the private session ends.
- Quotas are browser-managed. Treat quota errors as expected runtime failures.

## IndexedDB

IndexedDB is a transactional, asynchronous browser database for significant
amounts of structured data. It supports object stores, keys, indexes, and values
that can be handled by the structured clone algorithm.

Minimal setup:

```js
const openRequest = indexedDB.open("notes-db", 1);

openRequest.onupgradeneeded = () => {
  const db = openRequest.result;
  const store = db.createObjectStore("notes", { keyPath: "id" });
  store.createIndex("updatedAt", "updatedAt");
};

openRequest.onsuccess = () => {
  const db = openRequest.result;
  const tx = db.transaction("notes", "readwrite");
  const notes = tx.objectStore("notes");

  notes.put({
    id: "note-1",
    title: "Client-side storage",
    updatedAt: Date.now(),
  });
};

openRequest.onerror = () => {
  console.error("Failed to open IndexedDB", openRequest.error);
};
```

Use IndexedDB when:

- the data is too large for Web Storage;
- you need async reads and writes;
- you need object stores, indexes, or client-side queries;
- the app must keep working offline and sync later.

Avoid raw IndexedDB for tiny preferences. Its setup cost is higher than
`localStorage`, and many teams use a wrapper library when the app needs serious
offline behavior.

## Security Notes

- Browser storage belongs to the user's device. A local user, compromised device,
  browser extension, or XSS flaw may read or alter it.
- Treat data read from browser storage as untrusted input. Validate and sanitize
  it before use, just as you would validate network input.
- Do not use browser storage as an authorization boundary. The server must
  enforce authorization.
- Client-side encryption only helps if the key is not stored next to the data.
  If JavaScript can read both encrypted data and the key, XSS can usually read
  both too.
- Use Content Security Policy, output encoding, dependency review, and careful
  DOM APIs to reduce XSS risk.
- Use `HttpOnly`, `Secure`, and `SameSite` cookies for server-managed sessions,
  plus CSRF protections where the application needs them.
- Store expiration metadata yourself for `localStorage` and IndexedDB if data
  should expire.

## Choosing a Storage Option

Use this decision flow:

1. Must the server receive it automatically on requests?
   Use a cookie. For authentication, prefer an opaque `HttpOnly`, `Secure`,
   `SameSite` session cookie.
2. Is it sensitive?
   Avoid client-side storage unless there is a deliberate security design.
3. Is it temporary and specific to one tab?
   Use `sessionStorage`.
4. Is it small, non-sensitive, and simple?
   Use `localStorage`.
5. Is it large, structured, searchable, or part of an offline workflow?
   Use IndexedDB.
6. Is it an HTTP response or static asset cache?
   Use the Cache API, usually through a Service Worker.

## Common Corrections

- "Sessions" are not the same thing as `sessionStorage`. Sessions usually store
  server-side data and identify the browser with a cookie.
- `localStorage` is not "secure" just because it is not sent to the server. It is
  readable by same-origin JavaScript and exposed by XSS.
- Web Storage capacity is not a universal 5-10 MB rule. MDN currently documents
  10 MiB total Web Storage per origin in major browsers, split as 5 MiB
  `localStorage` and 5 MiB `sessionStorage`; other storage APIs have
  browser-specific quota systems.
- Do not store large app data in cookies. Cookies are attached to requests and
  can hurt performance.
- HTTPS protects data in transit. It does not automatically encrypt data stored
  at rest in Web Storage or IndexedDB.
- `SameSite` helps reduce CSRF exposure, but it is not a complete replacement
  for application-level CSRF defenses in every architecture.

## References

- [MDN: Using HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies)
- [MDN: Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie)
- [MDN: Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)
- [MDN: Storage quotas and eviction criteria](https://developer.mozilla.org/en-US/docs/Web/API/Storage_API/Storage_quotas_and_eviction_criteria)
- [MDN: IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [OWASP: HTML5 Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html)
- [OWASP: Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
- [express-session package documentation](https://www.npmjs.com/package/express-session)
