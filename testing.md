# Testing (The Physcology of Code Testability by Misko Hevery @Frontendmasters)

### Hard to test code:
- Mixing new operator with the logic
- Looking for things
- Work in constructor
- Global state
- Singletons
- Static methods
- Deep inheritance
- Too many conditions
- Mixing services/value
- Mixing concerns
- Structure of the code

### Testable code:
- Good OO
- Denpendency Injection
- Test Driven Development from the beginning

### Current state of the art of testing and problems:
- Testing entire app as a single unit
- Tests covering very large scenarios
- Using test frameworks that pretend to be user doing user based testing and not functional/logic level testing
- Testing real app with real dependencies which is slow and flaky and may give false results

### Better approach:
- Break down application into sub systems/modules and test them with external dependencies replaced by simulator
- Have lots of unit tests of individual components
- Have functional tests and scenario tests to test the wiring of these components

### Managing external dependencies:
- There are 3 ways to get your dependencies: create them, access them from global namespace or get them as inputs.
- Creating them is game over as we can't simulate them for testing.


### Writing testable code:
- Always have dependency injected rather than created or access from global namespace.
- Have separate module for object graph construction/lookup and business logic.




