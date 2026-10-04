# 8. Service layer and transactions

[Back to notes index](../README.md)

| [Previous: Persistence with Spring Data JPA](../chapters/07-persistence-with-spring-data-jpa.md) | [Notes index](../README.md) | [Next: Database migrations and configuration](../chapters/09-database-migrations-and-configuration.md) |
|:--|:--:|--:|

## What I am learning here

A service coordinates a use case. It can apply business rules, call repositories, and decide which data to return. The controller handles HTTP mapping, while the repository handles persistence operations.

A transaction groups database work into one unit. If a transactional operation fails, the database can roll back its changes so partial updates are not left behind.

## Put the use case in a service

~~~java
@Service
public class BookService {
    private final BookRepository repository;

    public BookService(BookRepository repository) {
        this.repository = repository;
    }

    @Transactional
    public BookResponse create(CreateBookRequest request) {
        Book book = new Book(request.title().trim());
        Book saved = repository.save(book);
        return new BookResponse(saved.getId(), saved.getTitle());
    }

    @Transactional(readOnly = true)
    public List<BookResponse> findAll() {
        return repository.findAll().stream()
            .map(book -> new BookResponse(book.getId(), book.getTitle()))
            .toList();
    }
}
~~~

Place a transaction around the service operation that represents one unit of database work. readOnly = true documents a read path and may help a persistence provider optimize it. A transaction does not automatically include a remote HTTP service or make a distributed operation atomic.

Keep transaction boundaries clear. Avoid calling a method through the same object and assuming proxy-based transaction behavior will always apply. Start with the public service method invoked by another Spring bean, and test rollback behavior for important updates.

## Questions to review

1. What belongs in a service layer?
2. What does a repository handle?
3. What is a database transaction?
4. Why group related writes into one transaction?
5. What does readOnly = true communicate?
6. Does one database transaction automatically cover a remote HTTP call?
7. Which method is a clear place for a transaction boundary?
8. Why test rollback behavior for important updates?

| [Previous: Persistence with Spring Data JPA](../chapters/07-persistence-with-spring-data-jpa.md) | [Notes index](../README.md) | [Next: Database migrations and configuration](../chapters/09-database-migrations-and-configuration.md) |
|:--|:--:|--:|

