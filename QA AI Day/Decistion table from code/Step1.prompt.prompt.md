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
Here is a Decision Table summarizing the main decision logic in the `BookingCoordinator` class, focusing on key conditions and outcomes in the booking flow:

| Condition/Rule                                      | Value/Branching Logic                                                                 | Outcome/Action                                                                                   |
|-----------------------------------------------------|---------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| Available seats < passengerCount                    | Yes                                                                                   | Throw `InvalidOperationException` ("Not enough seats available")                                 |
| bookingCounter % 3 == 0                             | Yes                                                                                   | Enable random surcharges in PricingEngine                                                        |
| DayOfWeek is Friday or Sunday                       | Yes                                                                                   | Weekday multiplier = 1.25, isPeakDay = true                                                      |
| DayOfWeek is Tuesday or Wednesday                   | Yes                                                                                   | Weekday multiplier = 0.9, isPeakDay = false                                                      |
| Month is June-August                                | Yes                                                                                   | Seasonal bonus = 50, currentSeason = "Summer"                                                    |
| Month is December-February                          | Yes                                                                                   | Seasonal bonus = 75, currentSeason = "Winter"                                                    |
| bookingCounter % 5 == 0                             | Yes                                                                                   | Add 20 to seasonal bonus, luckyBooking = true                                                    |
| specialRequests contains "wheelchair"               | Yes, airlineCode == "AA"                                                              | Surcharge = 0                                                                                    |
| specialRequests contains "wheelchair"               | Yes, airlineCode != "AA"                                                              | Surcharge += 25                                                                                  |
| specialRequests contains "meal"                     | Yes, airlineCode == "BA"                                                              | Surcharge += 15                                                                                  |
| specialRequests contains "meal"                     | Yes, airlineCode != "BA"                                                              | Surcharge += 20                                                                                  |
| specialRequests contains "seat"                     | Yes                                                                                   | Surcharge += 35                                                                                  |
| pricingEngine.ValidatePricingParametersAndCalculateDiscount | Returns true                                                                          | Subtract discountAmount from finalPrice                                                          |
| bookingCounter % 2 == 0                             | Yes                                                                                   | useEncryption = true for PartnerNotifier                                                         |
| bookingCounter < 5                                  | Yes, airlineCode == "AA"                                                              | ShouldNotifyPartner = true                                                                       |
| bookingCounter < 5                                  | Yes, airlineCode != "AA"                                                              | ShouldNotifyPartner = false                                                                      |
| temporaryData contains "lastFailureReason"          | Yes                                                                                   | ShouldNotifyPartner = false                                                                      |
| airlineCode == "BA" and specialRequests contains "meal" | Yes                                                                                   | RequiresSpecialNotification = true                                                               |
| airlineCode == "AA" and specialRequests contains "wheelchair" | Yes                                                                                   | RequiresSpecialNotification = true                                                               |
| specialRequests.Split(',').Length > 2               | Yes                                                                                   | RequiresSpecialNotification = true                                                               |
| finalPrice > 1000                                   | Yes                                                                                   | bookingStatus = "CONFIRMED_PREMIUM"                                                              |
| passengerCount > 5                                  | Yes                                                                                   | bookingStatus = "CONFIRMED_GROUP"                                                                |
| isPeakDay == true                                   | Yes                                                                                   | bookingStatus = "CONFIRMED_PEAK"                                                                 |
| Default                                             |                                                                                       | bookingStatus = "CONFIRMED"                                                                      |

This table covers the main decision points and their resulting actions or state changes in the booking process. If you need a more detailed or formal tabular format (e.g., for test case generation), let me know!
</assistant>