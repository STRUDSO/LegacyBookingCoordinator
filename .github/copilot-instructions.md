# Copilot Instructions for LegacyBookingCoordinator

## Project Overview
This is a legacy monolithic flight booking system. The main orchestration is handled by `BookingCoordinator`, which interacts with:
- `FlightAvailabilityService`: Checks seat availability
- `PricingEngine`: Applies dynamic pricing
- `PartnerNotifier`: Notifies airlines of bookings
- `AuditLogger`: Logs booking activity
- `BookingRepository`: Persists bookings (production-only DB)

## Key Architectural Patterns
- **Direct Instantiation**: Classes often use `new` for dependencies. Refactoring to use `ObjectFactory` is encouraged for testability.
- **SpecRec Integration**: Use the `SpecRec` NuGet package for testing. It provides `ObjectFactory`, `Context`, and test doubles (Parrot, CallLogger).
- **No Dependency Injection Container**: All dependency management is manual or via `ObjectFactory`.

## Developer Workflows
- **Build**: Standard .NET build via `dotnet build` on the solution file.
- **Test**: Run tests with `dotnet test`. Tests use `[Theory]` and `[SpecRecLogs]` for scenario coverage. Each `.verified.txt` file in the test logs directory becomes a test case.
- **Test Doubles**: Use `Context.SetOne<T>()`, `Context.Wrap<T>()`, or `Context.Parrot<T>()` to inject and log/test dependencies.
- **Test Isolation**: `Context` handles cleanup; no manual reset needed.

## Project-Specific Conventions
- **Object Creation**: Prefer `factory.Create<T>()` over `new` for all service instantiations. Example:
  ```csharp
  var logger = factory.Create<AuditLogger>(logDirectory, verboseMode);
  ```
- **Test Logging**: Use `Context.Wrap` to log all method calls for verification.
- **Parrot Test Doubles**: Use `Context.Parrot<T>()` to replay interactions from verified logs.
- **Scenario Testing**: Use `[SpecRecLogs]` to generate multiple test cases from log files.
- **Constructor Arguments**: Implement `IConstructorCalledWith` in test doubles to verify constructor parameters.

## Integration Points
- **External DB**: `BookingRepository` only works in production; use stubs or Parrots in tests.
- **Logging/Notification**: All side effects (logging, notifications) should be wrapped/tested using SpecRec utilities.

## Key Files
- `LegacyBookingCoordinator/BookingCoordinator.cs`: Main orchestration logic
- `LegacyBookingCoordinator.Tests/BookingCoordinatorTests.cs`: Example test patterns
- `README.md`: Detailed explanation of testing patterns and SpecRec usage

## Example: Test Setup
```csharp
[Theory]
[SpecRecLogs]
public async Task BookFlight_MultipleScenarios(Context context)
{
    context.Substitute<IBookingRepository>("💾")
           .Substitute<IFlightAvailabilityService>("✈️")
           .Substitute<IPartnerNotifier>("📣");
    var coordinator = new BookingCoordinator();
    return coordinator.BookFlight(/* params */);
}
```

---
For more details, see `README.md` and test files. Follow these patterns for new features and tests. If unclear, ask for clarification or examples from existing code.