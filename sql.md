## DB basics
- **Database**: a container to store organized data
    + Database softwares are **Database Management System** (DBMS)
- **Table**: a structured list of data of a specific type 
    + every table has a unique name
    + **name**: combination of several things including database name and table name
    + cannot have same table name under the same database but can have the same table name in a different database
- **Schema**: information about database and table layout and properties 
- **Column**: a single field in a table (particular piece of information)
    + Break up data correctly
    + Each column in a DB has an associated datatype
- **Row**: a record in a table
- **Primary Key**: a column (or set of columns) whose values uniquely identify every row in a table
    + Primary Key Rules:
        1. No 2 rows can have the same primary key value
        2. Every row must have a primary key value (PK columns may not allow NULL values)
    + Primary Key Best Practices
        1. Do not update values in primary key columns
        2. Do not reuse values in primary key columns
        3. Do not use values that might change in primary key columns

- **SQL** (Structured Query Language)
    + **Client-server DBMS** (Oracle, MySQL, Microsoft SQL server)
        1. **Server**: piece of SW responsible for all data access and manipulation, runs on database server (ex: Oracle DBMS)
        2. **Client**: piece of SW with which the user interacts (Oracle-provided tools, scripting language (Python, Perl), web application dev languages (PHP, JSP, ASP), programming languages (C, C++, JAVA) etc)
    + **PL/SQL** (Procedural Language/Structured Query Language): Oracle’s implementation of SQL
    + Client software for Oracle:
        1. **SQL*Plus**: Command-line tool 
        2. **SQL Developer**: Graphical client


## Basics
- For ***string*** values, use SINGLE quote `''`
- `NULL`: used to show that a column contains no value
    + **[NOTE]** `NULL` is *<u>no value</u>* as opposed to a field containing 0, or an empty string, or just spaces
    + columns destined to contain optional data are often created such that it can have **NULL** values
- Multiple SQL statements must be separated by <u>semicolons</u>
- SQL statements are <u>NOT</u> case sensitive (`SELECT` == `select` == `Select`)
    + Convention: use ***upper case*** for all **SQL keywords** and ***lowercase*** for ***column and table names***
- ***Fully qualified names***: prepending database name to table name and table name to column name
    ```
    SELECT table_name.column1 FROM table_name;
    ```
    `SELECT column1 FROM database_name.table_name;` exact same as `SELECT column1 FROM table_name;`
- Comments
    + **Inline**: `--`
    + **Multi-line**: enclose comment text with `/* */`
- Clause (Required and optional)
    + Required example: `FROM clause for SELECT statement`
    + Optional example: `ORDER BY for SELECT statement`

## SELECT statement
1. `SELECT`: 
    - query all rows of a column/columns from a table 
    - **[NOTE]** Just by `SELECT`, the *order of data* returned is **random**
    - it may or may not be the order in which the data was added to the table
    ```
    -- single column
    SELECT columnM FROM table_name;
    
    -- multiple columns: separate the column names by comma
    SELECT column1, column2, column3 FROM table_name;
    ```
2. `*`: wildcard
    - query all of the columns
    - **[NOTE]** Not recommended to use in unnecessary cases since retrieving unnecessary columns usually slows down the performance of retrieval
    ```
    SELECT * FROM table_name;
    ```
3. `DISTINCT`: query the *distinct rows* 
    ```
    -- single column
    SELECT DISTINCT columnM FROM table_name;
    
    -- muliple columns: query the unique combination of the columns
    SELECT DISTINCT columnM1, columnM2, columnM3 FROM table_name;
    ```

4. `ORDER BY`: sorting
    - Default ordering is ascending
    - Use `DESC` to sort in descending order
        + `DESC` ***ONLY*** applies to the column name that directly precedes it
        + When sorting in <u>descending order on multiple columns</u>, must specify `DESC` for each column
    - Oracle applies dictionary sort order == A is the treated the same as a
    - **[NOTE]** usually the columns used in an `ORDER BY` clause are ones that were selected for display 
    - HOWEVER this is NOT required, sorting data by a column that is not retrieved is perfectly <u>legal</u>
    ```
    -- select all rows of a column sorted in ascending order of columnN
    SELECT columnM FROM table_name ORDER BY columnN;
    
    -- select all rows of a column sorted in descending order of columnN
    SELECT columnM FROM table_name ORDER BY columnN DESC;

    -- select all rows of a column, first sorted by columnN1 then by columnN2 (both in ascending order)
    SELECT columnM FROM table_name ORDER BY columnN1, columnN2
    
    -- mixing up the order
    -- Select all rows of a column from table and first sort in descending order of columnN1 then ascending order of columnN2
    SELECT columnM FROM table_name ORDER BY columnN1 DESC, columnN2;

    -- select all rows of a column, ordered by sequence number (NOT ADVISED)
    -- sort by N1th and N2th column
    SELECT columnM FROM table_name ORDER BY N1, N2;
    ```

## WHERE (Filtering)
- `ORDER BY` clause comes after `WHERE` clause
- Oracle is case sensitive for matches (`'Fuse'` != `'fuse'`)
+ **[NOTE]** `NULL` == Unknown
    - when filtering for matches or non-matches, the database does not know whether or not null matches the specified value, and so the rows with `NULL` in the specified column will NOT be returned 
+ **[NOTE]** SQL vs Application Filtering
    - STRONGLY DISCOURAGED approach: 
        1. retrieve more data than that is actually required for the client application using SQL SELECT statement
        2. filter data at the application level by looping through the data that was sent
    - DBs are optimized for filtering
    -  making the client application do the database's job impacts application performance and creates applications that cannot scale properly
    - if data is filtered at the client, the server has to send unneeded data across the network connections, resulting in a waste of network bandwidth resources

+ **WHERE** clause operators

    | Operator 	| Description                                                |
    |-----------|------------------------------------------------------------|
    | `=`       | equality                                                   |
    | `<>`      | non-equality                                               |
    | `!=`      | non-equality                                               |
    | `<` 		| less than                                                  |
    | `<=` 		| less than or equal to                                      |
    | `>`       | greater than                                               |
    | `>=` 		| greater than or equal to                                   |
    | `BETWEEN`	| between 2 specified values (both ends are included)        |
    | `IS NULL`	| check for columns with NULL values                         |
    | `IN`		| specify a range of conditions, any of which can be matched |

    ```
    -- select column1 and column2 of rows in table where column3 is “value”
    SELECT column1, column2 FROM table_name WHERE column3 = value;

    -- select rows of column1 in table where column2 is between 5 and 10
    SELECT column1 FROM table_name WHERE column2 BETWEEN 5 AND 10;
    ```

+ Logical Operators (`AND` & `OR`):
    - combine multiple WHERE clauses
    - order of preference: `()` > `AND` > `OR` 
    ```
    SELECT column1 FROM table_name WHERE column2 = value2 AND column3 <= value3
    SELECT column1 FROM table_name WHERE column2 = value2 OR column3 = value3
    
    -- where column2 is value 2 or (column3 is value3 and column4 is value4)
    SELECT column1 FROM table_name WHERE column2 = value2 OR column3 = value3 AND column4 = value4;

    -- where (column2 is value 2 or column3 is value3) and column4 is value4
    SELECT column1 FROM table_name WHERE (column2 = value2 OR column3 = value3) AND column4 = value4;
    ```


+ **list**: elements enclosed by `()`
+ **[NOTE]** list of `OR`'s vs `IN` clause
    - `IN` executes quicker than list of `OR`'s
    - coding with `IN` is cleaner
    - `IN` operator can contain another `SELECT` statement, enabling highly dynamic `WHERE` clauses
    ```
    -- Where column2 is value 2 or value 3 or value 4    
    SELECT column1 FROM table_name WHERE column2 in (value2, value3, value4);
    ```
+ `NOT` operator: negates the condition coming next
    ```
    SELECT column1 FROM table_name WHERE column2 NOT IN (value2, value3, value4);
    SELECT column1 FROM table_name WHERE column2 NOT BETWEEN value1 AND value2;
    SELECT column1 FROM table_name WHERE NOT column2 IS NULL;
    SELECT column1 FROM table_name WHERE NOT column2 = value2;
    SELECT column1 FROM table_name WHERE NOT (column2 = value2 OR column3 = value3);
    ```

### Wildcard Filtering
- `LIKE` predicate: to use wildcards in search clauses
- **Wildcards**: special characters used to match parts of a value
    + `%`: match any number of occurrences of any character (represents 0, 1, or more characters at the specified location in the search pattern)
        - `%` cannot be used to match a row with the value NULL in the specified column
        - not even the clause `WHERE columnN LIKE '%'` will match a row with the value NULL as columnN
        - `%%` escape for % 
    + `_`: match just a single character
- **[NOTE]**
    1. Do not overuse wildcards
    2. Try not to use wildcards at the beginning of the search pattern unless absolutely necessary
        + search patterns beginning with wildcards are the slowest to process

    ```
    -- any value that starts with Jet (doesn't matter how many characters follow 'Jet')
    SELECT column1 FROM table_name WHERE column2 LIKE 'Jet%';
    
    -- escaping for %
    -- contains 100%
    SELECT column1 FROM table_name WHERE column2 LIKE '%100%%';

    SELECT column1 FROM table_name WHERE column2 LIKE '_ ton anvil';
    ```

### REGEXP_LIKE
- `REGEXP_LIKE`: 
    + looks for matches <u>within</u> the column values 
    + can also be used to match <u>entire</u> column values just like `LIKE` using `^` and `$` anchors
- Basic character matching
    ```
    -- Returning rows with 1000 included in the column
    -- Different since LIKE matches an entire column
    SELECT column1 FROM table_name WHERE REGEXP_LIKE(columnN, '1000');
    SELECT column1 FROM table_name WHERE columnN LIKE '1000';
    ```
- RegEx notes

    | Expression     | Description                                       | 
    |----------------|---------------------------------------------------|
    | `.`            | match any single character                        |
    | `|`            | match one or the other pattern (OR operator)      |
    | `[]`           | match one of specific characters                  |
    | `[^]`          | match any BUT the specified characters            |
    | `[0-9]`, [a-z] | match range of values (0 to 9, a to z)            |
    | `\`            | match special characters like . [] | - (escape)   |
    | `\\`           | match \
    ```
    -- Single character (.)
    -- If the column value includes a single character + 000, so like 1000, 2000 etc
    SELECT column1 FROM table_name WHERE REGEXP_LIKE(column2, '.000');

    -- OR (|) operator
    SELECT column1 FROM table_name WHERE REGEXP_LIKE(column2, 'PATTERN1|PATTERN2');

    -- column values including 1 ton or 2 ton or 3 ton
    SELECT column1 FROM table_name WHERE REGEXP_LIKE(column2, '[123] ton');
    -- column values including any single character except 1, 2, or 3 + ' ton'
    SELECT column1 FROM table_name WHERE REGEXP_LIKE(column2, '[^123] ton');

    -- Column values including '1', '2', or '3 ton' (oops)
    SELECT column1 FROM table_name WHERE REGEXP_LIKE(column2, '[1|2|3 ton]');

    -- find all rows with . inside the value for columnN
    SELECT column1 FROM table_name WHERE REGEXP_LIKE(columnN, '\.')
    ```

- **Character classes**

    | Class | Description                                       |
    |-------|---------------------------------------------------|    
    | `\d`	| any digit (same as `[0-9]`)                       |
    | `\D`	| any non-digit (same as `[^0-9]`)                  |
    | `\w`	| any letter or digit (same as `[a-zA-Z0-9]`        |
    | `\W`	| any non-letter or digit (same as `[^a-zA-Z0-9]`)  |
    | `\s`	| any white space character                         |
    | `\S`	| any non-white space character                     |

- **Repetition Metacharacters** (comes after character to be repeated)

    | Metacharacter | Description                                               |
    |---------------|-----------------------------------------------------------|
    | `*`       	| 0 or more matches                                         |
    | `+`		    | 1 or more matches (equivalent to `{1,}`)                  |
    | `?` 		    | 0 or 1 match (equivalent to `{0,1}`) => useful for plural |
    | `{n}`	    	| specific number of matches                                |
    | `{n,}` 		| no less than a specified number of matches                |
    | `{n,m}`	    | range of matches                                          |

    ```
    -- \( means (, \d means any digit, sticks? means stick or sticks, \) means )
    SELECT column1 FROM table_name WHERE REGEXP_LIKE(columnN, '\(\d sticks?\)');

    -- match any 4 consecutive digits 
    SELECT column1 FROM table_name WHERE REGEXP_LIKE(columnN, '\d{4}');
    ```

- **Anchor**: match text at specific locations

    | Anchor | Description                                                      |
    |--------|------------------------------------------------------------------|
    | `^`    | start of text <br> within `[]`, negates the set, otherwise refers to the start of a string |
    | `$`	 | end of text                                                      |

    ```
    -- any string starting with a digit or period (.)
    SELECT column1 FROM table_name WHERE REGEXP_LIKE(columnN, '^[1-9\.]');
    ```

## Calculated Fields
- **[NOTE]** Client vs Server formatting: always better to perform formatting on DB server rather than client because DBMS are efficient in it
### **Concatenation**:
+ Use `||` operator in Oracle 
+ Many other DBMS's allow to use `+` for concatenating strings
    ```
    -- Just by || will have a lot of extraneous spacing like 
    -- ACME		, (USA				)
    SELECT vend_name || ' , (' || vend_country || ')' FROM vendors;
    
    -- Use RTrim() function to remove any trailing spaces
    SELECT vend_name || ', (' || vend_country || ')'
    ```

### **Trim Operators**
| Operator | Description                                 |
|----------|---------------------------------------------|
| `LTrim`  | trims the left side (preceding) of a string |
| `RTrim`  | trims the right side (trailing) of a string |
| `Trim`   | trims both ends                             |

### Alias (keyword: AS) (aka Derived columns)
- Assigning names to calculated fields + alternative name for a field or value
- Renaming a column if the real table column name contains illegal characters (ex spaces)
- Expanding column names if the original names are either ambiguous or easily misread

```
-- vend_title will be the name of field returned
SELECT RTrim(vend_name) || ', (' || RTrim(vend_country) || ')' AS vend_title FROM vendors;
```

### Mathematical calculations
| Operator  | Description    |
|-----------|----------------|
| `+`       | addition       |
| `-`       | subtraction    |
| `*`       | multiplication |
| `/`       | division       |

```
SELECT prod_id, quantity * item_price AS total_price FROM orderItems;
```

### Testing calculations using dual
- By using `dual` as the table, can test functions and calculations
```
SELECT 3 * 2 FROM dual; -- returns 6
SELECT TRIM('     abc      '); -- returns 'abc'
```
## Functions
- Functions: less portable than SQL statements since every DBMS supports functions that others don't
- Types of function
    
    | Type          | Description                                                                    | Examples                                          |
    |---------------|--------------------------------------------------------------------------------|---------------------------------------------------|
    | Text 			| manipulate strings of text                                                     | trimming, padding, converting to upper/lower case |
    | Numeric       | mathematical operations on numeric data                                        | absolute numbers, algebraic calc                  |
    | Date and time | manipulate date and time values, extract specific components from these values | diff between dates and checking date validity     |
    | System 		| return info specific to the DBMS being used                                    | user login info, versions                         |

### Text functions
| Operator                      | Description                                                               |
|-------------------------------|---------------------------------------------------------------------------|
| `Length()`                    | returns the length of a string                                            |
| `Lower()`                     | converts string to lowercase                                              |
| `LPad(str, len [, ins])`      | pads char sequences to left of string (len = total length after padding)  |
| `LTrim()` 				    | trims white space from left of string                                     |
| `RPad()`						| pads spaces to right of string                                            |
| `RTrim()`						| trims white space from right of string                                    |
| `Soundex()`					| returns a string's SOUNDEX value                                          |
| `Substr(str, pos [, len])`	| returns substring of a string <br> index count starts from 1 but if 0 entered as position, same as 1. <br> if negative, substring starts from the index *p* ahead of tail
| `Upper()`         		    | converts string to uppercase                                              |

```
SELECT LPad('name', 6, ' ') FROM dual; -- '  name'
SELECT LPad('name', 11, 'ab') FROM dual; -- 'abababaname'
SELECT LPad('name', 11, NULL) FROM dual; -- NULL
```

## Sources
[Oracle PL/SQL in 10 minutes](https://learning.oreilly.com/library/view/oracle-plsql-in/9780768681987/)