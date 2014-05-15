# sails.sockets.join()

### Purpose
Subscribe a socket to a generic room.

### Usage

```js
sails.sockets.join(socket, roomName);
```

|   | Argument   | Type        | Details |
|---|------------|:-----------:|---------|
| 1 | `socket`   | ((string)) -or- ((Request)) | The socket to be subscribed.  May be specified by the socket's id or a Request (`req`) which originated from it.
| 2 | `roomName` | ((string))  | The name of the room to which the socket will be subscribed.  If the room does not exist yet, it will be created.

### Example

In a controller action:

```javascript
subscribeToFunRoom: function(req, res) {
  var roomName = req.param('roomName');
  sails.sockets.join(req, roomName);
  res.json({
    message: 'Subscribed to a fun room called '+roomName+'!'
  });
}
```

*Note: `req.socket` is only valid if the action is triggered via a socket request, e.g. `socket.get('/subscribeToFunRoom/someRoomName')`*

<docmeta name="uniqueID" value="sailssocketsjoin958690">
<docmeta name="displayName" value="sails.sockets.join()">

