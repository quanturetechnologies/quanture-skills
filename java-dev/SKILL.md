---
name: java-dev
description: Develops, reviews, and debugs Java/Spring Boot code following Quanture standards. Use when writing Java classes, REST APIs with Spring Boot, JPA entities, services, or any Java-related development work.
---

# Java Development — Quanture Standards

## Stack

- Java 17+ (LTS)
- Spring Boot 3.x
- Maven (preferred) or Gradle
- JUnit 5 + Mockito for tests

## Project structure

```
src/
├── main/java/com/quanture/
│   ├── controller/     # REST endpoints
│   ├── service/        # business logic
│   ├── repository/     # data access (JPA)
│   ├── model/          # entities/DTOs
│   └── config/         # configuration
└── test/java/com/quanture/
```

## REST controller pattern

```java
@RestController
@RequestMapping("/api/v1/items")
@RequiredArgsConstructor
public class ItemController {

    private final ItemService itemService;

    @GetMapping("/{id}")
    public ResponseEntity<ItemDto> getItem(@PathVariable Long id) {
        return ResponseEntity.ok(itemService.findById(id));
    }

    @PostMapping
    public ResponseEntity<ItemDto> createItem(@Valid @RequestBody CreateItemRequest req) {
        return ResponseEntity.status(201).body(itemService.create(req));
    }
}
```

## Security rules

- **Never** store credentials in `application.properties` in plaintext
- Use `application.yml` with environment variable references:
  ```yaml
  spring:
    datasource:
      password: ${DB_PASSWORD}
  ```
- Enable Spring Security on all endpoints by default — whitelist exceptions explicitly
- Use `@PreAuthorize` for method-level security
- Validate all inputs with `@Valid` + Bean Validation annotations
- Never expose stack traces in API responses — use `@ControllerAdvice`

## Exception handling

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(ResourceNotFoundException e) {
        return ResponseEntity.status(404).body(new ErrorResponse(e.getMessage()));
    }
}
```

## JPA entity

```java
@Entity
@Table(name = "items")
@Data
@NoArgsConstructor
public class Item {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @CreationTimestamp
    private LocalDateTime createdAt;
}
```

## SQL — parameterized queries only

```java
// GOOD (Spring Data JPA)
Optional<Item> findByNameAndActive(String name, boolean active);

// GOOD (JPQL)
@Query("SELECT i FROM Item i WHERE i.name = :name")
List<Item> findByName(@Param("name") String name);

// NEVER — native string concat with user input
```

## Advanced reference

- **Spring Security**: See [reference/security.md](reference/security.md)
- **Testing**: See [reference/testing.md](reference/testing.md)
- **Database migrations**: See [reference/migrations.md](reference/migrations.md)
