---
description: New prompt created from chat session
mode: agent
tools: ['extensions', 'usages', 'vscodeAPI', 'think', 'problems', 'changes', 'testFailure', 'openSimpleBrowser', 'fetch', 'findTestFiles', 'githubRepo', 'todos', 'runTests', 'runCommands', 'runTasks', 'edit', 'notebooks', 'search', 'new']
model: GPT-4.1
---
<user>
Can you craft a plan for creating a xunit test suite for #file:Step3.BookingCoordinator.Decistion.Table.md  against #file:BookingCoordinator.cs 
</user>
<assistant>
Absolutely! Here’s a strategic plan for creating an xUnit test suite that validates the logic in `BookingCoordinator.cs` against the decision table in `Step3.BookingCoordinator.Decistion.Table.md`.

---

## 1. **Clarify Test Objectives**

- **Goal:** Ensure that all decision table scenarios are covered by automated tests.
- **Scope:** Focus on the main booking flow (`BookFlight`) and its outcomes (exceptions, status, notifications, logging).
- **Test Type:** xUnit `[Theory]`-based scenario tests, using SpecRec patterns for orchestration and logging.

---

## 2. **Map Decision Table to Test Cases**

- **Extract Conditions:**  
  - Available Seats vs. `passengerCount`
  - Special Requests (none, present, specific values)
  - Airline Code (AA, BA, others)
  - Booking Counter (even/odd)
  - Failure Reason Exists (simulate via state)
  - Final Price > 1000
  - Passenger Count > 5

- **Actions/Results:**  
  - Exception thrown
  - Booking status (`CONFIRMED`, `CONFIRMED_PREMIUM`, `CONFIRMED_GROUP`)
  - Partner notification (with/without encryption, special requests)
  - Logging

---

## 3. **Design Test Data Matrix**

- **Create a test data source** (CSV, `[MemberData]`, or SpecRec logs) that enumerates all relevant combinations from the decision table.
- **Each row = one scenario**: Inputs + expected outputs/actions.

---

## 4. **Test Double Strategy**

- **Repository:** Use SpecRec Parrot or Substitute for `BookingRepository` (no real DB).
- **PartnerNotifier:** Wrap or Parrot to verify notification calls and encryption.
- **AuditLogger:** Wrap to verify logging.
- **FlightAvailabilityService:** Substitute to control available seats.
- **PricingEngine:** Substitute to control pricing and discounts.

---

## 5. **Test Structure**

- **File:** BookingCoordinatorTests.cs
- **Pattern:**  
  - `[Theory]` with `[MemberData]` or `[SpecRecLogs]`
  - Arrange: Set up context, inject test doubles, configure state (e.g., bookingCounter, temporaryData)
  - Act: Call `BookFlight`
  - Assert:  
    - Exception thrown (if not enough seats)
    - Booking status
    - Notification sent (with/without encryption, special requests)
    - Logging calls

---

## 6. **Edge Case Handling**

- **Simulate global state:** Manipulate `bookingCounter`, `temporaryData`, etc.
- **Special requests:** Test with none, one, multiple, and specific values ("meal", "wheelchair", etc.).
- **Airline codes:** Cover AA, BA, and generic.
- **Booking counter:** Test even/odd for encryption logic.
- **Failure reason:** Inject into `temporaryData` to test notification suppression.

---

## 7. **Verification Approach**

- **Use SpecRec logging:**  
  - Wrap dependencies to capture method calls and arguments.
  - Compare against expected actions from the decision table.
- **Assertions:**  
  - Exception type and message
  - Booking status value
  - Notification method calls and parameters
  - Logging method calls and content

---

## 8. **Test Coverage Review**

- **Checklist:**  
  - Each decision table row is covered by at least one test.
  - All branches in `BookFlight` are exercised.
  - All side effects (notification, logging) are verified.

---

## 9. **Documentation & Maintenance**

- **Document mapping:**  
  - Comment each test with the corresponding decision table row.
- **Update process:**  
  - When decision table changes, update test data and add new scenarios.

---

## 10. **Next Steps**

- Draft the test data matrix.
- Scaffold the test class and methods.
- Implement test doubles and scenario setup.
- Write and verify tests for each decision table row.

---

**Summary:**  
This plan ensures systematic coverage of all business logic as described in the decision table, using xUnit and SpecRec patterns for robust, maintainable tests. Would you like a sample test data matrix or a code scaffold for the first few scenarios?
</assistant>
<user>
Follow instructions in [copilot-prompt.prompt.md](vscode-userdata:/Users/stm/Library/Application%20Support/Code%20-%20Insiders/User/prompts/copilot-prompt.prompt.md).
</user>
<assistant>
A new prompt file has been created at BookingCoordinator.DecisionTable.xunit.prompt.md. It contains a detailed, reusable agent-mode prompt for generating an xUnit test suite for `BookingCoordinator.cs` against the decision table, including all context, mapping, and implementation guidelines. This file can be used directly to guide or automate test suite creation for this logic.
</assistant>