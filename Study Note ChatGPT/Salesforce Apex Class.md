## 1. **Introduction to Apex**

- **What is Apex?**
    
    - Apex is a **strongly typed**, **object-oriented**, **server-side programming language** that allows developers to execute flow and transaction control statements on the Salesforce platform.
        
    - Syntax is similar to Java.
        
    - Used for:
        
        - Business logic (Triggers, Classes)
            
        - Web services (REST/SOAP)
            
        - Batch jobs, schedulers, queueable jobs
            
        - Custom APIs
            
        - LWC/Aura backend logic
            
- **Where Apex Runs**
    
    - Executes in **multi-tenant environment** (shared resources)
        
    - Enforced by **Governor Limits** to ensure fair resource usage.
        

---

## 2. **Apex Class Basics**

An **Apex class** is a blueprint for creating objects (instances) in Apex.

### 2.1 Syntax

```apex
public class MyClass {
    // Fields (variables)
    public String myVariable;

    // Constructor
    public MyClass(String val) {
        myVariable = val;
    }

    // Method
    public void displayValue() {
        System.debug('Value: ' + myVariable);
    }
}
```

**Key components:**

- **Modifiers:** `public`, `private`, `protected`, `global`, `with sharing`, `without sharing`, `virtual`, `abstract`, `static`, `final`
    
- **Variables:** Store data
    
- **Methods:** Define behavior
    
- **Constructors:** Special methods called when creating an object
    

---

## 3. **Access Modifiers**

|Modifier|Scope|
|---|---|
|`private`|Only within the class|
|`public`|Any Apex code in the org|
|`protected`|Class and subclasses|
|`global`|Public + accessible across namespaces (managed packages)|

---

## 4. **Sharing Rules**

- **`with sharing`** — Enforces sharing rules (respects org-wide defaults, role hierarchy, sharing rules).
    
- **`without sharing`** — Ignores sharing rules (system context).
    
- **Default:** No explicit sharing — behaves like `without sharing` unless inside a `with sharing` class.
    

**Example:**

```apex
public with sharing class AccountService {
    public List<Account> getMyAccounts() {
        return [SELECT Id, Name FROM Account];
    }
}
```

---

## 5. **Class Types**

### 5.1 **Top-level class**

Defined in its own `.cls` file.

### 5.2 **Inner Class**

Class inside another class.

```apex
public class Outer {
    public class Inner {
        public void sayHello() {
            System.debug('Hello from Inner');
        }
    }
}
```

### 5.3 **Static Class**

Inner classes can be `static` (no reference to outer class instance).

---

## 6. **Methods in Apex**

### 6.1 Method Syntax

```apex
public returnType methodName(paramType paramName) {
    // Code
    return value; // if not void
}
```

### 6.2 Method Modifiers

- **`static`**: Belongs to class, not an instance.
    
- **`virtual`**: Can be overridden.
    
- **`abstract`**: Declared without implementation, must be overridden.
    
- **`override`**: Overrides a parent class method.
    
- **`final`**: Cannot be overridden.
    

---

## 7. **Constructors**

- Same name as class, no return type.
    
- Used to initialize objects.
    

Example:

```apex
public class Student {
    public String name;
    public Student(String n) {
        name = n;
    }
}
```

---

## 8. **Properties**

Shortcut for getter and setter methods.

```apex
public class Product {
    public String name { get; set; }
}
```

Custom getter/setter:

```apex
public String price {
    get { return price; }
    set { price = value > 0 ? value : 0; }
}
```

---

## 9. **Static vs Instance**

- **Instance Members:** Belong to object instances.
    
- **Static Members:** Belong to class itself.
    

---

## 10. **Inheritance**

- Apex supports **single inheritance**.
    

```apex
public virtual class Animal {
    public virtual void speak() { System.debug('Animal speaks'); }
}
public class Dog extends Animal {
    public override void speak() { System.debug('Woof!'); }
}
```

---

## 11. **Interfaces**

- Define method signatures without implementation.
    
- Implemented using `implements`.
    

```apex
public interface Shape {
    Decimal area();
}
public class Circle implements Shape {
    public Decimal radius;
    public Circle(Decimal r) { radius = r; }
    public Decimal area() { return Math.PI * radius * radius; }
}
```

---

## 12. **Abstract Classes**

- Cannot be instantiated.
    
- Can have abstract and concrete methods.
    

```apex
public abstract class Payment {
    public abstract void process();
}
```

---

## 13. **Virtual & Override**

- `virtual` → allows overriding.
    
- `override` → must match signature of base method.
    

---

## 14. **Exception Handling**

```apex
try {
    Integer x = 5 / 0;
} catch (Exception e) {
    System.debug(e.getMessage());
} finally {
    System.debug('Always runs');
}
```

Custom exception:

```apex
public class MyException extends Exception {}
```

---

## 15. **Collections in Classes**

- Lists, Sets, Maps often used in class variables/methods.
    

Example:

```apex
public class AccountUtils {
    public static List<Account> getTopAccounts(Integer limitSize) {
        return [SELECT Id, Name FROM Account LIMIT :limitSize];
    }
}
```

---

## 16. **SOQL in Classes**

```apex
public class AccountService {
    public List<Account> getActiveAccounts() {
        return [SELECT Id, Name FROM Account WHERE Active__c = 'Yes'];
    }
}
```

---

## 17. **Calling Classes from Triggers**

```apex
trigger AccountTrigger on Account (before insert) {
    AccountHandler.beforeInsert(Trigger.new);
}

public class AccountHandler {
    public static void beforeInsert(List<Account> accs) {
        for (Account acc : accs) {
            acc.Name = acc.Name + ' - Verified';
        }
    }
}
```

---

## 18. **Apex Class for LWC/Aura**

- Use `@AuraEnabled` annotation.
    

```apex
public with sharing class ContactController {
    @AuraEnabled(cacheable=true)
    public static List<Contact> getContacts() {
        return [SELECT Id, Name FROM Contact LIMIT 10];
    }
}
```

---

## 19. **Test Classes**

- Every class should have test coverage ≥ 75% for deployment.
    
- Use `@isTest` annotation.
    

Example:

```apex
@isTest
private class MyClassTest {
    @isTest
    static void testMethod1() {
        Account acc = new Account(Name='Test');
        insert acc;
        System.assertNotEquals(null, acc.Id);
    }
}
```

---

## 20. **Advanced Topics**

- **Batch Apex**
    
- **Queueable Apex**
    
- **Schedulable Apex**
    
- **Future methods**
    
- **Asynchronous Apex patterns**
    
- **Custom REST API classes**
    
- **Dynamic Apex** (Schema, Describe calls)
    

---

## 21. **Best Practices**

1. Use **with sharing** unless explicitly needing system mode.
    
2. Bulkify all database operations (handle `List` not single record).
    
3. Avoid SOQL/DML inside loops.
    
4. Write unit tests for all logic paths.
    
5. Use custom exceptions for meaningful error handling.
    
6. Keep classes small and single-purpose (SRP principle).
    
7. Use constants for repeated strings.
    

---

## 22. **Sample End-to-End Apex Class**

```apex
public with sharing class OpportunityService {
    public static void updateCloseDate(List<Id> oppIds, Date newDate) {
        List<Opportunity> opps = [SELECT Id, CloseDate FROM Opportunity WHERE Id IN :oppIds];
        for (Opportunity opp : opps) {
            opp.CloseDate = newDate;
        }
        update opps;
    }
}
```

**Test Class:**

```apex
@isTest
private class OpportunityServiceTest {
    @isTest
    static void testUpdateCloseDate() {
        Opportunity opp = new Opportunity(Name='Test Opp', StageName='Prospecting', CloseDate=Date.today());
        insert opp;

        OpportunityService.updateCloseDate(new List<Id>{opp.Id}, Date.today().addDays(10));
        Opportunity updatedOpp = [SELECT CloseDate FROM Opportunity WHERE Id = :opp.Id];
        System.assertEquals(Date.today().addDays(10), updatedOpp.CloseDate);
    }
}
```

---

If you want, I can **extend this note with full advanced sections** on **Batch, Queueable, Future methods, and Custom REST/SOAP Web Services**, so it becomes a one-stop reference for all Apex class patterns.

Do you want me to add those now so the note becomes fully _beginner-to-advanced complete_?