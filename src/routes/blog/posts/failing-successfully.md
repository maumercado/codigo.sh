---
title: Failing Succesfully
date: 2020-06-22T10:51:00.000Z
---

Opinions on error handling with javascript.

<!-- more -->

### Throw early, throw fast!

As developers, it becomes second nature to attempt to handle most errors in an application, which might lead the creation of interfaces that simply do not fail (error hiding), which leads to longer and harder to debug code when used by other developers.

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
* How does a user know what the correct argument is
* It ultimately sets the responsibility of handling errors to developers using this API, since they will are unaware of what those errors might be, unless they completely understand what the function is doing internally.
* A developer using this API, is also completely unaware that the handling of errors is up to its code.
* It necessarily requires more code to use such an API.
* Debugging this type of API can be daunting for the user must be aware of the context, and functionality that the original author desired.

Here's a representation on how the code that uses `doMagicalThing` might look like:

```js
async function mainUserOfMagic () {
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

Now let’s see a version of the `doMagicalThing` function with a simpler API

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

And here is an example on how to use the above function `failsFast`

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