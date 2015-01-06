---
layout: post
title: MySQL - view foreign keys
---

{{ page.title }}
================
View all foreign key dependencies for a specific table:

    select
    TABLE_NAME,COLUMN_NAME,CONSTRAINT_NAME, REFERENCED_TABLE_NAME,REFERENCED_COLUMN_NAME
    from INFORMATION_SCHEMA.KEY_COLUMN_USAGE
    where
    REFERENCED_TABLE_NAME = 'student';


List foreign keys for all tables:

    select
    TABLE_NAME,COLUMN_NAME,CONSTRAINT_NAME, REFERENCED_TABLE_NAME,REFERENCED_COLUMN_NAME
    from INFORMATION_SCHEMA.KEY_COLUMN_USAGE
    where
    REFERENCED_TABLE_NAME is not null;

Using MySQL Workbench, foreign keys can also be found in the GUI by right clicking the table name -> Alter table -> Foreign Keys (tab on the bottom)
