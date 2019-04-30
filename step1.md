# Tracing the problem
In the beginning, all tests fail. In the `printAges.js` module you see the text being constructed. Instead of a number, the age is noted as `NaN`. So, something is wrong with the calculation.

If you haven't spotted it yet, you could add one or more `console.log` statements to see the values of the different variables. However, the hints in the assignment should quickly alert you to a function call that looks like this:

```js
const birthday = getUserBirthDate(user)
```

Where is this function coming from? Your editor should be able to help you here. You'll notice at the top of the file there is this line:

```js
const getUserBirthDate = require('./data/getUserBirthDate')
```

Now we've traced the problem to its source. In the `src/data/getUserBirthDate.js` file a function is being exported, but it doesn't have an implementation yet.

# Fixing the problem
The empty function receives a single user object each time. By looking at `printAges.js` it looks like the function should return a number:

```js
const howLongTheyLived = currentDate() - birthday
```

Since we're using `birthday` in an arithmetic expression, its value should be a number. Currently, `birthday` always has the value `undefined` because the empty function never returns anything. That explains the `NaN`.

So, given a user, we should return a number. Just to see what happens. Let's return a number, any number:

```js
// src/data/getUserBirthDate.js
module.exports = function(user) {
    return 0
}
```

Re-running the tests should show you a different error. Now everyone is the same age, namely 48.

Our user objects (`users.js`) have a `dateOfBirth` property, of type string. We can convert that to a string to a number, so let's do that:

```js
// src/data/getUserBirthDate.js
module.exports = function(user) {
    return Date.parse(user.dateOfBirth)
}
```

Re-running the tests now shows us that **half** of all the tests pass. Looks like we made it work for the users in `users.js`, but not the ones in `users2.js`. Those users have a property `birthday`, and that property's value is already a number. So, we have to treat those users differently. In order to do that, we have to detect the type of the user using a conditional.

```js
// src/data/getUserBirthDate.js
module.exports = function(user) {
    if ("birthday" in user) {
        return user.birthday
    }
    return Date.parse(user.dateOfBirth)
}
```
