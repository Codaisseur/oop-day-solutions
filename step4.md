# Tracing the problem
The failing test tells you that the users in `users.js` are incomplete/incorrect. Compare the two data files and you'll notice that `users2.js` contains a constructor function. Each user object in that modules is created using `new User(... , ...)`. The challenge is to implement a similar constructor function in the previous module.

# Solving the problem
The solution is straight-forward. However, we mustn't rely too much on copy-pasting. Remember that the users in `users.js` **should** have property `dateOfBirth`:

```js
function User(name, dateOfBirth) {
  this.name = name
  this.dateOfBirth = dateOfBirth
  this.getBirthday = function() {
    return Date.parse(this.dateOfBirth)
  }
}

module.exports.data = [
  new User("Ava", "1990-04-01"),
  new User("James", "1968-02-06"),
  new User("Danielle", "1987-09-15"),
  new User("Darnell", "1982-06-22")
]
```