# Apache Kafka

## Kafka Architecture
<img src="https://github.com/TravisH0301/learning/assets/46085656/cd0422a3-0f95-47a0-abad-8ba93f4327a9" width="600">

### Messages 
A message is a unit of data within Kafka. Messages are created by the producer and it consists of the components as illustrated below:
<img src="https://github.com/TravisH0301/learning/assets/46085656/e47de5fa-a681-4800-8fdb-02f466e8b14a" width="600">

- Key: key can be null and the type of key is either binary, string or number. Keys are used to distribute messages across partitions of the topic.
- Value: contains the content of the message. The key-value pair is an important aspect of the messages.
- Compression type: indicates any compression applied to the messages
- Headers: Headers are optional and consist of key-value pairs holding metadata.
- Partition & Offset: Once the message is sent into the Kafka topic, a partition number and an offset id are given to the message.
- Timestamp: Timestamp of the message will be either added by a user or the system

### Producers

### Consumers

### Brokers and Cluster

### Topics and Partitions

### Magic byte & Schema ID
The payload of the messages may have a magic byte (1 byte) and a schema ID (4 bytes) prepended to the serialised message value. If the magic byte is "0" and the following 4 bytes are integers, this indicates, the messages have been serialised using the Confluent schema registry. 

<img src="https://github.com/TravisH0301/learning/assets/46085656/f2bdf746-90b8-4a5c-be0a-7c85643f9ca4" width="800">

As shown above, Confluent Kafka modules prepends a magic byte and a schema ID during serialisation and also expect them during deserialisation. This is done to ensure data formats are identical between producers and consumers. When messages don't contain these metadata, then, the byte payloads should be decoded using other deserialisation modules (ex. Avro).

## Serialisation
<img src="https://github.com/TravisH0301/learning/assets/46085656/0deea433-c262-44f0-ae1e-9986ea2d2789" width="600">

Both keys and values of the messages are stored in bytes in Kafka. There are several different serialisation formats, and it's essential that both the producer and the consumer of the Kafka topic should use the same serialisation format. And key and value can have different serialisers. Below are some example serialisation formats:
- String
- Integer, and Float for numbers
- JSON, Avro, Protobuf

## Features


## Troubleshooting
### Segmentation Fault
This error occurs when the program tries to access the memory beyond the reach. The following steps can be taken to identify and resolve the issue.
- Debug and track the root cause of the issue
- Increase memory stack size by $ulimit -s \<new value or unlimited\>
- Either upgrade or downgrade any module used in the program - e.g., Downgrade Confluent-Kafka from 2.0.0 to 1.9.0

### Wrong magic byte
      confluent_kafka.avro.serializer.SerializerError: Message deserialization failed for message at au.dse-martech.adobeAnalyticsStreamer-v1 [1] offset 760125: message does not start with magic byte
      
The above error message occurs when the Confluent Kafka module tries to deserialise a message that doesn't contain a magic byte that equals "0". Confluent Kafka modules use "0" as the magic byte and will expect to process the messages with it. In this case, the byte payloads need to be deserialised using other modules like Avro.

Below is an example of deserialising a message payload using the Avro module.

      from avro.datafile import DataFileReader
      from avro.io import DatumReader, BinaryDecoder
      from avro.schema import parse
      import requests
      import io
      
      # Retrieve schema from the schema registry
      schema_registry_url = "<schema_registry_url>"
      schema_subject = "<schema_subject>"
      schema_id = f"/subjects/{schema_subject}/versions/latest"
      response = requests.get(schema_registry_url + schema_id)
      schema_definition = response.json()['schema']
      schema = parse(schema_definition)
      
      # Convert payload bytes to file-like object for BinaryDecoder
      val = msg.value()
      bytes_reader = io.BytesIO(val)  # converts the payload bytes into file-like object
      decoder = BinaryDecoder(bytes_reader)  # wraps file-like object into the decoder object 
      
      # Read avro using the schema
      reader = DatumReader(schema)  # creates reader object
      message = reader.read(decoder)  # reader reads the payload by deserialising with the schema
      
      print(message)
