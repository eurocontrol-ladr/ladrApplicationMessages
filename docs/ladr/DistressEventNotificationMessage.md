# Distress Event Notification Message

```mermaid
    sequenceDiagram
    autonumber
    actor Contributor
    participant LADR
    actor User
    Contributor->>LADR: Distress Event Upload Message
    LADR->>Contributor: Distress Event Upload Validation Message
    rect rgb(255, 255, 0)
    LADR->> User: Distress Event Notification Message
    end    
    LADR->>User: Distress Event Message 
    User->>LADR: Distress Event Acknowledgment Message       
    User->>LADR: Distress Event Validation Message
```

## XML schema and samples

- Go to [Schema definition](https://github.com/eurocontrol-ladr/ladrApplicationMessages/tree/main/schemas/DistressEventNotificationMessage) on Github

- Go to [XML examples](https://github.com/hlepori/test_ladr/tree/main/samples) on Github

## UML description

```mermaid
classDiagram
class LadrMessage	
<<LADR_Application>> LadrMessage	
LadrMessage : + timestamp [1] Time	
class LadrMessageType	
<<LADR_Application>> LadrMessageType	
LadrMessageType : LADR_EVENT_NOTIFICATION_MESSAGE	
LadrMessageType <-- LadrMessage : +type [1]	
class LadrEvent	
<<LADR_Application>> LadrEvent	
LadrEvent : + identifier [1] UniversallyUniqueIdentifier	
LadrEvent : + eventIssuedTime [1] Time	
LadrMessage --> LadrEvent : +ladrEvent [1]	
```
