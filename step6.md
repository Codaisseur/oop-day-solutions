# Tracing the problem
Running the tests points you at the `userClass` module. The `users.js` module already uses the class, but nothing is currently exported.

# Solving the problem
Let's start by exporting a class:

```js
// src/data/userClass.js
module.exports = class User {
    constructor() {
    }
}
```

Now all tests pass, but we're not done. Our task is to extract the name property, so that it is only initialized in the parent class.

In `users.js` we see that the class has been renamed to `UserWithBirthday` and that it "extends" (read: "is a child class of") a class `User` which we are requiring from `userClass`. The constructor is calling the parent's constructor with `super()`. If we want to have access to the `name` in the parent class, we will have to pass it as an argument to the parent constructor. Also, we no longer have to worry about assigning it to a property in the `UserWithBirthday` class. So, it ends up looking like this:

```js
class UserWithBirthday extends User {
  constructor(name, birthday) {
    super(name)
    this.dateOfBirth = birthday
  }
  getBirthday() {
    return Date.parse(this.dateOfBirth)
  }
}
```

While the parent class looks like this:

```js
module.exports = class User {
    constructor(name) {
        this.name = name
    }
}
```

Now, all that is left is the `users2.js` module, which is currently not extending the parent class. In order to be consistent, let's rename that class to `UserWithBirthday` as well. Then we need to import and extend the parent class. This is an animation of the process:

![rename and extend](https://cd.sseu.re/rename-and-extend.gif)

And here is the final code:

```js
const User = require('./userClass')

class UserWithBirthday extends User {
  constructor(name, birthday) {
    super(name)
    this.birthday = birthday
  }
  getBirthday() {
    return this.birthday
  }
}

module.exports.data = [
  new UserWithBirthday("Ava", 638928000000),
  new UserWithBirthday("James", -60048000000),
  new UserWithBirthday("Danielle", 558662400000),
  new UserWithBirthday("Darnell", 393552000000)
]
```