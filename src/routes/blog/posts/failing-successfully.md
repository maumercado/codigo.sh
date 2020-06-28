---
title: Failing Succesfully
date: 2020-06-22T10:51:00.000Z
---

Opinions on error handling with javascript.

<!-- more -->

Throw early, throw fast!

As developers, it becomes second nature to attempt to handle errors in an application, especially if it is user-facing since we want our users to have a smooth experience. However, it is hard to account for all cases that might arise when using such an interface.

When building APIs or full-blown applications, it is also essential to recognize that there are errors beyond our control and that sometimes erroring becomes crucial.

Unnecessary error handling can lead to more lines of code than initially required, which, by extension, can lead to more errors.

Let's see an example.

```js
async function doMagicalThing (arg) {
   // This function never throws
  // that sounds awesome, right?
   if (!arg) return [null, null]
   try {
        const result = await anotherAmazingThing(arg)
        return [null, result]
    } catch (err) {
        return [err, null]
    }
}
```

That looks pretty neat, right? Yes, and no; There are different problems with such an interface.
1. How does a user know what the correct argument is
2. It ultimately sets the responsibility of handling errors to the API user, which is unaware of what those might be unless it completely understands what the function is doing internally.
3. The user is not aware that the handling of errors is up to its code.
4. It necessarily requires more code to use such an API.
5. Debugging this type of API can be daunting for the user must be aware of the context, and functionality that the original author desired.

Here's a representation on how the code that uses such an API might look like:

```js
async function main () {
    const [err, result] = await doMagicalThing('something')
    if (err) {
    // Im guessing nothing happens here?
        // should I handle all the errors possible here?
 console.error(err)
        throw err // ?
    }
    // What happens if the response is null?
    if (result) {
       return result
    } else {
        return null
     }
}
```

Now let's see a simpler version of the code above.

```js
async function failsFast (arg) {
   assert(arg && typeof arg === string, 'arg is required. String')
    try {
        const result = await anotherAmazingThing(arg)
        return result
    } catch (err) {
        if (err.response || err.code === 'Something') {
            console.log('Maybe do X')
            return null
        }
        throw err
    }
}
```

Now let's see a function that uses `failsFast`

```js
async function main () {
    try {
        // now I know I need to send a string
        const resp = failsFast('string')
        return resp
    } catch (err) {
     // I can do EXTRA validation here.
        return {something: 'here'}
    }
}
```

Our internal API `failsFast` fails immediately with a clear error if the user does not send the required arguments.
It explicitly handles a specific error, so a user of such API has enough information to handle errors better.
The amount of code to use the API properly gets minimized, which means less code to maintain, less surface for other bugs.

The faster you `throw` or `assert`, the less operation your system will do.

In summary, handle errors in your APIs or applications whenever necessary; if not, then `throw` or `assert` fast.