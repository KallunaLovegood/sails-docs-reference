# sails console

Quietly lift your sails app (i.e. with logging silenced), and enter the [node shell](http://nodejs.org/api/repl.html) in the context of the app's global scope.  This means you can access all of your model instances, their methods, and much more, all by using javascript commands.  

### Example

```sh
$ sails console

info: Starting app in interactive mode...

info: Welcome to the Sails console.
info: ( to exit, type <CTRL>+<C> )

sails> 
```

> Note that `sails console` still lifts the server, so routes will be accessible using a browser.



<docmeta name="uniqueID" value="sailsconsole198558">
<docmeta name="displayName" value="sails console">

