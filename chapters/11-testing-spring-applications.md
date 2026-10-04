# 11. Testing Spring applications

[Back to notes index](../README.md)

| [Previous: Security and authorization](../chapters/10-security-and-authorization.md) | [Notes index](../README.md) | [Next: HTTP clients and resilience](../chapters/12-http-clients-and-resilience.md) |
|:--|:--:|--:|

## What I am learning here

Tests provide fast feedback at different boundaries. A plain unit test checks Java behavior without starting Spring. A slice test loads a focused part of the application, such as MVC or JPA. A full application test verifies wiring across a larger boundary and takes more time.

## Test an HTTP contract

~~~java
@WebMvcTest(BookController.class)
class BookControllerTest {
    @Autowired
    MockMvc mockMvc;

    @MockitoBean
    BookService bookService;

    @Test
    void returnsBookAsJson() throws Exception {
        given(bookService.findById(7L))
            .willReturn(new BookResponse(7L, "Dune"));

        mockMvc.perform(get("/api/books/7"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.title").value("Dune"));
    }
}
~~~

The MVC test loads the controller boundary and sends a request without opening a real network port. The service is replaced by a test double, so this test checks HTTP routing and JSON output rather than database behavior.

Use a unit test for a business rule that does not require Spring. Use a repository test with a test database for persistence behavior. Use a full context test when wiring, configuration, or application startup is the behavior being checked. Tests should assert observable behavior, not private implementation details.

Keep test data small and descriptive. A test name should say what input is being exercised and what result matters. When a test fails, the assertion should make the broken behavior easy to locate.

## Questions to review

1. What does a plain unit test isolate?
2. What is a test slice?
3. What does @WebMvcTest focus on?
4. What does MockMvc simulate?
5. Why replace a service with a test double in an MVC test?
6. When is a full application test useful?
7. What should a test assert?
8. Why keep test data small and descriptive?

| [Previous: Security and authorization](../chapters/10-security-and-authorization.md) | [Notes index](../README.md) | [Next: HTTP clients and resilience](../chapters/12-http-clients-and-resilience.md) |
|:--|:--:|--:|

