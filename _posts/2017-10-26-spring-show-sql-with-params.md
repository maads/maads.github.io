---
layout: post
title: Spring - Show SQL with parameters
---

{{ page.title }}
================

In ```application.yml``` set the following properties to show SQL with parameters in the log. By default ```show-sql``` only shows you the statement, not the parameters used.

    spring:
        jpa:
            # rest of config omitted
            show-sql: true
    logging:
      level:
        org:
          hibernate:
            SQL: DEBUG
            type:
              descriptor:
                sql:
                  BasicBinder: TRACE

Works well in [JHipster](https://github.com/jhipster/generator-jhipster/) projects.