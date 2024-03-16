# `@reverecre/next-query-params`

## API

### `enum QueryParamKind`

**Members:**

- **`STRING`:** Immediately flushed single string value.
- **`STRING_DEFERRED`:** String value that uses local state updates and defers updating the URL.
- **`STRING_LIST`:** Value is set multiple times in the URL, decoded as a list.
- **`STRING_SET`:** Value is set multiple times in the URL, decoded as a set.
- **`STRING_LIST_DEFAULT_UNDEFINED`:** Value is undefined when not present, a list when it is.
- **`STRING_REQUIRED`:** String value that will throw an error if not found.
- **`NUMBER`:** Parsed and serialized as a number.
- **`NUMBER_DEFERRED`:** Number value that uses local state updates and defers updating the URL.

### `parseQuery`

Returns a normalized value of a parsed query param object.

**Example Usage**

```tsx
import { parseQuery, QueryParamKind } from "@reverecre/next-query-params";

const { sortDir } = parseQuery(req.query, {
  sortDir: QueryParamKind.STRING,
});
```

### `QueryParamsProvider`

Provides an application with the shared query parameter state. This is used to sync deferred updates across the application.

### `useQueryParams`

A hook that reads from Next.js's `useQuery` and returns read, write, and reset functions.

**Example Usage**

```tsx
import {
  QueryParamKind,
  QueryParamsProvider,
  useQueryParams,
} from "@reverecre/next-query-params";

function MyApp() {
  return (
    <QueryParamsProvider>
      <MyComponent />
    </QueryParamsProvider>
  );
}

function MyComponent() {
  const [query, setQuery, resetQuery] = useQueryParams({
    sortDir: QueryParamKind.STRING,
    sortBy: QueryParamKind.STRING,
    search: QueryParamKind.STRING_DEFERRED,
    selectedOptions: QueryParamKind.STRING_SET,
  });

  const updateSearch = useCallback(
    (next: string) => {
      setQuery({ search: next });
    },
    [setQuery]
  );

  // ...
}
```
