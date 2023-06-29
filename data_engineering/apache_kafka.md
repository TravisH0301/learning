# Apache Kafka

Kafka messages are key-value pairs with optional headers.  

## Magic byte & Schema ID
The payload of the messages may have a magic byte (1 byte) and a schema ID (4 bytes) prepended to the serialised message value. If the magic byte is "0" and the following 4 bytes are integers, this indicates, the messages have been serialised using the Confluent schema registry. 

<img src="https://github.com/TravisH0301/learning/assets/46085656/f2bdf746-90b8-4a5c-be0a-7c85643f9ca4" width="800">

As shown above, Confluent Kafka modules prepends a magic byte and a schema ID during serialisation and also expect them during deserialisation. This is done to ensure data formats are identical between producers and consumers. When messages don't contain these metadata, then, the byte payloads should be decoded using other deserialisation modules (ex. Avro).

# Troubleshooting
## Wrong magic byte
The following error message occurs when the Confluent Kafka module tries to deserialise a message that doesn't contain a magic byte that equals "0". Confluent Kafka modules use "0" as the magic byte and will expect to process the messages with it. In this case, the byte payloads need to be deserialised using other modules like Avro.

      confluent_kafka.avro.serializer.SerializerError: Message deserialization failed for message at au.dse-martech.adobeAnalyticsStreamer-v1 [1] offset 760125: message does not start with magic byte
