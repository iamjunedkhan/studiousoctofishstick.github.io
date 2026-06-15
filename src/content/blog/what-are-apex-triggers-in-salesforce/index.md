---
title: "What Are Apex Triggers in Salesforce? A Complete Beginner’s Guide"
description: "Learn what Apex Triggers are, when to use them, and how to write scalable trigger logic using Salesforce best practices."
pubDate: "Jun 15 2026"
heroImage: "./what-are-apex-triggers-in-salesforce.png"
category: "Apex"
author: "Juned Khan"
---

# What Are Apex Triggers in Salesforce? A Complete Beginner's Guide

If you've spent any time working with Salesforce development, you've likely heard the term **Apex Trigger**. Triggers are one of the most powerful features available to Salesforce developers because they allow you to automate business processes whenever records are created, updated, deleted, or restored.

In this guide, we'll explore what Apex Triggers are, why they are important, when you should use them, and how to write your first trigger following Salesforce best practices.

---

# What Is an Apex Trigger?

An Apex Trigger is a piece of Apex code that executes automatically before or after specific events occur on Salesforce records.

Think of a trigger as an event listener.

When something happens to a record, Salesforce automatically executes the trigger associated with that object.

For example:

- A new Account is created
- An Opportunity is updated
- A Contact is deleted
- A Lead is converted

The trigger can perform additional business logic in response to these actions.

---

# Why Do We Need Triggers?

Salesforce provides many automation tools such as:

- Validation Rules
- Flow
- Approval Processes
- Workflow Rules

However, there are situations where these tools are not enough.

Triggers are useful when you need:

- Complex business logic
- Cross-object updates
- Custom validations
- Integration processing
- Advanced calculations
- Data synchronization

For example, when an Opportunity is marked as Closed Won, you may want to:

1. Create a Project record
2. Send data to an external system
3. Update related records
4. Generate onboarding tasks

This type of logic is often handled through Apex Triggers.

---

# Trigger Events

Salesforce supports several trigger events.

## Before Events

Before triggers execute before data is saved to the database.

### before insert

Runs before a new record is created.

### before update

Runs before an existing record is updated.

### before delete

Runs before a record is deleted.

---

## After Events

After triggers execute after data has been committed to the database.

### after insert

Runs after a new record is created.

### after update

Runs after a record is updated.

### after delete

Runs after a record is deleted.

### after undelete

Runs when a record is restored from the Recycle Bin.

---

# Trigger Syntax

A simple trigger looks like this:

```apex
trigger AccountTrigger on Account (before insert) {

    for(Account acc : Trigger.new) {
        acc.Description = 'Created from Apex Trigger';
    }

}
```

In this example:

- The trigger runs before insert.
- Trigger.new contains all records being created.
- The Description field is automatically populated.

---

# Understanding Trigger Context Variables

Salesforce provides several built-in variables that help you understand the current operation.

## Trigger.new

Contains the new version of records.

```apex
for(Account acc : Trigger.new) {
    System.debug(acc.Name);
}
```

---

## Trigger.old

Contains the old version of records.

```apex
for(Account acc : Trigger.old) {
    System.debug(acc.Name);
}
```

---

## Trigger.newMap

Returns a Map of new records.

```apex
Map<Id, Account> accountMap = Trigger.newMap;
```

---

## Trigger.oldMap

Returns a Map of old records.

```apex
Map<Id, Account> oldAccountMap = Trigger.oldMap;
```

---

## Trigger.isInsert

Checks if the trigger is running during an insert event.

```apex
if(Trigger.isInsert) {

}
```

---

## Trigger.isUpdate

Checks if the trigger is running during an update event.

```apex
if(Trigger.isUpdate) {

}
```

---

## Trigger.isDelete

Checks if the trigger is running during a delete event.

```apex
if(Trigger.isDelete) {

}
```

---

# Before vs After Triggers

Choosing the correct trigger type is important.

| Before Trigger           | After Trigger                     |
| ------------------------ | --------------------------------- |
| Modify record values     | Create related records            |
| Perform validations      | Call integrations                 |
| Faster execution         | Access record Ids                 |
| No additional DML needed | Suitable for dependent operations |

### Use Before Triggers When

- Updating field values
- Performing validations
- Preventing bad data

### Use After Triggers When

- Creating child records
- Sending data externally
- Performing actions that require record Ids

---

# Bulkification: The Most Important Trigger Concept

Salesforce processes records in batches.

A trigger may receive:

- 1 record
- 10 records
- 200 records

Your code must handle all scenarios.

Bad Example:

```apex
trigger AccountTrigger on Account(after insert){

    for(Account acc : Trigger.new){

        Contact c = new Contact(
            LastName = acc.Name,
            AccountId = acc.Id
        );

        insert c;
    }

}
```

This performs DML inside a loop and can hit governor limits.

---

Good Example:

```apex
trigger AccountTrigger on Account(after insert){

    List<Contact> contacts = new List<Contact>();

    for(Account acc : Trigger.new){

        contacts.add(
            new Contact(
                LastName = acc.Name,
                AccountId = acc.Id
            )
        );
    }

    if(!contacts.isEmpty()){
        insert contacts;
    }

}
```

This follows Salesforce bulkification best practices.

---

# Trigger Best Practices

As your projects grow, following best practices becomes critical.

## One Trigger Per Object

Avoid creating multiple triggers on the same object.

Good:

```apex
AccountTrigger
```

Bad:

```apex
AccountInsertTrigger
AccountUpdateTrigger
AccountDeleteTrigger
```

---

## Move Logic to Handler Classes

Avoid writing business logic directly inside triggers.

Trigger:

```apex
trigger AccountTrigger on Account(after insert){

    AccountTriggerHandler.handleAfterInsert(
        Trigger.new
    );

}
```

Handler:

```apex
public class AccountTriggerHandler {

    public static void handleAfterInsert(
        List<Account> accounts
    ){

        // Business Logic

    }

}
```

---

## Avoid SOQL Inside Loops

Bad:

```apex
for(Account acc : Trigger.new){

    Contact con = [
        SELECT Id
        FROM Contact
        LIMIT 1
    ];

}
```

Good:

```apex
List<Contact> contacts = [
    SELECT Id
    FROM Contact
];
```

---

## Avoid DML Inside Loops

Always collect records first and perform a single DML operation.

---

# Real-World Example

Imagine a company wants every new Account to automatically receive a primary Contact.

Trigger:

```apex
trigger AccountTrigger on Account(after insert){

    List<Contact> contacts = new List<Contact>();

    for(Account acc : Trigger.new){

        contacts.add(
            new Contact(
                FirstName = 'Primary',
                LastName = acc.Name,
                AccountId = acc.Id
            )
        );
    }

    insert contacts;

}
```

Now every new Account automatically receives a Contact record.

---

# Common Interview Questions

### What is an Apex Trigger?

An Apex Trigger is code that executes automatically before or after record events occur.

### What is Trigger.new?

A collection containing the new version of records.

### What is the difference between before and after triggers?

Before triggers execute before records are saved, while after triggers execute after records are committed to the database.

### Why is bulkification important?

Bulkification prevents governor limit violations and ensures triggers work efficiently for large data volumes.

### How many records can a trigger process at once?

Up to 200 records in a single transaction.

---

# Conclusion

Apex Triggers are one of the most important building blocks of Salesforce development. They allow developers to automate complex business processes and extend Salesforce beyond standard declarative automation.

As you continue your Salesforce journey, mastering triggers will help you build scalable, maintainable, and enterprise-grade applications.

In the next article, we'll explore **Trigger Context Variables in Detail** and learn how Salesforce provides access to record data during every stage of the trigger lifecycle.
