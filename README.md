# Database Design: From Requirements to Implementation

## 1. Requirements Analysis
The process begins with analyzing the requirements document. This stage involves understanding the business needs and data relationships.

## 2. Entity-Relationship Diagram (ERD)
Based on the requirements, an ERD is created. This is a visual representation of the data structures and their relationships.

### Key Points:
- ERDs can vary based on individual interpretation of requirements
- In an ERD:
  - Data objects are called "entities"
  - Characteristics of entities are called "attributes"

## 3. Mapping ERD to Relational Schema
The ERD is then mapped to a relational schema. This process follows specific rules, ensuring that a single ERD typically results in a single database schema.

### Terminology Changes:
| ERD Term | Schema Term | Database Term |
|----------|-------------|----------------|
| Entity   | Relation    | Table          |
| Attribute| Field       | Column         |

## 4. Database Schema
The schema is the blueprint for the database structure.

## 5. Database Implementation
The final step is implementing the schema in a database management system.

### Key Concepts:
- A **row** (also called a record or tuple) represents a specific instance of an entity
- A **database** is a collection of tables
- A **relational database** contains tables with defined relationships between them

## Example: Employee and Department
In a relational database, you might have:
- An "Employees" table
- A "Departments" table
- A relationship allowing you to associate an employee with a specific department

## Visual Representation

```mermaid
graph TD
    A[Requirements Document] --> B[Entity-Relationship Diagram ERD]
    B --> C[Mapping Process]
    C --> D[Database Schema]
    D --> E[Database Implementation]
    
    subgraph "Terminology Changes"
    F[Entity] --> G[Relation] --> H[Table]
    I[Attribute] --> J[Field] --> K[Column]
    end
```

This diagram illustrates the flow from requirements to implementation, as well as the terminology changes across different stages of database design.


## 6. Physical Design: Mapping ERD to Schema

After creating the Entity-Relationship Diagram (ERD), the next crucial step is mapping it to a database schema. This process follows specific rules established by database design experts to optimize the schema and address common issues like null values and data duplication.

### Key Concept: Null Values
Null values in databases represent reserved but unused memory space. Proper schema design aims to minimize null occurrences.

### Mapping Process Steps

#### Step 0: Identify Special Relationships
Before the main mapping steps, identify one-to-one binary relationships with total participation. These will be mapped first.

#### Step 1: Mapping Regular Strong Entities
Convert primary entities from the ERD into database tables.

#### Step 2: Mapping Weak Entities
Transform weak entities, considering their dependency on strong entities.

#### Steps 3, 4, 5: Mapping Binary Relationships
3. Map one-to-one relationships
4. Map one-to-many relationships
5. Map many-to-many relationships

#### Step 6: Mapping Ternary Relationships
Handle relationships involving three entities.

#### Step 7: Mapping Unary Relationships
Address relationships where an entity is related to itself.

### Visual Representation of Mapping Process

```mermaid
graph TD
    A[ERD] --> B[Step 0: Identify Special Relationships]
    B --> C[Step 1: Map Strong Entities]
    C --> D[Step 2: Map Weak Entities]
    D --> E[Step 3-5: Map Binary Relationships]
    E --> F[Step 6: Map Ternary Relationships]
    F --> G[Step 7: Map Unary Relationships]
    G --> H[Resulting Database Schema]

    subgraph "Binary Relationships"
    I[One-to-One]
    J[One-to-Many]
    K[Many-to-Many]
    end
```

This diagram illustrates the step-by-step process of mapping an ERD to a database schema, highlighting the order and types of relationships addressed.

## Conclusion

The process of moving from an ERD to a physical database design requires careful consideration of entity types, relationships, and design principles. By following these steps, database designers can create efficient schemas that minimize data redundancy and null values while accurately representing the required data structures and relationships.

[Note: Further details on each mapping step can be added as you provide more specific information about the mapping rules and techniques.]




## 6. Physical Design: Mapping ERD to Schema

### Mapping Process Steps

#### Step 0: Identify Special Relationships
Before the main mapping steps, identify one-to-one binary relationships with total participation. These will be mapped first.

#### Step 1: Mapping Regular Strong Entities
Convert primary entities from the ERD into database tables.

Example:
- Strong Entity: Customer
- Attributes: Customer ID (Primary Key), Customer Name, Address
- Resulting Relation: CUSTOMER(CustomerID, CustomerName, Address)

##### Handling Different Attribute Types:
1. Simple Attributes: Mapped directly to columns.
2. Composite Attributes: Each component becomes a separate column.
   Example: Address (Street, City, State, ZIP)
3. Multivalued Attributes: Create a separate relation with a foreign key.
   Example: Employee Skills

#### Step 2: Mapping Weak Entities
Transform weak entities, considering their dependency on strong entities.

#### Steps 3, 4, 5: Mapping Binary Relationships

##### One-to-One Relationships (3 scenarios):
1. Total participation on both sides (Step 0)
2. Total participation on one side, optional on the other
3. Optional participation on both sides

##### One-to-Many Relationships (2 scenarios):
Focus on the "many" side, which can be:
1. Mandatory (total participation)
2. Optional (partial participation)

##### Many-to-Many Relationships:
Only one scenario, typically requiring a junction table.

#### Step 6: Mapping Ternary Relationships
Handle relationships involving three entities.

#### Step 7: Mapping Unary Relationships
Address relationships where an entity is related to itself.

### Visual Representation of Relationship Types

```mermaid
graph TD
    A[Binary Relationships] --> B[One-to-One]
    A --> C[One-to-Many]
    A --> D[Many-to-Many]
    
    B --> E[Total Participation Both Sides]
    B --> F[Total One Side, Optional Other]
    B --> G[Optional Both Sides]
    
    C --> H[Mandatory Many Side]
    C --> I[Optional Many Side]
    
    J[Other Relationships] --> K[Ternary Relationships]
    J --> L[Unary Relationships]
```

This diagram illustrates the different types of relationships and their scenarios in the mapping process.

### Example: Mapping a Strong Entity with Various Attribute Types

```mermaid
erDiagram
    EMPLOYEE {
        int EmployeeID PK
        string Name
        string Street
        string City
        string State
        string ZIP
    }
    EMPLOYEE_SKILLS {
        int EmployeeID FK
        string Skill
    }
    EMPLOYEE ||--o{ EMPLOYEE_SKILLS : has

```

This diagram shows how a strong entity (Employee) with a composite attribute (Address) and a multivalued attribute (Skills) is mapped to relations.

## Conclusion

The process of mapping an ERD to a database schema involves careful consideration of entity types, relationship cardinalities, and attribute characteristics. By following these steps and understanding the different scenarios for each type of relationship, database designers can create efficient and accurate schemas that properly represent the data model.

[Note: Further examples and specific mapping rules can be added for each step as needed.]





# Entity, Attribute, and Relationship Mapping to Database Tables

## 1. Mapping Strong Entities

When mapping a strong entity to a database table:

- Create a table for the entity
- Map attributes to columns based on their type:
  - **Simple attributes**: Become individual columns
  - **Composite attributes**: Each component becomes a separate column
  - **Derived attributes**: Typically not stored in the database
  - **Multi-valued attributes**: Require a separate table (see section 2)

## 2. Handling Multi-valued Attributes

For multi-valued attributes:

1. Create a separate table for the multi-valued attribute
2. Include the primary key of the main entity as a foreign key
3. Create a composite primary key consisting of:
   a) The foreign key from the main entity
   b) The multi-valued attribute itself

### Example: Employee Skills

```mermaid
erDiagram
    Employee ||--o{ Employee_Skills : has
    Employee {
        int id PK
        string name
        int age
    }
    Employee_Skills {
        int employee_id FK
        string skill
    }
```

Table representation:

```mermaid
classDiagram
    class Employee {
        +int id PK
        string name
        int age
    }
    class Employee_Skills {
        +int employee_id FK
        +string skill
    }
    Employee "1" -- "*" Employee_Skills : has
```



## 3. Complex Attributes (Composite and Multi-valued)

For attributes that are both composite and multi-valued:

1. Create a separate table
2. Include components of the composite attribute as columns
3. Include the primary key of the main entity as a foreign key
4. Create a composite primary key consisting of:
   a) The foreign key from the main entity
   b) One or more components of the complex attribute

### Example: Employee Addresses


```mermaid
erDiagram
    Employee ||--o{ Employee_Addresses : has
    Employee {
        int id PK
        string name
        int age
    }
    Employee_Addresses {
        int emp_id FK
        string city
        string country
    }
```

Table representation:

```mermaid
classDiagram
    class Employee {
        +int id PK
        string name
        int age
    }
    class Employee_Addresses {
        +int emp_id FK
        +string city
        string country
    }
    Employee "1" -- "*" Employee_Addresses : has
```

## 4. Composite Primary Key Cases

So far, we've encountered two cases where composite primary keys are used:

1. Mapping multi-valued attributes
2. Mapping complex attributes that are both composite and multi-valued

Additional cases involving composite primary keys will be encountered when mapping certain types of relationships.

## 5. Benefits of This Approach

- Maintains data integrity
- Avoids redundancy
- Properly represents the structure of the original entity and its attributes in the relational database schema

## 6. Next Steps

In subsequent stages of database design, we will explore:

- Mapping different types of relationships (one-to-one, one-to-many, many-to-many)
- Handling weak entities
- Additional scenarios requiring composite primary keys

Remember, the process of moving from an Entity-Relationship Diagram (ERD) to a physical database design requires careful consideration of entity types, relationships, and design principles to create an efficient and accurate database schema.




## 7. Mapping Weak Entities

Weak entities are entities that cannot be uniquely identified by their own attributes alone. They depend on the existence of another entity, known as the owner or identifying entity. When mapping weak entities to database tables, we follow these steps:

1. Create a separate table for the weak entity
2. Include all attributes of the weak entity
3. Include the primary key of the owner entity as a foreign key
4. Create a composite primary key consisting of:
   a) The foreign key from the owner entity
   b) The partial key of the weak entity

### Example: Employee (Strong Entity) and Dependent (Weak Entity)

```mermaid
erDiagram
    Employee ||--o{ Dependent : has
    Employee {
        int id PK
        string name
    }
    Dependent {
        int emp_id FK
        string name
        date date_of_birth
        string gender
    }
```

Table representation:

```mermaid
classDiagram
    class Employee {
        +int id PK
        string name
    }
    class Dependent {
        +int emp_id FK
        +string name
        date date_of_birth
        string gender
    }
    Employee "1" -- "*" Dependent : has
```

In this example:
- Employee is the strong (owner) entity
- Dependent is the weak entity
- The composite primary key of the Dependent table is (emp_id, name)

This is our third case of using a composite primary key, specifically for mapping weak entities.

## Composite Primary Key Cases (Updated)

We've now encountered three cases where composite primary keys are used:

1. Mapping multi-valued attributes
2. Mapping complex attributes that are both composite and multi-valued
3. Mapping weak entities

Each of these cases helps maintain the integrity and accuracy of our database schema, ensuring that we can uniquely identify all entities and their relationships.

