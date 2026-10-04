# 16. All code samples

[Back to notes index](../README.md)

| [Previous: Packaging and deployment](15-packaging-and-deployment.md) | [Notes index](../README.md) | [Next: Complete questions and answers](99-complete-q-and-a.md) |
|:--|:--:|--:|

## 1. Spring Boot foundations and first application

~~~xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>4.1.1</version>
    <relativePath/>
</parent>

<properties>
    <java.version>21</java.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-webmvc</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-webmvc-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
~~~

~~~java
package com.example.notes;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class NotesApplication {
    public static void main(String[] args) {
        SpringApplication.run(NotesApplication.class, args);
    }
}
~~~

~~~java
package com.example.notes;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
class GreetingController {
    @GetMapping("/api/greeting")
    GreetingResponse greeting() {
        return new GreetingResponse("Hello, Spring Boot");
    }
}

record GreetingResponse(String message) {}
~~~

~~~sh
./mvnw spring-boot:run
~~~

## 2. Project structure and configuration

~~~text
src/
├── main/
│   ├── java/com/example/notes/
│   │   └── NotesApplication.java
│   └── resources/
│       └── application.properties
└── test/
    └── java/com/example/notes/
pom.xml
mvnw
mvnw.cmd
~~~

~~~properties
server.port=8080
app.catalog.page-size=20
app.catalog.title=Learning shelf
~~~

~~~java
@ConfigurationProperties(prefix = "app.catalog")
public record CatalogProperties(int pageSize, String title) {}
~~~

## 3. Beans and dependency injection

~~~java
public interface NoticeSender {
    void send(String destination, String message);
}
~~~

~~~java
@Component
class ConsoleNoticeSender implements NoticeSender {
    @Override
    public void send(String destination, String message) {
        System.out.println("Sending to " + destination + ": " + message);
    }
}
~~~

~~~java
@Service
public class ReminderService {
    private final NoticeSender noticeSender;

    public ReminderService(NoticeSender noticeSender) {
        this.noticeSender = noticeSender;
    }

    public void remind(String email) {
        noticeSender.send(email, "Your reminder is ready.");
    }
}
~~~

## 4. REST controllers and HTTP requests

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

~~~java
@PostMapping
ResponseEntity<BookResponse> create(@RequestBody CreateBookRequest request) {
    BookResponse created = bookService.create(request);
    URI location = URI.create("/api/books/" + created.id());
    return ResponseEntity.created(location).body(created);
}
~~~

## 5. DTOs and request validation

~~~java
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.Size;

public record CreateMemberRequest(
    @NotBlank
    @Size(max = 80)
    String name,

    @NotBlank
    @Email
    String email
) {}
~~~

~~~java
@PostMapping
ResponseEntity<MemberResponse> create(
        @Valid @RequestBody CreateMemberRequest request) {
    MemberResponse created = memberService.create(request);
    URI location = URI.create("/api/members/" + created.id());
    return ResponseEntity.created(location).body(created);
}
~~~

## 6. Error handling and API responses

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

## 7. Persistence with Spring Data JPA

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

## 8. Service layer and transactions

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

## 9. Database migrations and configuration

~~~properties
spring.datasource.url=jdbc:postgresql://localhost:5432/catalog
spring.datasource.username=catalog_app
spring.jpa.hibernate.ddl-auto=validate
spring.flyway.enabled=true
~~~

~~~sql
CREATE TABLE book (
    id BIGINT GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
    title VARCHAR(200) NOT NULL
);
~~~

## 10. Security and authorization

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

## 11. Testing Spring applications

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

## 12. HTTP clients and resilience

~~~java
@Configuration
class CatalogClientConfiguration {
    @Bean
    RestClient catalogRestClient(RestClient.Builder builder) {
        return builder
            .baseUrl("https://catalog.example.test")
            .build();
    }
}
~~~

~~~java
@Component
class CatalogClient {
    private final RestClient restClient;

    CatalogClient(RestClient catalogRestClient) {
        this.restClient = catalogRestClient;
    }

    CatalogItem findById(long id) {
        return restClient.get()
            .uri("/api/items/{id}", id)
            .retrieve()
            .body(CatalogItem.class);
    }
}
~~~

## 13. Actuator, health, and observability

~~~properties
spring.application.name=notes-service
management.endpoints.web.exposure.include=health,info
management.endpoint.health.show-details=when-authorized
~~~

## 14. Scheduled and background work

~~~java
@Configuration
@EnableScheduling
class SchedulingConfiguration {}

@Component
class CatalogRefreshJob {
    @Scheduled(fixedDelayString = "PT5M")
    void refreshCatalog() {
        System.out.println("Refresh catalog data");
    }
}
~~~

## 15. Packaging and deployment

~~~sh
./mvnw clean verify
./mvnw package
java -jar target/notes-service-0.0.1-SNAPSHOT.jar
~~~

~~~dockerfile
FROM eclipse-temurin:21-jre
WORKDIR /app
COPY target/notes-service.jar app.jar
USER 10001
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
~~~

| [Previous: Packaging and deployment](15-packaging-and-deployment.md) | [Notes index](../README.md) | [Next: Complete questions and answers](99-complete-q-and-a.md) |
|:--|:--:|--:|
