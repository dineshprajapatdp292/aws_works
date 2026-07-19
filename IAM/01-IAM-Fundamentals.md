# Part 1: IAM Fundamentals & Mental Model
> **Level:** Beginner

---

# 📌 Overview

AWS Identity and Access Management (IAM) is the foundation of AWS security. Every request made to AWS services is evaluated through IAM to determine **who is making the request** and **whether they are allowed to perform the requested action**.

Understanding IAM is essential because every AWS service—such as S3, EC2, Lambda, DynamoDB, RDS, and many others—relies on IAM for authentication and authorization.

---

# 🧠 Core Concepts

---

## What is IAM?

### Simple Explanation

Think of a large company with many employees.

The company has:

- Developers
- Managers
- Finance Team
- HR
- Security Team

Not everyone should be allowed to enter every room.

For example:

- Developers should access development servers.
- Finance should access billing systems.
- HR should access employee records.
- Security administrators should manage permissions.

Someone must decide:

- Who can enter?
- Which rooms can they access?
- What actions can they perform?

That "security guard" in AWS is called **IAM (Identity and Access Management).**

---

### Technical Explanation

IAM is a **global AWS service** used to manage:

- Authentication (Who are you?)
- Authorization (What are you allowed to do?)

Whenever someone or something makes an AWS API request, AWS first checks IAM before allowing the action.

---

### IAM Answers Two Questions

1. **Who are you?**
2. **What are you allowed to do?**

These two questions are asked for every AWS API request.

---

## Authentication vs Authorization

### Authentication

Authentication means proving your identity.

Examples:

- Username & Password
- Access Key & Secret Access Key
- Multi-Factor Authentication (MFA)

Example:

> Logging into the AWS Management Console using your username and password.

---

### Authorization

Authorization determines what actions you are allowed to perform after authentication.

Examples:

- Can create EC2 instances
- Can read from S3
- Can delete DynamoDB tables
- Can access billing

Example:

A developer successfully logs into AWS but cannot delete an S3 bucket because their IAM permissions do not allow it.

---

### Comparison

| Authentication | Authorization |
|---------------|---------------|
| Verifies identity | Determines permissions |
| "Who are you?" | "What can you do?" |
| Happens first | Happens after authentication |

---

## Four Main IAM Building Blocks

IAM consists of four primary components.

### IAM Users

Represents an individual person or (legacy) application.

Example:

- Dinesh
- Rahul
- Priya

---

### IAM Groups

A collection of IAM Users that share the same permissions.

Example:

- Developers
- Finance
- DevOps
- Security

Instead of assigning permissions individually to each user, permissions are attached to the group.

---

### IAM Roles

An IAM Role is a temporary identity that can be assumed by trusted entities such as:

- EC2
- Lambda
- ECS
- Another AWS Account
- IAM Users
- Applications

Roles do not have permanent credentials.

---

### IAM Policies

Policies define permissions.

They answer questions like:

- Can read S3?
- Can launch EC2?
- Can terminate EC2?
- Can modify IAM?

Policies are written in JSON.

---

## IAM Request Flow

Whenever an AWS API request is made, AWS evaluates IAM before performing the requested action.

Flow:

```text
User / Application

        │

Authenticate

        │

Authorization Check

        │

IAM Policy Evaluation

        │

Allow or Deny

        │

AWS Service
```

IAM acts as the security checkpoint before AWS executes the request.

---

## Why IAM Exists

Without IAM:

- Everyone would have full access.
- Any employee could delete production data.
- No accountability would exist.
- Security would be impossible.

IAM provides:

- Identity management
- Permission management
- Accountability
- Least privilege
- Secure access to AWS resources

---

## Principle of Least Privilege (Introduction)

Every identity should receive only the permissions required to perform its job.

Example:

A developer responsible for uploading images should receive:

- S3 Upload Permission

They should **not** receive:

- Billing permissions
- IAM management permissions
- EC2 termination permissions

This minimizes the impact of accidental mistakes and security breaches.

---

## Shared Accounts vs Individual Identities

Each employee should have a separate IAM identity.

Reasons:

- Accountability
- CloudTrail auditing
- Easier troubleshooting
- Better security
- Easier permission management

Sharing one administrator account prevents organizations from knowing who performed a specific action.

---

## Implicit Deny

One of IAM's most important security principles.

Every IAM identity starts with **no permissions**.

If no policy grants access:

**Result: Access is denied.**

This is called **Implicit Deny**.

Only an explicit Allow policy grants permissions.

---

# 🏢 Real-World Scenarios

---

## Scenario 1 — Company Office Security

Imagine a company office.

Different employees require different access.

Examples:

- Developers → Development Servers
- Finance → Billing Systems
- HR → Employee Records

A security guard controls who can enter each room.

AWS IAM acts exactly like that security guard.

---

## Scenario 2 — FinTech Company

A FinTech company has:

- Developers
- Finance Team
- Security Team

Developers should not access financial records.

Finance should not manage EC2 instances.

Security administrators manage IAM.

IAM ensures every department receives only the permissions required for its responsibilities.

---

## Scenario 3 — Investigating an S3 Bucket Deletion

Suppose someone accidentally deletes an S3 bucket.

If everyone shares the same administrator account:

CloudTrail only shows:

```
Admin deleted bucket
```

No one knows who actually performed the action.

With separate IAM users:

CloudTrail shows exactly which employee performed the deletion.

This improves accountability and auditing.

---

## Scenario 4 — Junior Developer Accident

A junior developer accidentally receives AdministratorAccess.

While testing a script, they accidentally delete a production backup bucket.

Because the permissions were too broad:

- Production backups are lost.
- Recovery becomes difficult.
- Business operations may stop.

This illustrates why permissions should always follow the Principle of Least Privilege.

---

# ⚠️ Common Mistakes & Misconfigurations

### Giving Everyone AdministratorAccess

Danger:

- Anyone can modify or delete production resources.
- High blast radius during mistakes.
- Increased security risk.

---

### Sharing One Administrator Account

Danger:

- No accountability.
- Difficult investigations.
- Impossible to identify who performed an action.

---

### Using the Root Account Daily

Danger:

- Root has unrestricted permissions.
- Compromising the root account compromises the entire AWS account.

The root account should be used only for tasks that specifically require it.

---

### Using Long-Lived IAM User Access Keys for Applications

Danger:

- Keys can leak.
- Keys may be committed to GitHub.
- Manual rotation is required.
- Modern AWS workloads should use IAM Roles instead.

---

### Granting More Permissions Than Necessary

Danger:

- Larger attack surface.
- Greater damage if credentials are compromised.
- Violates Least Privilege.

---

# 🎯 Interview Angle

Common interview questions from this topic:

### 1. What is AWS IAM?

**Strong Answer:**

IAM is AWS's Identity and Access Management service used to authenticate identities and authorize access to AWS resources.

---

### 2. What is the difference between Authentication and Authorization?

Authentication verifies identity.

Authorization determines permissions after identity has been verified.

---

### 3. Why is IAM important?

Because it controls who can access AWS resources and what actions they are allowed to perform.

---

### 4. Why should organizations avoid shared administrator accounts?

Because shared accounts eliminate accountability and make auditing difficult.

---

### 5. What happens if an IAM User has no policies attached?

Access is denied.

This is called **Implicit Deny**.

---

### 6. What is the Principle of Least Privilege?

Grant only the minimum permissions required to perform a task.

---

# ✅ Practice Questions

- [x] **Should everyone share one administrator account? Why or why not?**  
**Answer:** No. Every user should have their own IAM identity for accountability, auditing, and better security. Shared accounts make it impossible to know who performed an action.

- [x] **Who should receive AdministratorAccess in an organization?**  
**Answer:** Only a small number of trusted cloud or security administrators should have AdministratorAccess. Regular employees should receive only the permissions required for their jobs.

- [x] **Why are separate IAM identities important during security investigations?**  
**Answer:** Separate identities allow services like CloudTrail to identify exactly who performed an action. This helps with auditing, troubleshooting, and incident investigations.

- [x] **What happens if an IAM User has no permissions attached?**  
**Answer:** The user cannot access AWS resources because IAM applies an **Implicit Deny** by default. Permissions must be explicitly granted through policies.

- [x] **Explain Authentication vs Authorization in your own words.**  
**Answer:** Authentication verifies who you are using credentials like a password or access key. Authorization decides what actions you are allowed to perform after you are authenticated.

- [x] **Why does AWS use Implicit Deny by default?**  
**Answer:** Implicit Deny follows a secure "deny by default" approach, preventing accidental access. Users receive permissions only when they are explicitly allowed.

- [x] **Explain the Principle of Least Privilege with a real-world example.**  
**Answer:** Least Privilege means giving only the minimum permissions needed to perform a task. For example, a developer who uploads files to S3 should only have S3 upload permission, not AdministratorAccess.

---

# 🔑 Key Takeaways

- IAM is the foundation of AWS security and controls authentication and authorization.
- Every AWS API request is evaluated through IAM before the requested action is performed.
- IAM consists of four primary building blocks: **Users, Groups, Roles, and Policies.**
- Every IAM identity starts with **Implicit Deny** until permissions are explicitly granted.
- Always follow the **Principle of Least Privilege** by granting only the permissions required for a specific job.
- Never share administrator accounts; use individual identities for accountability and auditing.
- Avoid using long-lived IAM User access keys for applications—modern AWS environments use IAM Roles instead.
