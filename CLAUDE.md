# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this module.

## Module Overview

The `test_utils` module is a Maven-based shared testing library that provides common testing utilities and templates for the entire ERP microservices ecosystem. It contains reusable testing components, patterns, and infrastructure that promote consistent testing practices across all Java-based modules.

## Technology Stack

- **Build Tool**: Maven
- **Java Version**: Java 8 (inherited from application parent)
- **Parent**: ERP Microservices Application (1.0.0-SNAPSHOT)
- **Testing Framework**: JUnit-based utilities
- **Packaging**: JAR library for test dependencies

## Project Structure

```
test_utils/
├── LICENSE                     # Apache 2.0 license
├── README.md                  # Module documentation
├── pom.xml                    # Maven build configuration
└── src/
    ├── main/java/test/utils/
    │   └── GwtTemplate.java    # Given-When-Then test template
    ├── docker/                 # Docker utilities for testing
    │   ├── Dockerfile         # Test container configuration
    │   └── people_and_organizations.sql  # Test data SQL
    └── test/java/             # Unit tests for test utilities
        └── META-INF/
            └── MANIFEST.MF    # Manifest file
```

## Build and Development Commands

### Maven Commands
```bash
# Build the library
mvn compile

# Run tests
mvn test

# Package JAR
mvn package

# Install to local repository
mvn install

# Generate dependency report
mvn dependency:tree
```

### Docker Utilities
```bash
# Build test container
docker build -f src/docker/Dockerfile -t erp-test-utils .

# Run test database
docker run -p 5432:5432 erp-test-utils
```

## Core Testing Utilities

### GwtTemplate Class
The `GwtTemplate` provides a standardized Given-When-Then testing structure:
- **Behavioral Testing**: Encourages behavior-driven test writing
- **Structured Approach**: Consistent test organization across projects
- **Readable Tests**: Clear separation of test phases
- **Documentation**: Self-documenting test structure

```java
public abstract class GwtTemplate {
    protected abstract void given();
    protected abstract void when();  
    protected abstract void then();
    
    protected void and() {
        // Continue the current phase
    }
    
    protected void but() {
        // Exception or alternative in current phase
    }
}
```

### Usage Example
```java
public class ServiceTest extends GwtTemplate {
    private Service service;
    private InputData input;
    private Result result;
    
    @Test
    public void shouldProcessValidInput() {
        given();
        when();
        then();
    }
    
    @Override
    protected void given() {
        // Setup test data and preconditions
        service = new Service();
        input = createValidInput();
    }
    
    @Override
    protected void when() {
        // Execute the operation under test
        result = service.process(input);
    }
    
    @Override
    protected void then() {
        // Verify the expected outcomes
        assertThat(result).isNotNull();
        assertThat(result.isSuccess()).isTrue();
    }
}
```

## Docker Testing Utilities

### Test Database Container
The module includes Docker configuration for testing databases:
- **PostgreSQL Setup**: Pre-configured test database container
- **Test Data**: SQL scripts for test data initialization
- **Isolation**: Each test run gets a fresh database instance
- **Performance**: Fast container startup for test execution

### Test Data Management
- **SQL Scripts**: Database initialization scripts for testing
- **Sample Data**: Realistic test data for various scenarios
- **Data Cleanup**: Automated cleanup between test runs
- **Schema Management**: Test schema setup and teardown

## Development Workflow

### Adding New Test Utilities
1. **Identify Patterns**: Look for common testing patterns across modules
2. **Create Abstractions**: Design reusable testing abstractions
3. **Test the Utilities**: Write tests for testing utilities themselves
4. **Documentation**: Document usage patterns and best practices
5. **Integration**: Update dependent modules to use new utilities

### Template Extensions
1. **Domain-Specific Templates**: Create specialized templates for different domains
2. **Integration Templates**: Templates for integration testing patterns
3. **Performance Templates**: Templates for performance testing
4. **Mock Integration**: Templates for mock-based testing

## Testing Best Practices

### Given-When-Then Pattern
- **Given**: Setup test preconditions, data, and mocks
- **When**: Execute the single operation being tested
- **Then**: Verify all expected outcomes and side effects
- **Clear Separation**: Each phase should have a single responsibility

### Test Organization
- **One Assert Per Test**: Focus each test on a single behavior
- **Descriptive Names**: Test names should describe the expected behavior
- **Test Data Builders**: Use builder pattern for complex test data
- **Isolation**: Tests should not depend on each other

### Performance Testing
- **Baseline Measurements**: Establish performance baselines
- **Resource Monitoring**: Monitor memory, CPU, and I/O during tests
- **Load Testing**: Test under realistic load conditions
- **Regression Detection**: Detect performance regressions automatically

## Integration with Testing Frameworks

### JUnit Integration
- **Test Base Classes**: Extend GwtTemplate for structured tests
- **Custom Assertions**: Domain-specific assertion methods
- **Test Rules**: Custom JUnit rules for setup and cleanup
- **Parameterized Tests**: Support for data-driven testing

### Mock Framework Integration
- **Mock Setup**: Utilities for setting up common mocks
- **Stub Configuration**: Pre-configured stub behaviors
- **Verification Helpers**: Utilities for mock verification
- **Test Doubles**: Support for various test double patterns

### Spring Test Integration
- **Test Configuration**: Spring test context configuration
- **Transaction Management**: Test transaction setup and rollback
- **Mock Beans**: Spring mock bean configuration utilities
- **Test Profiles**: Spring profile management for testing

## Docker Test Environment

### Container Configuration
The Docker setup provides:
- **PostgreSQL Database**: Containerized test database
- **Data Initialization**: Automated test data loading
- **Port Configuration**: Consistent port mapping for tests
- **Volume Management**: Temporary volume management for test data

### Usage in Tests
```java
@Testcontainers
public class DatabaseIntegrationTest extends GwtTemplate {
    
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:12")
            .withDatabaseName("test_db")
            .withUsername("test")
            .withPassword("test");
    
    @Test
    public void shouldConnectToDatabase() {
        given(); // Setup database connection
        when();  // Execute database operation
        then();  // Verify database state
    }
}
```

## Module Dependencies

### Core Dependencies
- **JUnit**: Core testing framework
- **Hamcrest**: Matcher library for assertions
- **Mockito**: Mock object framework (from parent)
- **Spring Test**: Spring testing utilities (when available)

### Docker Dependencies
- **Testcontainers**: Container-based testing (optional)
- **PostgreSQL Driver**: Database connectivity for tests
- **Docker**: Container runtime for test execution

## Usage Across Modules

### Module Integration
All Java-based modules should:
1. **Include as Dependency**: Add test_utils as test-scoped dependency
2. **Extend Templates**: Use GwtTemplate for structured testing
3. **Leverage Utilities**: Use provided testing utilities and patterns
4. **Follow Patterns**: Adopt consistent testing practices

### Dependency Declaration
```xml
<dependency>
    <groupId>erp_microservices</groupId>
    <artifactId>test-utils</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <scope>test</scope>
</dependency>
```

## Best Practices for Extension

### Creating New Templates
- **Single Responsibility**: Each template should serve a specific testing pattern
- **Composition Over Inheritance**: Favor composition for complex testing scenarios
- **Parameterization**: Support parameterized testing where appropriate
- **Documentation**: Include usage examples and guidelines

### Test Data Management
- **Builders**: Use builder pattern for complex test data creation
- **Factories**: Create factories for common test objects
- **Fixtures**: Provide reusable test fixtures
- **Cleanup**: Ensure proper cleanup of test resources

## Important Notes

- **Foundation Library**: Core testing foundation for all Java modules
- **Consistency**: Promotes consistent testing practices across the ERP system
- **BDD Support**: Encourages behavior-driven development practices
- **Docker Integration**: Provides containerized testing capabilities
- **Legacy Java**: Currently uses Java 8 - consider upgrade coordination
- **Shared Resource**: Changes impact all dependent test suites
- **Performance Impact**: Test utilities should be optimized for fast execution