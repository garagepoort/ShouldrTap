ShouldrTap
==========

ShouldrTap is our own implementation of the observer pattern where the update method is specific for a certain event.


Why necessary?
==============

The Java Observer pattern has some flaws. One of its major flaws is that the update method just has an Object as parameter. There is no way to specify what kind of object should be past to the update method. This results in the use of instanceof.  Also if many different classes are observing the same class it is not always the case that one notification is meant for everyone. So a lot of observing classes execute there update method without doing anything in them except for checking if they have to do something based on the class of the object being passed.


Class explanation:
=================

```java
public void test(){}
```

We tried to make a library that uses specific events and event handlers.
Class overview

    Shoulder
    Tapper
    AbstractEvent

Shoulder
========

Shoulder can be considered as the event handler.  It uses generics to determine what kind of event it can handle.  When extending the Shoulder class, the update(T event) method should be overwritten. The constructor should also be implemented and the argument given to super() should be the classtype of the event this shoulder can handle.
