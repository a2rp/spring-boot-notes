# 7. Persistence with Spring Data JPA

[Back to notes index](../README.md)

| [Previous: Error handling and API responses](../chapters/06-error-handling-and-api-responses.md) | [Notes index](../README.md) | [Next: Service layer and transactions](../chapters/08-services-and-transactions.md) |
|:--|:--:|--:|

## What I am learning here

JPA maps Java entity objects to relational database rows. Spring Data JPA creates repository implementations from an interface, so common queries do not need hand-written plumbing. Entities model persistence; request and response DTOs model the HTTP boundary.

## Map one entity and its repository

~~~java
import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.Id;

@Entity
public class Book {
    @Id
    @GeneratedValue
    private Long id;

    @Column(nullable = false)
    private String title;

    protected Book() {}

    public Book(String title) {
        this.title = title;
    }

    public Long getId() {
        return id;
    }

    public String getTitle() {
        return title;
    }
}
~~~

~~~java
import org.springframework.data.jpa.repository.JpaRepository;

public interface BookRepository extends JpaRepository<Book, Long> {
    List<Book> findByTitleContainingIgnoreCase(String title);
}
~~~

The entity needs an identifier and a no-argument constructor that JPA can use. The repository type parameters are the entity type and ID type. Spring Data derives a query from the method name for common cases.

Relationships such as one-to-many and many-to-many affect loading, serialization, and database writes. Model them only when the domain requires them. Avoid returning a JPA entity directly from a controller because lazy relationships and persistence fields are not an API contract.

## Questions to review

1. What does JPA map?
2. What does @Entity mark?
3. What does @Id identify?
4. Why does a JPA entity need an identifier?
5. What implementation does Spring Data create?
6. What do JpaRepository's type parameters represent?
7. What can a derived query method do?
8. Why keep JPA entities out of API responses?

| [Previous: Error handling and API responses](../chapters/06-error-handling-and-api-responses.md) | [Notes index](../README.md) | [Next: Service layer and transactions](../chapters/08-services-and-transactions.md) |
|:--|:--:|--:|

