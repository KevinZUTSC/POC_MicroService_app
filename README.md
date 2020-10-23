# POC_MicroService_app

Token Based Security (using Spring security)
API documentation and Live Try-out links with Swagger
In Memory DB with H2
Using JPA and JDBC template to talk to relational database
How to request and respond for paginated data
Frontend

Organizing Components, Services, Directives, Pages etc in an Angular App
How to chain RxJS Observables (by making sequntial AJAX request- its different that how you do with promises)
Techniques to Lazy load Data (Infinite Scroll)
Techniques to load large data set in a data-table but still keeping DOM footprint less
Routing and guarding pages that needs authentication
Basic visulaization
Build

How to build all in one app that includes (database, sample data, RESTfull API, Auto generated API Docs, frontend and security)
Portable app, Ideal for dockers, cloud hosting.
In Memory DB (H2)
I have included an in-memory database for the application. Database schema and sample data for the app is created everytime the app starts, and gets destroyed after the app stops, so the changes made to to the database are persistent only as long as the app is running
Creation of database schema and data are done using sql scripts that Springs runs automatically. To modify the database schema or the data you can modify schema.sql and data.sql which can be found at /src/main/resources

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

# Navigate to PROJECT_FOLDER/webui (should contain package.json )
npm install
# build the project (this will put the files under dist folder)
ng build --prod --aot=true
Build Backend (SpringBoot Java)
# Maven Build : Navigate to the root folder where pom.xml is present 
mvn clean install

#OR

# Gradle Build : Navigate to the root folder where build.gradle is present 
gradle build
Start the API and WebUI server
# Start the server (9119)
# port and other configurations for API servere is in [./src/main/resources/application.properties](/src/main/resources/application.properties) file

# If you build with maven jar location will be 
java -jar ./target/app-1.0.0.jar

# If you build with gradle jar location will be 
java -jar ./build/libs/app-1.0.0.jar
Accessing Application
Cpmponent	URL	Credentials
Frontend	http://localhost:9119	demo/demo
H2 Database	http://localhost:9119/h2-console	Driver:org.h2.Driver
JDBC URL:jdbc:h2:mem:demo
User Name:sa
Swagger (API Ref)	http://localhost:9119/swagger-ui.html	
Redoc (API Ref)	http://localhost:9119/redoc/index.html	
Swagger Spec	http://localhost:9119/api-docs	
To get an authentication token

curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{"username": "demo", "password": "demo" }' 'http://localhost:9119/session'
or POST the username and password to http://localhost:9119/session

after you get the authentication token you must provide this in the header for all the protected urls

curl -X GET --header 'Accept: application/json' --header 'Authorization: [replace this with token ]' 'http://localhost:9119/version'
To get an authentication token
