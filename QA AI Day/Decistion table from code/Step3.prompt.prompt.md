---
description: New prompt created from chat session
mode: agent
tools: ['extensions', 'usages', 'vscodeAPI', 'think', 'problems', 'changes', 'testFailure', 'openSimpleBrowser', 'fetch', 'findTestFiles', 'githubRepo', 'todos', 'runTests', 'runCommands', 'runTasks', 'edit', 'notebooks', 'search', 'new']
model: GPT-4.1
---
<user>
Can you create a Decision Table based on this class:
#file:BookingCoordinator.cs 


</user>
<assistant>
Here is a decision table for the main booking flow in `BookingCoordinator.BookFlight`. This covers the key conditions and resulting actions/outputs based on the code logic:

```markdown
| Available Seats | Special Requests | Airline Code | Booking Counter | Failure Reason Exists | Final Price > 1000 | Passenger Count > 5 | Result/Action                                 |
|-----------------|-----------------|--------------|-----------------|----------------------|--------------------|---------------------|-----------------------------------------------|
| < passengerCount| Any             | Any          | Any             | Any                  | Any                | Any                 | Throw InvalidOperationException (not enough seats) |
| >= passengerCount| None            | Any          | Any             | No                   | No                 | No                  | Status: CONFIRMED, Notify Partner (if AA & <5) |
| >= passengerCount| None            | Any          | Any             | No                   | Yes                | No                  | Status: CONFIRMED_PREMIUM, Notify Partner      |
| >= passengerCount| None            | Any          | Any             | No                   | No                 | Yes                 | Status: CONFIRMED_GROUP, Notify Partner        |
| >= passengerCount| None            | Any          | Any             | Yes                  | Any                | Any                 | Status: CONFIRMED, Do NOT Notify Partner       |
| >= passengerCount| Present         | BA           | Any             | No                   | Any                | Any                 | If "meal" in requests, Notify Special Requests |
| >= passengerCount| Present         | AA           | Any             | No                   | Any                | Any                 | If "wheelchair" in requests, Notify Special Requests |
| >= passengerCount| Present         | Any          | Any             | No                   | Any                | Any                 | If >2 requests, Notify Special Requests        |
| >= passengerCount| Any             | Any          | Even            | No                   | Any                | Any                 | Use encryption for partner notification        |
| >= passengerCount| Any             | Any          | Odd             | No                   | Any                | Any                 | Do not use encryption for partner notification |
| >= passengerCount| Any             | Any          | Any             | Any                  | Any                | Any                 | Log booking activity and pricing calculation   |
```

**Notes:**
- "Available Seats" is determined by `availabilityService.CheckAndGetAvailableSeatsForBooking`.
- "Special Requests" triggers surcharges and may require special partner notification.
- "Booking Counter" affects surcharges, encryption, and log directory.
- "Failure Reason Exists" (in `temporaryData`) disables partner notification.
- "Final Price > 1000" and "Passenger Count > 5" affect booking status.
- All successful bookings are logged and saved.

Let me know if you want a more detailed table for a specific method or condition!
</assistant>