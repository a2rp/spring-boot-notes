# 6. Error handling and API responses

[Back to notes index](../README.md)

| [Previous: DTOs and request validation](../chapters/05-dtos-and-validation.md) | [Notes index](../README.md) | [Next: Persistence with Spring Data JPA](../chapters/07-persistence-with-spring-data-jpa.md) |
|:--|:--:|--:|

## What I am learning here

An API should return a useful status and a safe error body when something fails. Controllers should not repeat the same exception handling logic. @RestControllerAdvice centralizes translation from application exceptions to HTTP responses.

Spring supports ProblemDetail responses for structured HTTP errors. The response can include a status, a short title, and a detail that is safe to show to the client.

## Map a missing resource to 404

~~~java
@RestControllerAdvice
class ApiExceptionHandler {
    @ExceptionHandler(BookNotFoundException.class)
    ProblemDetail handleBookNotFound(BookNotFoundException exception) {
        ProblemDetail problem = ProblemDetail.forStatusAndDetail(
            HttpStatus.NOT_FOUND,
            exception.getMessage()
        );
        problem.setTitle("Book not found");
        return problem;
    }
}
~~~

Throw BookNotFoundException from the service when the requested ID does not exist. The advice maps that application condition to 404 Not Found. Keep internal stack traces and database details out of the response.

Use different responses for different situations: 400 for invalid input, 401 for missing authentication, 403 for insufficient permission, 404 for a missing resource, and 500 for an unexpected server failure. Log unexpected failures with a request identifier, while returning a calm generic message to the caller.

## Questions to review

1. Why centralize API error handling?
2. What does @RestControllerAdvice apply to?
3. Which status represents a missing resource?
4. What does ProblemDetail contain?
5. Should a response reveal database credentials or stack traces?
6. What is the difference between 401 and 403?
7. Where should missing-resource conditions usually be detected?
8. How should an unexpected server failure be shown to a client?

| [Previous: DTOs and request validation](../chapters/05-dtos-and-validation.md) | [Notes index](../README.md) | [Next: Persistence with Spring Data JPA](../chapters/07-persistence-with-spring-data-jpa.md) |
|:--|:--:|--:|

