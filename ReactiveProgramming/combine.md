# Combine
Customize handling of asynchronous events by combining event-processing operators.

## Overview
The Combine framework provides a declarative Swift API for processing values over time. These values can represent many kinds of asynchronous events. Combine declares publishers to expose values that can change over time, and subscribers to receive those values from the publishers.

+ The Publisher protocol declares a type that can deliver a sequence of values over time. Publishers have operators to act on the values received from upstream publishers and republish them.
+ At the end of a chain of publishers, a Subscriber acts on elements as it receives them. Publishers only emit values when explicitly requested to do so by subscribers. This puts your subscriber code in control of how fast it receives events from the publishers it’s connected to.

You can combine the output of multiple publishers and coordinate their interaction. For example, you can subscribe to updates from a text field’s publisher, and use the text to perform URL requests. You can then use another publisher to process the responses and use them to update your app.

By adopting Combine, you’ll make your code easier to read and maintain, by centralizing your event-processing code and eliminating troublesome techniques like nested closures and convention-based callbacks.

## Understanding the problem
After seing the overview above, lets breakdown each of the terms and types typed above.

First of all, we need to understand whay is a publisher and a subscriber.

### Publisher
It’s a protocol that defines the values type that are going to be emitted over time. The events that a Publisher can emit are a value, an error, and a successful completion.

The protocol has two associated types that must be defined:
+ Output: The value types that will be sent. It could be a String, an Int, a custom Struct, etc.
+ Failure: The error type to use when we want to emit a failure event.

### Subscriber
A Subscriber can receive the events that a Publisher emits and perform the task needed with those events.

It’s also a protocol and, as the Publisher, has two associated types:
+ Input: The value type that the Subscriber is going to receive.
+ Failure: The error type when the Publisher emits a failure event.

> **Note** Both the Input and the Failure must match the Publisher’s Output and Failure types.

### Operators
There are methods that can transform the values that a Publisher emits. We can chain as many operators as we want. For example, if we're making a network request and we get a 500 error, we could capture that error, transform it to a custom Error type, and send that value.


### Combine in Action
We will be using AnyPublisher type, which is an implementation of the Publisher protocol that we can use to return the Publisher instance that we want.

**Also, the sink method allows us to process the values that the Publisher sends.**

### Convinience Publishers
We can take advantage of some of the built-in Publishers that Combine provides instead of creating our own. Let’s see the most common ones.

### Just
Emits a value only one time, then finishes. This type of Publisher is useful when we want to replace an error with some default value.

### Fail
The Publisher ends with a specific error. For example, if in our previous example we wouldn’t want to return a default value when getting an error but a specific error instead, we can use the Fail Publisher.

### Future
It might or might not emit a value and finish or fail. It’s useful for asynchronous code, like API calls. We need to fulfill a promise with some result, it could be successful or failure. If the promise is fulfilled, that promise is sent to the subscriber.

### Subjects
It’s a subclass of the Publisher protocol that can emit values on demand to the Subscribers by calling the send(_:)method. The framework already has two Subjects implemented that are ready to go.

### PassThroughSubject
What the PassThroughSubject basically does, is emits new values to the subscribers without storing any state or values itself.

### CurrentValueSubject
It’s very similar to PassThroughSubject with a key difference, CurrentValueSubject does store the actual value and it publishes it to the subscribers whenever that value changes. Because it stores the current value, any new subscriber will receive that value at the moment of the subscription.

### Reference
https://developer.apple.com/documentation/combine
https://blorenzop.medium.com/introduction-to-combine-framework-in-swift-4e50ccd6afe2#:~:text=Introduction%20to%20Combine%20framework%20in%20Swift&text=Combine%20is%20Apple's%20framework%20for,pattern%20or%20in%2Dblocks%20callbacks.
