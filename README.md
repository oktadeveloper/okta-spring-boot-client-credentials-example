# OAuth 2.0 Client Credentials With Spring Security
 
This example app shows how to implement the client credentials grant with Spring Boot and Spring Security 5.

Please read [How to Use Client Credentials Flow with Spring Security](https://developer.okta.com/blog/2021/05/05/client-credentials-spring-security) to see how this app was created.

**Prerequisites:** HTTPie, [Java 11](https://adoptopenjdk.net/) and an [Okta Developer Account](https://developer.okta.com).

> [Okta](https://developer.okta.com/) has Authentication and User Management APIs that reduce development time with instant-on, scalable user infrastructure. Okta's intuitive API and expert support make it easy for developers to authenticate, manage, and secure users and roles in any application.

* [Getting Started](#getting-started)
* [Links](#links)
* [Help](#help)
* [License](#license)

## Getting Started

The repository contains three sub-projects:

- `/secure-server` - a simple test server
- `/client-webclient` - a client built using the new WebClient
- `/client-resttemplate` - a client build using the deprecated RestTemplate

To run the sample app, the first step is to configure an Okta OIDC app for all three of the projects. Then, you can run the simple server (which has one endpoint at root). With the server running, you can run either or both of the clients. The clients demonstrate how to use the client credentials grant with Spring's WebClient and RestTemplate in Spring Security 5.

Before you begin, you’ll need a free Okta developer account. Install the [Okta CLI](https://cli.okta.com) and run `okta register` to sign up for a new account. If you already have an account, run `okta login`. 

Navigate a shell to the `/secure-server` sub-project. Run `okta apps create`. Select the default app name, or change it as you see fit. Choose **4: Service (Machine-to-Machine)** and press **Enter**. Select **1: Okta Spring Boot Starter**.

The `secure-server/src/main/resources/application.properties` should look like the following (with you own values for the issuer, client ID, and client secret.
```properties
okta.oauth2.issuer=https\://{yourOktaDomain}/oauth2/default
okta.oauth2.client-id={yourClientID}
okta.oauth2.client-secret={yourClientSecret}
```

### Add a Custom Scope to Your Authorization Server

Because the custom scope `mod_custom` is used in a `@Preauthorize` annotation, you need to add this custom scope to your Okta authorization server. Run `okta login` and open the resulting URL in your browser. Sign in to the Okta Admin Console. You may need to click the **Admin** button to get to your dashboard.

Go to **Security** > **API**. Select the **Default** authorization server by clicking on **default** in the table.

Select the **Scopes** tab. Click **Add Scope**.

Give the scope the following **Name**: `mod_custom`.

Give the scope whatever **Display Name** and **Description** you would like, or leave it blank. Click **Create** to continue.

### Test Client Credentials with Spring Boot

Start the server from the `secure-server` directory.
```bash
./mvnw spring-boot:run
```

The values above can be used to fill in the necessary values in the `src/main/resources/application.properties` file in **both** of the client directories.

`src/main/resources/application.properties`
```properties
spring.security.oauth2.client.registration.okta.client-id={yourClientId}
spring.security.oauth2.client.registration.okta.client-secret={yourClientSecret}
spring.security.oauth2.client.registration.okta.authorization-grant-type=client_credentials
spring.security.oauth2.client.registration.okta.scope=mod_custom
spring.security.oauth2.client.provider.okta.token-uri=https://{yourOktaUri}/oauth2/default/v1/token
spring.main.web-application-type=none
```

Open a shell and navigate to either of the client sub-project directories. Run the client.
```bash
./mvnw spring-boot:run
```

## Links

This example uses the following open source libraries:

* [Okta Spring Boot Starter](https://github.com/okta/okta-spring-boot)
* [Spring Boot](https://spring.io/projects/spring-boot)
* [Spring Security](https://spring.io/projects/spring-security)

## Help

Please post any questions as comments on the [blog post](https://developer.okta.com/blog/2021/05/05/client-credentials-spring-security), or visit our [Okta Developer Forums](https://devforum.okta.com/). 

## License

Apache 2.0, see [LICENSE](LICENSE).
