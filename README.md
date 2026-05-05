# Fast-API

### Campusx

<a href="https://www.youtube.com/playlist?list=PLKnIA16_RmvZ41tjbKB2ZnwchfniNsMuQ"> LINK </a>

---


# FastAPI Summary

---

## Setup:-

1) make a virtual environment  
`python -m venv venv_name`

2) activate virtual environment  
`Source venv/bin/activate`  #(mac , linux)  
`venv\Scripts\activate`  #(windows)

3) install Libraries/Packages in that virtual env  
`pip install fastapi uvicorn pydantic`  
`pip install -r requirements.txt`   #(to install all the stuff in the requirements.txt)  
`pip freeze > requirements.txt`  #(to write all the packages/libraries you have in the requirements.txt folder)  

4) deactivate venv  
`deactivate`

- Coding Workflow:—  
1) activete venv  `venv\Scripts\activate`  
2) code , install/uninstall packages, run the app, etc..  
3) deactivate venv `deactivate`


---

## Run app:-

`uvicorn file_name:fastAPI_object_name --reload`

if main.py file has -> app = FastAPI()  
->  
`uvicorn main:app --reload`

---

## Using uv :-

1) `pip install uv`

2) Create a new Project folder and open it in vscode

3) `uv init .`   # to make the Project a uv project

4) `uv add fastapi uvicorn pydantic` # to install the library

5) `uv run file_name:fastAPI_object_name --reload` # Run using uv

---

### Project Structure : —

Main_Project_folder  
	   |-venv/  
	   |-main.py or app.py (this is the file where we write our API code)  
	   |-other files & folders (models.py, utils.py, database.py, etc..)  
	   |-requirements.txt  
	   |-.gitignore  
	   |-.env  
	   |-etc ….   

---

### Swagger UI Documentation:

Ex :- `http://localhost:8000/docs`  
Ex :- `http://website.com/docs`

disable in production :-

`app = FastAPI(docs_url=None, redoc_url=None, openapi_url=None)`


--------

## Import fastAPI Library

`from fastapi import FastAPI` # import the FastAPI class from the fastapi module  
`app = FastAPI()` # Create Object of FastAPI class called app (this is our main application object that we will use to define our routes and handle requests)



------------

## API Endpoints:


### Structure of API Endpoint:-

```
@app.request_name("/path") 
# Define the route using a Request method (GET, POST, PUT, DELETE) and the path ("/path") that will trigger this function when accessed. This is a decorator that tells FastAPI to execute the following function whenever a request is made to the specified path with the specified HTTP method.

def function_name(): 
# Define a function that will be executed when the specified route is accessed. This function will contain the logic to handle the request and return a response.
    
    return {status: ..., message: ...} 
    # Return a response in the form of a dictionary (or any other data structure) that will be sent back to the client when the route is accessed. The content of the response can be customized based on the requirements of your application.
```

1) @app.get("/path") -> GET (Read) is Used to Retrieve Data from the Server.  
2) @app.post("/path") -> POST (Create) is Used to Send Data to the Server to Create a New Resource / or when Using ML inference.  
3) @app.put("/path") -> PUT (Update) is Used to Update an Existing Resource on the Server.  
4) @app.delete("/path") -> DELETE (Delete) is Used to Remove a Resource from the Server.  

---

#### Root Endpoint:-

```
@app.method("/") # -> for root endpoint
def function_name():
    code...
    return ....
```

#### Path Endpoints:-

```
@app.method("/path/path") # -> for any other endpoint
def function_name():
    code...
    return ....
```

---

we can also use async functions to handle requests asynchronously, which can improve performance for I/O-bound operations.  

```
@app.request_name("/path")
async def function_name():
    return {status: ..., message: ...}
```

---
---

## Endpoint:  

### GET 

Takes a path, path parameters, query parameters, and returns a JSON response.

```
@app.get("/")
def read_root():
    return {"Hello": "World"}
```

```
@app.get("/items/{item_id}")
def read_item(item_id: int, q: str = None):
    return {"item_id": item_id, "q": q} 
```

---
---

## Path Parameters:

Dynamic Part of a URL, used as a identifier to access specific resource. 

mostly used in -> GET, PUT, DELETE requests

Ex:-

Client Request:- `http://localhost:8000/path/parameter`

```
@app.get("/path/{parameter}")  # define the path parameter
def function_name(parameter: data_type): 
    # mention the parameter in the function definition and specify its data type 

    # use the Parameter in the function logic to perform operations or retrieve data based on the parameter value
```

- parameter name must match in the URL and function parameter

```
@app.get("/path/{parameter_same_name}")
def function_name(parameter_same_name: str):
```

- We can use Multiple Path Parameters in a single endpoint 

```
@app.get("/path/{parameter1}/{parameter2}/....")
@app.get("/path/{parameter1}/path/{parameter2}/....")
```

---

## Path() Function:

Used to add additional metadata and validation rules to path parameters in FastAPI.   
It allows you to specify constraints, default values, and other properties for the parameters in your API endpoints.


`from fastapi import Path`

```
@app.get("/path/{parameter}")
def function_name(parameter: data_type = Path(..., title="Parameter Title", description="Description of the parameter", min_length=0, max_length=100, gt=0, lt=100, example=42, regex="^[a-zA-Z0-9]+$")):
    # The "..." indicates that the parameter is required. 
    # title: A human-readable title for the parameter, which can be used in API documentation.
    # description: A detailed description of the parameter, providing more context for API users.
    # min_length and max_length: Constraints on the length of string parameters.
    # gt (greater than) and lt (less than): Constraints on numeric parameters.
    # example: An example value for the parameter, which can be displayed in API documentation.
    # regex: A regular expression pattern that the parameter value must match.
```

Example:

```
@app.get('/view/{patient_id}')
def view_patient(patient_id: str = Path(..., description="The ID of the patient to view", example="P001")):
    data = load_data()

    if patient_id in data:
        return data[patient_id]
    else:
        return {"error": "Patient not found"}
```  

---
---

## Query Parameters:

Query parameters are additional pieces of information that can be sent in the URL to provide more context or specify certain conditions for the request.  

They come after the ? in the URL 

mostly used in GET requests to filter, sort, or paginate data.

Ex:-

Client Request:-  
`http://localhost:8000/path ? parameter1=value1 & parameter2=value2`

```
@app.get("/path") # No need to define query parameters in the decorator, as they are not part of the URL path.
def function_name(parameter: data_type = default_value):
    # The parameter is defined as a function argument with a default value (10,"abc",None,etc..), making it optional. 
    # If the client includes the query parameter in the request, its value will be passed to the function; otherwise, it will be None.
    
    # Use the query parameter in the function logic to filter, sort, or paginate data based on the parameter value.
```

dont do mistake -> http://localhost:8000/path / ?parameter=value  
(this is not valid since there should not be a "/" before the ? symbol in the URL structure for query parameters)


- we can use multiple query parameters in a single endpoint using &  
`URL: http://localhost:8000/path ? parameter1=value1 & parameter2=value2 & parameter3=value3`


```
@app.get("/path")
def function_name(parameter1: data_type = default_value, parameter2: data_type = default_value, parameter3: data_type = default_value):
```

- Rule: The variable name in your function must match the key used in the URL. 

`URL: http://localhost:8000/path ? abc1=value1 & abc2=value2 & abc3=value3`

```
@app.get("/path")
def function_name(abc1: data_type = default_value, abc2: data_type = default_value, abc3: data_type = default_value):
    # based on the names of the query parameters in the URL, FastAPI will automatically assign the values to the corresponding function parameters.
    # If the client does not include a query parameter in the request, the function will use the default value specified in the function definition.
```

- we can also write the query parameters in any order in the URL & function parameter since they are assigned based on their names.

`URL: http://localhost:8000/path ? abc2=value2 & abc1=value1 & abc3=value3`  

```
@ app.get("/path")
def function_name(abc3: data_type = default_value, abc1: data_type = default_value, abc2: data_type = default_value):
    # FastAPI will still correctly assign the values to the corresponding function parameters based on their names, regardless of the order in which they appear in the URL or function definition.
```

---

## Query() Function:

Used to add additional metadata and validation rules to query parameters in FastAPI.   
It allows you to specify constraints, default values, and other properties for the query parameters in your API endpoints.

`from fastapi import Query`

```
@app.get("/path")
def function_name(query_parameter: data_type = Query(... or default_value, title="Parameter Title", description="Description")):
    # The "..." indicates that the query parameter is required.
    # The default value makes the query parameter optional, so even if the client does not include it in the request, the function will still execute with the default value. eg:- (10,"abc", None, etc..)
    # title: A human-readable title for the query parameter, which can be used in API documentation.
    # description: A detailed description of the query parameter, providing more context for API users.
    # min_length and max_length: Constraints on the length of string parameters.
    # gt (greater than) and lt (less than): Constraints on numeric parameters.
    # example: An example value for the query parameter, which can be displayed in API documentation.
    # regex: A regular expression pattern that the query parameter value must match.
```

Example:

```
@app.get("/users")
def search_items(sort: str = Query("asc", description="Sort order: 'asc' for ascending, 'desc' for descending", regex="^(asc|desc)$")):
   
    # load data

    if sort == "asc":
        # sort data in ascending order
        return data
    elif sort == "desc":
        # sort data in descending order
        return data
```


---
---


## Path Parameters VS Query Parameters:

Both are used to pass data to the server, but they serve different purposes and are used in different contexts:

#### 1) Path Parameters: 

- Used to identify specific resources in the database, etc. 
- They are part of the URL path and are typically used in GET, PUT, DELETE requests to access, update, or delete specific resources. 
- They are required and must be included in the URL.
- They are assigned based on their position in the URL, so the order of path parameters in the URL must match the order of parameters in the function definition.
- Ex: `http://localhost:8000/items/123` (where 123 is a path parameter representing the item ID)

#### 2) Query Parameters: 

- Used to filter, sort, or paginate data. 
- They are not part of the URL path but are included in the URL after a ? and are typically used in GET requests to provide additional context or specify conditions for the request. 
- They are optional and can be included or omitted by the client.
- They are assigned based on their names in the URL, so the order of query parameters in the URL does not matter as long as their names match the function parameters.
- Ex: `http://localhost:8000/items?sort=asc&limit=10` (where sort and limit are query parameters used to specify sorting order and the number of items to return)

---

## Path parameters + Query parameters

Both path parameters and query parameters can be used together in the same endpoint to provide a more flexible and powerful API.

IN the URL the path parameters come first, followed by the query parameters after the ? symbol.

`URL: http://localhost:8000/path_parameter1/path_parameter2 ? query_parameter1=value1 & query_parameter2=value2`

This is not valid since path parameters must come before query parameters in the URL structure.

`URL: http://localhost:8000/path/{path_parameter1} ? query_parameter1=value1 {path_parameter2} ? query_parameter2=value2`

```
@app.get("/path/{path_parameter1}/{path_parameter2}/....") # only define the path parameters in the decorator, as query parameters are not part of the URL path.
def function_name(
    path_parameter1: data_type, 
    path_parameter2: data_type, 
    query_parameter1: data_type = default_value, 
    query_parameter2: data_type = default_value):
    # The function can access both the path parameters and query parameters to perform operations or retrieve data based on the provided values.
```

here first the path parameters are assigned based on their position in the URL, and then the query parameters are assigned based on their names in the URL.

- Note:-

- Path parameters must always be defined in proper ORDER as per the order in URL. since they are assigned based on their position in the URL
- query parameters must always be SAME NAME in the URL and function parameter since they are assigned based on their names and they always come after the path parameters in the function definition.

can we do like this -> fun(path_para1, query_para1, path_para2, query_para2) ?   
-> Yes, we can define the function parameters in any order, but it is a common convention to list path parameters first followed by query parameters for better readability and clarity.

---

### with Path() and Query() functions for additional metadata and validation:

`URL: http://localhost:8000/path/{path_parameter1}/{path_parameter2} ? query_parameter1=value1 & query_parameter2=value2`

```
@app.get("/path/{path_parameter1}/{path_parameter2}/....")
def function_name(
    path_parameter1: data_type = Path(..., description="Description of path parameter 1", example="Example value for path parameter 1", regex="^[a-zA-Z0-9]+$", gt=0, lt=100, min_length=1, max_length=50),
    path_parameter2: data_type = Path(..., description="Description of path parameter 2", example="Example value for path parameter 2", etc...),
    query_parameter1: data_type = Query(... or default_value, description="Description of query parameter 1", example="Example value for query parameter 1", etc...),
    query_parameter2: data_type = Query(... or default_value, description="Description of query parameter 2", example="Example value for query parameter 2", etc...)
    ):
    # The function can access both the path parameters and query parameters with their respective metadata and validation rules to perform operations or retrieve data based on the provided values.
```

Example:-

`URL: http://localhost:8000/items/{section_id} ? sort=asc&limit=10`

```
@app.get("/items/{section_id}")
def read_item(
    section_id: int = Path(..., description="The ID of the section to retrieve", example=123), 
    sort: str = Query("asc", description="Sort order: 'asc' for ascending, 'desc' for descending", regex="^(asc|desc)$"), 
    limit: int = Query(10, description="The number of items to return", gt=0, lt=100)
    ):

    # load data
    data = load_data()

    # fetch items based on section_id (ex:- Laptop, Mobile, etc..)
    items = data.get(section_id, [])

    # sort items based on the sort query parameter
    if sort == "asc":
        items.sort()  # sort in ascending order
    elif sort == "desc":
        items.sort(reverse=True)  # sort in descending order
    
    # limit the number of items returned based on the limit query parameter
    limited_items = items[:limit]

    return limited_items
```


- We can use Path , Query Parameter with any Request (GET, POST, PUT, DELETE)

---
---


## HTTP Status Codes:

 HTTP status codes are standardized codes that indicate the outcome of an HTTP request. They are categorized into five classes based on their first digit:

### 1) 1xx (Informational):

These codes indicate that the request has been received and is being processed. They are rarely used in practice.

### 2) 2xx (Successful): 

These codes indicate that the request was successfully received, understood, and accepted. Common examples include:

- 200 OK: The request was successful, and the server is returning the requested data. (GET, POST)
- 201 Created: The request was successful, and a new resource has been created as a result. (POST)
- 204 No Content: The request was successful, but there is no content to return in the response. (DELETE)

### 3) 3xx (Redirection): 

These codes indicate that further action is needed to complete the request, often involving redirection to another URL. Common examples include:

- 301 Moved Permanently: The requested resource has been permanently moved to a new URL.
- 302 Found: The requested resource is temporarily located at a different URL.  

### 4) 4xx (Client Error): 

These codes indicate that there was an error with the client's request. Common examples include:

- 400 Bad Request: The server could not understand the request due to invalid syntax. (missing field, wrong data type, etc.)
- 401 Unauthorized: The client must authenticate itself to get the requested response. (login required)
- 403 Forbidden: The client does not have access rights to the content. (logged in but not authorized)
- 404 Not Found: The server cannot find the requested resource. (wrong URL, resource does not exist, etc.)

### 5) 5xx (Server Error): 

These codes indicate that the server failed to fulfill a valid request. Common examples include:

- 500 Internal Server Error: The server encountered an unexpected condition that prevented it from fulfilling the request. (bug in the server code, database connection failure, etc.)
- 502 Bad Gateway: The server, while acting as a gateway or proxy, received an invalid response from the upstream server. (upstream server is down, network issues, etc.)
- 503 Service Unavailable: The server is currently unable to handle the request due to temporary overload or maintenance. 


### Http Response :-

we raise a HTTPException with the appropriate status code and an optional detail message to indicate the nature of the error. 

```
from fastapi import HTTPException
raise HTTPException(status_code=...., detail="Error message describing the issue")
```

we can also send using JsonResponse

```
from fastapi.responses import JSONResponse
return JSONResponse(status_code=..., content={"error": "Error message describing the issue"})
```


---
---

## Pydantic :-

Pydantic helps us to do data validation in FastAPI. so that we can easily validate the incoming request data and ensure that it meets the expected format and constraints before processing it in our API endpoints.
 
Example:- Type Validation (Age = "Twenty Two") , Data Validation (Age = -10 or 200)

#### 1) Define a Pydantic model that represents the structure of the data you expect to receive in the request body. This model will include fields with their respective data types and validation rules.

```
class Patient_class(BaseModel):
    name: Annotated[str, Field(..., description="The name of the patient", example="Ujwal Sahu")]
    age: Annotated[int, Field(..., description="The age of the patient", example=22, gt=0, lt=150)]
    email: Annotated[str, Field(..., description="The email address of the patient", example="ujwal.sahu@example.com")]
    gender: Annotated[str, Field(default=None, description="The gender of the patient", example="Male")]
```

#### 2) Make a dictionary or JSON like structure containing the incoming request data.

```
patient_data = {
    "name": "Ujwal Sahu",
    "age": 22,
    "email": "ujwal.sahu@example.com"
}
```

#### 3) Make object of pydantic model using pydatic class & giving it the incoming request data as input. 

If the data is valid, the Pydantic model will create an instance of the model with the validated data.   
If the data is invalid, Pydantic will raise a validation error, which you can catch and handle appropriately in your API endpoint.

`patient = Patient_class(**patient_data)` # here we are unpacking the dictionary using ** and passing it to the Patient class model.


#### 4) Use the Object.variables to access the validated data and perform your desired operations in the API endpoint.

```
def create_patient(patient: Patient):
    # here we are accepting the patient object as a parameter in the function and we can be sure that the data is valid and in correct format because it has been validated by the pydantic model.
    # now we can use this patient object to do whatever we want such as insert it in the database or return it in the response etc ...
    print(patient.name)
    print(patient.age)
    print(patient.email)
    # store in db
```

- STEPS:-  
1) Create a Pydantic class model for your schema
2) Create a dictionary or JSON like structure containing the incoming request data.
3) Create an Object of the Pydantic model by unpacking the dictionary and passing it to the Pydantic class model.
4) Use the Object.variables to access the validated data in the endpoint.


------

## Pydantic Class Model Full Structure :-

```
class class_name(BaseModel):

    var_name: Annotated[

        Data_type, # for Required field    # # for Required Field   # int | str | list[str] | Optional[int] | Literal[make,female] | etc.
        # OR
        Optional[Datatype], # for Optional field

        Field (
            ..., # for Required field
            # OR
            default = default_value, # for Optional field

            # METADATA
            title="....",
            description="....",
            example=....,

            # VALIDATION RULES
            gt=..., ge=..., lt=..., le=...,              # numbers
            min_length=..., max_length=...,              # strings
            pattern="...",                              # regex
            min_items=..., max_items=...,                # lists
        
            # OTHER
            alias="...",
            strict=True,
        )
    ]
        
    var_name2: Annotated[
        .....
    ]
    .....
    

    ### Field Validator -----

    # used for complex custom validation logic that cannot be easily handled by the built-in validation rules provided by Pydantic.
    # and we can also transform that value in the field validator function and return that transformed value, and store that transformed value in the variable.
    # xtra:- the pydantic automatically does type conversion if possible, "30" -> 30 -> age: int . so we can also mention does this automatic type conversion should happen before or after the custom validation function using mode="before" or "after" in the field validator function.
    
    @field_validator('var_field_name', mode="before/after") # here we specify the field name for which we want to write the custom validation logic
    @classmethod
    def function_name(cls, value):  # we also write "cls" and "value" in the function parameters
        # perform custom validation logic or updation on the value of var_name
        # example :- check if email ends with @icici.com, update the name and make it uppercase, etc..
        return value


    ### Model Validator (custom validation) -----

    # when you want to do some complex custom validation that involves multiple fields of the model, then you can use the Model Validator to write your own custom validation logic for the entire model. 
    # and we can also transform the values of multiple fields in the model validator function and return those transformed values as a dictionary, and store those transformed values in the respective variables of the model.
    # why we need model validator ? -> since Field Validator only works with 1 field at a time, so if we want to do validation that involves multiple fields then we need model validator.
    
    @model_validator(mode="before/after") # here we specify the mode for the model validator function, whether we want to do the validation before or after the automatic type conversion happens.
    @classmethod
    def function_name(cls, model):  # we also send the cls , and the entire model in the function parameters
        # custom logic that involves multiple fields of the model
        # Example:- if the patient age is more than 60 then there must be a emergency contact number provided, so we can write a custom validation logic to check that. & if the emergency contact number is not provided then we can raise a validation error.
        # here 2 fileds are involved Age & Emergency Contact Number
        return model # in the end we return the model so that anything changed is implemented.


    ### Computed Fields -----

    # sometimes we want to do some computation on the fields of the model and store that computed value in a new variable in the model,
    # so we can also do that using model validator function, we can do some computation on the fields of the model and then return those computed values in a dictionary and store those computed values in new variables in the model.
    
    @computed_field
    @property
    def function_name(self) -> datatype:
        # computation logic using the fields of the model
        # Example:- we want to compute the BMI of the patient using the height and weight fields of the model and store that computed BMI value in a new variable called bmi in the model, so we can do that using computed_field and model validator function.
        # bmi = weight / (height * height)
        # return bmi
        return computed_value
    # the function name will become the variable/fieled name with this retuned value
    # if Function name is BMI then we can access the computed value using patient_object.BMI
```

---

### Nested Pydantic Models :-

sometimes we want to have a nested structure in our data model, such as a patient having multiple medical records or a doctor having multiple patients, so we can also do that using Pydantic models by nesting one model inside another model.

```
class Class_1(BaseModel):
    var1: data_type
    var2: data_type
    .....

class Class_2(BaseModel):
    var3: data_type
    var4: data_type
    var5: Class_1  # here we are nesting Class_1 inside Class_2, so we can have a nested structure in our data model.
```

when we create an object of Class_2, we can also create an object of Class_1 and assign it to var5, so that we can have a nested structure in our data model.

Example :-

```
# First Model
class address(BaseModel):
    street: str
    city: str
    state: str
    zip_code: str

# Second Model (Main Model)
class patient(BaseModel):
    name: str
    age: int
    email: str
    address: address # here we are nesting the address model inside the patient model, so now the patient model has an address field which is of type address model, and we can use that to store the address details of the patient in a structured way. and we can also do validation on the nested model fields using the same validation rules as we do for the main model fields.

# first make object of the parent model 
address_data = {
    "street": "123 Main St", 
    "city": "New York", 
    "state": "NY", 
    "zip_code": "10001"
}
address_obj = address(**address_data)

# Then make object of the main model and send the object of the nested model in the main model data
patient_data = {
    "name": "Ujwal Sahu",
    "age": 65,
    "email": "ujwal.sahu@example. com",
    "address": address_obj # send the object of the nested model in the main model data
}
patient_obj = patient(**patient_data)

# Access the variables of the main model and nested model using the object of the main model
print(patient_obj.name) # Ujwal Sahu
print(patient_obj.age) # 65
print(patient_obj.address.street) # 123 Main St
print(patient_obj.address.city) # New York
print(patient_obj.address.state) # NY
print(patient_obj.address.zip_code) # 10001
```

-------

### Serialization of Pydantic Models :-

so after we got the object of the pydantic model with all the validated data, then we can easily convert that object into a dictionary or JSON format to send it in the response or to store it in the database, so this process of converting the pydantic model object into a dictionary or JSON format is called serialization, and Pydantic provides built-in methods to do that serialization easily.


`patient_dict = patient1.model_dump()`  
this will convert the patient1 object into a dictionary format, and we can also do model_dump_json() to convert it into JSON format. and we can also pass some parameters in the model_dump() function to customize the serialization process such as exclude_unset=True to exclude the fields that are not set by the user and only include the fields that are set by the user in the output dictionary, and many other parameters for customization.

`patient_dict = patient1.model_dump(include=['name', 'age'])`  
this will only include the name and age fields in the output dictionary and exclude all other fields from the output dictionary.

`patient_dict = patient1.model_dump(exclude=['gender', 'age'])`  
this will exclude the gender and age fields from the output dictionary and include all other fields in the output dictionary.

`patient_dict = patient1.model_dump(exclude_unset=True)`  
this will exclude the fields that are not set by the user and only include the fields that are set by the user in the output dictionary. # so exculde all the stuff which were optional & had default values but user did not send any value for those fields, so those fields will be excluded from the output dictionary and only the fields that user sent values for will be included in the output dictionary.

`patient_json = patient1.model_dump_json()`   
this will convert the patient1 object into a JSON format string, and we can also pass some parameters in the model_dump_json() function to customize the serialization process such as exclude_unset=True to exclude the fields that are not set by the user and only include the fields that are set by the user in the output JSON string, and many other parameters for customization.


-------

## Using Pydantic in FastAPI Endpoint (POST , PUT):-

In POST, PUT Request the Data is sent in the request body in JSON format

the data directly comes in the request body in POST and PUT requests, and we just have to write the Object name in the function parameter of the API endpoint function and that data will be stored in the object variable.

```
@app.post("/path")
def function_name(object_name):
    object.var # access the data.
    # Here we are not validating the input data using Pydantic model.
```

Pydantic models are commonly used in FastAPI for POST and PUT requests endpoints to validate the incoming request data, where the data is sent in the request body by the frontend or user.

So to validate the incoming request data in POST and PUT requests

instead of manually doing step 2) creating a Dict of input data, and step 3) creating an object of the Pydantic model using that unpacked input data dict

we can directly use the Pydantic model as a parameter in the API endpoint function, and FastAPI will automatically handle the validation of the incoming request data against the defined Pydantic model, 

basically it will pass the incoming request data to the Pydantic model for validation, 

and if the validation fails then it will automatically return a 422 Unprocessable Entity error response with details about the validation errors,

and if the validation is successful then the validated data will be stored in the parameter variable of the function as an object of the Pydantic model.



#### So only need to this steps : ---

### 1) Define a Pydantic model class that represents the structure of the data you expect to receive in the request body, including any necessary validation rules and metadata for the fields.

```
class pydantic_class_name(BaseModel):
    var_name: datatype
    var2_name: datatype
    var3_name: Annotated[datatype, Field(..., description="Description of the field", example="Example value for the field", etc...)]
    ....
```

### 2) Use the Pydantic model as a parameter in your API endpoint function, So that the Request Body data will be  
-> automitically validated using the Pydantic model  
-> the validated data will stored in the parameter variable of the function as an object of the Pydantic model  

```
@app.post("/path")
def function_name(pydantic_object_name: pydantic_class_name): # here we are using the Pydantic model as a parameter in the API endpoint function, so that the incoming request data will be automatically validated against the defined Pydantic model, and if the validation is successful then the validated data will be stored in the object. 
```

### 3) Inside the API endpoint function, you can access the validated data using -> Pydantic_object.variable_name

`object.var_name` # to access the value of var_name field from the object of the Pydantic model




Example :-

```
# 1) Create a Pydantic Model for Patient Update 

class PatientUpdate(BaseModel):
    name: Annotated[Optional[str], Field(default=None)]
    city: Annotated[Optional[str], Field(default=None)]
    age: Annotated[Optional[int], Field(default=None, gt=0)]
    gender: Annotated[Optional[Literal['male', 'female']], Field(default=None)]
    height: Annotated[Optional[float], Field(default=None, gt=0)]
    weight: Annotated[Optional[float], Field(default=None, gt=0)]


# Update Endpoint with PUT Request
@app.put('/edit/{patient_id}')
def update_patient(
    patient_id: str, 

    # 2) Use the Pydantic model as a parameter in the API endpoint function to validate the incoming request body data for patient update
    patient_update: PatientUpdate
    ):


    # 3) Inside the API endpoint function, you can access the validated data using -> patient_update.variable_name
    
    # load existing data
    data = load_data()

    # check if patient exists or not
    # if not exists then raise error
    if patient_id not in data:
        raise HTTPException(status_code=404, detail='Patient not found')
    
    # get the existing patient info from the data using the patient_id as key and store it in a variable called existing_patient_info
    existing_patient_info = data[patient_id]

    # The patient_update variable is an object of the PatientUpdate model, so first we convert it into dictionary so that easy to work with it , and we only get those fields which user sent values for in the request by using exclude_unset=True parameter in the model_dump() function, so that we can update only those fields in the existing patient info and rest all fields will remain unchanged.
    updated_patient_info = patient_update.model_dump(exclude_unset=True)

    # Now we will takes the values from the updated_patient_info dictionary and update those values in the existing_patient_info dictionary, so that we will get the updated patient info with the new values for the fields which user sent in the request and rest all fields will remain unchanged with their existing values.
    for key, value in updated_patient_info.items():
        existing_patient_info[key] = value

    # save data
    save_data(data)

    return JSONResponse(status_code=200, content={'message':'patient updated'})
```

----

## Response Model in FastAPI  :-

So like we Validated the user Input data using Pydantic models, similarly we can also validate the output response data that we are sending back to the user from the endpoint function using Pydantic models, 

so that we can ensure that the response data is also in the correct format and has the correct data types before sending it back to the user, 

and we can also use the same Pydantic model for both input validation and response validation, or we can use different Pydantic models for input and response validation, it is up to us.

Example: if the input and output response data are same then we can use the same pydantic class model, if output is differnt from the input data then we can make a different Pydantic model for validating the response data 


### 1) Create a Pydantic model for the response data

```
class Pydantic_Response_class_name(BaseModel):
    feild1: data_type
    feild2: data_type
    feild3: Annotated[data_type, Field(..., description="Description of the field", example="Example value for the field", etc...)]
    .....
```

### 2) Mention the response model in the endpoint function using response_model parameter in the route decorator

we dont have to manually unpack the return data and validate it by creating a pydantic object of response class we simply return the response data in the return statement and the validation is automatically handelled.

and soo when you will return the response data from that endpoint then it will first validate the data using pydantic model and then send the response to the user

and if the response data is invalid and does not match the schema defined in the response model then it will raise an error & not not return anything. and the code will run to next line and there you can return the error response just try catch block & return a custom error response to the user.

Response Models are mostly used in GET requests since GET requests are used to retrieve data from the server, But we can also use it in POST, PUT requests if we want to validate the response data that we are sending back to the user after creating a new resource or updating an existing resource.

```
@app.get("/path", response_model=Pydantic_Response_class_name)
def function_name():
    # code logic to get the response data
    response_data = {
        "feild1": value1,
        "feild2": value2,
        "feild3": value3
    }
    try:
        return JsonResponse(status_code=200, content={'message': 'Success', 'data': response_data})
    except ValidationError as error:
        return JsonResponse(status_code=400, content={'message': str(error)})
```


Example :--

```
# 1) Create a Pydantic model for the response data
class PatientResponseModel(BaseModel):

    name: str
    age: int
    email: str
    bmi: float  

# 2) Mention Response Model in the endpoint function using response_model parameter in the route decorator
@app.get("/patient/{patient_id}", response_model=PatientResponseModel)
def get_patient(patient_id: int):
    # fetch patient data from db based on patient_id
    patient_data = {
        "name": "Ujwal Sahu",
        "age": 22,
        "email": "ujwal@example.com",
        "bmi": 22.5
    }
    
    # return data
    try:
        return JsonResponse(status_code=200, content={'message': 'Patient found', 'data': patient_data})            
    except ValidationError as error:
        return JsonResponse(status_code=400, content={'message': str(error)})
```


---
---

## Requests Structures :-

### 1) GET REQUEST

- Used to Retrieve Data from the Server.
- Input:- Path Parameters, Query Parameters
- Output:- JSON Response containing the status code , response msg(error,success) , requested data

`URL: http://localhost:8000/path/{Path_parameter_1}/{Path_parameter_2} ? Query_parameter_1=value1 & Query_parameter_2=value2 `

```
@app.get("/path/{Path_parameter_1}/{Path_parameter_2}", response_model=Response_Pydantic_Model_Class)
def function_name(
    # Path Parameters -> Name can be anything but Must be in same order as in URL since they are assigned based on their position in the URL
    Path_parameter_1: data_type = Path(..., description="Description of path parameter 1", example="Example value for path parameter 1", regex="^[a-zA-Z0-9]+$", gt=0, lt=100, min_length=1, max_length=50),
    Path_parameter_2: data_type = Path(..., description="Description of path parameter 2", example="Example value for path parameter 2", etc...),
    # Query Parameters -> Name must be same as in URL since they are assigned based on their names in the URL, & they can be in any order in the function parameter.
    Query_parameter_1: data_type = Query(... or default_value, description="Description of query parameter 1", example="Example value for query parameter 1", etc...),
    Query_parameter_2: data_type = Query(... or default_value, description="Description of query parameter 2", example="Example value for query parameter 2", etc...)
    ):

    # Code Logic 
    # Use the path parameters and query parameters to perform operations or retrieve data based on the provided values.

    # return success (200) or error (40X, 50X) & requested data
    return JSONResponse(status_code=..., content={"message/error": "...."}) 
```

---

### 2) POST REQUEST

- Used to Send Data to the Server to Create a New Resource / or when Using ML inference.
- Input:- Path Parameters, Query Parameters, Request Body (JSON Data)
- Output:- JSON Response containing the status code , response msg(error,success) , requested data (optional, since POST is used to create a new resource, not retrieve data)

`URL: http://localhost:8000/path/{Path_parameter_1}/{Path_parameter_2} ? Query_parameter_1=value1 & Query_parameter_2=value2`

Data will be sent in the request body in JSON format

```
# 1) Create a Pydantic model for the request body data validation
class PydanticModelClass(BaseModel):
    var1: data_type
    var2: data_type
    var3: Annotated[data_type, Field(..., description="Description of the field", example="Example value for the field", etc...)]
    .....   

@app.post("/path/{Path_parameter_1}/{Path_parameter_2}", response_model=Response_Pydantic_Model_Class)
def function_name(
    # mostly we dont use Path and Query parameters in POST request but we can use if we want to.
    Path_parameter_1: data_type = Path(..., description="Description of path parameter 1", example="Example value for path parameter 1", regex="^[a-zA-Z0-9]+$", gt=0, lt=100, min_length=1, max_length=50),
    Path_parameter_2: data_type = Path(..., description="Description of path parameter 2", example="Example value for path parameter 2", etc...),
    Query_parameter_1: data_type = Query(... or default_value, description="Description of query parameter 1", example="Example value for query parameter 1", etc...),
    Query_parameter_2: data_type = Query(... or default_value, description="Description of query parameter 2", example="Example value for query parameter 2", etc...),
    
    # 2) Mention the Pydantic model as a parameter in the function definition to validate the incoming request body data
    Request_body_parameter: PydanticModelClass # here we are accepting the request body data as a parameter in the function and we are using a Pydantic model class to validate that incoming request body data, so that we can be sure that the incoming request body data is valid and in correct format before processing it in our API endpoint function.
    ):

    # Code Logic
    # Create a new resource, or perform ML inference.

    # 3) Inside the function, you can access the validated data from the request body using the Request_body_parameter variable, which is an object of the Pydantic model class. You can then use this validated data to perform your desired operations, such as inserting it into a database, using it for ML inference, etc. 
    Request_body_parameter.variable_name # to perform operations using that data
    
    # return success (200, 201) or error (40X, 50X) & requested data (optional)
    return JSONResponse(status_code=..., content={"message/error": "...."})
```

---

### 3) PUT REQUEST

- Used to Update an Existing Resource on the Server.
- Input:- Path Parameters, Query Parameters, Request Body (JSON Data)
- Output:- JSON Response containing the status code , response msg(error,success) , requested data (optional, since PUT is used to update an existing resource, not retrieve data)

`URL: http://localhost:8000/path/{Path_parameter_1}/{Path_parameter_2} ? Query_parameter_1=value1 & Query_parameter_2=value2`

Data will be sent in the request body in JSON format

```
# 1) Create a Pydantic model for the request body data validation
class PydanticModelClass(BaseModel):
    var1: data_type
    var2: data_type
    var3: Annotated[data_type, Field(..., description="Description of the field", example="Example value for the field", etc...)]
    .....

@app.put("/path/{Path_parameter_1}/{Path_parameter_2}", response_model=Response_Pydantic_Model_Class)
def function_name(
    Path_parameter_1: data_type = Path(..., description="Description of path parameter 1", example="Example value for path parameter 1", regex="^[a-zA-Z0-9]+$", gt=0, lt=100, min_length=1, max_length=50),
    Path_parameter_2: data_type = Path(..., description="Description of path parameter 2", example="Example value for path parameter 2", etc...),
    Query_parameter_1: data_type = Query(... or default_value, description="Description of query parameter 1", example="Example value for query parameter 1", etc...),
    Query_parameter_2: data_type = Query(... or default_value, description="Description of query parameter 2", example="Example value for query parameter 2", etc...),
    
    # 2) Mention the Pydantic model as a parameter in the function definition to validate the incoming request body data
    Request_body_parameter: PydanticModelClass # here we are accepting the request body data as a parameter in the function and we are using a Pydantic model class to validate that incoming request body data, so that we can be sure that the incoming request body data is valid and in correct format before processing it in our API endpoint function.
    ):

    # Code Logic
    # here we can use the path parameters and query parameters to identify the existing resource that we want to update.

    # 3) Inside the function, you can access the validated data from the request body using the Request_body_parameter variable, which is an object of the Pydantic model class. You can then use this validated data to perform your desired operations, such as inserting it into a database.
    Request_body_parameter.variable_name # to perform operations using that data
    
    # return success (200, 201) or error (40X, 50X) & requested data (optional)
    return JSONResponse(status_code=..., content={"message/error": "...."})
```

---

### 4) DELETE REQUEST

- Used to Delete an Existing Resource from the Server.
- Input:- Path Parameters, Query Parameters
- Output:- JSON Response containing the status code , response msg(error,success) , requested data (optional, since DELETE is used to delete an existing resource, not retrieve data)

`URL: http://localhost:8000/path/{Path_parameter_1}/{Path_parameter_2} ? Query_parameter_1=value1 & Query_parameter_2=value2`

```
@app.delete("/path/{Path_parameter_1}/{Path_parameter_2}", response_model=Response_Pydantic_Model_Class)
def function_name(
    Path_parameter_1: data_type = Path(..., description="Description of path parameter 1", example="Example value for path parameter 1", regex="^[a-zA-Z0-9]+$", gt=0, lt=100, min_length=1, max_length=50),
    Path_parameter_2: data_type = Path(..., description="Description of path parameter 2", example="Example value for path parameter 2", etc...),
    Query_parameter_1: data_type = Query(... or default_value, description="Description of query parameter 1", example="Example value for query parameter 1", etc...),
    Query_parameter_2: data_type = Query(... or default_value, description="Description of query parameter 2", example="Example value for query parameter 2", etc...)
    ):

    # Code Logic
    # perform delete operation based on the provided path parameters and query parameters, such as deleting a resource from the database, etc.

    # return success (200, 204) or error (40X, 50X) & requested data (optional)
    return JSONResponse(status_code=..., content={"message/error": "...."})
```


---
---

## Main() function to run the app :-

we can directly run the fastapi app without a def main() function using `uvicorn main:app --reload` and internally it sets the host(0.0.0.0), port(8000) and runs the app.

even if you want to keep the main function then you can do like this below using Logger for better logging and debugging instead of print statements

```
# Using Logger for better logging and debugging instead of print statements

# Configure logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

# Use:-
logger.info("msg print")
logger.error("error msg print")

# Main function to run the app
if __name__ == "__main__":
    import uvicorn
    
    logger.info("Starting Application...")
    uvicorn.run(
        app,
        host="0.0.0.0",
        port=8000,
        log_level="info"
    )

```

Then run the code using `python main.py` or `uv run python main.py`  
So that it calls the main function & in the main function it will run the same cmd: `uvicorn main:app --host=0.0.0.0 --port=8000` which is same as `uvicorn main:app --reload` 


---
---

### FastAPI BackEnd Logic :-

- 1) If the user provided a invalid data in the request such as negative age or negative height then the pydantic will automatically raise an error and send error message to the frontend.
- In the Frontend we can put conditions and checks so that the user can only fill the valid data in the form , so that less chance of getting invalid data in the request.
- 2) If the Data is valid but the logic is invalid, ex:- user_id not present in the database then we can handle that logic in the code and send appropriate error response to the frontend 
- 3) If the data is valid and logic is also valid then we can perform the task & send the success response to the frontend with response data if needed.

---

- Identity -> Path Parameter
- Filter -> Query Parameter
- Data -> Request Body (JSON Data)

---
---

## Read World Example :-

```
# ============================================================
# 🚀 IDEAL REAL-WORLD FASTAPI FLOW (PRODUCTION-STYLE STRUCTURE)
# Flow Covered:
# Request → Validation → Service → DB → Response
# ============================================================

# =========================
# 1) IMPORTS
# =========================
from fastapi import FastAPI, HTTPException, Depends
from pydantic import BaseModel, EmailStr, Field
from typing import Dict, Optional
import uuid
import logging

# =========================
# 2) APP INIT + LOGGING
# =========================
app = FastAPI()

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

# =========================
# 3) DATABASE LAYER (SIMULATED)
# =========================
# 👉 In real-world → Replace with PostgreSQL / MongoDB
class FakeDB:
    def __init__(self):
        self.users: Dict[str, dict] = {}

    def create_user(self, user_data: dict) -> dict:
        self.users[user_data["id"]] = user_data
        return user_data

    def get_user_by_email(self, email: str) -> Optional[dict]:
        for user in self.users.values():
            if user["email"] == email:
                return user
        return None

    def get_user_by_id(self, user_id: str) -> Optional[dict]:
        return self.users.get(user_id)


# Dependency Injection for DB
def get_db():
    return db


db = FakeDB()


# =========================
# 4) REQUEST MODEL (VALIDATION)
# =========================
# 👉 Step: Request → Validation (auto-handled by Pydantic)
class UserCreate(BaseModel):
    name: str = Field(..., min_length=2, description="User name")
    email: EmailStr
    age: int = Field(..., gt=0, lt=120)


# =========================
# 5) RESPONSE MODEL
# =========================
# 👉 Step: DB → Response validation
class UserResponse(BaseModel):
    id: str
    name: str
    email: EmailStr
    age: int


# =========================
# 6) SERVICE LAYER (BUSINESS LOGIC)
# =========================
# 👉 Step: Validation → Service → DB
class UserService:

    def __init__(self, db: FakeDB):
        self.db = db

    def create_user(self, user_data: UserCreate) -> dict:
        # -----------------------------
        # BUSINESS RULES (SERVICE LAYER)
        # -----------------------------
        # 1) Check duplicate email
        existing_user = self.db.get_user_by_email(user_data.email)
        if existing_user:
            raise HTTPException(status_code=400, detail="Email already exists")

        # 2) Generate unique ID
        user_id = str(uuid.uuid4())

        # 3) Prepare DB object
        new_user = {
            "id": user_id,
            "name": user_data.name,
            "email": user_data.email,
            "age": user_data.age
        }

        # -----------------------------
        # 👉 Service → DB
        # -----------------------------
        saved_user = self.db.create_user(new_user)

        logger.info(f"User created with ID: {user_id}")

        return saved_user

    def get_user(self, user_id: str) -> dict:
        # -----------------------------
        # 👉 Service → DB
        # -----------------------------
        user = self.db.get_user_by_id(user_id)

        if not user:
            raise HTTPException(status_code=404, detail="User not found")

        return user


# Dependency Injection for Service
def get_user_service(db: FakeDB = Depends(get_db)):
    return UserService(db)


# =========================
# 7) API ENDPOINTS (CONTROLLER)
# =========================

# ------------------------------------------------------------
# CREATE USER
# ------------------------------------------------------------
@app.post("/users", response_model=UserResponse)
def create_user(
    user: UserCreate,  # 👉 Step: Request → Validation
    service: UserService = Depends(get_user_service)  # 👉 Inject Service
):
    # -----------------------------
    # 👉 Step: Request enters API
    # FastAPI automatically validates using UserCreate
    # -----------------------------

    # 👉 Step: Validation → Service
    created_user = service.create_user(user)

    # -----------------------------
    # 👉 Step: DB → Response
    # FastAPI validates response via UserResponse
    # -----------------------------
    return created_user


# ------------------------------------------------------------
# GET USER
# ------------------------------------------------------------
@app.get("/users/{user_id}", response_model=UserResponse)
def get_user(
    user_id: str,  # 👉 Path Parameter (Resource Identifier)
    service: UserService = Depends(get_user_service)
):
    # -----------------------------
    # 👉 Step: Request → Service
    # -----------------------------
    user = service.get_user(user_id)

    # -----------------------------
    # 👉 Step: DB → Response
    # -----------------------------
    return user


# =========================
# 8) ROOT ENDPOINT (HEALTH CHECK)
# =========================
@app.get("/")
def root():
    return {"message": "API is running 🚀"}


# ============================================================
# 🔄 COMPLETE FLOW SUMMARY (END-TO-END)
# ============================================================
# 1) CLIENT REQUEST →
#       POST /users  with JSON body
#
# 2) FASTAPI →
#       Parses request → sends data to Pydantic model
#
# 3) VALIDATION (Pydantic) →
#       - Type check (email, int)
#       - Constraints (age > 0, name length)
#       ❌ If invalid → 422 response
#
# 4) CONTROLLER →
#       Receives validated object → calls Service
#
# 5) SERVICE →
#       - Business rules (no duplicate email)
#       - Data transformation (generate UUID)
#
# 6) DATABASE →
#       - Store / retrieve data
#
# 7) RESPONSE →
#       - Sent back through response_model validation
#
# 8) CLIENT RECEIVES CLEAN JSON ✅

```
