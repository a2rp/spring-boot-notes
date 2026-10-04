# 4. REST controllers and HTTP requests

[Back to notes index](../README.md)

| [Previous: Beans and dependency injection](../chapters/03-beans-and-dependency-injection.md) | [Notes index](../README.md) | [Next: DTOs and request validation](../chapters/05-dtos-and-validation.md) |
|:--|:--:|--:|

## What I am learning here

A REST controller maps HTTP requests to application behavior. @RestController combines controller discovery with response-body serialization, so a returned Java object is converted to JSON by the configured message converter.

HTTP methods communicate intent. GET reads data, POST creates or starts work, PUT replaces a resource, PATCH changes part of it, and DELETE removes it. A response should use a status code that matches the outcome.

## Read a resource and accept a query value

~~~java
@RestController
@RequestMapping("/api/books")
class BookController {
    private final BookService bookService;

    BookController(BookService bookService) {
        this.bookService = bookService;
    }

    @GetMapping("/{id}")
    BookResponse findById(@PathVariable long id) {
        return bookService.findById(id);
    }

    @GetMapping
    List<BookResponse> search(@RequestParam(defaultValue = "") String title) {
        return bookService.search(title);
    }
}
~~~

The path variable identifies one resource. A request parameter filters or adjusts a collection request. Keep controller methods focused on HTTP input and output; put business decisions in a service.

When a create operation needs a specific status or header, return ResponseEntity:

~~~java
@PostMapping
ResponseEntity<BookResponse> create(@RequestBody CreateBookRequest request) {
    BookResponse created = bookService.create(request);
    URI location = URI.create("/api/books/" + created.id());
    return ResponseEntity.created(location).body(created);
}
~~~

A 201 response means a resource was created. The Location header points to the new resource. Use a request DTO rather than accepting a persistence entity directly.

## Questions to review

1. What does @RestController add?
2. What does GET usually do?
3. Which HTTP method usually creates a resource?
4. What does @PathVariable read?
5. What does @RequestParam read?
6. Which application layer should hold business decisions?
7. What status does ResponseEntity.created use?
8. Why should a controller accept a request DTO?

| [Previous: Beans and dependency injection](../chapters/03-beans-and-dependency-injection.md) | [Notes index](../README.md) | [Next: DTOs and request validation](../chapters/05-dtos-and-validation.md) |
|:--|:--:|--:|

