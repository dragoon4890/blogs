---
title: "Compiler Design"
date: "2026-01-04"
description: "Some notes"
---


I was revisiting compiler design which we studied and realized some stuff which I didnt back then.


- First step is lexical analysis , which is just conversion of 
lexemes (raw input) and getting tokens.

    - It doesnt have to be meaningful, thats a procedure for the next
        step
    
    - This step is also involved in clearing up whitespaces etc.

- Next step is syntax analysis, which is done by parser.


First of all, before dwelling into the why we need to think about 
why such segregations and multiple parsers exist.

- we will start with Top down specifically LL(1) parser which is also
called Predictive parser .

- Last time ,I studied, I  just followed the procedure to solve questions involving FIRST(X) and FOLLOW(X).

There's a simple very simple 