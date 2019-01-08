# Tracing the problem
The failing assertions in the tests are pointing you in the right direction right off the bat. 

In the `printAges` function, a function `user.getBirthday` is called. That is an object property, whose value is a function. The other failing test also tells you that an object is missing that same property. So, let's go to `users.js` and add that function:

```js
module.exports.data = [
  {
    name: "Ava",
    dateOfBirth: "1990-04-01",
    getBirthday: function() {}
  },
  // ...snip...
```

Re-running the test tells you that now "users data 1" now has a user named "James" that is missing a property `getBirthday`. So, let's go ahead and add that function to all the objects.

# Solving the problem
If you add empty functions to all 8 user objects, you'll be back at a familiar place. Now everyone is `NaN` years old. A way to solve this would be to duplicate a bit. For example, the user "Ava" in `users2.js` could be defined like this:

```js
{
    name: "Ava",
    dateOfBirth: "1990-04-01",
    getBirthday: function() {
        return Date.parse("1990-04-01")
    }
}
```

This solution requires you to copy paste some of the values in your data. A better solution would be to use the `this` reference:

```js
{
    name: "Ava",
    dateOfBirth: "1990-04-01",
    getBirthday: function() {
        return Date.parse(this.dateOfBirth)
    }
}
```

The user "Ava" in `users2.js` will have a different implementation of the `getBirthday` method:

```js
{
    name: "Ava",
    birthday: 638928000000,
    getBirthday: function() {
        return this.birthday
    }
},
```
