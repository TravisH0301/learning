# Data Modelling (Database modelling)
An abstrat that organises elements of data and they relate to each other. End state is a database.

## Process
- Gather requirements
- Build data model using Entity Relationship Model or Unified Modelling Language (Typically Entity Relationship Diagram is used)
  - Conceptual data modelling: establishes entities, attributes and relationships. (no cardinality)
  - Logical data modelling: defines column information (type, length, etc) and cadinality of relationships. 
  - Physical data modelling: described database-specific implementation of the data model. Primary & foreign keys, view, indexes and authorisation, etc. are defined.

*Entity: Basic object of entity relationship diagram. (ex. tables in database)
*Attribute: facts of entities. (ex. columns in tables)
*Cardinality: the possible occurence numbers of attributes between tables. (ex. one-to-one => 1 primary key in table A & 1 foreign keys in table B)



