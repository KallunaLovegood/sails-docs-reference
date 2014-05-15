# sails new

`sails new <appName>` creates a new Sails project in a folder called **appName**.

##### Options:

  * `--no-linker` Disable automatic asset linking in your view and static HTML files (the relevant grunt tasks will not be created)
  * `--template=[template language]` Use a different template language than the default (e.g. `jade`).  Requires that a views generator for that language (e.g. `sails-generate-views-jade`) be installed in your global node path (e.g. `~/node_modules/` works).

> `sails new` is really just a special [generator]() which runs [`sails-generate-new`](http://github.com/balderdsahy/sails-generate-new).  In other words, running `sails new foo` is an alias for running `sails generate new foo`, and like any Sails generator, the actual generator module which gets run can be overridden in your global `~/.sailsrc` file.


<docmeta name="uniqueID" value="sailsnew912235">
<docmeta name="displayName" value="sails new">

