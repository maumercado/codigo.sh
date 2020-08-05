---
title: Circuit Breaker 
date: 2020-07-29T17:51:00.000Z
---

The Circuit Breaker Design Pattern - A tool that would prove invaluable when dealing with remote service failures.

<!-- more -->

Commonly, making remote calls it's a natural part of any system created, but those system can fail by any number of reasons.
The circuit breaker design pattern can help in these specific types of failures.

### Handling remote API failures

In the previous post [Failing successfully](./failing-successfully.md), it was shown that
the best strategy, when dealing with unknown errors, was to fail properly, and "noisily", instead than hiding them. 
This post will demonstrate how to properly handle failures when attempting to obtain information, or execute procedures on a remote/external API.

## What is a circuit breaker

> A Circuit breaker is a design pattern used in modern software development. It is used to detect failures and encapsulates the 
> logic of preventing a failure from constantly recurring, during maintenance, temporary external system failure 
> or unexpected system difficulties.
> -- [Wikipedia Circuit Breaker](https://en.wikipedia.org/wiki/Circuit_breaker_design_pattern)

If you are familiar with the concept of circuit breakers from electrical engineer, be aware of comparing that circuit breaker, 
with the software definition of it, since it's software analog, its simply smarter.

## So how does it work?

A circuit breaker has 3 states:

* Open
* Half Open
* Closed

This states have a set of variables to let allow the circuit breaker to make proper decisions on when to forward a request
or when to use a cached response or even when to simply fail the request to the service, without even attempting to connect to it.

The variables for this state are, a failure count which as its name implies keeps count of how many times a service has failed
to reply, a threshold of failures, which is simply a maximum amount of times an external service is allowed to fail, and a timeout value, 
which is the time in which a service should reply successfully.

### Closed state

This is the normal state, the circuit is closed, meaning any request destined to an external service goes through
the following steps:

1. The circuit breaker makes a call to the service.
2. If the call to the service is successful it will reset the failure count to 0.
3. If the call fails however, it will increment the failure count by 1, and compare
with the maximum threshold of allowed fails.
4. If the fail counter is greater than the maximum allowed failures, then the circuit is opened.

### Open state

Whenever a circuit breaker has detected that the number of failures for a given external service, is greater than the threshold allowed,
it automatically opens the circuit, however, this state can be automatically reset by doing random checks against the service.

This state behaves like so:

1. When in this state, no calls to the service are allowed, and a timeout is set
2. If a client makes a request, the circuit breaker can simply respond with a failure, or a cached 
value of a previous request.
3. Once the timeout has passed the circuit breaker goes to half open state.

### Half open state

1. In this state, the circuit breaker will allow for request to be made to the external service
2. If the request fails then the circuit will be flipped to open, waiting for the timeout again, to
check for the availability of the service once more.
3. If the request succeeds then the circuit is closed, allowing for new requests to be made,
it resets the failure counter and the timeout.


![Circuit Breaker Diagram](circuit-breaker-diagram.png "Circuit Breaker Diagram")

## Usage and customizations

Whenever dealing with remote services, this is a good pattern to use.

Imagine creating a web service, the service in order to obtain any information of the current user
needs to make a request to an external service.

In  normal circumstances whenever this service fail, one scenario is that the user will see an error, 
after a certain amount of time, let's say 30 seconds. 
What could also happen is that the user becomes desperate and creates request for the same information,
several times, opening sockets to the external services that will remain open for, let's assume another 30 seconds.

So now we have a user that has waited for 30 seconds, but at the same time has attempted to make the same request several times,
which can open several connections, to a set of microservices that deal with user information.
This would end up in the degradation of the user experience, as well as the backend system.

The reason the backend system is also affected is because there is a number of connections now open inside of a number
of microservices, that will close only after 30 seconds, depending obviously on what language used by this service,
the system settings etc.

Enter circuit breakers, with a simple one, your user will most likely see the first request as an error, and wait for
whatever the timeout threhold set for the circuit breaker to close the connection, and depending on the behaviour configured
by your circuit breaker, the user can even get a cached copy of the response, or your circuit breaker can even reply
immediatly with an error, which reduces the number of connections that would open on the backend if that wasn't the case,
and provides the user with a much faster experience, even tho' it might not be the desired one.

The circuit breaker it's written as a singleton, since it maintains the same state for all requests done by your application,
it would be pointless if a new instance is created every time a request were to be made, because the state would always then
be closed, which is the initial state of the circuit breaker.

## Code example

I made a presentation on the circuit breaker a while back, and the presentation contains a simple example of the circuit breaker
implementation in nodeJS. You can see that presentation [here](https://cbr.maumercado.com)

## Conclusion

The circuit breaker allows developer to write resilient applications.
Handling service request errors with a circuit breaker, can provide your users with a much better experience, and
at the same time reduce the amount of open connections, whenever a service has failed, since the circuit breaker does
not allow connections to a given service to be made, but instead it replaces it with an immediate error, or a cached response.

The circuit breaker has 3 states:
* Closed
* Open
* Half Open

It uses the singleton pattern in order to have a global knowledge of the external services states in the entire application.
