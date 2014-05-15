# sails.sockets.leave( `socket`, `roomName` )
### Purpose
Unsubscribe a socket from a generic room.

### Overview
#### Parameters
|   |          Description        | Accepted Data Types | Required ? |
|---|-----------------------------|---------------------|------------|
| 1 | Socket object |      `object`       | Yes        |
| 2 |           Room Name        | `string`            | Yes         |

### Example Usage
```javascript
// Controller action

leaveFunRoom: function(req, res) {
    var roomName = req.param('roomName');
    sails.sockets.leave(req.socket, roomName);
    res.json({
      message: 'Left a fun room called '+roomName+'!'
    });
}
```

*Note: `req.socket` is only valid if the action is triggered via a socket request, e.g. `socket.get('/leaveFunRoom/someRoomName')`*

<docmeta name="uniqueID" value="sailssocketsleave425459">
<docmeta name="displayName" value="sails.sockets.leave( `socket`, `roomName` )">

