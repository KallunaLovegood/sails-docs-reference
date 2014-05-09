# DDOS

The prevention of [denial of service attacks](https://www.owasp.org/index.php/Application_Denial_of_Service) is a [complex problem](http://en.wikipedia.org/wiki/Denial-of-service_attack#Handling) which involves multiple layers of protection, up and down the networking stack.
This type of attack has achieved [notoriety](http://www.darkreading.com/vulnerabilities-and-threats/10-strategies-to-fight-anonymous-ddos-attacks/d/d-id/1102699) in recent years due to widespread media coverage of groups like Anonymous.

At the API layer, there isn't much that can be done in the way of prevention.  However, Sails offers a few settings to mitigate certain types of DDOS attacks:

+ The session in Sails can be [configured]() to use a separate session store (e.g. [Redis](http://redis.io/)), allowing your application to run without relying on the memory state of any one API server.  This means that multiple copies of your Sails app may be deployed to as many servers as is necessary to handle traffic.  This is achieved by using a [load balancer](), which directs each incoming request to a free server with the resources to handle it, eliminating any one app server as a single point of failure.
+ Socket.io connections may be [configured]() to use a separate [socket store]() (e.g. Redis) for managing pub/sub state and message queueing. This eliminates the need for sticky sessions at the load balancer, preventing would-be attackers from directing their attacks against the same server again and again.