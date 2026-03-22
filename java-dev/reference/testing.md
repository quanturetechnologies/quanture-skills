# Java Testing Reference

## Unit test (JUnit 5 + Mockito)

```java
@ExtendWith(MockitoExtension.class)
class ItemServiceTest {

    @Mock
    private ItemRepository itemRepository;

    @InjectMocks
    private ItemService itemService;

    @Test
    void findById_returnsItem_whenExists() {
        Item item = new Item(1L, "Test");
        when(itemRepository.findById(1L)).thenReturn(Optional.of(item));

        ItemDto result = itemService.findById(1L);

        assertThat(result.getName()).isEqualTo("Test");
    }

    @Test
    void findById_throws_whenNotFound() {
        when(itemRepository.findById(99L)).thenReturn(Optional.empty());

        assertThrows(ResourceNotFoundException.class,
            () -> itemService.findById(99L));
    }
}
```

## Integration test (Spring Boot)

```java
@SpringBootTest
@AutoConfigureMockMvc
class ItemControllerIT {

    @Autowired
    private MockMvc mockMvc;

    @Test
    void getItem_returns200() throws Exception {
        mockMvc.perform(get("/api/v1/items/1")
                .header("Authorization", "Bearer " + testToken))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.id").value(1));
    }
}
```

## Run

```bash
mvn test
mvn verify  # includes integration tests
```
