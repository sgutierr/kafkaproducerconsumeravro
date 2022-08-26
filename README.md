
# Kafka Producer/Consumer example using Avro and Apicurio Service Registry
Example of a Kafka Producer and Consumer application using AVRO for de/serializing objects and APIcurio service registry

The application used in this exercise simulates the intranet of a bank. In this application you can create bank accounts with an initial balance, and operate on those balances. It is a simple CRUD application built with Quarkus that uses Panache to persist the data in an in-memory H2 database.

During this exercise you will use messages to extend the application features. You will assign a type to each bank account based on the current balance.

**Notice that you might need to replace some strings in the application.properties file.**
# Prerequisites

Install an APIcurio Servie Registry and copy the  `Service Registry URL` value. You will use this value to connect to the Red Hat Integration Service Registry.

# Demo steps:

## 1. Review configuration 

 - By using your editor of choice, open the `pom.xml` file and check the `quarkus-apicurio-registry-avro` dependency.
 - Review the AVRO schema for a  `NewBankAccount`  message. This message must include the bank account ID, and the current balance. Both fields must use a  `Long`  type. This is the path  `src/main/avro/new-bank-account.avsc`. During the compilation phase, Quarkus transforms the AVRO schema into a Java class, and stores it in the `target/generated-sources/avsc` directory.
 - The  `Emitter`  must send a  `NewBankAccount`  message.
 - SmalRye Reactive Messaging is an implementation of Eclipse MicroProfile Reactive Messaging Interface https://smallrye.io/smallrye-reactive-messaging/3.18.0/getting-started/
 
## 2. Set application.properties variables

By using your editor of choice, open the src/main/resources/application.properties file, and configure the following Kafka settings:
Replace these variables by your correspondent environment variables: 
 - "YOUR_KAFKA_BOOTSTRAP_HOST"
 - "YOUR_KAFKA_BOOTSTRAP_PORT"
 - "ABSOLUTE_PATH_TO_YOUR_WORKSPACE_FOLDER"
 - "YOUR_SERVICE_REGISTRY_URL"
 Note: Quarkus auto detects the appropriate serializer based on the `@Channel` declaration, the structure of the message, and the presence of the Registry libraries. In this case, the selected serializer for the outgoing channel is `io.apicurio.registry.serde.avro.AvroKafkaSerializer`.
    
## 3. Compile application
In your command-line terminal, navigate to the `kafka-producers/quarkus` directory. Compile the application.
**`./mvnw clean package quarkus:dev`**

## 4. Test
1.  Open a new tab in the browser, and navigate to  `http://localhost:8080`.
    
2.  In the  Create a Bank Account  area, type  `200000`  in the  Initial Balance  field, and click  Create. Notice that the front end displays the created account at the bottom of the page without a type.
    
    The application stores the new bank account in the database, and after that sends a message about creation for future processing by your created consumer. For that reason the bank account type is empty.
    
3.  Return to the command line terminal and verify that the consumer processed the message.
4.  Check your APIcurio Service Registry artifacts to see the new AVRO Schema.
