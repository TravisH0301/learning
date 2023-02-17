# Encoding & Schema

- [Data Representation](#data-representation)
- [Encoding](#encoding)
  - [Textual Format Encoding](#textual-format-encoding)
  - [Binary Format Encoding](#binary-format-encoding)
  - [Avro](#avro)
    - [Avro Schema](#avro-schema)
    - [Schema Compatibility](#schema-compatibility)
   
## Data Representation
- In-memory: Data is kept in data structures (ex. objects, lists, arrays). These are optimised for efficient access and manipulation by the CPU
- In file / Over network: Data needs to be encoded into a sequence of bytes. 

Encoding refers to the translation from in-memory representation to a byte sequence (a.k.a serialisation or marshalling) <br>
Decoding refeers to the translation from a byte sequence to in-memory representation (a.k.a deserialisation or unmarshalling)

## Encoding
Many programming languages have their own encoding formats. For example, Python has pickle. <br>
The encoding can be done in either textual format (human-readable) or binary format (machine-readable).

### Textual Format Encoding
- JSON, XML and CSV are widely used textual format encoding
- Somewhat human-readable
- Optional schema
- Good for data interchage but not scalable due to larger size compared to binary format

### Binary Format Encoding
- Example: Thrift, Protocol Buffers and Avro 
- Requires schema for correct decoding
- Less verbose than textual format encoding

### Avro
Avro binary encoding is very compact encoding.

#### Avro Schema
It has 2 types of schemas; Avro IDL & JSON.
- AVRO IDL:

  record Person { <br>
    &emsp; string &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; userName; <br>
    &emsp; union {null, long} &emsp; favoriteNumber = null; <br>
    &emsp; array<string> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp; interests; <br>
  } <br>
  
- JSON:

  { <br>
  &emsp; "type": "record", <br>
  &emsp; "name": "Person", <br>
  &emsp; "fields": [ <br>
  &emsp;&emsp; {"name": "userName", &emsp;&emsp;&emsp;&nbsp;&nbsp; "type": "string"}, <br>
  &emsp;&emsp; {"name": "favoriteNumber", &emsp; "type": ["null", "long"]*, "default": null}, <br>
  &emsp;&emsp; {"name": "interests", &emsp;&emsp;&emsp;&nbsp;&emsp; "type": {"type": "array", "items": "string"}} <br>
  &emsp; ] <br>
  } <br>
  
  \**In Avro, to use NULL as a defualt value, it has to be in the data type as an union with other type. (ex. null & long)*
  
#### Schema Compatibility
In Avro, writer's schema (publisher) and reader's schema (consumer) don't have to be the same. 
Avro library translates the data from the writer's schema to the reader's schema.
  
- Forward Compatibility: When the reader's schema can work with the change in the writer's schema 
- Backward Compatibility: When the writer's schema can work with the change in the reader's schema 
  
Avro can also work with different versions of the schema by including the version number at the beginning
of the every encoded record. Then when decoding the record, the right version of the writer's schema can be used
to correctly decode the record.
  

  
