## Jackson
- `@JsonIgnoreProperties`: not to include in JSON output or set even if they were included
- `@JsonIgnoreProperties(ignoreUnknown=true)`: to ignore any unknown properties in JSON input without exception
- `@JsonProperty("JSON_PROPERTY_NAME)`: maps the specified JSON field to the variable
- `@JsonInclude`: 
    + indicate when value of the annotated property or all properties of the annotated class, is to be serialized
    + without this annotation, properties values are always included
    + BUT by using the annotation, one can specify simple exclusion rules to reduce amount of properties to write out
    + **[NOTE]** inclusion criteria value() is checked on Java Object level for the annotated typed NOT on JSON output → even with `JsonInclude.Include.NON_NULL`, it is possible that JSON null values are outputted, if object reference in question is not null
    + To base inclusion on value of contained value(s) → need to specify content() annotation
    
    **[EXAMPLE]** Specifying only `value()` as `JsonInclude.Include.NON_EMPTY` for a Map would execute Maps with no values, BUT would include Maps with null values 
    
    => to exclude Map with only null value:
    ```
    @JsonInclude(value=Include.NON_EMPTY, content=Include.NON_NULL)
    ```

## Lombok

## Validator


## Java Lang Annotation
- `@Target`: 
    + indicates the contexts in which an annotation type is applicable (use when making a custom annotation)
    + specify the contexts as an array of `ElementType`

- `@Retention`
    + indicates how long annotations with the annotated type are to be retained
    + specify how long by `RetentionPolicy` 

- `@Documented` 
    + indicates that annotations with a type are to be documented by java doc and similar tools by default
    + if a type declaration is annotated with `@Documented`, its annotations become part of the public API of the annotated elements

## Java X Validation (Constraints)
- `@Pattern`: annotated `CharSequence` must match the specified regEx



## Swagger Doc
- `@ApiModelProperty`: adding and manipulating data of a model property
    + optional elements
        - `required`: defines if the property is required or not
        - `example`: sample value for the property → use it with actual example would allow the user to see how many digits, patterns, etc for the field
        - `dataType`: specify the data type of the property
 - `@ApiModel`