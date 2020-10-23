# POC_MicroService_app

Token Based Security (using Spring security)
API documentation and Live Try-out links with Swagger
In Memory DB with H2
Using JPA and JDBC template to talk to relational database

Organizing Components, Services, Directives, Pages etc in an Angular App
Techniques to Lazy load Data (Infinite Scroll)
Techniques to load large data set in a data-table but still keeping DOM footprint less
Routing and guarding pages that needs authentication

Spring security
Security is enabled by default, to disable, you must comment this line in src/main/java/com/config/SecurityConfig.java
When security is enabled, none of the REST API will be accessesble directly.

To test security access http://localhost:9119/version API and you should get a forbidden/Access denied error. In order to access these secured API you must first obtain a token. Tokens can be obtained by passing a valid userid/password

userid and password are stored in H2 database. To add/remove users, modify the data.sql couple of valid users and their passwords are demo\demo and admin\admin

To get a token call POST /session API with a valid userid and password. for example you may you can use the folliwing curl command to get a token

curl -X POST --header 'Content-Type: application/json' -d '{ "username":"demo", "password":"demo" }' 'http://localhost:9119/session'
the above curl command will return you a token, which should be in the format of xxx.xxx.xxx. This is a JSON web token format. You can decode and validate this token at jwt.io wesite. Just paste the token there and decode the information. to validate the token you should provide the secret key which is mrin that i am using in this app.
after receiving this token you must provide the token in the request-header of every API request. For instance try the GET /version api using the below curl command (replace xxx.xxx.xxx with the token that you received in above command) and you should be able to access the API.

curl -X GET --header 'Accept: application/json' --header 'Authorization: xxx.xxx.xxx' 'http://localhost:9119/version'
Build Frontend (optional step)
Code for frontend is allready compiled and saved under the webui/dist when building the backend app (using maven) it will pickup the code from webui/dist. However if you modified the frontend code and want your changes to get reflected then you must build the frontend

