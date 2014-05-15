# Route Target Syntax
### Overview
TODO: talk about the different kinds of custom routes you can define

<!--
##### Upload Limit
By default routes are limited to `10mb` uploads, to change the upload limit set the `uploadLimit` config on your route:
```javascript
'/': {
  ...,
  uploadLimit: '100mb'
}
```
The limit setting uses `express.limit()` internally, and supports any valid [connect.limit()](http://www.senchalabs.org/connect/limit.html) values 

-->

### CORS (Cross origin resource sharing)
Additionally, you can also enable [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing) on a route:
```javascript
'/': {
  ...,
  cors: true
  // cors: 'http://sailsjs.org, http://sailsjs.com'
}
```

#### CORS Support vs. CSRF Protection
+ If CORS is enabled on a route, `req.csrfToken()` and `res.locals._csrf` are set to `null` to prevent cross-site request forgery attacks via accidental exposure of the [CSRF token]().

### skipAssets

If you're using a wildcard route like `/*`, or trying to implement vanity URLs like `/:user/:project`, you'll often want to bypass your route handler when the request is for an asset (e.g. `/images/logo.png`).  You can do this by adding `skipAssets: true` to your route configuration.  This will skip your handler for any URLs that contain a dot character, so that the static handler can fetch them (so long as another route without `skipAssets` doesn't also match the URL!).

### skipRegex

If skipping every URL containing a dot is too permissive, or you need a route's handler to be skipped based on different criteria entirely, you can use `skipRegex`.  This option allows you to specify a regular expression or array of regular expressions to match the request URL against; if any of the matches are successful, the handler is skipped.  Note that unlike the syntax for binding a handler with a regular expression, `skipRegex` expects *actual RegExp objects*, not strings.  For example:

```
'/*': {
   controller: 'MyController',
   action: 'myAction',
   skipRegex: [/^\/\d+$/, /^.*foo$/]
}
```   

This would run `MyController.myAction` for every URL _except_ those which contain only numbers, and those ending in the string "foo".









--



<!-- This is implemented by the request interpreter, a thin normalization layer. -->

<!-- This normalization layer is called the request interpreter. -->

<!-- > Future: put in a note about how you might write a hook to add support for a new transport to the Sails router. Maybe just link to the README in core
"In fact, the router is designed to support...."" -->

listens for HTTP requests, but it also supports WebSockets.
(e.g. URLs typed into the browser, AJAX, cURL, etc.) 
The Sails router listens for HTTP requests




You can design your app's URLs however you like


Sails lets you design your URL scheme however you like, but it also provides some convenient helpers for resourceful routing.


1. "explicit routing" (config/routes.js)
2. "resourceful routing" (blueprint routes aka shadows)
3. "asset routing" (drop a file `foo.bar` in `assets` and it's accessible at http://localhost:1337/foo.bar)  er rlike cgi, like static middleware, like apache, like www

























> This section is actively under construction for v0.10.





Sails uses three different strategies to route incoming requests.

Sails uses a number of different strategies to route requests. This section lists them top-to-bottom, in order of precedence.


Sails uses a number of different strategies to route requests. Here they are top-to-bottom, in order of precedence:

#### 1. Static Assets
Flat files in your `assets` directory- (these are sometimes referred to as &lsquo;public&rsquo;)

If you have an image file at `/assets/images/foo.jpg`, it will be made available automatically via the route:  `/images/foo.jpg`


#### 2. Static Routes
This object routes static URLs to handler functions-- In most cases, these functions are actions inside of your controllers. For convenience, you can also connect routes directly to views or external URLs.

By default, your root route (aka home page) points to a view located at `views/home/index.ejs`. (This would also work if you had a file at: `/views/home.ejs`)


But what if you want your home page to display a signup form located at `views/user/signup.ejs`?
```javascript
'/' : {
  view : 'user/signup'
}
```

Let&rsquo;s say you&rsquo;re building an email client, like Gmail. You might want your home route to serve an interface using custom logic. In this scenario, you have a custom controller `MessageController` with an `inbox` action: `'/' : 'message.inbox'`

Alternatively, you can use the more verbose syntax:
```javascript
'/': {
  controller  : 'message',
  action    : 'inbox'
}
```

If you decided to call your action `index` instead of `inbox`, since the `index` action is the default, you can shortcut even further to: `'/': 'MessageController'`


Up until now, we haven&rsquo;t specified a specific HTTP method/verb. The routes above will apply to ALL verbs! If you want to set up a route only for one in particular (GET, POST, PUT, DELETE, etc.), just specify the verb before the path. 

For example, if you have a `UserController` with a `signup` action, and somewhere else, you&rsquo;re serving a signup form that looks like:
```html
<form action="/signup">
  <input name="username" type="text"/>
  <input name="password" type="password"/>
</form>
```
You could define the following route: `'post /signup' : 'user.signup'`.

Finally, here&rsquo;s an example of how you would route all GET requests to the `/google` route to Google&rsquo;s website: `'get /google' : 'http://google.com'`

##### Regular Expressions in Routes

Sails v0.10 allows you to use regular expressions to define the URLs that your route matches.  The syntax for a regular expression route is:

`"r|<regular expression string>|<comma-delimited list of param names>"`

That's the letter "**r**", followed by a pipe, a regular expression string *without delimiters*, another pipe, and a list of parameter names that should be mapped to parenthesized groups in the regular expression.  For example:

`"r|^/\d+/(\w+)/(\w+)$|foo,bar": "MessageController.myaction"`

Will match `/123/abc/def`, running the `myaction` action of `MessageController` and supplying the values `abc` and `def` as `req.param('foo')` and `req.param('bar')`, respectively.

#### 3. Advanced Route config

##### Setting Locals (variables accessible in your view) in Sails v0.10

Just like you can set variables to be available in your view scope like `res.locals.foo = 'bar'` in a controller or action, you can now also pass locals down to your views via route configuration.  Note, this is only relevant for declared routes. It won't work on implicit blueprint routes.

Here's an example of what can be done!



```
// myApp/config/routes.js

module.exports.routes = {

  '/'" {
    view: 'static/index'
  },
  
  '/fiddlesticks': {
    view: 'session/new',

    locals: {
      foo: 'asdg',
      bar: 'sadgasgd',
    }
  }
}

```





#### 4. Action Blueprints
These routes can be disabled in `config/controllers.js` by setting: `module.exports.controllers.routes.actions = false`
 

All of your controllers' actions are automatically bound to a route.  For example: If you have a controller, `FooController`:
+ its action `bar` is accessible at `/foo/bar`
+ its action `index` is accessible at `/foo/index`, and also `/foo`



#### 6. Shortcut CRUD blueprints
These routes can be disabled in `config/controllers.js` by setting: `module.exports.controllers.routes.shortcuts = false`

If you have a model, `Foo`, and a controller, `FooController`, you can access CRUD operations for that model at:
+ `/foo/find/:id?`  ->  search lampshades using specified criteria or with id=:id
+ `/foo/create` ->  create a lampshade using specified values   
+ `/foo/update/:id` ->  update the lampshade with id=:id    
+ `/foo/destroy/:id`  ->  delete lampshade with id=:id

#### 7. REST blueprints
These routes can be disabled in `config/controllers.js` by setting: `module.exports.controllers.routes.rest = false`
 
If you have a model, `Foo`, and a controller, `FooController`, you can access CRUD operations for that model at:
+ `get /foo/:id?` ->  search lampshades using specified criteria or with id=:id
+ `post /foo`   -> create a lampshade using specified values
+ `put /foo/:id`  ->  update the lampshade with id=:id
+ `delete /foo/:id` ->  delete lampshade with id=:id

#### 8. Default 404 (not found) handler
Finally, if nothing else matched, the default 404 handler is triggered. See `config/404.js` to adjust your app&rsquo;s 404 logic.


<docmeta name="uniqueID" value="RouteTargetSyntax278177">
<docmeta name="displayName" value="Route Target Syntax">

