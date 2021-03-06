# routes

**Required**: false

**Type**: Object

**Description**: Define route restrictions.

Inside the routes object, you can define many separate routes and set custom rules within them. The custom rules you can set are:

- **requestsPerExpiry**: *Integer* How many requests can be accepted until user is rate limited? Defaults to: `100`
- **completedActionsPerExpiry**: *Integer* How many completed actions can be accepted until the user is rate limited? This is useful for certain actions such as registering a user - they can have a certain amount of requests but a different (obviously smaller) amount of "completed actions". So if users have recently been successfully registered multiple times under the same IP (or other discriminator), they can be rate limited. They may be allowed 100 requests per certain expiry for general validation and the like, but only a small fraction of that for intensive procedures. Defaults to the value in `requestsPerExpiry` or `5` if not set.
- **expiry**: *Integer* How long should the requests be stored (in seconds) before they are set back to 0? If set to -1, values will never expire and will stay that way indefinitely or must be manually removed. Defaults to: `900` (15 minutes)
- **completedExpiry**: *Integer* How long should the "completed actions" (such as the amount of users registered from a particular IP or other discriminator) be stored for (in seconds) before it is set back to 0? If set to -1, such values will never expire and will stay that way indefinitely or must be manually removed. Defaults to the value in `expiry` or `900` (15 minutes) if not set.
- **errorMsgs**: *Object* Seperate error messages for too many requests and too many completed actions. They have been given the respective key names "requests" and "actions". This will be returned to the user when they are being rate limited. If no string was set in `requests`, it will default to `"As you have made too many requests, you are being rate limited."`. Furthermore, if a value has not been set in `completed`, it will resolve to the string found in `requests`. Or, if that wasn't set either, `"As you performed too many successful actions, you have been rate limited."` will be it's value.

## Example

``` javascript
routes: {
    //Overwrite default route rules - not all of the keys must be set,
    //only the ones you wish to overwrite
    default: {
        "expiry": 1000
    },
    exampleRoute: {
        requestsPerExpiry: 100,
        completedActionsPerExpiry: 5,
        expiry: 900,
        completedExpiry: 900,
        errorMsgs: {
            requests: "As you have made too many requests, you are being rate limited.",
            completed: "As you performed too many successful actions, you have been rate limited."
        }
    },
    //If not all keys are set, they will revert to
    //the default values
    exampleRoute2: {
        requestsPerExpiry: 500
    }
}
```