## Functional interfaces
### Unary operator (`UnaryOperator<T>`)
**Definition**: Operation that accepts a single input argument and returns result of the same type as its argument

**Example**:
``` 
UnaryOperator<String> operator = str -> return str + "; ";
```
### Consumer (`Consumer<T>`)
**Definition**: Operation that accepts a single input argument and returns no result (void)

**Example**:
```
Consumer<String> operator = System.out::print;
```

### Predicate (`Predicate<T>`)
**Definition**: Operation that accepts a single input argument and returns a boolean 

**Example**:
```
Predicate<String> predicate = str -> str.length() > 5;
```

### Bi-function (`BiFunction<T, U, R>`)
**Definition**: Function that accepts 2 arguments (type T and type U) and produces a result of type R

**Example**:
```
BiFunction<User, Item, String> bifunction = (user, item) -> user.getName() + " bought " + item.getName();
```

## Stream
### Stream vs Parallel Stream
- ***Stream***: sequential stream, each operation is done sequentially → set up time is much shorter than parallel
- ***Parallel stream***: each operation is applied to all items inside the stream simultaneously → useful when an API call has to be made for each item in a list and the total time of API calls is longer than the time to set up the parallel stream and call the API multiple times simultaneously

### Intermediate operations
- `Filter`
- `Limit`
- `Distinct`
- `Sorted`
- `map` vs `flat map`
    + `map`: given a stream of object, when one wants to change the type of the stream from that object to another type (ex. string of the object), use `map`
    + `flat map`: given a stream of object which has a collection within itself, when one wants to see all of the items inside that the object's collection, use `flat map`
    + Examples
    ```
        class Customer { 
            private String name; 
            private Integer age; 
            private List<Item> items; 
        } 
        // map example 
        List<Integer> ages = customers.stream().map(Customer::getAge).collect(Collectors.toList()); 
        // flat map example 
        List<Item> items = customers.stream().flatmap(customer -> customer.getItems().stream()).collect(Collectors.toList());
    ```

### Terminal operations
- **[NOTE]**: once a terminal operation is applied on a stream, that stream is closed so it cannot be reused!
- `Count`
- `anyMatch`, `allMatch`, `noneMatch` (short-circuit terminal operations)
    + use `anyMatch` instead of `filter().count() > 0`
- `findFirst`, `findAny`
- `forEach`
- `max`, `min`, `Optional`
    + `Optional<T>`: either data of Type T or null is inside
- `collect` and `Collector`

### Iterate

## Bean
- Java Beans:
    + class used with instantiation
    + private fields
    + no args constructor + getters & setters

## Java Persistence API (JPA)
- Java API specification describing the management of relational data in applications using Java Platform
- ***Persistence Entity***:
    + lightweight Java class whose state is typically persisted to a table in a relational database
    + instances of an entity correspond to individual rows in the table
    + entities typically have relationships with other entities, and these relationships are expressed through object/relational metadata
- JPA query syntax very similar to that of SQL, but operate against ***entity objects*** rather than directly with database tables
