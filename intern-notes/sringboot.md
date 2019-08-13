## Rest API
### Annotation on Class
- `@RestController`: Use this annotation to specify that the class represents a RESTful API
- `@CrossOrigin(origins = "http://localhost:____")`: When integrating with **AngularJS** which acts as the client

### Annotation on Method
- GET, POST, PUT, DELETE
    + `@GetMapping("/get_uri_pattern")`: defines the function as a GET method
    + `@PostMapping("/post_uri_pattern")`: defines the function as a POST method 
    + `@PutMapping("/put_uri_pattern")`: defines the function as a PUT method
    + `@DeleteMapping("/delete_uri_pattern")`: defines the function as a DELETE method
    + `@RequestMapping(path="/request_uri_pattern", method = {RequestMethod.METHOD, ...})`: function represents the request methods defined in the method variable - if the method variable is not defined, then all of the request methods are allowed 
    + `@ResponseStatus(HttpStatus.STATUS)`: Specifies the Http Status code that will returned as a response of the API call
        - `HttpStatus.CREATED` = for POST
        - `HttpStatus.NO_CONTENT` = for DELETE
        - `HttpStatus.NOT_FOUND` = for case when the queried item was not found in the database

### Annotation on Arguments
- Path Variable, Request Header, Request Parameter, Request Body
    + `@PathVariable`: 
        - Variable that is embedded inside the uri
        - The argument variable name and the variable embedded in uri of the mapping annotation should match
        - **[Example]**
            ```
            GetMapping("/{id}") public User getUserFromId(@PathVariable String id) {
                return customRepository.findUserById(id);
            }
            ```
    + `@RequestHeader`
        - Specifying that the variable is in the request header
        - The argument variable name must match the variable name in the request header
    + `@RequestParam`
        - Specifying that the variable is a request parameter 
        - The argument variable name must match the name of the request parameter
    + `@RequestBody`
        - Specifying that the request will have a JSON body which resembles the variable class structure

## Configuration
- ***Yaml file***(.yml): create app.properties file or application.yml file to specify configurations like server port, application name, datasource, profile etc (ex. Itinerary Core has four yml files, basic and 3 extending from the basic which are for different environments)

- **Configuration class**: access a property defined in the configuration file using `@ConfigurationProperties(key)` and specify that the class represents a configuration using the `@Configuration` annotation

## Mongo DB
- `@Id` notation on DTO: specify that a specific variable represents the id of the data
- ***Repository***: need to create a repository interface that extends the `MongoRepository<Dto , idType>` and define custom methods like `List<DTO> findBySpecificVariable(SpecificVariableType value)` 
- `@Autowired`: inside the RESTful API class, declare an instance of the custom repository with the `@Autowired` annotation to specify that the data for the DTO will be retrieved from the mongo DB 

## Exception Handler
- Use `@ControllerAdvice` when creating a global exception handler (instead of having each controller/api handle the exception that are thrown within themselves)
- Use `@Exception Handler({CustomException1.class, ...})` to specify the exception type(s) that the method handles

## pom.xml
- when defining versions of dependencies, it is a better idea to define the version of the dependency as a property and use the `$` to specify the version rather than hardcoding so that when ever the version changes, all the instances of the versions can be changed at once
- **[Example]**
    ```
        <properties>
            ...
            <springfox.version>2.8.0</springfox.version>
            ...
        </properties>
        <dependencies>
            ...
            <dependency>
                <groupId>io.springfox</groupId>
                <artifactId>springfox-swagger2</artifactId>
                <version>${springfox.version}</version>
            </dependency>
            <dependency>
                <groupId>io.springfox</groupId>
                <artifactId>springfox-swagger-ui</artifactId>
                <version>${springfox.version}</version>
            </dependency>
            ...
        </dependencies>
    ```

## Other annotations
- `@Query`
    + declares finder queries directly on repository methods
    + `value` (optional element): defines the JPA query to be executed when the annotated method is called
- `@Repository` 
- `@Autowired`
- `@Injected` 
- `@Component`
- `@Controller`
    + declaring that the class acts as a controller in Spring MVC 
- `@Service`
    + usually used for class that belongs to the service layer like business logic
- `@Bean`: Java bean
    + class used with instantiation
    + private fields
    + no args constructor + getters & setters
    + for classes with `@Component`, `@Service`, `@Repository`, `@Controller` annotations, they are already registered as Bean's so no need to add the `@Bean` annotation
