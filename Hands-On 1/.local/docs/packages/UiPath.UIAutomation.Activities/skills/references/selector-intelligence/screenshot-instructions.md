<screenshot_instructions>
## Screenshot Analysis

The screenshot shows the application with the target element highlighted in green. Use it to identify visual context that helps you choose reliable attributes.

**What to look for:**
    **Labels and headers near the target** - These often map to `name`, `aria-label`, or `labeledby` attributes
    **Named parent containers** - Dialogs, panels, sections with stable titles/names that provide anchoring context
    **Table structure** - If in a table, identify visible column/row headers instead of using numeric indices

**What to avoid:**
    Dynamic content you can see (numbers, dates, user names, status messages) - Use wildcards or avoid these attributes
    Content in editable fields (placeholders, data) - The current value will change

**Examples:**

  1. **Table cell scenario**
     - Screenshot shows: Cell with ""$75,000"" in a row with ""John Doe"" and under column ""Salary""
     - ❌ Bad: Use row/column numbers you can count visually → `tableRow='3' tableCol='2'` (breaks when data changes)
     - ✓ Good: Use visible headers → `rowName='John Doe' colName='Salary'`

  2. **Input field with placeholder**
     - Screenshot shows: Empty text field with grayed-out text ""Enter your email address""
     - ❌ Bad: Use the placeholder text → `visibleinnertext='Enter your email address'` (placeholders change frequently)
     - ✓ Good: Use the visible label above/beside it → `aria-label='Email'` or `labeledby` attribute

  3. **Editable field with current value**
     - Screenshot shows: Text field containing ""John Smith"" with label ""Full Name:""
     - ❌ Bad: Use the current content → `name='John Smith'` (user input changes)
     - ✓ Good: Use stable identifier → `automationid='*fullName*'` or `labeledby='fullNameLabel'`

  4. **Button inside a dialog**
     - Screenshot shows: ""Save"" button inside a dialog titled ""Edit Employee Details""
     - ❌ Bad: Just the button → `<webctrl tag='BUTTON' visibleinnertext='Save' />` (many ""Save"" buttons exist)
     - ✓ Good: Anchor to dialog → `<webctrl role='dialog' aria-label='Edit Employee Details' /><webctrl tag='BUTTON' visibleinnertext='Save' />`

</screenshot_instructions>