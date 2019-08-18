## Class
+ encapsulation of data and methods that operate on that data
+ provide encapsulation, which reduces the conceptual burden of writing code
+ enable code reuse, through the use of inheritance and polymorphism

## Design Pattern
+ general repeatable solution to a commonly occurring problem
+ in OOP, design patterns address reuse and maintainability

## Template method pattern vs Strategy pattern
+ Similarity
    - behavioral patterns
    - used to make algorithms reusable
    - general and very widely used
+ Difference
    1. Template method: 
        - a skeleton algorithm is provided in a superclass
            + subclasses can override methods to specialize the algorithm
        - the superclass algorithm may have "hooks" - calls to placeholder methods that can be overridden by sbuclases to provide additional functionality
            + sometimes a hook is not implemented, thereby forcing subclasses to provide additional functionality
            + sometimes it offers a "no-operation" or some baseline functionality
    2. Strategy:    
        - typically applied when a family of algorithms implements a common interface
            + these algorithms can then be selected by clients
+ Examples:
    1. Template method
        - quicksort: subclasses can implement:
            1. their own selection algorithm (ex: using randomized median finding, selecting an element at random, etc)
            2. their own partitioning method (ex: using the DNF partitioning algorithms, etc)
    2. Strategy 
        - quick sort: pass in an object that implements a compare method into quicksort

## Observer pattern
+ a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically
+ Observed and Observer objects
    1. **Observed object** must implement
        - register an observer
        - remove an observer
        - notify all currrently registered observers
    2. **Observer object** must implement
        - update/notify the observer
+ Example:
    1. service that logs user requests and keeps track of the 10 most visited pages
    2. multiple client applications that use this info (ex: leaderboard display, ad placement algos, recommendation system)
    - instead of having the clients poll the service, the service (== observed object) provides clients with register and remove capabilities
    - as soon as its state changes, the service enumerates through registered observers, calling each observer's update method

## Push vs Pull Observer pattern
1. Push:
    - observed objects push information to their observers
    - observed object notifies the observer that its data is ready and includes the relevant information that the observer is subscribing to
    - leaves all information transfer in the subject's control
        + the subject calls update for each observer and passes the relevant information along with this call
    - more object-oriented: the subject is pushing its own data out, rather than making its data accessible for the observers to pull
    - simpler and safer: subject always knows when the data is being pushed out to observers == do not need to worry about an observer pulling data in the middle of an update to the data (-> requires synchronization)

2. Pull:
    - observers pull information they need from the observed object
    - the observer's job is to retrieve the information from the observed

    - heavier load on the observers
    - allows the observer to query the subject only as often as is needed
    - by the time the observer retrieves the information from the subject, the data could have changed 
    - less responsibility on the subject for tracking exactly which information the observer needs, as long as the subject knows when to notify the observer
    - requires the subject to make its data publicly accssible by the observers
    - best works when the observers are running with varied frequency and it suits them best to get the data they need on demand 

## Singleton pattern vs Flyweight pattern
+ Definition
    1. Singleton:
        - ensures a class has only 1 instance
        - provides a global point of access to the single instance
    2. Flyweight:
        - minimizes memory use by sharing as much data as possible with other similar objects
        - way to use objects in large numbers when a simple repeated representation would use an unacceptable amount of memory
        - since multiple clients may refer to the same flyweight object, for safety flyweights should be *immutable*
+ Similarity
    - keep a singly copy of an object
+ Difference
    1. Singleton
        - used to ensure all clients see the same object
        - used where there is a single shared object (ex: database connection, server configurations, a logger etc)
        - not immutable (ex: requests can be added to the database connection object)
        - creational pattern
        - global variable
    2. Flyweight:
        - used to save memory
        - used where there is a family of shared objects (ex: objects describing character fonts, nodes shared across multiple BSTs)
        - invariable immutable
        - structural pattern
        - pointer to a canonical representation
+ Example
    1. Singleton:
        - Logger (all code should log to a single place)
    2. Flyweight:
        - String interning: a method of storing only one copy of each distinct string value
            + makes some string processing more time- or space-efficient at the cost of requiring more time when the string is created or interned
            + the distinct values are usually stored in a hash table
+ Note
    - sometimes a singleton object is used to create flyweights 

## Class Adapter Pattern vs Object Adapter Pattern
+ **Adapter Pattern**:
    - allows the interface of an existing class to be used from another interface
    - used to make existing classes work with others without modifying their source code
+ Definition
    1. Class adapter pattern:
        - building an adapter via subclassing
        - adapter inherits both the interface that is expected and the interface that is pre-existing
        - allows re-use of implementation code in both the target and adaptee
            + adapter does NOT have to contain boilerplate pass-throughs or cut-and-paste reimplementation of code in either the targer or the adaptee
        - has disadvantages of inheritance (ex: changes in base class may cause unforeseen misbehaviors in derived classes etc)
        - can be used in place of either the target or the adaptee
            + advantage if need for a 2-way adapter
            + disadvantage otherwise as it dilutes the purpose of the adapter and may lead to incorrect behavior if the adapter is used in an unexpected manner
        - allows details of the behavior of the adaptee to be changed by overriding the adaptee's methods
    
    2. Object adapter pattern:
        - building an adapter via composition
        - adapter contains an instance of the class it wraps and the adpater makes calls to the instance of the wrapped object
        - as members of the class hierarchy, tied to specific adaptee and target concrete classes
        - purer in its approach to the purpose of making the adaptee behave like the targe
            + by implementing the interface of the target only, the object adapter is only useful as a target
        - use of an interface for the target allows the adaptee to be used in place of any prospective target that is referenced by clients using that interface
        - use of composition for the adaptee similarily allows flexibility in the choice of the concrete classes
            + if adaptee is a concrete clas, any subclass of adaptee will work equally well within the object adapter pattern
            + if adaptee is an interface, any concrete class implementing that interface will work
        - disadvantage: if target is not based on an interface, target and all its clients may need to change to allow the object adapter to be substituted 

+ Example
    1. Object adapter pattern
        1. have legacy code that returns objects of type `stack`
        2. new code expects inputs of type `deque` (== more general than `stack` but does NOT subclass `stack`)
        - create a new type, `stack-adapter`, which implements the deque methods, and can be used anywhere deque is required
            + the `stack-adapter` class has field of type `stack` == **object composition**
            + implements the deque methods with code that uses methods on the composed `stack` object
            + deque methods that are not supported by the underlying stack throw unsupported operation exceptions

## Creational Patterns
1. Builder Pattern
    - build a complex object in phases
    - avoids mutability and inconsistent state by using an mutable inner class that has a build method that returns the desired object
    - benefits:
        1. breaks down the construction process
        2. can give names to steps 
        3. compared to a constructor, deals far better with optional parameters and when the pattern list is very long

2. Static Factory Pattern
    - function for construction of objects
    - benefits:
        1. function's name can make what it is doing much clearer compared to a call to a constructor
        2. function is not obliged to create a new object == it can return a flyweight, a subtype that is more optimized (ex: can choose to construct an object that uses an integer in place of a Boolean array if the array size is not more than the integer word size)

3. Factory Method Pattern
    - defines interface for creating an object, but lets subclasses decide which class to instantiate
    - disadvantage: makes subclassing challenging

4. Abstract Factory Pattern
    - provides an interface for creating families of related objects without specifying their concrete classes
    - allows to interchange concrete implementations without changing the code that uses them, even at runtime
        + price for this flexibility == more planning and upfront coding, as well as code that may be harder to understand, because of the added indirections