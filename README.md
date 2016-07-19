![joi Logo](https://raw.github.com/hapijs/joi/master/images/joi.png)

Object schema description language and validator for JavaScript objects.

This is a fork of version 6.10.1 of [hapijs/joi](https://github.com/hapijs/joi/tree/v6.6.1). The sole purpose for this fork was to allow for stricter string date validation, see [`string.isoDate(format)`](API.md/#stringisodate).

Lead Maintainer: [Richard Merry](https://github.com/rmerry)

# Example

```javascript
var Joi = require('joi');

var schema = Joi.object().keys({
    username: Joi.string().alphanum().min(3).max(30).required(),
    password: Joi.string().regex(/^[a-zA-Z0-9]{3,30}$/),
    access_token: [Joi.string(), Joi.number()],
    birthyear: Joi.number().integer().min(1900).max(2013),
    email: Joi.string().email()
}).with('username', 'birthyear').without('password', 'access_token');

Joi.validate({ username: 'abc', birthyear: 1994 }, schema, function (err, value) { });  // err === null -> valid
```

The above schema defines the following constraints:
* `username`
    * a required string
    * must contain only alphanumeric characters
    * at least 3 characters long but no more than 30
    * must be accompanied by `birthyear`
* `password`
    * an optional string
    * must satisfy the custom regex
    * cannot appear together with `access_token`
* `access_token`
    * an optional, unconstrained string or number
* `birthyear`
    * an integer between 1900 and 2013
* `email`
    * a valid email address string

# Usage

Usage is a two steps process. First, a schema is constructed using the provided types and constraints:

```javascript
var schema = {
    a: Joi.string()
};
```

Note that **joi** schema objects are immutable which means every additional rule added (e.g. `.min(5)`) will return a
new schema object.

Then the value is validated against the schema:

```javascript
Joi.validate({ a: 'a string' }, schema, function (err, value) { });
```

If the value is valid, `null` is returned, otherwise an `Error` object.

The schema can be a plain JavaScript object where every key is assigned a **joi** type, or it can be a **joi** type directly:

```javascript
var schema = Joi.string().min(10);
```

If the schema is a **joi** type, the `schema.validate(value, callback)` can be called directly on the type. When passing a non-type schema object,
the module converts it internally to an object() type equivalent to:

```javascript
var schema = Joi.object().keys({
    a: Joi.string()
});
```

When validating a schema:

* Keys are optional by default.
* Strings are utf-8 encoded by default.
* Rules are defined in an additive fashion and evaluated in order after whitelist and blacklist checks.

# API
See the [API Reference](API.md).
