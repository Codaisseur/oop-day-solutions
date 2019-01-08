# Tracing the problem
All tests are failing. In `printAges.js` there is a function called `getBirthday` being used, and it's received as a second parameter to our `printAges` function:

```javascript
module.exports.printAges = (users, getBirthday) => {
    // ...
    const birthday = getBirthday(user)
    // ...
}
```

Who is passing that second parameter? Well, when we run the tests, the tests are. If you check out the `printAges.spec.js` file, you'll see that it is, in turn, getting the function from the data files. Both `users.js` and `users2.js` are now exporting an empty function.


# Solving the problem
Each of the exported functions is being used in combination with the users from the same file. So, it looks like each function only needs to handle its _own_ users.

A solution:

```js
// users.js
module.exports.getUserBirthDate = function(user) {
  return Date.parse(user.dateOfBirth)
}
```

and the other users
```js
// users2.js
module.exports.getUserBirthDate = function(user) {
  return user.birthday
}
```
