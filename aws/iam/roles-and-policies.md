# IAM Roles and Policies

## Which IAM Roles Should I Keep?

Keep AWS service-linked roles. These are managed by AWS for services such as Backup, Support, or IAM Identity Center. Deleting them usually gives no real benefit, and AWS may recreate them automatically.

## Which IAM Roles Can I Delete?

Delete roles that only existed for a project you have permanently removed. If the app, Lambda, backend, or deployment is gone for good, its old role can usually go too.

## What About Cross-Account Roles?

Be more careful with these. Open the **Trust relationships** tab and check who can assume the role and what permissions it has. If it is unused, especially if it has broad access like `AdministratorAccess`, deleting it is usually safer than leaving it behind.

## How Should I Review IAM Policies?

Do not try to review every AWS-managed policy. In IAM policies, filter by **Customer managed** and ignore the AWS-managed ones. That view shows the policies you actually created, which makes cleanup much easier.

## Simple Cleanup Order

Start by keeping service-linked roles, then remove roles from deleted projects, then inspect cross-account roles carefully, and finally review only customer-managed policies.
