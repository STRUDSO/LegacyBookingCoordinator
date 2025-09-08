## **Task Researcher**: Deep Analysis of xUnit Test Suite Creation for BookingCoordinator.cs Using Decision Table

### Research Executed

#### File Analysis
- BookingCoordinator.cs
  - Contains the main orchestration logic for flight bookings, with complex state, side effects, and multiple service integrations.
- `/Users/stm/src/mjo/LegacyBookingCoordinator/QA AI Day/Decistion table from code/Step3.BookingCoordinator.Decistion.Table.md`
  - Provides a comprehensive decision table mapping input conditions to expected outcomes for the booking process.

#### Code Search Results
- `xunit|[Theory]|[Fact]`
  - Found usage of `[Theory]` and `[SpecRecLogs]` in existing test files, indicating parameterized scenario testing.
- `BookingCoordinator`
  - Existing test file: BookingCoordinatorTests.cs covers some orchestration scenarios.

#### External Research
- #githubRepo:"xunit/xunit decision table test"
  - Found patterns for mapping decision tables to `[Theory]`-based tests using `[MemberData]` or inline data.
- #fetch:https://xunit.net/docs/getting-started/netfx/visual-studio
  - Official xUnit documentation recommends `[Theory]` for scenario-driven tests, with `[MemberData]` for complex input sets.

#### Project Conventions
- Standards referenced: copilot-instructions.md (use `[Theory]`, `SpecRecLogs`, and test doubles via `Context`)
- Instructions followed: decisiontable.instructions.md (decision table mapping for test coverage)

---

### Key Discoveries

#### Project Structure
- Tests are located in BookingCoordinatorTests.cs
- Test doubles and scenario logging are handled via SpecRec utilities (`Context`, `Parrot`, `Wrap`)

#### Implementation Patterns
- Use `[Theory]` for parameterized tests
- Use `[MemberData]` or custom data sources to map decision table rows to test cases
- Use SpecRec's `Context` for dependency injection and logging
- Each decision table row should be a distinct test scenario

#### Complete Examples
```csharp
public static IEnumerable<object[]> BookingScenarios => new[]
{
    new object[] { /* AvailableSeats, SpecialRequests, AirlineCode, ... ExpectedResult */ },
    // ... one entry per decision table row
};

[Theory]
[MemberData(nameof(BookingScenarios))]
public void BookFlight_DecisionTableScenario(
    int availableSeats, string specialRequests, string airlineCode, int bookingCounter,
    bool failureReasonExists, decimal finalPrice, int passengerCount, string expectedResult)
{
    // Arrange: Setup Context, test doubles, and BookingCoordinator
    // Act: Call BookFlight with scenario parameters
    // Assert: Verify outcome matches expectedResult
}
```

#### API and Schema Documentation
- xUnit `[Theory]` and `[MemberData]` documentation: https://xunit.net/docs/getting-started/netfx/visual-studio
- SpecRec scenario logging: see README.md and test files

#### Configuration Examples
```xml
<ItemGroup>
  <PackageReference Include="xunit" Version="2.4.2" />
  <PackageReference Include="SpecRec" Version="*" />
</ItemGroup>
```

#### Technical Requirements
- Each decision table row must be covered by a test scenario
- Use test doubles for all external dependencies (repository, notifier, logger, etc.)
- Assert both result and side effects (notifications, logging, exceptions)

---

### Recommended Approach

**Use xUnit `[Theory]` tests with `[MemberData]` to map each decision table row to a test scenario. Leverage SpecRec's `Context` for dependency injection and logging. Assert both the returned booking status and all relevant side effects (notifications, exceptions, logging) as specified in the decision table.**

---

### Implementation Guidance

- **Objectives**: Achieve full scenario coverage for `BookingCoordinator.BookFlight` as defined by the decision table.
- **Key Tasks**:
  1. Create a static data source mapping each decision table row to test parameters.
  2. Implement `[Theory]` tests using `[MemberData]` for scenario-driven coverage.
  3. Use SpecRec's `Context` to inject test doubles and log interactions.
  4. Assert expected outcomes and side effects for each scenario.
- **Dependencies**: xUnit, SpecRec NuGet package, existing test doubles.
- **Success Criteria**: Each decision table row is covered by a passing test that verifies both the result and all specified side effects.

---

**Research documentation saved in `./.copilot-tracking/research/20240908-xunit-decision-table-bookingcoordinator-research.md`.**

Would you like a sample test suite scaffold or guidance on mapping specific decision table rows to test cases?