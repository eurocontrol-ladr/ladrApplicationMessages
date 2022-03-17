# Welcome to the LADR Schemas documentation

```mermaid
    sequenceDiagram
    autonumber
    actor Contributor
    participant LADR
    actor User
    Contributor->>LADR: LADR Event Upload Message
    LADR->>Contributor: LADR Event Upload Validation Message
    LADR->>User: LADR Event Notification Message
    LADR->>User: LADR Event Message 
    User->>LADR: Event Notification Acknowledgment Message       
    User->>LADR: Event Validation Message
```

## How to use this web site

- Use the side bar opposite to access the various sections of this manual;
- Use the search engine to look for a specific entry;
- Use the buttons *< Previous* and *Next >* at the bottom of each page to navigate across the different sections of the manual.

## Acronyms

|Acronyms|Definition|
|:-|:--|
|AIRM|ATM Information Reference Model|
|FIXM|Flight Information Exchange Model|
|LADR|Location of Aircraft in Distress Repository|
|...|...|


## References

- LADR: ...
- AIRM: ...
- FIXM: ...
 
