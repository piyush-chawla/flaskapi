## Welcome to the flaskapi wiki!


## **Installation** 

For Mac / Linux users, perform below to setup the environment

$ sudo apt update

$ sudo apt install python3-pip

Setup the virtual environment

$ virtualenv flaskapi

$ source  flaskapi/bin/activate

$ pip install -r requirements.txt

### **RUN**

$ python api.py

```
$ python api.py
/home/ubuntu/Documents/REST-auth/venv/lib/python3.10/site-packages/flask_sqlalchemy/__init__.py:834: FSADeprecationWarning: SQLALCHEMY_TRACK_MODIFICATIONS adds significant overhead and will be disabled by default in the future.  Set it to True or False to suppress this warning.
  warnings.warn(FSADeprecationWarning(
 * Serving Flask app "api" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
 * Restarting with stat
/home/ubuntu/Documents/REST-auth/venv/lib/python3.10/site-packages/flask_sqlalchemy/__init__.py:834: FSADeprecationWarning: SQLALCHEMY_TRACK_MODIFICATIONS adds significant overhead and will be disabled by default in the future.  Set it to True or False to suppress this warning.
  warnings.warn(FSADeprecationWarning(
 * Debugger is active!
 * Debugger PIN: 608-482-789
127.0.0.1 - - [13/May/2022 23:36:27] "POST /api/users HTTP/1.1" 201 -
127.0.0.1 - - [13/May/2022 23:36:42] "GET /api/resource HTTP/1.1" 200 -
127.0.0.1 - - [13/May/2022 23:37:21] "GET /api/users HTTP/1.1" 405 -
127.0.0.1 - - [13/May/2022 23:42:29] "GET /api/ HTTP/1.1" 404 -
127.0.0.1 - - [13/May/2022 23:42:34] "GET /api/resources HTTP/1.1" 404 -
127.0.0.1 - - [13/May/2022 23:42:56] "GET /api/resource HTTP/1.1" 200 -
127.0.0.1 - - [13/May/2022 23:53:03] "POST /api/users HTTP/1.1" 201 -
127.0.0.1 - - [13/May/2022 23:53:32] "GET /api/resource HTTP/1.1" 200 -
127.0.0.1 - - [14/May/2022 00:35:55] "GET /api/resource HTTP/1.1" 200 -
127.0.0.1 - - [14/May/2022 00:36:18] "GET /api/users/C1 HTTP/1.1" 404 -
127.0.0.1 - - [14/May/2022 00:36:52] "GET /api/users/1 HTTP/1.1" 200 -
127.0.0.1 - - [14/May/2022 00:36:59] "GET /api/users/2 HTTP/1.1" 200 -
127.0.0.1 - - [14/May/2022 00:41:21] "POST /api/users HTTP/1.1" 400 -
127.0.0.1 - - [14/May/2022 00:41:30] "GET /api/users HTTP/1.1" 405 -
127.0.0.1 - - [14/May/2022 00:41:30] "GET /favicon.ico HTTP/1.1" 404 -
127.0.0.1 - - [14/May/2022 00:42:03] "POST /api/users HTTP/1.1" 201 -
127.0.0.1 - - [14/May/2022 00:43:27] "GET /api/resource HTTP/1.1" 200 -
127.0.0.1 - - [14/May/2022 00:44:06] "GET /api/resource HTTP/1.1" 401 -
127.0.0.1 - - [14/May/2022 00:45:01] "GET /api/token HTTP/1.1" 200 -
127.0.0.1 - - [14/May/2022 00:45:32] "GET /api/resource HTTP/1.1" 200 -
127.0.0.1 - - [14/May/2022 00:45:40] "GET /api/resource HTTP/1.1" 200 -
127.0.0.1 - - [14/May/2022 00:58:53] "GET /api/token HTTP/1.1" 200 -
127.0.0.1 - - [14/May/2022 00:59:03] "GET /api/token HTTP/1.1" 200 -
127.0.0.1 - - [14/May/2022 00:59:32] "GET /api/resource HTTP/1.1" 200 -
```

### **Documentation**

* POST /api/users/
  
  To creat a new user, request must contain a username and password. 

* GET /api/users/<int:id>
  
  To fetch the user details

* GET /api/token
  
  Return an authentication token.
  This request must be authenticated using a HTTP Basic Authentication header.

* GET /api/resource
  
  Return a protected resource.
  This request must be authenticated using a HTTP Basic Authentication header. Instead of username and password, the client can provide a valid 
  authentication token in the username field. If using an authentication token the password field is not used and can be set to any value.

### **Example**

Below is example of registering a new user:

```
$ curl -i -X POST -H "Content-Type: application/json" -d '{"username":"C2","password":"Cisco@1234"}' http://127.0.0.1:5000/api/users
HTTP/1.0 201 CREATED
Content-Type: application/json
Content-Length: 23
Location: http://127.0.0.1:5000/api/users/3
Server: Werkzeug/1.0.1 Python/3.10.4
Date: Sat, 14 May 2022 04:42:03 GMT

{
  "username": "C2"
}
$
```


Now we can use above user to access resource

```
$ curl -u C2:Cisco@1234 -i -X GET http://127.0.0.1:5000/api/resource
HTTP/1.0 200 OK
Content-Type: application/json
Content-Length: 27
Server: Werkzeug/1.0.1 Python/3.10.4
Date: Sat, 14 May 2022 04:43:27 GMT

{
  "data": "Hello, C2!"
}
$
```


Below is example of unauthorized access

```
$ curl -u Test:Cisco -i -X GET http://127.0.0.1:5000/api/resource
HTTP/1.0 401 UNAUTHORIZED
Content-Type: text/html; charset=utf-8
Content-Length: 19
WWW-Authenticate: Basic realm="Authentication Required"
Server: Werkzeug/1.0.1 Python/3.10.4
Date: Sat, 14 May 2022 04:44:06 GMT

Unauthorized Access
$
```

Below is how we can generate token and authenticate using token. While using token we don't have to provide username and password field can be set to any arbitrary value.  

```
$ curl -u C2:Cisco@1234 -i -X GET http://127.0.0.1:5000/api/token
HTTP/1.0 200 OK
Content-Type: application/json
Content-Length: 163
Server: Werkzeug/1.0.1 Python/3.10.4
Date: Sat, 14 May 2022 04:45:01 GMT

{
  "duration": 600, 
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MywiZXhwIjoxNjUyNTA0MTAxLjQ1NTgwMjd9.Q-l8d7YPDtC2X7l7UbURNdR3ffI58v9ZuZdiR41s6Yo"
}
$ 
$ curl -u eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MywiZXhwIjoxNjUyNTA0MTAxLjQ1NTgwMjd9.Q-l8d7YPDtC2X7l7UbURNdR3ffI58v9ZuZdiR41s6Yo:x -i -X GET http://127.0.0.1:5000/api/resource
HTTP/1.0 200 OK
Content-Type: application/json
Content-Length: 27
Server: Werkzeug/1.0.1 Python/3.10.4
Date: Sat, 14 May 2022 04:45:32 GMT

{
  "data": "Hello, C2!"
}
$ curl -u eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MywiZXhwIjoxNjUyNTA0MTAxLjQ1NTgwMjd9.Q-l8d7YPDtC2X7l7UbURNdR3ffI58v9ZuZdiR41s6Yo:123 -i -X GET http://127.0.0.1:5000/api/resource
HTTP/1.0 200 OK
Content-Type: application/json
Content-Length: 27
Server: Werkzeug/1.0.1 Python/3.10.4
Date: Sat, 14 May 2022 04:45:40 GMT

{
  "data": "Hello, C2!"
}
$
```
