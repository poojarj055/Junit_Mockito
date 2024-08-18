### **Mockito Overview**

Mockito is a popular Java-based mocking framework used in unit testing. It allows you to create and configure mock objects, which simulate the behavior of real objects in a controlled way. This is particularly useful when testing the behavior of a class that depends on other classes or external systems.

#### **Why Use Mockito?**

- **Isolation of Units:** Mockito helps isolate the unit of code under test from its dependencies, ensuring that the test only checks the logic within the class being tested, not the behavior of its dependencies.
  
- **Simulation of External Dependencies:** You can simulate complex, time-consuming, or external dependencies (e.g., databases, web services) without needing to access them directly.

- **Behavior Verification:** Mockito allows you to verify that certain methods were called on mock objects with specific parameters, making it easier to validate interactions between objects.

#### **Basic Concepts and Annotations:**

1. **@Mock:**
   - Used to create and inject mock objects.
   - Example:
     ```java
     @Mock
     private Service service;
     ```

2. **@InjectMocks:**
   - Automatically injects the mocks (annotated with `@Mock`) into the class under test.
   - Example:
     ```java
     @InjectMocks
     private Controller controller;
     ```

3. **@Spy:**
   - Allows partial mocking. You can spy on a real object by wrapping it with a spy object, which lets you mock only specific methods while using the real implementation for others.
   - Example:
     ```java
     @Spy
     private List<String> spyList = new ArrayList<>();
     ```

4. **@Captor:**
   - Used to create an argument captor, which captures arguments passed to mocked methods for further assertions.
   - Example:
     ```java
     @Captor
     private ArgumentCaptor<String> captor;
     ```

5. **@RunWith(MockitoJUnitRunner.class):**
   - Enables Mockito annotations in test classes.
   - Example:
     ```java
     @RunWith(MockitoJUnitRunner.class)
     public class MyTest {
         // test logic here
     }
     ```

#### **Basic Usage Examples:**

1. **Creating a Mock Object:**
   ```java
   // Without annotations
   Service service = Mockito.mock(Service.class);

   // With @Mock annotation
   @Mock
   private Service service;
   ```

2. **Stubbing Methods:**
   - Stubbing allows you to define the behavior of a method when it is called on a mock object.
   - Example:
     ```java
     when(service.getData()).thenReturn("Mocked Data");
     ```

3. **Verifying Interactions:**
   - Verifying ensures that certain methods were called with specific parameters.
   - Example:
     ```java
     service.process("Input");
     verify(service).process("Input");
     ```

4. **Argument Captors:**
   - Argument captors are used to capture method arguments for further assertions.
   - Example:
     ```java
     @Captor
     ArgumentCaptor<String> captor;

     @Test
     public void testArgumentCaptor() {
         service.process("Input");
         verify(service).process(captor.capture());
         assertEquals("Input", captor.getValue());
     }
     ```

5. **Partial Mocks (Spies):**
   - Spies are used to create partial mocks. You can mock some methods while using the real implementation for others.
   - Example:
     ```java
     @Spy
     private List<String> spyList = new ArrayList<>();

     @Test
     public void testSpy() {
         spyList.add("one");
         spyList.add("two");

         verify(spyList).add("one");
         verify(spyList).add("two");

         assertEquals(2, spyList.size());
     }
     ```

6. **Injecting Mocks:**
   - Mocks can be injected into the class under test using `@InjectMocks`.
   - Example:
     ```java
     @InjectMocks
     private Controller controller;

     @Mock
     private Service service;

     @Test
     public void testController() {
         when(service.getData()).thenReturn("Mocked Data");

         String result = controller.processData();

         assertEquals("Processed: Mocked Data", result);
     }
     ```

#### **Example Usage of Mockito:**

```java
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;

class Service {
    public String getData() {
        return "Real Data";
    }
}

class Controller {
    private Service service;

    public Controller(Service service) {
        this.service = service;
    }

    public String processData() {
        String data = service.getData();
        return "Processed: " + data;
    }
}

public class ControllerTest {

    @Mock
    private Service service;

    @InjectMocks
    private Controller controller;

    public ControllerTest() {
        MockitoAnnotations.openMocks(this);  // Initialize mocks
    }

    @Test
    public void testProcessData() {
        // Stubbing the service method
        when(service.getData()).thenReturn("Mocked Data");

        // Calling the method under test
        String result = controller.processData();

        // Verifying the interaction and asserting the result
        verify(service).getData();
        assertEquals("Processed: Mocked Data", result);
    }
}
```

### **How Mockito is Used in Testing:**

- **Unit Testing:** Mockito is mainly used in unit tests to mock dependencies of the class under test. This ensures that you are only testing the logic in the class and not the behavior of its dependencies.

- **Behavior-Driven Development (BDD):** Mockito supports BDD by allowing behavior verification (e.g., `verify(mockObject).someMethod()`), which can be useful in ensuring that a specific behavior occurs under certain conditions.

- **Test-Driven Development (TDD):** Mockito can be used to mock dependencies during the TDD process, where you first write the test and then develop the code to satisfy the test.

Mockito simplifies the process of writing tests by allowing you to focus on the logic being tested rather than the dependencies. This makes your tests more reliable and easier to maintain.
