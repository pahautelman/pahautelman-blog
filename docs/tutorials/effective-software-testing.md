# Effective Software Testing: A Developer's Guide

Effective and systematic software testing is critical for ensuring the reliability and quality of software systems. This guide provides a comprehensive overview of best code testing practices, inspired by the book 'Effective Software Testing: A Developer's Guide' by the software quality and testing expert Maurício Aniche 🐐.

The goal is to help developers understand the importance of proper testing, how to approach it systematically, and how to implement it effectively in their projects.

<br/>

## Why Testing is Important

**Quality Assurance**: Testing is essential because software failures can have severe consequences for businesses and end-users. Developers bear responsibility for the quality of the software they produce.

**Bug Prevention**: While simplicity in code design can reduce bugs, it cannot eliminate them. Testing is necessary to catch errors that arise from complex interactions within the software.

**Cost-Efficiency**: The cost of fixing bugs in production is often higher than the cost of thorough testing during development. Systematic testing reduces the risk of releasing buggy software, saving time and resources in the long run.

**Code Improvement**: Testing often leads to improvements in code quality. As developers write tests, they frequently identify areas where the code can be refactored for better clarity, efficiency, or maintainability. This process of writing tests and refactoring code results in more readable, maintainable, and robust software.

**Confidence in Changes**: A comprehensive test suite gives developers confidence to make changes or add new features, knowing that existing functionality can be quickly verified.

<br/>

## The Testing Pyramid

### Unit Testing
- **Focus**: Tests individual units (e.g., methods or functions) in isolation
- **Benefits**: Fast, easy to write, and control
- **Limitations**: May not catch bugs that arise from interactions between units

### Integration Testing
- **Focus**: Tests interactions between different units or with external systems
- **Benefits**: Identifies issues in the integration layer
- **Limitations**: More complex and time-consuming to write

### System Testing
- **Focus**: Tests the entire system as a whole, including all components and their interactions
- **Benefits**: Provides a realistic view of how the system behaves in production
- **Limitations**: Slower to execute, more prone to flaky results, and harder to write

### When to Use Each
- **Unit Tests**: For all business rules and logic
- **Integration Tests**: For complex integrations with external services
- **System Tests**: For critical workflows and high-risk areas of the application


<br/>

## Specification-Based Testing

Specification-based testing derives test cases from the software's requirements, ensuring the code meets its intended functionality.

### Steps for Specification-Based Testing

1. **Understand the Requirements**: Identify what the software should and should not do based on requirements or user stories
2. **Explore the Program**: Investigate how the software handles various inputs and outputs
3. **Identify Partitions**: Group inputs into partitions where the software is expected to behave similarly
4. **Identify Boundaries**: Focus on edge cases, such as values around decision points (e.g., >, <, == operators)
5. **Devise Test Cases**: Create tests based on identified partitions and boundaries, prioritizing edge cases and unique combinations
6. **Automate Test Cases**: Implement tests using tools like JUnit, ensuring tests are easy to read and maintain
7. **Augment with Creativity**: Use experience and intuition to identify additional test cases that may not be directly derived from the specifications

### Example Walkthrough: User Registration Function

Consider a user registration function with a username and password:

**Requirements**: The username must be 3-20 characters long and contain only alphanumeric characters. The password must be at least 8 characters long and contain at least one number, one capital letter, and one symbol.

**Partitions**:

* Username: null, too short, too long, invalid, valid
* Password: null, too short, invalid, valid

**Boundaries**: The test cases will include usernames of different lengths, from the shortest (2 characters—invalid, 3 characters—valid) to the longest (20 characters—valid, 21 characters—invalid). The same applies to password length: 7 characters—invalid, 8 characters—valid.

**Test Cases**:

* Test an empty username
* Test an empty password
* Test a valid username that is too short
* Test an invalid username of 3 and 20 characters
* Test a valid username
* Test a valid username that is too short
* Test a valid password that is too short
* Test an invalid password of 8 characters
* Test a valid password

**Augment with Experience**: We might know from experience that users may attempt to use symbols in their passwords that could pose security risks (e.g., backticks (`), pipe (|), angle brackets (<, >), etc.). Test cases should be added to ensure that passwords containing such characters are deemed invalid.

<br/>

## Structural Testing and Code Coverage

Structural testing, also known as white-box testing, involves testing the internal structure of the code to ensure thorough coverage of all its components. Unlike specification-based (black-box) testing, which focuses on functional requirements, structural testing delves into the code to ensure that all paths, branches, and conditions are adequately tested.

### Steps for Structural Testing

1. **Perform Specification-Based Testing**: Begin with tests derived from functional requirements to validate expected behaviour
2. **Review Code**: Carefully read the unit implementation to understand the developer's key coding decisions
3. **Run Tests with Coverage Tools**: Execute the devised test suite using code coverage tools
4. **Analyze Coverage Gaps**: Identify any untested code segments revealed by the coverage analysis
5. **Decide on Testing Missed Code**: Evaluate whether the untested code is significant enough to warrant additional testing
6. **Iterate and Refine**: Revisit the source code to identify other test cases that could be derived from the code structure

### Code Coverage Criteria

**Line Coverage**: Ensures that every line of code is executed at least once during testing. It is the most basic level of coverage.

**Branch Coverage**: Tests each branch of decision points (e.g., every true/false path of an if statement). This ensures that all possible outcomes of conditional statements are exercised.

**Condition Coverage**: It focuses on testing each individual condition within a decision point, ensuring that each condition has been evaluated as true or false.

**Modified Condition/Decision Coverage (MC/DC)**: MC/DC ensures that each condition within a decision affects the outcome of that decision independently of other conditions. This reduces the number of test cases while still providing rigorous testing of complex conditional logic.

**Path Coverage**: Ensures that all possible execution paths through the code are tested. This is more comprehensive than branch coverage as it accounts for all the possible execution sequences.

> ***Fun fact:** MC/DC is frequently required by industry standards for safety-critical systems, such as aviation, medicine, and infrastructure.*

### How to Apply MC/DC (Modified Condition/Decision Coverage)

1. **Identify the conditions and decision**: List all individual conditions in the decision and identify the overall decision being evaluated
2. **Create a truth table**: List all possible combinations of the conditions and calculate the decision outcome for each combination
3. **Determine independence pairs for each condition**: Find pairs of test cases for each condition where only that condition changes value and the change causes the decision outcome to change
4. **Select the minimum set of test cases**: Choose test cases that demonstrate the independent effect of each condition
5. **Verify coverage**: Confirm each condition independently affects the decision

### Example Walkthrough of MC/DC Coverage

For the decision `if (A && (B || C))`:

| Test | A | B | C | Result |
|------|---|---|---|--------|
| 1    | T | T | T | T      |
| 2    | T | T | F | T      |
| 3    | T | F | T | T      |
| 4    | T | F | F | F      |
| 5    | F | T | T | F      |
| 6    | F | T | F | F      |
| 7    | F | F | T | F      |
| 8    | F | F | F | F      |

**Condition A**: 

* Test with `A = true` and `A = false` ensuring different outcomes, with B or C kept constant.
* Test case 1: `A = true, B = true, C = true` (Result: `true`)
* Test case 5: `A = false, B = true, C = true` (Result: `false`)
* Explanation: These test cases show A independently affecting the outcome when B and C are true. When A changes from `true` to `false`, the overall result is `false`, and it demonstrates A's independent effect.
* Other pairs of test cases: {2, 6}, {3, 7}

**Condition B**: 

* Test with `B = true` and `B = false`, ensuring different outcomes, with A or C constant.
* Test case 2: `A = true, B = true, C = false` (Result: `true`)
* Test case 4: `A = true, B = false, C = false` (Result: `false`)
* Explanation: These test cases demonstrate B's independent effect. When B changes from `true` to `false`, the overall result changes from `true` to `false`.
* No other pairs of tests.

**Condition C**:

* Test with `C = true` and `C = false`, ensuring different outcomes, with A or B constant.
* Test case 3: `A = true, B = false, C = true` (Result: `true`)
* Test case 4: `A = true, B = false, C = false` (Result: `false`)
* Explanation: These test cases show C's independent effect. When C changes from `true` to `false`, the overall result changes from `true` to `false`.
* No other pairs of tests.

**Select Minimum Sets of Test Cases**:

* The test cases {2, 3, 4, 6} exercise the independent effect of all conditions.
* {2, 6} exercise A
* {2, 4} exercise B
* {3, 4} exercise C
* So we only need to write four tests to ensure thorough coverage of this decision.

Use this calculator to find minimal test cases needed to achieve MC/DC coverage: [MC/DC Calculator](https://pahautelman.github.io/mcdc-calculator/)

### Additional Notes

**Augmenting Specification-Based Testing**: Structural testing should augment specification-based testing. This ensures that functional requirements are met and that the code behaves as expected in all scenarios.

**Using Source Code for Structural Testing**: The source code is valuable for identifying test cases that may not be evident from the requirements alone. Structural testing leverages the code to uncover potential issues that specification-based testing might miss.

<br/>

## Designing Contracts

Designing contracts is a crucial aspect of software development that helps ensure the reliability and correctness of complex systems. This section will explore the concept of contracts, including pre-conditions, post-conditions, and invariants, and how they differ from validation.

### Understanding Contracts

Contracts in software development are formal agreements between different parts of a program about their behaviour. They specify what conditions must be met before a method is called (pre-conditions), what conditions will be true after the method executes (post-conditions), and what conditions remain constant throughout the execution of a method or the lifetime of an object (invariants).

#### Pre-conditions, Post-conditions, and Invariants

**Pre-conditions**:

* Define what the method needs to function properly
* Ensure that input values received by the method adhere to what is required
* Example: A method that calculates square root might have a pre-condition that the input must be non-negative

**Post-conditions**:

* Specify what the method guarantees as an outcome
* Ensure that the method returns what it promises to other methods
* Help detect bugs by throwing exceptions instead of returning invalid values
* Example: A method that sorts an array might have a post-condition that the returned array is in ascending order

**Invariants**:

* Conditions that always hold true for a class or object
* Maintained throughout the execution of methods
* Example: A binary search tree class might have an invariant that the left child is always smaller than the parent node

### Contracts vs. Validation

While both contracts and validation aim to ensure correct program behaviour, they serve different purposes:

**Contracts** define the expected behaviour and interactions between different parts of a program. They are typically checked during development and may be disabled in production for performance reasons.

**Validation** is about checking the correctness of data, usually at runtime, and is typically always active, even in production environments.

### Implementing Contracts

There are several ways to implement contracts in code:

**Using assert keyword (in Java)**:
```java
assert taxValue >= 0 : "Calculated tax value cannot be negative!";
```
* Can be disabled via JVM parameter in production
* Use with caution if you don't have full control of your production environment

**Using conditional statements**:
```java
if (taxValue < 0) {
    throw new IllegalStateException("Calculated tax value cannot be negative!");
}
```
* More suitable when you need the checks to always be active

**Documentation**: Clearly stating pre-conditions and post-conditions in method documentation is crucial and helps other developers understand how to use the method correctly.

### Strong vs. Weak Conditions

* **Strong conditions** halt execution when violated
* **Weak conditions** log errors but allow execution to continue
* Strong conditions are generally preferred as they reduce the range of potential errors in the code

### Example: Complex Financial System

Consider a large software system handling complex financial processes:

* The system chains calls to several subroutines in a complex flow
* Results from one class are passed to the next class, and so on
* At some point, a TaxCalculator class is called
* Based on the requirements, calculations in TaxCalculator only make sense for positive numbers

To handle this restriction:

1. Define clear contracts for each class, including TaxCalculator
2. Establish pre-conditions (e.g., input must be positive), post-conditions (e.g., calculated tax is non-negative), and invariants for the class
3. Implement these contracts in the code using assertions or conditional checks
4. Document the contracts clearly in the class and method documentation

By implementing contracts, we can catch errors early, improve code reliability, and make the system's behaviour more predictable and maintainable.

<br/>

## Property-Based Testing

*[TODO]*

<br/>

## Test Doubles and Mocks

In software development, classes often depend on other classes to perform their functions. While testing multiple classes together can be desirable, testing a unit in isolation is frequently necessary without relying on its dependencies. This is where test doubles come into play.

Consider a scenario where class A depends on classes B and C, which in turn have their own dependencies:

```
A -> B -> Database
  -> C -> External service
```

Testing A and its concrete dependencies might be too slow, too complex, or require too much setup. Test doubles allow us to isolate A and test it effectively.

### Types of Test Doubles

**Dummies**:

* Simple objects passed to the class under test but never actually used
* They fulfil parameter requirements without impacting the test

**Fake Objects**:

* Have working implementations of the simulated class, but in a simpler way
* Example: A fake database class using an ArrayList instead of a real database

**Stubs**:

* Provide hard-coded answers to calls made during the test
* Don't have fully working implementations
* Most popular type of simulation
* Used when you need a dependency to return a specific value for the method under test to continue execution

**Mocks**:

* Similar to stubs, but with additional capabilities
* Allow configuration of how they respond to method calls
* Record all interactions, enabling assertions on these interactions

**Spies**:

* Wrap around real objects and observe their behaviour
* Used when the actual implementation is easier to use, but you still want to assert how the method under test interacts with the dependency
* Less common than other test doubles

### Advantages of Using Test Doubles

* **Control**: Test doubles provide complete control over the dependency's behaviour, allowing simulation of various scenarios
* **Speed**: Tests with doubles run faster as they bypass the overhead of real implementations
* **Design Reflection**: Using test doubles encourages developers to think critically about class interactions, leading to better-designed interfaces and contracts

### Introduction to Mockito

Mocking frameworks like Mockito offer a simple API for setting up stubs and defining expectations on mock objects. Here are some crucial methods in Mockito:

**Creating a mock**:
```java
MockObject mock = mock(ClassToMock.class);
```

**Defining stub behaviour**:
```java
when(mock.someMethod()).thenReturn(someValue);
```

**Verifying interactions**:
```java
verify(mock).someMethod();
```

**Example using Mockito**:
```java
// Create a mock
List<String> mockedList = mock(List.class);

// Define behavior
when(mockedList.get(0)).thenReturn("first");

// Use the mock
System.out.println(mockedList.get(0)); // Outputs "first"

// Verify interaction
verify(mockedList, times(1)).get(0);
```

### Designing for Testability

It's important to note that changing product code to make testing easier is acceptable and often encouraged. This practice can lead to improvements in the overall code quality and design. When you find your code difficult to test, it's usually a sign that the code could benefit from refactoring. Common changes include:

- Introducing interfaces for better abstraction
- Breaking down oversized methods into smaller, more focused ones
- Using dependency injection to make dependencies more flexible and more accessible to mock

Remember, code that is easy to test is often more modular, has clearer responsibilities, and is generally of higher quality.

### Advanced Mocking Techniques

#### Argument Capturing

Argument capturing allows us to inspect arguments passed to mock methods. This is particularly useful when verifying complex objects passed to a method or when the method doesn't return a value.

#### Simulating Exceptions

Simulating exceptions with mocks helps us test how the system would behave in unexpected scenarios. This is particularly valuable when your application interacts with external systems that may not always behave as expected.

```java
@Test
void handleDatabaseConnectionFailure() {
    // Arrange
    DatabaseService mockDb = mock(DatabaseService.class);
    when(mockDb.connect()).thenThrow(new ConnectionException("Database unavailable"));
     
    MyService service = new MyService(mockDb);

    // Act & Assert that our service handles a database connection failure
    // correctly translate failure into ServiceUnavailableException
    assertThrows(ServiceUnavailableException.class, () -> {
        service.performCriticalOperation();
    });
}
```

### Best Practices and Common Pitfalls

- **Limit Mocking**: Don't overuse mocks. Tests with real dependencies are often more effective at finding real bugs
- **Mock Wisely**: Good candidates for mocking include slow dependencies, external infrastructure, and cases that are hard to simulate (e.g., exceptions)
- **Avoid Mocking**: Entities, value objects, or classes that represent business concepts; native Java libraries and utility methods; simple objects that are easy to instantiate
- **Keep Tests Clean**: Use helper methods or classes to encapsulate common setup logic
- **Design Stable Contracts**: For mocks to work effectively in large codebases, design careful and stable contracts between classes
- **Be Aware of Coupling**: Mocks can increase coupling between test and production code. Be mindful of this when designing your tests

### Handling Unmockable Components

Some components, like static methods or date/time operations, can be challenging to mock. Strategies for dealing with these include:

#### Wrapper Classes

Encapsulate unmockable components in wrapper classes that can be easily mocked.

Example for date and time operations:
```java
public class Clock {
  public LocalDateTime now() {
    return LocalDateTime.now();
  }
}
```

#### Dependency Injection

Dependency Injection (DI) is a design pattern that makes it easier to test classes by allowing dependencies to be easily swapped out:

```java
// Without Dependency Injection
public class UserService {
    private DatabaseConnection db = new DatabaseConnection();

    public User getUser(int id) {
        return db.fetchUser(id);
    }
}

// With Dependency Injection
public class UserService {
    private DatabaseConnection db;

    // Constructor Injection
    public UserService(DatabaseConnection db) {
        this.db = db;
    }

    public User getUser(int id) {
        return db.fetchUser(id);
    }
}

// In your test
@Test
void testGetUser() {
    // Arrange
    DatabaseConnection mockDb = mock(DatabaseConnection.class);
    when(mockDb.fetchUser(1)).thenReturn(new User(1, "John Doe"));

    UserService userService = new UserService(mockDb);

    // Act
    User user = userService.getUser(1);

    // Assert
    assertEquals("John Doe", user.getName());
    verify(mockDb).fetchUser(1);
}
```

#### Mocking Static Methods

Mockito 3.4.0 and later versions provide support for mocking static methods:

```java
import static org.mockito.Mockito.*;
import static org.mockito.ArgumentMatchers.*;

class MyServiceTest {

    @Test
    void testStaticMethodMocking() {
        try (MockedStatic<UtilityClass> mockedStatic = mockStatic(UtilityClass.class)) {
            // Arrange
            mockedStatic.when(() -> UtilityClass.staticMethod(anyString()))
                        .thenReturn("mocked result");

            // Act
            String result = MyService.methodUsingStaticUtility("input");

            // Assert
            assertEquals("expected result", result);
            mockedStatic.verify(() -> UtilityClass.staticMethod("input"));
        }
    }
}
```

### Conclusion

Test doubles and mocks are powerful tools for creating effective unit tests. By understanding when and how to use them, developers can create robust test suites that verify code correctness and contribute to better software design. Remember to balance the use of test doubles with tests that use real dependencies to ensure comprehensive coverage and realistic testing scenarios.

By incorporating these advanced techniques and design principles, you can create more robust and flexible tests. *Remember that the goal is not just to increase test coverage but to improve the overall design and reliability of our software.*

<br/>

## Test-Driven Development

Test-driven development (TDD) is a software development methodology in which tests are written before the actual code implementation. This approach inverts the traditional development process, where code is typically implemented first and tested later. TDD emphasizes writing automated test cases for each piece of functionality before writing the code to implement that functionality.

### TDD Process

The TDD process consists of three primary steps, often referred to as the Red-Green-Refactor cycle:

1. **Write a Test (Red)**: Begin by writing an automated test for the next piece of functionality. Initially, this test will fail because the required implementation does not yet exist
2. **Implement the Functionality (Green)**: Write the minimum code necessary to pass the test. The focus is on implementing just enough to meet the test's expectations
3. **Refactor**: Clean up and improve the production and test code while ensuring all tests continue to pass. Refactoring should reduce the amount of code needed to satisfy the tests and improve the production code's readability, maintainability, and design

These steps are repeated iteratively until the full functionality is implemented and all tests pass.

### Advantages of TDD

Adopting a test-driven development approach can provide several benefits:

- **Improved Code Quality**: By writing tests first, TDD encourages better design decisions and helps catch defects early in the development process, leading to higher-quality code
- **Rapid Feedback**: The tight feedback loop provided by TDD allows developers to quickly identify and address issues, preventing them from compounding over time
- **Enhanced Collaboration**: TDD fosters collaboration among developers, testers, and business analysts by ensuring alignment on requirements and test scenarios, reducing misunderstandings and improving team communication
- **Cost Efficiency**: By reducing defects and improving code quality, TDD can lead to lower maintenance costs and a reduced total cost of ownership for the software project
- **Improved Code Design**: Writing tests first can expose design issues early, as the test code often acts as the first client of the class or component being developed

### What Does Research Say About TDD?

The research on the effectiveness of TDD has produced mixed results:

- Janzen (2005) found that TDD practitioners produced less complex algorithms and more comprehensive test suites than non-TDD practitioners
- Janzen and Saiedian (2006) observed that TDD led to better use of object-oriented concepts and more effective class responsibility distribution
- George and Williams (2003) reported that while TDD initially reduced productivity for inexperienced developers, 92% of developers found it improved code quality
- Dogša and Batič (2011) concluded that TDD improved class design due to its simplicity
- Erdogmus et al. (2005) found that TDD increased productivity but did not significantly change code quality
- Nagappan et al. (2008) observed that TDD reduced pre-release defect density by 40-90% compared to projects that did not use TDD
- Müller and Hagner (2002) did not find that TDD accelerated implementation or improved reliability compared to traditional approaches
- Siniaalto and Abrahamsson (2007) found that TDD's benefits were unclear in small-scale projects
- Shull et al. (2010) reviewed multiple studies on TDD and did not observe a consistent effect on internal code quality

Recent studies suggest that the iterative nature of TDD, rather than TDD itself, contributes to improved focus and flow, which can ultimately impact code quality.

### When Not to Use TDD

While TDD is a valuable technique in many software development contexts, there are situations where it may not be the most appropriate approach:

- **Well-understood Problems**: If the problem and its solution are well-understood, writing tests before coding may add little value. Direct coding might be more efficient in such cases, though tests should still be written promptly
- **Lack of Test Automation**: TDD relies heavily on automated testing to provide quick feedback. If automated testing is not feasible, the TDD approach may not be practical

### Integrating TDD and Proper Testing

It's important to note that TDD is not solely focused on testing; rather, it aids in developing production code by ensuring it meets requirements through automated test cases. Once the TDD cycle is complete, engaging in effective and systematic testing, such as specification-based and structural testing, is essential to ensure comprehensive coverage. The tests created during the TDD process become part of the overall testing suite.

### Example Walkthrough

Consider this problem:

```java
/**
 * Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
 *
 * You may assume that each input would have exactly one solution, and you may not use the same element twice.
 *
 * You can return the answer in any order.
 *
 * Example1:
 * Input: nums = [2, 7, 11, 15], target = 9
 * Output: [0, 1]
 * Explanation: Because nums[0] + nums[1] == 0, we return [0, 1]
 *
 * Example2:
 * Input: nums = [3,3], target = 6
 * Output: [0,1]
 *
 * Constraints:
 * <ul>
 *     <li> 2 <= nums.length <= 104 </li>
 *     <li> -109 <= nums[i] <= 109 </li>
 *     <li> -109 <= target <= 109 </li>
 * </ul>
 */
public int[] twoSum(int[] nums, int target) {
    return null;
}
```

#### 🔴 Red Phase
Let's start by devising test cases that check the method constraints.
```java
import org.example.Main;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;
 
import java.util.stream.IntStream;
import java.util.stream.Stream;
 
import static org.junit.jupiter.api.Assertions.assertThrows;
 
public class MainTest {
 
    private Main main = new Main();
 
    static Stream<Arguments> invalidInputProvider() {
        // nums length < 2
        int[] nums1 = {1};
        Arguments arguments1 = Arguments.of(nums1, 1);
 
        // nums length > 104
        int[] nums2 = IntStream.range(0, 105).toArray();
        Arguments arguments2 = Arguments.of(nums2, 1);
 
        // -109 < num[i]
        int[] nums3 = {1, 6, 4, -110, 2, 3};
        Arguments arguments3 = Arguments.of(nums3, 1);
 
        // nums[i] > 109
        int[] nums4 = {2, 4, 6, 109, 110};
        Arguments arguments4 = Arguments.of(nums4, 1);
 
        // target < -109
        int[] nums5 = {-100, -10};
        int target5 = -110;
        Arguments arguments5 = Arguments.of(nums5, target5);
 
        // target > 110
        int[] nums6 = {55, 11, 67};
        int target6 = 110;
        Arguments arguments6 = Arguments.of(nums6, target6);
 
        return Stream.of(
            arguments1,
            arguments2,
            arguments3,
            arguments4,
            arguments5,
            arguments6
        );
    }
 
    @ParameterizedTest
    @MethodSource("invalidInputProvider")
    @DisplayName("Test invalid method inputs")
    void twoSum_breakMethodConstraints_shouldFail(int[] nums, int target) {
        assertThrows(IllegalArgumentException.class, () -> {
            main.twoSum(nums, target);
        });
    }
}
```
This first test forces us to define what should happen if the input is invalid, a case not specified in the method requirements. We decided to throw an `IllegalArgumentException`.


#### 🟢 Green Phase
Implementing the solution is straightforward. We only need to add the method preconditions.
```java
public int[] twoSum(int[] nums, int target) {
    if (nums.length < 2 || nums.length > 104) {
        throw new IllegalArgumentException("Nums size is not valid!");
    }
    if (Arrays.stream(nums).anyMatch(num -> num < -109 || num > 109)) {
        throw new IllegalArgumentException("Nums element is not in bounds!");
    }
    if (target < -109 || target > 109) {
        throw new IllegalArgumentException("Target is not valid number!");
    }
 
    return null;
}
```

#### ⚫ Refactor Phase
For refactoring, one could consider extracting the validation checks to a separate method, but we will not do that as the method is not too complex yet.

*The process continues iteratively:*

#### 🔴 Red Phase (Next Cycle)
In the next cycle, we'll extend the test suite to verify that the method actually outputs the correct values.
```java
static Stream<Arguments> correctValuesAndOutputProvider() {
        // simple length 2 input array
        int[] nums1 = {-109, 109};
        int target1 = 0;
        int[] expected1 = {0, 1};
        Arguments arguments1 = Arguments.of(nums1, target1, expected1);
 
        // length 3 input array
        int[] nums2 = {5, 17, 92};
        int target2 = 109;
        int[] expected2 = {1, 2};
        Arguments arguments2 = Arguments.of(nums2, target2, expected2);
 
        // length 4 input array
        int[] nums3 = {1, 6, 6, 10};
        int target3 = 12;
        int[] expected3 = {1, 2};
        Arguments arguments3 = Arguments.of(nums3, target3, expected3);
 
        // length 104 input array
        int[] nums4 = new int[104];
        for (int i = 0; i < 102; i++) {
 nums4[i] = 0;
 }
 nums4[102] = 9;
 nums4[103] = 10;
        int target4 = 19;
        int[] expected4 = {102, 103};
        Arguments arguments4 = Arguments.of(nums4, target4, expected4);
 
        return Stream.of(
 arguments1,
 arguments2,
 arguments3,
 arguments4
 );
 }
 
 @ParameterizedTest
 @MethodSource("correctValuesAndOutputProvider")
 @DisplayName("Test correct method output")
    void twoSum_correctInput_correctOutput(int[] nums, int target, int[] expected) {
        int[] result = main.twoSum(nums, target);
        Arrays.sort(expected);
        Arrays.sort(result);
 
        assertArrayEquals(expected, result);
 }
```

#### 🟢 Green Phase
Implementing the solution that passes the tests.
```java
public int[] twoSum(int[] nums, int target) {
    if (nums.length < 2 || nums.length > 104) {
        throw new IllegalArgumentException("Nums size is not valid!");
    }
    if (Arrays.stream(nums).anyMatch(num -> num < -109 || num > 109)) {
        throw new IllegalArgumentException("Nums element is not in bounds!");
    }
    if (target < -109 || target > 109) {
        throw new IllegalArgumentException("Target is not valid number!");
    }
 
    int[] result = new int[2];
 
    outterLoop:
    for (int i = 0; i < nums.length - 1; i++) {
        for (int j = i+1; j < nums.length; j++) {
            if (nums[i] + nums[j] == target) {
                result[0] = i;
                result[1] = j;
                break outterLoop;
            }
        }
    }
 
    return result;
}
```

#### ⚫ Refactor Phase
😛😛

#### 🔴 Red Phase (Final Cycle)
Next, let's test the case where the method can't find a result.
```java
@Test
void twoSum_correctInput_cantFindResult() {
    int[] nums = {1, 3, 5, 7};
    int target = 5;
 
    assertNull(main.twoSum(nums, target));
}
```

#### 🟢 Green Phase
Updating the implementation to handle this case.
```java
public int[] twoSum(int[] nums, int target) {
    if (nums.length < 2 || nums.length > 104) {
        throw new IllegalArgumentException("Nums size is not valid!");
    }
    if (Arrays.stream(nums).anyMatch(num -> num < -109 || num > 109)) {
        throw new IllegalArgumentException("Nums element is not in bounds!");
    }
    if (target < -109 || target > 109) {
        throw new IllegalArgumentException("Target is not valid number!");
    }
    int[] result = null;
 
    outterLoop:
    for (int i = 0; i < nums.length - 1; i++) {
        for (int j = i+1; j < nums.length; j++) {
            if (nums[i] + nums[j] == target) {
                result = new int[]{i, j};
                break outterLoop;
            }
        }
    }
 
    return result;
}
```

#### ⚫ Final Refactor
Now, the method meets the expected functionality. We could consider rewriting the nested loop, adding post-conditions, or improving the method runtime (`O(n)` is possible) for further refactoring. These are left as exercises for the reader.
```java
TODO
```


## Writing Larger Tests

*[TODO]*

## Test Code Quality

*[TODO]*

---

*This guide provides a comprehensive overview of effective software testing practices. For more advanced topics and specific implementation details, consider referring to additional resources and the original book by Maurício Aniche.*
