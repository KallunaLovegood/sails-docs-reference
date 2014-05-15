# Models

### Overview

A model represents a collection of structured data, usually corresponding to a single table or collection in a database.  Models are defined by creating a file in an app's `api/models/` folder.

```javascript
// api/models/Product.js
module.exports = {
  attributes: {
    nameOnMenu: { type: 'string' },
    price: { type: 'string' },
    percentRealMeat: { type: 'float' },
    numCalories: { type: 'integer' }
  }
}
```

### Using models

Models may be accessed from our controllers, policies, services, responses, tests, and in custom model methods.  There are many built-in methods available on models, the most important of which are the query methods: [find](), [create](), [update](), and [destroy]().  These methods are [asynchronous]() - under the covers, Waterline has to send a query to the database and wait for a response.

Consequently, query methods return a deferred query object.  To actually execute a query, `.exec(cb)` must be called on this deferred object, where `cb` is a callback function to run after the query is complete.

Waterline also includes opt-in support for promises.  Instead of calling `.exec()` on a query object, we can call `.then()`, `.spread()`, or `.fail()`, which will return a [Q promise](https://github.com/kriskowal/q).



### Class methods

Besides data, models may also have **class methods**: functions built into the model itself that perform some task. **Class methods** are functions available at the top level of a model.  These are useful for pulling code out of our controllers and into reusuable functions that can be called from anywhere (i.e. don't depend on `req` or `res`.)

```javascript
// api/models/User.js
module.exports = {

  attributes: {

    name: { 
      type: 'string'
    },
    enrolledIn: {
      collection: 'Course', via: 'students'
    }
  },

  /**
   * Enrolls a user in one or more courses.
   * @param  {Object}   options
   *            => courses {Array} list of course ids
   *            => id {Integer} id of the enrolling user
   * @param  {Function} cb
   */
  enroll: function (options, cb) {

    User.findOne(options.id).exec(function (err, theUser) {
      if (err) return cb(err);
      if (!theUser) return cb(new Error('User not found.'));
      theUser.enrolledIn.add(options.courses);
      theUser.save(cb);
    });
  }
};
```



### Adapters

Like most MVC frameworks, Sails supports [multiple databases]().  That means the syntax to query and manipulate our data is always the same, whether we're using MongoDB, MySQL, or any other supported database.

Waterline builds on this flexibility with its concept of adapters.  An adapter is a bit of code that maps methods like `find()` and `create()` to a lower-level syntax like `SELECT * FROM` and `INSERT INTO`.  The Sails core team maintains open-source adapters for a handful of the [most popular databases](), and a wealth of [community adapters]() are also available.

Custom Waterline adapters are actually [pretty simple to build](), and can make for more maintainable integrations; anything from a proprietary enterprise system, to an open API like LinkedIn, to a cache or traditional database.


### Connections

A **connection** represents a particular database configuration.  It includes an adapter, as well as information like the host, port, username, password, and so forth.  Connections are defined in [`config/connections.js`]().

```javascript
// in config/connections.js
// ...
{
  adapter: 'sails-mysql',
  host: 'localhost',
  port: 3306,
  user: 'root',
  password: 'g3tInCr4zee&stUfF'
}
// ...
```

The default database connection for a Sails app is located in the base model configuration (`config/models.js`), but it can also be overriden on a per-model basis by specifying a [`connection`]().


### Object Relational Mapping (ORM)

Sails comes installed with its default ORM called Waterline, a datastore-agnostic tool that dramatically simplifies interaction with one or more databases.  It provides an abstraction layer on top of the underlying database, allowing you to easily query and manipulate your data without writing nasty integration code.

For instance, in schemaful databases like Postgres, Oracle, and MySQL, models are represented by tables.  In MongoDB, they're represented by Mongo "collections".  In Redis, they're represented using key/value pairs.

But in each case, the code you write to create new records, fetch/search for existing records, update records, or destroy records is _exactly the same_.  Waterline allows you to query and join (or `.populate()`) records between models, _even if_ the data for each model lives in a different database.

This means that you can switch some or all of your app's models from Mongo, to Postgres, to MySQL, to Redis, and back again - without changing any code. For the times we still need database-specific functionality, Waterline provides a query interface that allows us to talk directly to our models' underlying database driver (see [.query()]() and [.native()]().)



### `sails.models`

If you need to disable global variables in Sails, you can still use `sails.models.<model_identity>` to access your models.

A model's `identity` is different than its `globalId`.  The `globalId` is determined automatically from the name of the model, whereas the `identity` is the all-lowercased version.  For instance, you the model defined in `api/models/Kitten.js` has a globalId of `Kitten`, but its identity is `kitten`. For example:

```javascript
// Kitten === sails.models.kitten
sails.models.kitten.find().exec(function (err, allTheKittens) {
  // We also could have just used `Kitten.find().exec(...)`
  // if we'd left the global variable exposed.
})
```

### Analogy

Imagine a file cabinet full of completed pen-and-ink forms. All of the forms have the same fields (e.g. "name", "birthdate", "maritalStatus"), but for each form, the _values_ written in the fields vary.  For example, one form might contain "Lara", "2000-03-16T21:16:15.127Z", "single", while another form contains "Larry", "1974-01-16T21:16:15.127Z", "married".

Now imagine you're running a hotdog business.  If you were _very_ organized, you might set up your file cabinets as follows:

+ **Employee** (contains your employee records)
  + `fullName`
  + `hourlyWage`
  + `phoneNumber`
+ **Location** (contains a record for each location you operate)
  + `streetAddress`
  + `city`
  + `state`
  + `zipcode`
  + `purchases`
    + a list of all the purchases made at this location
  + `manager`
    + the employee who manages this location
+ **Purchase** (contains a record for each purchase made by one of your customers)
  + `madeAtLocation`
  + `productsPurchased`
  + `createdAt`
+ **Product** (contains a record for each of your various product offerings)
  + `nameOnMenu`
  + `price`
  + `numCalories`
  + `percentRealMeat`
  + `availableAt`
    + a list of the locations where this product offering is available.


In your Sails app, a **model** is like one of the file cabinets.  It contains **records**, which are like the forms.  `Attributes` are like the fields in each form.



### Notes
+ This documentation on models is not applicable if you are overriding the built-in ORM, [Waterline](https://github.com/balderdashy/waterline).  In that case, your models will follow whatever convention you set up, on top of whatever ORM library you're using (e.g. Mongoose.)




<docmeta name="uniqueID" value="Models416997">
<docmeta name="displayName" value="Models">

