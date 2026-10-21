# Lec 28 : Introduction to Owl

## OWL (Web Ontology Language)

- OWL is a knowledge representation language used to build ontologies
- OWL 2 is the latest versionof W3C standard

Three types of entities :
- Classes
- Propreties
- Individuals

All the entities are identified by using IRIs.

>[!NOTE]
 Identifiers and Naminig convention is similar as we discussed before.

---

## OWL Syntax

Funtional-Style syntax (Is the most used).

Axioms, Entities, Expression all together capture the knowledge of a domain.

>Refer : the slides for better understanding the OWL Syntax

---

## Class Hierarchy

Class , subclass, equivalent classes , disjointclasses

similarly we have property hierarchy.

>[!QUESTION] What is difference btw object property and Data property ?

Domain and Range of Propertites.

---

> [!NOTE]
> OWL also follows the open world and no unique name assumption.

> Refer : Slides for Logical Class Constructions and Logical Quantifiers and Cardinality restriction.


---

## Enumeration of Individuals

- List all possible and the only members of a class
- Classes defined this way are called **closed** classes

---

## Property Characteristics

- Inverse Property
- Symmetric Property
- Reflexive Property
- Asymmetric Property
- Irreflexive Property
- Functional Property
- Inverse Functional Property
- Transitive Property
- Disjoint Property

We can also have **Property Chains**, combining properties.

---

## OWL 2 Profiles

- An OWL 2 profile is a fragment or sublanguage of OWL 2.
- Each Profile is a trimmed down version of OWL 2 that trades expressivity with efficiancy of reasoning.
- OWL 2 Full, OWL 2 DL, OWL 2 EL, OWL 2 QL , OWL 2 RL

> More on OWL profile in slides.


---

>[!EXAMPLE] Comparing every knowleadge reprentation in slides.

---