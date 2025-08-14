## 1. **What is an Apex Trigger?**

- **Definition:** An Apex Trigger is code that executes automatically **before or after** specific **DML events** on a Salesforce object.
    
- **Purpose:** Automates business processes such as data validation, default value setting, record creation/updation, and cross-object logic.
    
- **Runs on:** Standard objects (like `Account`, `Contact`) or custom objects (like `Invoice__c`).
    

---

## 2. **Trigger Events**

A trigger can listen to one or more of the following events:

|Event|Runs When...|
|---|---|
|**before insert**|Before a record is inserted into the DB|
|**before update**|Before a record is updated|
|**before delete**|Before a record is deleted|
|**after insert**|After a record is inserted|
|**after update**|After a record is updated|
|**after delete**|After a record is deleted|
|**after undelete**|After a record is restored from the Recycle Bin|

---

## 3. **When to use Before vs After Triggers**

- **Before triggers**
    
    - Modify field values before saving
        
    - Validate data without extra DML statements
        
    - Example: Setting default values, enforcing formatting
        
- **After triggers**
    
    - Access system-generated fields (like Id)
        
    - Perform actions on related records
        
    - Example: Creating related child records, sending notifications
        

---

## 4. **Basic Trigger Syntax**

```apex
trigger TriggerName on ObjectName (trigger_events) {
    if (Trigger.isBefore) {
        if (Trigger.isInsert) {
            // Logic for before insert
        }
        if (Trigger.isUpdate) {
            // Logic for before update
        }
    }
    if (Trigger.isAfter) {
        if (Trigger.isInsert) {
            // Logic for after insert
        }
    }
}
```

---

## 5. **Trigger Context Variables**

|Variable|Description|
|---|---|
|`Trigger.new`|List of new records in insert/update/undelete|
|`Trigger.old`|List of old records in update/delete|
|`Trigger.newMap`|Map<Id, SObject> of new records|
|`Trigger.oldMap`|Map<Id, SObject> of old records|
|`Trigger.isInsert`|True if DML is insert|
|`Trigger.isUpdate`|True if DML is update|
|`Trigger.isDelete`|True if DML is delete|
|`Trigger.isUndelete`|True if DML is undelete|
|`Trigger.isBefore`|True if before trigger|
|`Trigger.isAfter`|True if after trigger|

---

## 6. **Example — Simple Trigger**

```apex
trigger AccountNameFormat on Account (before insert, before update) {
    for (Account acc : Trigger.new) {
        if (acc.Name != null) {
            acc.Name = acc.Name.trim();
        }
    }
}
```

**Purpose:** Ensures no leading/trailing spaces in Account Name.

---

## 7. **Bulkification**

- **Why:** Triggers run for a batch of records, not just one.
    
- **How:** Use collections (`List`, `Map`, `Set`), avoid SOQL/DML inside loops.
    

**Bad (Non-Bulkified):**

```apex
for (Account acc : Trigger.new) {
    insert new Contact(LastName='Test', AccountId=acc.Id);
}
```

**Good (Bulkified):**

```apex
List<Contact> contactsToInsert = new List<Contact>();
for (Account acc : Trigger.new) {
    contactsToInsert.add(new Contact(LastName='Test', AccountId=acc.Id));
}
insert contactsToInsert;
```

---

## 8. **Calling a Handler Class (Best Practice)**

Keep triggers clean by calling an Apex class.

**Trigger:**

```apex
trigger AccountTrigger on Account (before insert, after insert) {
    if (Trigger.isBefore && Trigger.isInsert) {
        AccountHandler.beforeInsert(Trigger.new);
    }
    if (Trigger.isAfter && Trigger.isInsert) {
        AccountHandler.afterInsert(Trigger.new);
    }
}
```

**Handler Class:**

```apex
public class AccountHandler {
    public static void beforeInsert(List<Account> accs) {
        for (Account acc : accs) {
            acc.Name = acc.Name + ' - Verified';
        }
    }
    public static void afterInsert(List<Account> accs) {
        // Create related contacts, send emails, etc.
    }
}
```

---

## 9. **Trigger Order of Execution (Key Steps)**

1. Load original record from database (or initialize for insert)
    
2. Run system validation rules
    
3. Run `before` triggers
    
4. Run system validation again
    
5. Save record (but not committed)
    
6. Run `after` triggers
    
7. Run assignment rules, auto-response rules, workflow, processes, flows
    
8. Commit DML to database
    
9. Run post-commit logic (async)
    

---

## 10. **Avoiding Recursion**

When a trigger updates the same object, it can call itself again.

**Recursion Control Example:**

```apex
public class TriggerHelper {
    public static Boolean isFirstRun = true;
}
trigger AccountTrigger on Account (before update) {
    if (TriggerHelper.isFirstRun) {
        TriggerHelper.isFirstRun = false;
        // Your logic
    }
}
```

---

## 11. **Advanced Trigger Patterns**

- **One Trigger per Object** (recommended)
    
- **Trigger Frameworks** like:
    
    - Kevin O’Hara’s Apex Trigger Framework
        
    - fflib Apex Commons
        
- **Use constants** for event names
    
- **Separate logic** from the trigger body
    

---

## 12. **Example — Cross-Object Logic**

When an Account is created, automatically create a Contact.

```apex
trigger CreateContactOnAccount on Account (after insert) {
    List<Contact> contactsToInsert = new List<Contact>();
    for (Account acc : Trigger.new) {
        contactsToInsert.add(new Contact(
            LastName = acc.Name,
            AccountId = acc.Id
        ));
    }
    insert contactsToInsert;
}
```

---

## 13. **Testing Triggers**

- Must have **75% coverage** to deploy.
    
- Use `@isTest` classes.
    
- Create test data inside the test.
    

```apex
@isTest
private class AccountTriggerTest {
    @isTest
    static void testAccountTrigger() {
        Account acc = new Account(Name='Test Account');
        insert acc;
        acc.Name = 'Updated';
        update acc;
        delete acc;
    }
}
```

---

## 14. **Best Practices**

1. **One Trigger per Object** → easier maintenance.
    
2. **Bulkify logic** → always assume multiple records.
    
3. **Avoid SOQL/DML in loops** → move outside.
    
4. **Use handler classes** → keep triggers small.
    
5. **Avoid hardcoding Ids** → use dynamic queries or metadata.
    
6. **Control recursion** → prevent infinite loops.
    
7. **Test for all scenarios** → insert, update, delete, undelete.