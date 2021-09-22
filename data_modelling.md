# Data Modelling (Database modelling)
An abstrat that organises elements of data and they relate to each other. End state is a database.

## Process
- Gather requirements
- Build data model using Entity Relationship (ER) Model or Unified Modelling Language (Typically Entity Relationship Diagram is used)
  - Conceptual data modelling: establishes entities, attributes and relationships. (no cardinality)
  - Logical data modelling: defines column information (type, length, etc) and cadinality of relationships. 
  - Physical data modelling: described database-specific implementation of the data model. Primary & foreign keys, view, indexes and authorisation, etc. are defined.

*Entity: Basic object of entity relationship diagram. (ex. tables in database)<br>
*Attribute: facts of entities. (ex. columns in tables)<br>
*Cardinality: the possible occurence numbers of attributes between tables. (ex. one-to-one => 1 primary key in table A & 1 foreign keys in table B)

## Relational Model
This model organises data into rows and columns with a unique key identifying each row (ex. primary key). Each table represents one entity type (ex. Customer).<br>
ER model converts into relational model through physical data modelling.

### Difference between Entity Relationship Model and Relational Model
- ER model contains cardinality as the relationship between entities and relational model has less contraint in the relationship between tables.
- ER model is high-level model of entity relationship (with ER diagram) and relational model is implementation model.
- ER model: Entity | Attribute (no Domain & Key refers to unique identifier) | Relationship btw entities 
- Relational model: Relation/Table | Column/Attribute (Domain represents acceptable values & Degree refers to # of attributes) | Row/Record/Tuple

