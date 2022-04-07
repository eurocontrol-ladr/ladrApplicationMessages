# Distress Event Upload Validation Message

```mermaid
    sequenceDiagram
    autonumber
    actor Contributor
    participant LADR
    actor User
    Contributor->>LADR: Distress Event Upload Message
    rect rgb(0, 102, 255)
    LADR->>Contributor: Distress Event Upload Validation Message
    end
    LADR->> User: Distress Event Notification Message
    LADR->>User: Distress Event Message 
    User->>LADR: Distress Event Notification Message       
    User->>LADR: Distress Event Validation Message
```

```mermaid	
classDiagram	
class LadrApplicationMessage	
<<LADR_Application>> LadrApplicationMessage	
class LadrApplicationMessageMetadata	
<<LADR_Application>> LadrApplicationMessageMetadata	
LadrApplicationMessageMetadata : + timestamp [1] DateTimeUtc	
class LadrApplicationMessageType	
<<LADR_Application>> LadrApplicationMessageType	
LadrApplicationMessageType : DISTRESS_EVENT_UPLOAD_VALIDATION_MESSAGE	
LadrApplicationMessageMetadata --> LadrApplicationMessageType : +type [1]	
LadrApplicationMessageMetadata : + identifier [1] UniversallyUniqueIdentifier	
LadrApplicationMessageMetadata : + receiptTime [1] DateTimeUtc	
class LadrApplicationMessageValidationStatus	
<<LADR_Application>> LadrApplicationMessageValidationStatus	
LadrApplicationMessageValidationStatus : NOT_VALID	
LadrApplicationMessageValidationStatus : VALID	
LadrApplicationMessageMetadata --> LadrApplicationMessageValidationStatus : +validationStatus [1]	
LadrApplicationMessageMetadata : + tval [1] DateTimeUtc	
LadrApplicationMessageMetadata : + referencedMessageIdentifier [1] UniversallyUniqueIdentifier	
LadrApplicationMessageMetadata : + tunv [1] DateTimeUtc	
LadrApplicationMessage --> LadrApplicationMessageMetadata : +metadata [1]	
class DistressEvent	
<<LADR_Application>> DistressEvent	
DistressEvent : + contributorCode [0..1] ContributorCode	
DistressEvent : + identifier [0..1] UniversallyUniqueIdentifier	
LadrApplicationMessage --> DistressEvent : +distressEvent [0..1]	
```	

