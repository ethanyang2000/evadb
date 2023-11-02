# Project Introduction
This project aims to design and implement REST APIs for EvaDB. With REST APIs, developers can access EvaDB with HTTP requests and JSON payloads with no need of EvaQL expertise.
# Implementation details
Generally, all HTTP handler functions use the input parameters from both URLs and request bodies to construct EvaQL query string. Then these queries are executed using Python API.

For SELECT statement, the response is set to be streaming, in case that a large table is returned.
## endpoints
This project maps EvaQL queries into endpoints using Python Flask. The endpoints can be mainly categorized into table, entry, functions, and others.
### table
For all table resources, I use /table/{tableName} as the URL, where {tableName} is a parameter to specify which table we are trying to manipulate. It should be a string located in URL path. We have two methods for this URL, POST and DELECT. 

The POST method is responsible for creating a table based on the JSON object provided in the request body. The JSON should have a field named columns with a dictionary as its value. Each key-value pair in the dictionary refers to a column in the table, and the key refers to the column name while the value refers to the data type.

The DELECT method is responsible for dropping the table by the name provided in the URL. 
### entry
For all entry resources, I use /entry/{tableName}, where the {tableName} is also a string parameter in the URL path. 

The POST method is responsible for inserting a row into the specified table. The detailed values to insert is stored as a dictionary where keys are column names and values are values to insert. This dictionary should be included in the request body as the value of a field value_to _insert in a JSON object.

The DELECT method is responsible for deleting rows in a table. This method accepts an additional string parameter named predicates as an URL query. It is used for selecting specific rows in the table (i.e., the WHERE clause in EvaQL).
### function
For all function resources, I use /function/{functionName}, where {functionName} is a string parameter in the URL path to specify the function name we are trying to access.

The POST method is responsible for creating a function. The JSON object in the request body requires four fields: input, output, type, and impl. Values of the input and the output fields are dictionaries to specify the data names and types. The field type requires a string to specify the function type and the field impl also requires a string to specify the path of file which provides the implementation.

The DELECT method is responsible for deleting a function by name.
### others
This category includes remaining EvaQL quires.

The GET method of /show refers to the SHOW statement, which lists the registered user-defined functions.

The POST method of /explain refers to the EXPLAIN statement, which lists the query plan associated with a EvaDB query. The query to be explained should be provided as a string value of the query field in the request body JSON object.

The POST method of /load/{tableName} is responsible for loading files into a table. The in-path parameter {tableName} specifies the name of the table to load. Three fields are required in the JSON object of the request body: type, path, and columns. The value of type, which specifies the type of file to load, is one of these: {“VIDEO”, “IMAGE”, “CSV”}. And the value of path specifies the path of the file. If the type is “CSV”, the column names and data types are also required to create the table. They should be provided as a dictionary in the columns field in request body.

The POST method of /select/{tableName} is responsible for selecting rows from a table. The in-path parameter {tableName} specifies the table to select from. And a JSON object with fields of where, orderBy, and columns in the request body is required. The value of columns is a dictionary, where each key refers to a column name we want to select. The value of where is a string that is used as predicates of SELECT. The value of orderBy is also a string to specify the column name that we want to order the outputs by.
# Future work
For the next EvaDB project, some potential tasks can be:

Detailed documentation with sample cases.

Parse the response from EvaDB to create fine-grained responses for APIs.

Implement test cases with higher coverage.

Develop monitoring modules to measure API usage.
