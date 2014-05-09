# Socket Hijacking

Unfortunately, cross-site request forgery attacks are not limited to the HTTP protocol.  WebSocket hijacking (sometimes known as [CSWSH](http://www.christian-schneider.net/CrossSiteWebSocketHijacking.html)) is a commonly overlooked vulnerability in most realtime applications.  Fortunately, since Sails treats both HTTP and WebSocket requests as first-class citizens, its built-in [CSRF protection]() and [configurable CORS rulesets]() apply to both protocols.

You can prepare your Sails app against CSWSH attacks by enabling the built-in protection in [`config/csrf.js`]() and ensuring that a `_csrf` token is sent with all relevant incoming socket requests.  Additionally, if you're planning on allowing sockets to connect to your Sails app cross-origin (i.e. from a different domain, subdomain, or port) you'll want to configure your CORS settings accordingly.  You can also define the `authorization` setting in [`config/sockets.js`]() as a custom function which allows or denies the initial socket connection based on your needs.

#### Notes
+ CSWSH prevention is only a concern in scenarios where people use the same client application to connect sockets to multiple web services (e.g. cookies in a browser like Google Chrome can be used to connect a socket to Chase.com from both Chase.com and Horrible-Hacker-Site.com.)

