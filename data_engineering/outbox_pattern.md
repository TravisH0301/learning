# Outbox Pattern in Event-Driven Architecture
- [Synchronous vs. Asynchronous Communication](#synchronous-vs-asynchronous-communication)
- [Event-Driven Architecture and it's challenge](#event-driven-architecture-and-its-challenge)
- [The Outbox Pattern](#the-outbox-pattern)
- [Outbox Schema](#outbox-schema)
- [Error Handling](#error-handling)
- [Retention Policy](#retention-policy)
- [Conclusion](#conclusion)

Microservices require communication between services and it can be achieved via synchronous or asynchronous data exchanges. <br>
Synchronous communication is commonly achieved by API request call, whereas, asynchronous communication is achieved using a message broker.

## Synchronous vs. Asynchronous Communication
- Synchronous communication can lead to scalability and reliability issues.
- Asynchronous communication allows services to process requests at their own pace, enhancing scalability and reducing coupling.

## Event-Driven Architecture and it's challenge
Asynchronous communication provides Event-Driven architecture, yet, one of its challenges is "guaranteed message delivery". <br>
For the following scenario, the Service A makes changes to the Production DB, and the changes are captured and delivered to the Downstream Consumers. <br>

![image](https://user-images.githubusercontent.com/46085656/227776576-e759ee42-f785-4878-95bc-d504247c2b60.png)

This design can cause an inconsistent state between the Production DB and the Downstream Consumers. <br>
For example, if the Message Broker becomes unavailable at the time of message delivery and rollback on the Production DB fails, this will cause the inconsistency.

## The Outbox Pattern
To tackle this challenge, the outbox pattern can be implemented. And the pattern consists of a two-step transactions:
    1. Commit a single atomic database transaction to make changes to the Production DB as well as writing the payload to the outbox table.
    2. Dispatcher to capture the data change in the outbox table (via polling or transaction log tailing) and publish the payloads to the Message Broker.

![image](https://user-images.githubusercontent.com/46085656/227777314-986d40eb-7a15-456c-84d3-6484e6486346.png)

In this way, the outbox pattern decouples the Production DB and the Message Broker by having the outbox table as a buffer in between them.

## Outbox Schema
The outbox schema typically includes the followings:
- id: A unique identifier for each record in the outbox table. This can be a primary key to ensure idempotency.
- timestamp: The date and time the event was created. This can be used for ordering and processing events in the correct sequence.
- event_type: A categorization of the event, such as "user_created", "order_placed", or "inventory_updated". This helps consumers of these events to understand the context and take appropriate actions.
- aggregate_id: The identifier of the related aggregate root entity in the originating service, such as a user ID or order ID. This is useful for correlating events and data across services.
- aggregate_type: The type of aggregate root entity, such as "user", "order", or "inventory_item". This provides additional context for event consumers.
- payload: The actual data associated with the event. This is typically stored as a JSON string or a serialized object, containing all relevant information for the event.
- status: The status of the event, indicating if it is pending, sent, or failed. This is used to control the flow of the message and ensures "at least once" delivery.
- version: An optional field to track the version of the event payload schema. This can be useful when evolving the schema over time.

## Error Handling
Retry logic can be used for messages which failed to get dispatched to the Message Broker. And upon failing the entire retry logic, the message can either be discarded or stored for an investigation.

## Retention Policy 
To control the size of the outbox table, a rentention policy can be implemented to delete or archieve records after certain period of time.

## Conclusion
The outbox pattern is a strategy to ensure consistent and reliable communication between services, especially in distributed systems. <br>
Implementing the outbox pattern can help mitigate the challenges associated with tight coupling and data inconsistencies.

