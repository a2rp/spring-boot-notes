# 10. Security and authorization

[Back to notes index](../README.md)

| [Previous: Database migrations and configuration](../chapters/09-database-migrations-and-configuration.md) | [Notes index](../README.md) | [Next: Testing Spring applications](../chapters/11-testing-spring-applications.md) |
|:--|:--:|--:|

## What I am learning here

Spring Security adds authentication and authorization through a filter chain. Authentication establishes who is making a request. Authorization decides what that user may do. When the security starter is present, Spring Boot secures web requests by default until I define the application's intended access rules.

## Make access rules explicit

~~~java
@Configuration
class SecurityConfiguration {
    @Bean
    SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http
            .authorizeHttpRequests(authorize -> authorize
                .requestMatchers("/api/status").permitAll()
                .anyRequest().authenticated()
            )
            .httpBasic(Customizer.withDefaults())
            .build();
    }
}
~~~

This example leaves the status route public and requires authentication elsewhere. HTTP Basic is useful for a small demonstration, but a real application should choose an authentication method appropriate to its clients and deployment.

Authorization can also be checked at a service method when a rule is about the operation rather than the URL. Add method security deliberately and test both allowed and denied users.

Do not disable CSRF protection as a routine setup step. Browser-based authentication that automatically sends credentials has different risks from a stateless token API. Decide based on the authentication design, and keep passwords and signing keys in a secure configuration source.

## Questions to review

1. What does authentication establish?
2. What does authorization decide?
3. Where does Spring Security apply its filters?
4. What happens when the security starter is added without custom rules?
5. What does permitAll mean for a matched route?
6. Why is HTTP Basic only a simple example here?
7. When might service-method authorization help?
8. Why should CSRF protection not be disabled without a reason?

| [Previous: Database migrations and configuration](../chapters/09-database-migrations-and-configuration.md) | [Notes index](../README.md) | [Next: Testing Spring applications](../chapters/11-testing-spring-applications.md) |
|:--|:--:|--:|

