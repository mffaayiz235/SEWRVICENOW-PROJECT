**Laptop Request Catalog Item – Code Document**

---

## 1. Client Script – Dynamic Form Behavior

**Purpose:** Show or hide fields based on Laptop Type selection.

```javascript
// Client Script: onChange for Laptop Type field
function onChange(control, oldValue, newValue, isLoading) {
    if (isLoading || newValue == '') {
        return;
    }

    // Example: Show Accessories Required field for High-End laptops
    if (newValue == 'High-End Laptop') {
        g_form.setDisplay('accessories_required', true);
    } else {
        g_form.setDisplay('accessories_required', false);
    }
}
```

---

## 2. Client Script – Reset Form Functionality

**Purpose:** Clear all form fields when Reset button is clicked.

```javascript
// Client Script function to reset all fields
function resetForm() {
    // List of fields to reset
    var fields = [
        'employee_name', 
        'employee_id', 
        'department', 
        'laptop_type', 
        'purpose', 
        'accessories_required', 
        'expected_delivery_date'
    ];
    
    // Loop through fields and clear values
    for (var i = 0; i < fields.length; i++) {
        g_form.setValue(fields[i], '');
    }
    
    alert('Form has been reset!');
}
```

---

## 3. UI Action – Reset Button

**Purpose:** Attach the Reset functionality to a button on the form.

**Properties:**

* Action Name: `Reset Form`
* Type: `Form Button`

**Script:**

```javascript
// Call resetForm() when button is clicked
resetForm();
```

---

## 4. Business Rule – Auto-fill Employee Details

**Purpose:** Auto-populate Employee Name and Department when Employee ID is entered.

**Type:** `Before` insert/update on the Catalog Item table

```javascript
(function executeRule(current, previous /*null when async*/) {
    if (current.employee_id) {
        var gr = new GlideRecord('sys_user');
        if (gr.get('employee_number', current.employee_id)) {
            current.employee_name = gr.name;
            current.department = gr.department.name;
        }
    }
})(current, previous);
```

---

## 5. Flow Designer Script – Manager Approval (Optional Script Step)

**Purpose:** Assign approval to the requester’s manager automatically.

```javascript
// Script Step in Flow Designer
var manager = current.requested_for.manager;
if (manager) {
    var approvalGR = new GlideRecord('sysapproval_approver');
    approvalGR.initialize();
    approvalGR.approver = manager;
    approvalGR.sysapproval = current.sys_id;
    approvalGR.insert();
}
```

---

## 6. Notes

* Ensure all **field names in scripts** match the variable names used in your ServiceNow Catalog Item.
* The **Client Scripts** should be associated with the proper form fields (`onChange`, `onLoad`, or `onSubmit`).
* **UI Actions** must be added to the Catalog Item form and linked to the `resetForm()` function.
* **Business Rules** run server-side and must be tested for proper `GlideRecord` queries.
* Flow Designer steps can use the script if you need dynamic assignment; otherwise, approvals can be configured via the UI.

---

This document contains all the code necessary for the **Laptop Request Catalog Item** project in ServiceNow, ready for submission.
