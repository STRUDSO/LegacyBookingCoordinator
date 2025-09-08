---
mode: agent
description: "Generate an xUnit test suite for BookingCoordinator.cs, validating all logic against the decision table in Step3.BookingCoordinator.Decistion.Table.md."
---

# Task: xUnit Test Suite for BookingCoordinator Decision Table

## Context
- **Source File:** `LegacyBookingCoordinator/BookingCoordinator.cs`
- **Decision Table:** `QA AI Day/Decistion table from code/Step3.BookingCoordinator.Decistion.Table.md`
- **Project:** Legacy monolithic flight booking system
- **Testing Framework:** xUnit + SpecRec
- **Test File:** `LegacyBookingCoordinator.Tests/BookingCoordinatorTests.cs`

## Instructions

### 1. **Test Objective**
Create a comprehensive xUnit test suite that:
- Covers all scenarios and branches described in the decision table
- Validates outcomes, side effects, and state transitions in `BookingCoordinator.BookFlight`
- Uses SpecRec patterns for test doubles, logging, and scenario isolation

### 2. **Mapping Decision Table to Tests**
- Each row in the decision table must be represented by at least one test case
- Test cases should cover:
  - Exception handling (e.g., not enough seats)
  - Booking status assignment
  - Partner notification logic (including encryption and special requests)
  - Logging and audit

### 3. **Test Data Matrix**
- Create a test data source (e.g., `[MemberData]`, SpecRec logs, or inline arrays) that enumerates all relevant combinations:
  - Available seats vs. `passengerCount`
  - Special requests (none, present, specific values)
  - Airline code (AA, BA, others)
  - Booking counter (even/odd)
  - Failure reason (simulate via state)
  - Final price > 1000
  - Passenger count > 5

### 4. **Test Double Strategy**
- Use SpecRec's `Context` to inject and log dependencies:
  - Substitute or Parrot for `BookingRepository`, `FlightAvailabilityService`, `PartnerNotifier`, `AuditLogger`, `PricingEngine`
- Control state and outputs to match decision table scenarios

### 5. **Test Structure**
- Use `[Theory]` and `[MemberData]` or `[SpecRecLogs]` for scenario coverage
- Arrange: Set up context, inject test doubles, configure state
- Act: Call `BookFlight`
- Assert:
  - Exception thrown (if applicable)
  - Booking status
  - Notification sent (with/without encryption, special requests)
  - Logging calls

### 6. **Edge Cases & State Simulation**
- Manipulate `bookingCounter`, `temporaryData`, and other global state as needed
- Test with various special requests and airline codes
- Simulate failure reasons and booking counter parity

### 7. **Verification**
- Use SpecRec logging to verify method calls and arguments
- Assert on exception types, booking status, notification logic, and logging

### 8. **Documentation**
- Comment each test with the corresponding decision table row
- Maintain mapping between decision table and test cases

### 9. **References**
- [BookingCoordinator.cs](../../LegacyBookingCoordinator/BookingCoordinator.cs)
- [Step3.BookingCoordinator.Decistion.Table.md](../../QA%20AI%20Day/Decistion%20table%20from%20code/Step3.BookingCoordinator.Decistion.Table.md)
- [SpecRec Patterns](../instructions/copilot-instructions.md)
- [Decision Table Guidelines](../instructions/decisiontable.instructions.md)

---

## Acceptance Criteria
- All decision table scenarios are covered by tests
- Tests are isolated, repeatable, and use SpecRec for dependency management
- Test outcomes match expected results from the decision table
- Test code is maintainable and clearly mapped to business logic

---

## Example Test Case Scaffold
```csharp
[Theory]
[MemberData(nameof(GetDecisionTableScenarios))]
public void BookFlight_DecisionTableScenario(BookingTestScenario scenario)
{
    // Arrange: Set up context, inject test doubles, configure state
    // ...existing code...

    // Act & Assert: Validate outcome per decision table
    // ...existing code...
}
```

---

## Next Steps
- Draft test data matrix
- Scaffold test class and methods
- Implement test doubles and scenario setup
- Write and verify tests for each decision table row
