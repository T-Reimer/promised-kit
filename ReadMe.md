## Promised-Kit

Promised-Kit is a libary of common promise values that I use in my projects.
Feel free to use them or contribute to improve the set.
I would consider adding more popular promise features by request on the
condition that each feature
stays in it's own respective file to minimize the impact it has on size when
a project only uses one of the features.

## GateKeeper

```
const {GateKeeper} = require("promised-kit/GateKeeper");
```

GateKeeper is a simple setup function to bundle concurrent requests together
into one request. You can call the function it creates multiple times and it
would only run the main function once (until the promise settles) and return
the same values to all of the calls.
It uses `deep-equal` on the arguments passed in to combine the requests,
meaning that in effect each different set of requests generate their own
promises.

```
    const get = GateKeeper(async (userId) => await getUser(userId));

    get(12);
    get(12);

    get(20);
```

The above example would call the final `getUser()` request twice. Once for user id 12 and once for 20.

On the other hand once the original promise has resolved it doesn't hold unto the final value but will run again.

```
    const get = GateKeeper(async (userId) => await getUser(userId));

    get(12).then(() => {
        get(12);
    });
```

This example would call the `getUser` function twice as the first request already finished by the time the second
request was received.
