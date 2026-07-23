# Aurora PostgreSQL on AWS

## What Is RDS?

Amazon RDS is a managed database service. AWS handles the operational work like backups, failover, patching, storage growth, and infrastructure, while you still use a real database engine.

## What Is Aurora PostgreSQL?

Aurora PostgreSQL is a PostgreSQL-compatible database that AWS runs for you. It is designed for high availability and scaling, so you get PostgreSQL-like behavior without managing the database servers yourself.

## Why Does Aurora Need a VPC?

Aurora must live inside a VPC because the database needs to exist in a private AWS network. That network gives you control over who can reach the database and keeps it separate from the public internet by default.

## Why Are Subnets Required?

Aurora needs a DB subnet group with at least two subnets in two different Availability Zones. The point is resilience: if one Availability Zone fails, Aurora can still operate in another.

## What Is a Security Group?

A security group is the firewall in front of your database. It decides which systems may connect.

## How Should Database Access Be Set Up?

Use a dedicated security group for the database so its rules stay separate from the rest of your infrastructure. During development, if your backend is not deployed yet, allow only your own public IP. Later, replace that with access from the backend itself, usually through a backend security group or a fixed backend IP. End users should not connect directly to the database.

## What Should the Inbound Rule Look Like?

For PostgreSQL, use port `5432` and prefer the `PostgreSQL` rule type if AWS offers it, otherwise use `Custom TCP`. If you need temporary direct access, allow only your own public IP with `/32`, which means exactly one address. If there are no inbound rules at all, that is safe because nobody can connect.

## What Should I Avoid?

Do not open the database to `0.0.0.0/0`. That exposes it to the whole internet, and public databases are constantly scanned. Also avoid overly broad rule types when a specific PostgreSQL rule is available.

## What About Outbound Rules?

In a normal Aurora setup, leaving outbound traffic open is usually fine. Security groups are stateful, and Aurora mainly responds to incoming connections rather than starting its own.

## Can Aurora Be Kept at $1 per Month?

No. AWS can send cost alerts, but it does not automatically stop Aurora when a budget is reached.

## Is Aurora a Good Choice for a Tiny Budget?

Usually no. Aurora is built for reliability, availability, and scale, so even an idle setup can cost more than a small personal project needs. If your main goal is the cheapest possible PostgreSQL database, Aurora is usually not the first choice.

## What Is the Safer Security Model?

The usual pattern is backend first, database second. Clients talk to the backend, and only the backend talks to Aurora. That reduces exposure and follows least privilege.

Typical flow:

```text
Internet
  |
  v
Backend API
  |
  v
Security Group
  |
  v
Aurora PostgreSQL
```

## Is Aurora the Same as Standard PostgreSQL?

Not exactly. Aurora PostgreSQL is highly compatible with PostgreSQL, but AWS changed the storage layer underneath it. So it behaves like PostgreSQL in most practical ways, while still being its own managed AWS system.
