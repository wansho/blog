---
layout: post
title: "PostgreSQL-RBAC-with-schema"
category: rbac
---

![postgresql-rbac](/assets/image/rbac/postgresql-rbac.svg)

## schema

A PostgreSQL database cluster contains one or more named databases. Roles and a few other object types are shared across the entire cluster. A client connection to the server can only access data in a single database, the one specified in the connection request.

Users of a cluster do not necessarily have the privilege to access every database in the cluster. Sharing of role names means that there cannot be different roles named, say, `joe` in two databases in the same cluster; but the system can be configured to allow `joe` access to only some of the databases.

When a new database is created, PostgreSQL by default creates a schema named `public` and grants access on this schema to a backend role named `public`. All new users and roles are by default granted this `public` role, and therefore can create objects in the `public` schema.

https://www.postgresql.org/docs/current/ddl-schemas.html

同一个库下，不同的 schema 下，可以包含相同表名的表，并不是所有一个库下的所有表，被分到不同的 schema 下面。

一个 cluster 中可以有很多数据库。

所有默认创建的新表，都属于 public schema。


postgresql 除了定义了数据库这个概念以外，还定义了模式这个概念。模式比数据库更小，模式是表的集合，一个模式可以包含视图、索引、数据类型、函数和操作符等。一个数据库可以包含多个模式。引入模式，可以使得多个用户使用一个数据库，并且不会相互干扰。

## rbac

有一个默认的角色 public，也有一个默认的 schema public，所有的用户都拥有 public 角色。

PostgreSQL 中有角色这个概念。PostgreSQL 是支持 RBAC 的。

创建用户，赋予角色：

postgresql 的角色即用户，在创建角色的时候，勾选创建用户选项。

![](/assets/image/rbac/image-20221206151920304.png)

创建完用户后，选中对应的角色，给该角色配置可以访问的模式：

![](/assets/image/rbac/image-20221206152906976.png)
