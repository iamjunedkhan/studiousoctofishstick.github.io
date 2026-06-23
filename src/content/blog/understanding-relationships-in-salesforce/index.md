---
title: "Understanding Relationships in Salesforce: A Complete Guide"
description: "Learn the different types of relationships in Salesforce, including Lookup, Master-Detail, Many-to-Many, Self, and Hierarchical relationships. Understand when to use each relationship type and how they impact your Salesforce data model."
pubDate: "Jun 23 2026"
heroImage: "./cover.png"
category: "Salesforce Architecture"
author: "Juned Khan"
---
# Understanding Relationships in Salesforce

*Learn how Salesforce objects connect with each other and how relationships help you build powerful data models.*

## Introduction

One of the most important concepts in Salesforce is **relationships between objects**. Relationships allow different records to connect and interact with each other, creating a structured and meaningful data model.

Think about a real-world business scenario. A customer can have multiple orders, an account can have several contacts, and an opportunity can contain many products. Without relationships, all of this information would exist in isolation, making it difficult to manage and analyze.

In this article, we'll explore the different types of relationships in Salesforce, when to use them, and best practices for designing a scalable data model.

---

# Why Relationships Matter

Relationships help you:

* Connect related records together
* Avoid duplicate data
* Build meaningful reports and dashboards
* Improve data integrity
* Create automation across connected records
* Design scalable applications

For example:

| Object      | Related Object | Relationship                                |
| ----------- | -------------- | ------------------------------------------- |
| Account     | Contact        | One Account can have many Contacts          |
| Account     | Opportunity    | One Account can have many Opportunities     |
| Opportunity | Product        | Opportunities can contain multiple Products |
| Case        | Contact        | Cases can be linked to Contacts             |

Without relationships, users would need to manually enter the same information repeatedly.

---

# Types of Relationships in Salesforce

Salesforce provides several relationship types, but the most commonly used are:

1. Lookup Relationship
2. Master-Detail Relationship
3. Many-to-Many Relationship
4. Self Relationship
5. Hierarchical Relationship
6. External Lookup Relationship
7. Indirect Lookup Relationship

Let's look at each one.

---

# 1. Lookup Relationship

A **Lookup Relationship** is the most flexible relationship type in Salesforce.

It creates a loose connection between two objects.

### Example

* Contact → Account
* Case → Contact
* Custom Object → Account

### Key Characteristics

* Parent and child records are independent.
* Child records can exist without a parent.
* Record ownership remains separate.
* Security settings are managed independently.
* Roll-up summary fields are not supported.

### Real-World Example

Imagine a Job Application object linked to a Candidate object.

If the Candidate record is deleted, the Job Application can still remain in the system.

```
Candidate
   |
   | Lookup
   ↓
Job Application
```

### When to Use Lookup Relationships

Use a Lookup Relationship when:

* Parent and child records should remain independent.
* Child records may exist without a parent.
* Ownership should be managed separately.

---

# 2. Master-Detail Relationship

A **Master-Detail Relationship** creates a tightly coupled relationship between two objects.

The child record depends entirely on the parent record.

### Example

* Invoice → Invoice Line Items
* Project → Tasks
* Order → Order Items

### Key Characteristics

* Child records cannot exist without a parent.
* Parent controls ownership and security.
* Deleting the parent deletes all child records.
* Supports Roll-Up Summary Fields.
* Child record inherits sharing settings from the parent.

### Real-World Example

An Order contains multiple Order Items.

If the Order is deleted, all associated Order Items are automatically removed.

```
Order
  |
  | Master-Detail
  ↓
Order Item
```

### When to Use Master-Detail Relationships

Choose Master-Detail when:

* The child record has no meaning without the parent.
* You need Roll-Up Summary Fields.
* Security should be inherited from the parent.

---

# Lookup vs Master-Detail

This is one of the most common Salesforce interview questions.

| Feature                    | Lookup   | Master-Detail |
| -------------------------- | -------- | ------------- |
| Parent Required            | No       | Yes           |
| Child Exists Independently | Yes      | No            |
| Ownership                  | Separate | Inherited     |
| Roll-Up Summary            | No       | Yes           |
| Cascade Delete             | Optional | Automatic     |
| Security Inheritance       | No       | Yes           |

### Quick Rule

If the child cannot exist without the parent, use **Master-Detail**.

Otherwise, use **Lookup**.

---

# 3. Many-to-Many Relationship

Salesforce does not directly support many-to-many relationships.

Instead, they are created using a **Junction Object**.

### Example

Students can enroll in multiple Courses.

Courses can have multiple Students.

```
Student
   |
Enrollment
   |
Course
```

### How It Works

The Junction Object contains:

* Master-Detail relationship to Student
* Master-Detail relationship to Course

### Benefits

* Supports reporting
* Maintains data integrity
* Allows additional fields on the relationship

For example:

Enrollment can store:

* Enrollment Date
* Status
* Grade

---

# 4. Self Relationship

A Self Relationship occurs when an object relates to itself.

This is implemented using a Lookup Relationship.

### Example

Employee Management Structure

```
Employee
   |
Manager (Employee)
```

John reports to Sarah.

Both are records in the same Employee object.

### Common Use Cases

* Employee hierarchies
* Partner relationships
* Product bundles
* Organization structures

---

# 5. Hierarchical Relationship

A Hierarchical Relationship is available only on the User object.

It allows one user to reference another user.

### Example

```
Sales Representative
      |
      ↓
Sales Manager
```

### Common Uses

* Approval processes
* Reporting structures
* Manager relationships

### Key Point

Only the standard User object supports Hierarchical Relationships.

---

# 6. External Lookup Relationship

External Lookup Relationships are used with Salesforce Connect.

They connect external data stored outside Salesforce.

### Example

Salesforce record linked to data stored in:

* ERP systems
* SAP
* Oracle databases
* External applications

### Benefits

* Access external data without importing it
* Reduce storage consumption
* View data in real time

---

# 7. Indirect Lookup Relationship

An Indirect Lookup Relationship links external objects to Salesforce objects using an external ID.

### Example

```
External Order System
        |
External ID
        |
Salesforce Account
```

This is commonly used when integrating enterprise systems with Salesforce.

---

# Relationship Queries in SOQL

Understanding relationships becomes even more important when writing SOQL.

## Child to Parent Query

Using dot notation:

```sql
SELECT Id,
       Name,
       Account.Name
FROM Contact
```

Here we access Account fields from Contact.

---

## Parent to Child Query

Using subqueries:

```sql
SELECT Id,
       Name,
       (
           SELECT Id,
                  LastName
           FROM Contacts
       )
FROM Account
```

This retrieves Accounts and their related Contacts.

---

# Best Practices

## 1. Choose the Right Relationship Type

Don't automatically use Master-Detail.

Ask:

* Does the child need a parent?
* Do I need Roll-Up Summaries?
* Should ownership be inherited?

---

## 2. Plan for Future Growth

Relationships are difficult to change later.

Design your data model carefully before implementation.

---

## 3. Avoid Excessive Relationship Depth

Deep relationship chains can make:

* Reports slower
* SOQL queries complex
* Maintenance harder

Keep your model simple whenever possible.

---

## 4. Use Naming Conventions

For custom relationship fields:

Examples:

* Account__c
* Project__c
* Manager__c

Consistent naming improves maintainability.

---

## 5. Document Your Data Model

Every project should include:

* Object diagrams
* Relationship mappings
* Field documentation

This helps developers, admins, and architects understand the system quickly.

---

# Common Interview Questions

### What is the difference between Lookup and Master-Detail?

Lookup is a loose relationship where records are independent. Master-Detail is a tightly coupled relationship where the child depends on the parent.

### Can a child record have multiple Master records?

Only through a Junction Object that contains two Master-Detail relationships.

### Can a Lookup Relationship be converted to Master-Detail?

Yes, but only if all existing child records have valid parent records and certain conditions are met.

### How many Master-Detail relationships can a custom object have?

Up to two Master-Detail relationships.

---

# Conclusion

Relationships are the foundation of every Salesforce data model. Whether you're building a simple application or designing an enterprise-scale solution, understanding how objects connect is essential.

The two relationship types you'll use most often are **Lookup** and **Master-Detail**, but knowing when to use Junction Objects, Self Relationships, and External Relationships can help you design cleaner and more scalable solutions.

Before creating any object, always ask one simple question:

**"How does this record relate to the rest of my data?"**

The answer will guide you toward the right relationship type and a stronger Salesforce architecture.
