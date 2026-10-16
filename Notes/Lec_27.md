# Lec 27 : Introduction to Description Logics

## Introduction

Description Logics are family of knowledge representation.
The language provide the formal underpinning for OWL (Web Ontonlogy Lnaguage).
They are **decidable** fragments of First Order Logic.

Since DLs are logics , they have formal semantic Precise
- specification of meaning of the constructs.
- Logical deduction to infer additional facts based on explicitly stated facts.

The computation of inference is called **reasoning**.

Differences between DLs and other modeling languages (UML,ER)
- Reasoning
- Ability to reperesents complex relationships

## Open World Assumption

- **Closed World Assumption** is the assumption that anything not known to be true is false.
- **Open World Assumption** is the assumption that anything not known to true is unknown.


## No Unique Name Assumption

In Unique Name Assumption the assumption is that entites with different names are different.

> We will be using IRA to differentiate

## Basic Building Blocks

- **Individuals** : same as constants (FOL), resources (RDF), instances
- **Concepts** : sets of individuals or unary predicates (FOL)
- **Roles** : binary relations between the individuals or binary predicaties (FOL)

>[!Note] Naming conventions
> Individuals : lower camelcase
> Concepts : camelcase
> Roles : lower camelcase

## DL Syntax

> refer slides

- top
- bottom
- U
- empty property

### Nominals

Nominals is a concept that has exactly one instance (singleton SWE design pattern)

They are closed concept where we can enumerate the list of its instances

Enumerations can be achieved by using the union.

### Cardinality restrictions

- atleast
- atmost
- exact

## Description Logics

> refer slides

---
