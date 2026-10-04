# Spring Boot Study Notes

This repository is my personal set of Spring Boot study notes. I record what I learn while working with Java and Spring Boot, keeping concepts, examples, and review questions together so I can revisit how an application is structured and why each layer exists.

## Scope

These notes focus on Java web applications with Spring Boot 4.1.1. They cover application setup, configuration, dependency injection, REST endpoints, validation, data access, transactions, security, testing, observability, and deployment. Examples use Java and Maven, with notes about where Gradle fits.

Spring Boot 4.1.1 requires Java 17 or later. The examples use Java 21 as a practical baseline. The official requirements also list Maven 3.6.3 or later and Gradle 8.14 or later in the 8.x line, or Gradle 9.x. Version requirements can change, so I check the linked official documentation when setting up a new project.

## How these notes are organized

The first 15 chapters cover the main concepts in a practical order. Each chapter includes explanations, Java examples, review questions, and bordered links to the previous and next chapter. Chapter 98 collects the code samples, and Chapter 99 contains the questions with their answers.

## Core topics

1. [Spring Boot foundations and first application](chapters/01-foundations-and-first-application.md)
2. [Project structure and configuration](chapters/02-project-structure-and-configuration.md)
3. [Beans and dependency injection](chapters/03-beans-and-dependency-injection.md)
4. [REST controllers and HTTP requests](chapters/04-rest-controllers-and-http-requests.md)
5. [DTOs and request validation](chapters/05-dtos-and-validation.md)
6. [Error handling and API responses](chapters/06-error-handling-and-api-responses.md)
7. [Persistence with Spring Data JPA](chapters/07-persistence-with-spring-data-jpa.md)
8. [Service layer and transactions](chapters/08-services-and-transactions.md)
9. [Database migrations and configuration](chapters/09-database-migrations-and-configuration.md)
10. [Security and authorization](chapters/10-security-and-authorization.md)
11. [Testing Spring applications](chapters/11-testing-spring-applications.md)
12. [HTTP clients and resilience](chapters/12-http-clients-and-resilience.md)
13. [Actuator, health, and observability](chapters/13-actuator-health-and-observability.md)
14. [Scheduled and background work](chapters/14-scheduled-and-background-work.md)
15. [Packaging and deployment](chapters/15-packaging-and-deployment.md)
16. [All code samples](chapters/98-all-code-samples.md)
17. [Complete questions and answers](chapters/99-complete-q-and-a.md)

## Learning in practice

I use these notes to understand how a request moves through an application, from the web layer to business logic and data access. Small examples show one responsibility at a time; later chapters connect those responsibilities into a maintainable service. I update the notes as I test code and learn how the framework behaves.

## Reference links

- [Spring Boot reference documentation](https://docs.spring.io/spring-boot/reference/)
- [Spring Boot 4.1.1 system requirements](https://docs.spring.io/spring-boot/system-requirements.html)
- [Spring Boot 4.1 release announcement](https://spring.io/blog/2026/06/10/spring-boot-4/)

## Links

- Portfolio: https://www.ashishranjan.net
- GitHub: https://github.com/a2rp
- CodePen: https://codepen.io/ash1198
- LinkedIn: https://www.linkedin.com/in/aashishranjan
- Facebook: https://www.facebook.com/theash.ashish/
- YouTube: https://www.youtube.com/@ashishranjan-ashz?sub_confirmation=1
- Email: mailto:ash.ranjan09@gmail.com

## Support

- Support: https://a2rp-donation-page.netlify.app/
- Buy Me a Coffee: https://buymeacoffee.com/ashishranjan
- Patreon: https://www.patreon.com/ashishranjan