# README

## Dependency:

The program needs dep to download all the packages needed in the code

## How to use:
1. Compile the code using this command
```
  go build -o [binary name]
```
2. Run program

```
  ./[binary name] <command> [argumnets]
```
3. a.the coomands are: 
   * `gt`  generates an empty .yaml template 
   * `gs`  generates a service from a .yaml file
## .yaml:

1. The yaml file should contain the name of the service.
2. The yaml file should contain at least one endpoint inorder to generate the code
3. The transport method and path should be provided for each and every endpoint in order to generate the code.
4. Model is optional.
5. Caching layer is optional.

## The generated files:

1. Transport file that contains all the code needed inroder to have a rest api
2. Encoders file that is needed to help the transport layer to decode and encode the informations
3. Service file that contains all the code needed to run the service
4. Server file that contains the Serve function that need to be called to run the program
5. model file that contains everything specifed in the model section of the yaml file ( it has setters and getters)
6. Endpoint file that contains all the endpoints specified in the yaml file
7. Repository can be generated and connected to a database.
8. The supported databases are mysql and postgress
9. Caching layer using redis can be generated to cash all Get requests for a specified amount of time

## Future features:
The option to create and run the service in a Docker container will be added.


## Example of a .yaml file:

```yaml
name: userService
redis_cache:
  host: host
  password: password
  db: 0

endpoints:
  -
    name: CreateUser
    args: 
      - user User
    output: 
      - id string
    transport: 
      method: POST
      path: /user
  -
    name: GetUser
    args: 
      - id string
    output: 
      - user User
    cache_time: 10000
    transport:
      method: GET
      path: /user/{id}
  -
    name: UpdateUser
    args: 
      - id string
      - profilePic string
    output: 
      - message string
    transport:
      method: PUT
      path: /user/update
  -
    name: GetAllUsers
    args:
    output: 
      - users []User
    cache_time: 10000
    transport:
      method: GET
      path: /user
  
repository:
  value: true
  db:
    name: mysql
    address: address

model:
  -
    name: User
    attr: 
      - firstName string 
      - lastName string  
      - profilePic string 
      - token string
```
