# res.clearCookie()

Clears cookie (`name`) in the response.

### Usage

```js
res.clearCookie(name [,options]);
```

### Details

The path option defaults to "/".


### Example
```javascript
res.cookie('name', 'tobi', { path: '/admin' });
res.clearCookie('name', { path: '/admin' });
```





