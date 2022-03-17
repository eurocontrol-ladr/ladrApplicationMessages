# LADR Event Notification Message

```mermaid
    sequenceDiagram
    autonumber
    actor Contributor
    participant LADR
    actor User
    Contributor->>LADR: LADR Event Upload Message
    LADR->>Contributor: LADR Event Upload Validation Message
    rect rgb(255, 255, 0)
    LADR->> User: LADR Event Notification Message
    end    
    LADR->>User: LADR Event Message 
    User->>LADR: Event Notification Acknowledgment Message       
    User->>LADR: Event Validation Message
```

## XML schema and samples

- Go to [Schema definition](https://github.com/eurocontrol-ladr/ladrMessages/tree/main/schemas/ladrEventNotificationMessage) on Github

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
