# req.query

An object containing the parsed query-string, defaulting to `{}`.

### Usage
```js
req.query;
```

### Example

If the request is `GET /search?q=mudslide`:

```js
req.query.q
// -> "mudslide"
```


