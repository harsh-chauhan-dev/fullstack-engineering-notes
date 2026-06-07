# 02. E-R Model (Entity-Relationship Model) in DBMS

The **Entity-Relationship (E-R) Model** is a conceptual data modeling technique used to design databases. It helps represent real-world objects, their attributes, and the relationships between them before creating actual database tables.

The E-R Model was introduced by **Peter Chen** in 1976.

---

## Why Do We Use the E-R Model?

Before creating database tables, we need a blueprint of the system.

The E-R Model helps us:

- Understand data requirements
- Visualize database structure
- Reduce data redundancy
- Improve database design
- Convert business requirements into database tables

---

# Basic Components of E-R Model

## 1. Entity

An **Entity** is a real-world object, person, place, thing, or event about which data is stored.

### Examples

- Student
- Teacher
- Course
- Employee
- Product

### Representation

Rectangle

```text
+---------+
| Student |
+---------+
```

---

## 2. Attribute

An **Attribute** describes the properties of an entity.

### Example

```text
Student
├── Student_ID
├── Name
├── Email
└── Age
```

### Representation

Oval

```text
      (Name)
         |
+---------------+
|    Student    |
+---------------+
```

---

# Types of Attributes

## A. Simple Attribute

Cannot be divided further.

### Examples

- Age
- Gender
- Salary

---

## B. Composite Attribute

Can be divided into smaller parts.

### Example

```text
Name
├── First_Name
└── Last_Name
```

Another example:

```text
Address
├── Street
├── City
├── State
└── Pincode
```

---

## C. Single-Valued Attribute

Contains only one value for an entity.

### Example

```text
Student_ID = 101
```

---

## D. Multi-Valued Attribute

Contains multiple values.

### Example

```text
Phone_Number

9876543210
8765432109
```

### Representation

Double Oval

```text
((Phone_Number))
```

---

## E. Derived Attribute

Can be calculated from another attribute.

### Example

```text
Age ← Date_of_Birth
```

If Date of Birth is stored, Age can be calculated.

### Representation

Dashed Oval

```text
- - (Age) - -
```

---

## F. Key Attribute

Uniquely identifies an entity.

### Example

```text
Student_ID
-----------
```

### Representation

Underlined Attribute

---

## 3. Relationship

A **Relationship** shows how entities are connected.

### Example

```text
Student ---- Enrolls ---- Course
```

A student enrolls in a course.

### Representation

Diamond

```text
+---------+      <>      +--------+
| Student |----Enrolls---| Course |
+---------+              +--------+
```

---

# Types of Relationships

## A. One-to-One (1:1)

One entity is related to only one entity.

### Example

```text
Person ↔ Passport
```

One person has one passport.

```text
Person (1) -------- (1) Passport
```

---

## B. One-to-Many (1:M)

One entity can be related to many entities.

### Example

```text
Department → Employees
```

One department contains many employees.

```text
Department (1) ------ (M) Employee
```

---

## C. Many-to-One (M:1)

Many entities relate to one entity.

### Example

```text
Students → College
```

Many students belong to one college.

```text
Student (M) ------ (1) College
```

---

## D. Many-to-Many (M:N)

Many entities relate to many entities.

### Example

```text
Students ↔ Courses
```

A student can enroll in many courses, and a course can have many students.

```text
Student (M) ------ (N) Course
```

---

# ER Diagram Symbols

| Symbol | Meaning |
|----------|----------|
| Rectangle | Entity |
| Oval | Attribute |
| Double Oval | Multi-Valued Attribute |
| Dashed Oval | Derived Attribute |
| Underlined Attribute | Key Attribute |
| Diamond | Relationship |
| Double Rectangle | Weak Entity |

---

# Example ER Diagram: Student Management System

```text
                (Name)
                   |
                   |
           +---------------+
           |    Student    |
           +---------------+
                   |
               Enrolls
                <>
                   |
           +---------------+
           |    Course     |
           +---------------+
                   |
             (Course_ID)
```

---

# Advantages of E-R Model

- Easy to understand
- Provides a visual representation of data
- Helps design databases efficiently
- Reduces redundancy
- Improves communication between developers and clients
- Easy conversion into relational tables

---

# Disadvantages of E-R Model

- Large systems can create complex diagrams
- Not suitable for detailed implementation
- Time-consuming for huge databases
- Cannot represent every database constraint

---

# Definition for Exams

**Entity-Relationship (E-R) Model** is a high-level conceptual data model used to represent entities, attributes, and relationships among data in a database. It provides a graphical way to design databases before implementation.

---

# Quick Revision

```text
Entity       → Real-world object
Attribute    → Property of an entity
Relationship → Connection between entities

1:1 → One-to-One
1:M → One-to-Many
M:1 → Many-to-One
M:N → Many-to-Many

Rectangle → Entity
Oval      → Attribute
Diamond   → Relationship
```