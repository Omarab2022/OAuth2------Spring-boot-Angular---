# OAuth2 :  ( Spring-boot+Angular ) 
 Fullstack app spring angular using Oauth2 

 ## before login : 

<img width="1416" alt="Screen Shot 2024-02-04 at 15 48 43" src="https://github.com/Omarab2022/OAuth2------Spring-boot-Angular---/assets/99898445/b5283fb6-c12f-42d5-add6-bccfa0627a64">

## after login : 

<img width="1440" alt="Screen Shot 2024-02-04 at 15 49 12" src="https://github.com/Omarab2022/OAuth2------Spring-boot-Angular---/assets/99898445/0a185bf0-a44e-48d6-9887-5417334c3e71">


 # Spring Boot Backend

## Install

```
mvn clean package
```

## Run on local

First, start a database as described bellow.

```
mvn spring-boot:run
```

The application will be available at `http://localhost:8080`

## Packages

### Config

Here will be placed all the necessary configuration for the application to run correctly.

The `GoogleOpaqueTokenIntrospector` is used to use a custom instrospector for the resources handler. As the Spring 
Security OAuth2 dependency does not handle the Google workflow.

The `SecurityConfig` contains the Spring Security 6 configuration, with the protected routes, exception handler and 
OAuth2 configuration.

The `WebClientConfig` contains the WebFlux configuration to communicate with the authorization server of Google when
validating an access_token.

The `WebConfig` contains the CORS configuration.

### Controllers

Here are the available controllers. One for the authentication, which returns the Google URL to authenticate and the
callback endpoint to obtain the access_token.

The other endpoints are a public one and a private one to test the authentication.

### Dtos

Here will be placed the Data Transfer Objects. Objects which will be returned to the frontend.

## Authentication

The authentication is handled with the OAuth2 workflow.

The application can be stateless or stateful, the behavior will be the same. Having a stateful application, I can
store some user's information in the session. Using a stateless application, all the user's information must be sent
from the frontend to the backend at each request.

The authentication workflow starts by requesting a login URL to Google. This login URL is unique and contains the 
information of the application which delegates the authentication to Google. Once the login is done by the user, a code
is returned to a callback endpoint. This code will be exchanged by an access_token. This access_token can now be used 
against Google to obtain the user's information.

The access_token must be stored in the frontend and used for every private request. 


# Frontend

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 16.0.4.

Angular Frontend connected to a Spring Boot backend.

The frontend is as simple as possible to just show the logic of authentication via a JWT.

## Run on local

```
yarn start
```

The application will run on `http://localhost:4200`

## Components

### App Component

Main wrapper component. It has the logic to display the Login form (if requested by the user), the protected content
(if the user is authenticated) or the welcome content (by default when loading the frontend).

### Login Form Component

The login form only displays a link with the Google's URL to log the user. This URL comes from the backend.

### Protected Content Component

This component requests a protected endpoint in the backend when loading.

### Welcome Content Component

This component request a public endpoint in the backend when loading.

## Authentication

The Authentication uses the OAuth2 workflow. 

To Authenticate a user, a request must be done first to the backend to obtain a Google URL where the user must log in.
Once logged, Google will call the frontend with a code. This code must sent to the backend to obtain an access_token.
The access_token will now be used to request protected endpoints. The access_token will be sent as a Bearer token in the
HTTP headers.
