---
description: New prompt created from chat session
mode: agent
tools: ['extensions', 'usages', 'vscodeAPI', 'think', 'problems', 'changes', 'testFailure', 'openSimpleBrowser', 'fetch', 'findTestFiles', 'githubRepo', 'todos', 'runTests', 'runCommands', 'runTasks', 'edit', 'notebooks', 'search', 'new']
model: GPT-4.1
---
<user>
Follow instructions in [BookingCoordinator.DecisionTable.xunit.prompt.md](file:///Users/stm/src/mjo/LegacyBookingCoordinator/.github/prompts/BookingCoordinator.DecisionTable.xunit.prompt.md).
</user>
<assistant>

</assistant>
<user>
Follow instructions in [BookingCoordinator.DecisionTable.xunit.prompt.md](file:///Users/stm/src/mjo/LegacyBookingCoordinator/.github/prompts/BookingCoordinator.DecisionTable.xunit.prompt.md).
</user>
<assistant>
Here is the plan for the test data matrix:

- Each scenario will be represented by a `BookingTestScenario` object with the following fields:
  - `availableSeats`
  - `passengerCount`
  - `specialRequests` (string or array)
  - `airlineCode`
  - `bookingCounter`
  - `failureReasonExists`
  - `finalPrice`
  - `expectedBookingStatus`
  - `expectException`
  - `expectPartnerNotification`
  - `expectSpecialNotification`
  - `expectEncryption`
  - `expectLogging`
  - `expectedResultAction` (for mapping to decision table row)

I will enumerate at least one scenario for each decision table row, including edge cases (e.g., not enough seats, premium price, group booking, special requests, encryption, notification logic, logging).

Next, I will scaffold the test class and methods in `BookingCoordinatorDecisionTableTests.cs`. 
```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```
The test suite is now discovered and running, but all tests are failing due to two main issues:

1. **Exception Type Mismatch:**  
   - The test expects `InvalidOperationException` for not enough seats, but receives `CanNotUseInTestsException` because the real `BookingRepository`, `FlightAvailabilityService`, etc. throw this in tests.
   - This means the test doubles are not properly injected or configured to avoid real class instantiation.

2. **Verify/Prefix Uniqueness Error:**  
   - Multiple calls to `context.Verify` or `Assert.Throws` in the same test method cause SpecRec/Verify to complain about duplicate prefixes.
   - This is because each scenario in `[Theory]` is using the same method name and parameters, causing conflicts.

**Next steps:**
- Fix test double injection so that no real classes are instantiated (use SpecRec's Substitute/Parrot correctly).
- Refactor the test so that only one call to `context.Verify` or `Assert.Throws` is made per test case, and ensure unique prefixes for each scenario.

I will update the test to resolve these issues. The root cause of the test failures is:

- The test is still instantiating real classes for dependencies, which throw `CanNotUseInTestsException` instead of the expected business exceptions.
- SpecRec's `Verify` is being called with the same method name for all `[Theory]` scenarios, causing prefix uniqueness errors.

**To fix:**
- Ensure all dependencies are properly substituted with SpecRec test doubles so that no real class is ever instantiated.
- Pass a unique test name to `context.Verify` for each scenario, using scenario data.

Would you like me to proceed with a patch that implements these fixes?
</assistant>