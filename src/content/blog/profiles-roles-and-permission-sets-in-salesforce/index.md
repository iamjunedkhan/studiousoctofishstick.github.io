---
title: "Profiles, Roles, and Permission Sets in Salesforce"
description: "Learn the difference between Profiles, Roles, and Permission Sets in Salesforce, when to use each one, and best practices for managing user access."
pubDate: "Jul 2 2026"
heroImage: "./profiles-roles-and-permission-sets-in-salesforce.png"
category: "Salesforce Fundamentals"
author: "Juned Khan"
---

# Profiles, Roles, and Permission Sets in Salesforce

Managing user access is one of the most important aspects of Salesforce administration. Every user should have access to only the data and features they need to perform their job—nothing more, nothing less.

Salesforce achieves this through **Profiles**, **Roles**, and **Permission Sets**. Although these terms are often used together, they serve different purposes. Understanding when to use each one will help you build a secure and scalable Salesforce org.

---

# Profiles

A **Profile** defines **what a user can do** in Salesforce.

Every Salesforce user must have **exactly one Profile**. Without a profile, a user cannot log in or access the platform.

A profile controls permissions such as:

- Object permissions (Create, Read, Edit, Delete)
- Field-level security
- App access
- Tab visibility
- Apex class access
- Visualforce page access
- Login hours
- Login IP ranges

### Example

Suppose you have a **Sales Representative**.

Their profile may allow them to:

- Create Opportunities
- Edit Leads
- View Accounts
- Access the Sales app

But it might prevent them from:

- Deleting Accounts
- Accessing Setup
- Managing users

The profile establishes the user's baseline permissions.

---

# Roles

A **Role** determines **which records a user can access**.

Unlike profiles, roles are focused on **record visibility**, not permissions.

Salesforce uses a **Role Hierarchy**, allowing managers to automatically access records owned by users below them.

### Example Role Hierarchy

```text
CEO
│
├── Sales Director
│     ├── Sales Manager
│     │      ├── Sales Executive A
│     │      └── Sales Executive B
│
└── Support Director
```

In this example:

- Sales Executive owns Opportunities.
- Sales Manager can view those Opportunities.
- Sales Director can also view them.
- CEO can view everything.

The executives cannot automatically see each other's records unless sharing rules or other sharing mechanisms allow it.

---

# Permission Sets

A **Permission Set** provides **additional permissions** to selected users without changing their profile.

Think of it as an extension to a profile.

Instead of creating multiple profiles for small permission differences, you can simply assign Permission Sets where needed.

A user can have:

- One Profile
- Multiple Permission Sets

### Example

Imagine all Sales users share the same profile.

However, only a few users need access to:

- Data Loader
- Custom reports
- A custom application
- A specific Apex-enabled feature

Instead of creating another profile, simply create a Permission Set containing those permissions and assign it to the required users.

This keeps your security model much easier to maintain.

---

# Profiles vs Roles vs Permission Sets

| Feature                    | Profile | Role        | Permission Set   |
| -------------------------- | ------- | ----------- | ---------------- |
| Controls user permissions  | ✅      | ❌          | ✅               |
| Controls record visibility | ❌      | ✅          | ❌               |
| Required for every user    | ✅      | ❌          | ❌               |
| One per user               | ✅      | Usually one | Multiple allowed |
| Used for additional access | ❌      | ❌          | ✅               |

---

# When to Use Each

## Use a Profile when

- Defining a user's baseline access
- Controlling object permissions
- Managing field-level security
- Restricting login hours or IP ranges

---

## Use a Role when

- Organizing users into a reporting structure
- Providing managers access to subordinate records
- Managing record visibility through the role hierarchy

---

## Use a Permission Set when

- Granting temporary access
- Providing access to a specific feature
- Avoiding the creation of unnecessary profiles
- Giving extra permissions to selected users

---

# Best Practices

### Keep the number of Profiles small

Avoid creating a new profile for every department or minor variation. Too many profiles become difficult to maintain.

---

### Prefer Permission Sets for additional access

This is Salesforce's recommended approach. Use profiles for baseline permissions and Permission Sets for exceptions.

---

### Use Roles only for record visibility

Do not use roles to control what users can do. Their primary purpose is controlling access to records.

---

### Follow the principle of least privilege

Grant only the permissions users need to perform their responsibilities. Additional access can always be provided later if required.

---

# A Simple Way to Remember

If you're ever unsure which one to use, remember this:

- **Profile** → What a user **can do**
- **Role** → Which records a user **can see**
- **Permission Set** → Extra permissions for specific users

Keeping these responsibilities separate makes your Salesforce security model cleaner, easier to manage, and more scalable.

---

# Conclusion

Profiles, Roles, and Permission Sets each play a distinct role in Salesforce security.

A Profile establishes a user's baseline permissions, a Role determines record visibility through the role hierarchy, and Permission Sets provide additional permissions without requiring new profiles.

Using them correctly not only improves security but also makes your Salesforce org easier to maintain as your team grows.
