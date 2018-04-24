---
title: Design Patterns
date: 2016-09-11 23:13:11
tags:
---
### MVVM
```
View|ViewController <===> View Model <===> Model

```

- Every View retains a View Model and a View Controller target while every View Model retains it`s View.
- In View Model we handle the logy of the View and the data.
- Since View retains ViewModel and ViewModel retains View, there is strong circle reference which cause memory leak.so all references must be weak both in View and ViewModel.
<!--more-->
## Creational Patterns


## Structural Patterns


## Behavioural Patterns
### Strategy pattern

![strategy](http://design-patterns.readthedocs.io/zh_CN/latest/_images/Strategy.jpg)

Define a family of algorithms, encapsulate each one, and make theminterchangeable. Strategy lets the algorithm vary independently fromclients that use it.

##### Main Idea
- Defines a protocol(interface) that contains the algorithm you want to encapsulate.It defines a method of a family of algorithm.
- Implement this protocol in various way that rewrite the algorithm in different ways.
- The context object representing the situation to apply certain algorithm. The context object contains a instance variable of type of the algorithm protocol.
- By invoke the method defined in the protocol of different context, can we get different algorithm under different context. Its a good way to reuse code as all algorithms are written in the implementations of protocol.

### Observer Pattern

![Observer Pattern](https://sourcemaking.com/files/v2/content/patterns/Observer.svg)

The observer pattern is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods. It is mainly used to implement distributed event handling systems. The Observer pattern is also a key part in the familiar model–view–controller (MVC) architectural pattern. The observer pattern is implemented in numerous programming libraries and systems, including almost all GUI toolkits.

##### Main Idea
- Protocol Subject defines methods: register, unregister, notify. The implementation object of Subject maintains a list of observers that are to notify.
- Register and Unregister method control the content of the list of observer.  Notify method iterate through the list and invoke observer`s update method with Subject`s data wanted to notify.
- Protocol Observer defines the update method that when invoked by subject and received the data to update , update the data in object to the data just received.
- Implementation of Observer maintains an instance variable of Subject.When Observer instance is created, Subject instance are take as default parameter. The subject instance must be a singleton among all observers.
- When observer`s update method is invoked by subject,observers get the data send by Subject. Observer can update the data stored as do other custom action. 
