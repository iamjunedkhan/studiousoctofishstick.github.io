---
title: 'POST 2'
description: 'Learn what Apex Triggers are, when to use them, and how to write scalable trigger logic using Salesforce best practices.'
pubDate: 'Jun 14 2026'
heroImage: './cover.jpeg'
category: 'Apex'
author: 'Juned Khan'
---

# Understanding Apex Triggers in Salesforce

Apex Triggers are one of the most fundamental concepts in Salesforce development. They allow developers to execute custom business logic before or after records are inserted, updated, deleted, or undeleted.

In this article, we'll explore what triggers are, when to use them, and some best practices for building maintainable trigger frameworks.

---

## What You Will Learn

After reading this article, you'll understand:

- What Apex Triggers are
- Different trigger events
- Trigger context variables
- How to write a basic trigger
- Common mistakes to avoid
- Salesforce trigger best practices

---

## Why Are Triggers Needed?

Salesforce provides many declarative tools such as:

- Validation Rules
- Flow Builder
- Approval Processes
- Assignment Rules

However, there are situations where complex business logic requires custom code.

For example:

> Whenever an Account is created, automatically create a related custom record and perform additional validation checks.

This is where Apex Triggers become useful.

---

## Trigger Events

Salesforce supports several trigger events.

| Event | Description |
|---------|------------|
| before insert | Executes before record creation |
| after insert | Executes after record creation |
| before update | Executes before record update |
| after update | Executes after record update |
| before delete | Executes before deletion |
| after delete | Executes after deletion |
| after undelete | Executes when a record is restored |

---

## Basic Trigger Example

Below is a simple trigger that updates a custom field whenever an Account is created.

```apex
trigger AccountTrigger on Account (before insert) {

    for(Account acc : Trigger.new) {
        acc.Description = 'Created via Apex Trigger';
    }

}
````

This trigger runs before an Account is inserted and updates the Description field.

---

## Understanding Trigger Context Variables

Salesforce provides several context variables that help developers understand what operation is occurring.

### Trigger.new

Contains the new version of records.

```apex
for(Account acc : Trigger.new) {
    System.debug(acc.Name);
}
```

### Trigger.old

Contains the previous version of records.

```apex
for(Account acc : Trigger.old) {
    System.debug(acc.Name);
}
```

### Trigger.isInsert

Returns true if the operation is an insert.

```apex
if(Trigger.isInsert) {
    System.debug('Insert Operation');
}
```

### Trigger.isUpdate

Returns true when records are updated.

```apex
if(Trigger.isUpdate) {
    System.debug('Update Operation');
}
```

---

## Best Practices

### One Trigger Per Object

Avoid creating multiple triggers on the same object.

Good:

```text
AccountTrigger
```

Avoid:

```text
AccountValidationTrigger
AccountUpdateTrigger
AccountNotificationTrigger
```

Instead, call helper classes from a single trigger.

---

### Keep Logic Out of Triggers

Bad:

```apex
trigger AccountTrigger on Account (before insert) {

    // Hundreds of lines of logic

}
```

Good:

```apex
trigger AccountTrigger on Account (before insert) {
    AccountTriggerHandler.handleBeforeInsert(Trigger.new);
}
```

This improves readability and maintainability.

---

### Bulkify Your Code

Always assume multiple records can be processed at once.

Bad:

```apex
for(Account acc : Trigger.new) {

    Contact con = [
        SELECT Id
        FROM Contact
        WHERE AccountId = :acc.Id
    ];

}
```

Good:

```apex
Set<Id> accountIds = new Set<Id>();

for(Account acc : Trigger.new) {
    accountIds.add(acc.Id);
}

List<Contact> contacts = [
    SELECT Id, AccountId
    FROM Contact
    WHERE AccountId IN :accountIds
];
```

---

## Common Mistakes

### SOQL Inside Loops

Avoid:

```apex
for(Account acc : Trigger.new) {

    Account existing = [
        SELECT Id
        FROM Account
        WHERE Id = :acc.Id
    ];

}
```

This can quickly hit governor limits.

---

### DML Inside Loops

Avoid:

```apex
for(Account acc : accounts) {
    update acc;
}
```

Instead:

```apex
update accounts;
```

---

## Interview Questions

### What is the difference between before and after triggers?

Before triggers are used to update field values before saving records.

After triggers are used when record IDs are required or when related records need to be created.

### Can we perform DML on Trigger.new?

In before triggers, field values can be modified directly.

In after triggers, Trigger.new records are read-only.

### What are Governor Limits?

Governor Limits prevent a single transaction from consuming excessive Salesforce resources.

---

## Conclusion

Apex Triggers are a powerful tool for implementing custom business logic in Salesforce.

When writing triggers:

* Keep them lightweight
* Use helper classes
* Bulkify your code
* Avoid SOQL and DML inside loops
* Follow a trigger framework

By following these best practices, you'll create scalable and maintainable Salesforce applications.

Happy coding! 🚀

```
```