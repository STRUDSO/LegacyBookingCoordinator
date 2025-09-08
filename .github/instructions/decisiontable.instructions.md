---
applyTo: '**'
---
# Decision Table Guidelines for Code Logic

**Purpose:**  
A decision table helps you systematically map all possible combinations of input conditions and the resulting actions or outputs. This is especially useful for endpoints, business logic, and complex flows.

## Steps to Create a Decision Table

1. **Identify Conditions:**  
   List all input variables or conditions that affect the decision (e.g., query parameters, request body fields, service states).
2. **List Possible Values:**  
   For each condition, enumerate all possible values (e.g., Yes/No, null/value, valid/invalid).
3. **Map All Combinations:**  
   Create a table where each row represents a unique combination of condition values.
4. **Define Decisions/Actions:**  
   For each combination, specify the expected action, output, or result (e.g., return Ok, return NotFound, throw exception).
5. **Name Columns Clearly:**  
   Use meaningful names for conditions and decision columns.
6. **Export/Share:**  
   Tables can be exported as Markdown, CSV, or pasted into Excel/Google Sheets for further editing.

## Decision Table Template (Markdown)

```markdown
| Condition 1         | Condition 2         | ... | Decision/Action         |
|---------------------|---------------------|-----|-------------------------|
| Value 1             | Value 1             | ... | Action/Output 1         |
| Value 1             | Value 2             | ... | Action/Output 2         |
| Value 2             | Value 1             | ... | Action/Output 3         |
| Value 2             | Value 2             | ... | Action/Output 4         |
```

## Example for an API Endpoint

Suppose your endpoint depends on:
- `postalCode` validity (Valid/Invalid)
- `deliveryDate` (Provided/Not Provided)
- `pointOfSaleBranchId` (Exists/Does Not Exist)

Your table might look like:

```markdown
| PostalCode Valid | DeliveryDate Provided | BranchId Exists | Result                |
|------------------|----------------------|-----------------|-----------------------|
| Yes              | Yes                  | Yes             | Return Ok             |
| Yes              | Yes                  | No              | Return NotFound       |
| Yes              | No                   | Yes             | Return Ok             |
| No               | Any                  | Any             | Return NotFound/Error |
```

## Tips

- Always cover all possible combinations.
- Use the table to clarify requirements and edge cases with stakeholders.
- Keep tables updated as logic changes.

**Use this template and workflow whenever you need to clarify or generate decision tables for your codebase.**