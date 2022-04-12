# Topics to be discussed

## Message identifier

### Requirement

ICAO doc 10150 does not require the messages sent to or from LADR to have an identifier. However, it may be useful that the exchanged LADR messages have one, for tracing purposes, and/or so that a response message can indicate the message to which it is responding.

Note: The ICAO FF-ICE concept has a requirement along these lines.

### To be discussed

In some prototype's samples, some LADR messages had a message number, expressed as an unsigned integer. It is understood that this field corresponds to the field MESSAGE NUMBER as standardised in the MCC standard interface. As LADR will centralise DistressEventUplaodMessages sent from multiple accredited LADR contributors, the risk exists of potential collisions between message identifiers sent by different sources (for instance two distinct LADR contributors using the same value "12345" for identifying to two distinct messages sent to the LADR). 

To mitigate the risk, the use of UUIDs may be considered for identifying LADR messages. UUIDs are 128 bits values enabling distributed systems to uniquely identify something without significant central coordination and with confidence that the same identifier will never be unintentionally used by anyone for anything else. A UUID is meant to be universally unique, i.e. the probably of a duplicate identifier being generated within the IT universe is negligible for the foreseeable future. UUIDs are defined in IETF RFC 4122 and ISO/IEC 9834-8, the latter specifying robust UUID generation procedures.

The ISO/IEC 9834 is already used in an Aviation/ATM context, with AIXM and FIXM using UUID v4.0 for uniquely identifying their respective features of interest. The FF-ICE message identifiers are also ISO UUID v4.

An example of UUID v4 reads as follows: `282123e9-958a-4e99-a007-a0d838a148f0`

Off-the-shelf libraries exist that support the generation of UUID v4. Online UUID v4 generation services also exist that can be leveraged (e.g. https://www.uuidgenerator.net/version4).

It is proposed to reuse UUID v4 for identifying LADR messages. In the current LADR schemas, this is represented as follows:

```xml
<xs:complexType name="LadrApplicationMessageMetadataType">
 <xs:sequence>
  <xs:element name="identifier" type="ladr:UniversallyUniqueIdentifierType" minOccurs="1" maxOccurs="1"/>
  <xs:element name="referencedMessageIdentifier" type="ladr:UniversallyUniqueIdentifierType" minOccurs="1" maxOccurs="1"/>
<!-- [...] -->			
<xs:complexType name="UniversallyUniqueIdentifierType">
 <xs:simpleContent>
  <xs:extension base="ladr:RestrictedUniversallyUniqueIdentifierType">
   <xs:attribute name="codeSpace" use="required" fixed="urn:uuid" type="xs:string"/>
<!-- [...] -->
<xs:simpleType name="RestrictedUniversallyUniqueIdentifierType">
 <xs:restriction base="xs:string">
  <xs:pattern value="[a-fA-F0-9]{8}-[a-fA-F0-9]{4}-4[a-fA-F0-9]{3}-[89abAB][a-fA-F0-9]{3}-[a-fA-F0-9]{12}"/> 
<!-- [...] -->	 
```

As an example, a distress event upload message would read as follows:

```xml
<ladr:LadrApplicationMessage>
 <ladr:metadata>
  <ladr:identifier codeSpace="urn:uuid">eca23996-df29-4ee2-a886-bf8f150a75a4</ladr:identifier>
  <ladr:type>DISTRESS_EVENT_UPLOAD_MESSAGE</ladr:type>	 
```

and the corresponding distress event upload validation message informing about the validity of the previous message would read as follows:

```xml
<ladr:LadrApplicationMessage>
 <ladr:metadata>
  <ladr:identifier codeSpace="urn:uuid">7f79eaf3-3053-48b6-b699-293d54ee0d67</ladr:identifier>	 
  <ladr:type>DISTRESS_EVENT_UPLOAD_VALIDATION_MESSAGE</ladr:type>	 
  <ladr:referencedMessageIdentifier codeSpace="urn:uuid">eca23996-df29-4ee2-a886-bf8f150a75a4</ladr:referencedMessageIdentifier>	 
```

### Resolution

> TODO

---

## Contributor Code    

### Requirement (from ICAO Doc 10150)

|Field|Format|LADR functionality|Example|
|:-|:-|:-|:-|
|Contributor Code|NNN|Establish contributor domain for data validation.| 001 |

The pattern NNN is currently enforced in the LADR schemas as follows.

```xml
<xs:simpleType name="ContributorCodeType">
  <xs:restriction base="xs:string">
    <xs:pattern value="([0-9]){3}"/>
  </xs:restriction>
</xs:simpleType>
```

### To be discussed

In some prototype's samples, Contributor Code is sometimes set to `USMCC`. This value does not match the format specified by Doc 10150 / does not validate.

```xml
<!-- Raw text 352362 1 27925787A2BFDFF 2022-01-19 13:00:05 -->
<LadrMessage>
  <contributor>
    <identifier>USMCC</identifier>      
```

What would be the right pattern for this field? Should this code be normalized?

Note: In principle, EUROCONTROL will foresee a process to connect contributors to the LADR.


### Resolution

> TODO

---

## Data source

### Requirement (from ICAO Doc 10150)

|Field|Format|LADR functionality|Example|
|:-|:-|:-|:-|
|Data source|TTTTTT|Enable identification of the source of data (manufacturerm type of ADT)| GCP-01 |

The pattern TTTTTT is currently enforced in the LADR schemas as follows.

```xml
<xs:simpleType name="DataSourceType">
  <xs:restriction base="xs:string">
	  <xs:pattern value="([A-Z]|[0-9]){6}"/>
  </xs:restriction>
</xs:simpleType>
```

### To be discussed

In some prototype's samples, data source is sometimes set to `SPMCC`, i.e. 5 characters. This value does not match the format specified by Doc 10150 / does not validate.

```xml
<ladr:LadrMessage>
  <ladr:flight>
    <fx:emergency>
      <ladr:dataSource>SPMCC</ladr:dataSource>
```

What would be the right pattern for this field?

### Resolution

> TODO

---

## ADT activation method

### Requirement (from ICAO Doc 10150)

|Field|Format|LADR functionality|Example|
|:-|:-|:-|:-|
|ADT activation method|TTTTTT|Defined code which indicated if the activation was manual or automatic, and what parameter exceedance triggered the automatic activation, if applicable. Also incorporates cancellation message to provide information when the ADT system no longer transmits due to the activating condition no longer being fulfilled.|431832|

The pattern TTTTTT is currently enforced in the LADR schemas as follows.

```xml
<xs:simpleType name="AdtActivationMethodType">
	<xs:restriction base="xs:string">
		<xs:pattern value="([A-Z]|[0-9]){6}"/>
	</xs:restriction>
</xs:simpleType>
```
### To be discussed

Looking at the examples provided by Doc 10150, should the format rather be NNNNNN (i.e. six numeric characters) ?

|ADT Activator|
|:-|
|431401|
|431832|
|431100|
|531604|
|401300|

In some prototype's samples, 
- ADT Activation Method is sometimes set to `MANUAL`. This value matches the pattern, but is this truly a value that is expected/allowed?

```xml
<ladr:LadrMessage>
  <ladr:flight>
    <fx:emergency>
      <ladr:dataSource>SPMCC</ladr:dataSource>
      <ladr:ADTActivationMethod>MANUAL</ladr:ADTActivationMethod>
```

- ADT Activation Method is sometimes set to `AUTOMATIC-A`. This value does not match the pattern / does not validate.

```xml
<ladr:LadrMessage>
  <ladr:flight>
    <fx:emergency>
      <ladr:dataSource>SPMCC</ladr:dataSource>
      <ladr:ADTActivationMethod>AUTOMATIC-A</ladr:ADTActivationMethod>
```

What would be the right pattern for this field: TTTTTT, NNNNNN, a mix... ? Any need for predefined values?

### Resolution

> TODO

---

## Time at position

### Requirement
ICAO Doc10150 includes the mandatory data element “Date and Time of transmission”.
This data is expected to be provided by the contributor on input to the LADR.

### To be discussed
Some samples of LADR input data provide a “timeAtPosition” element but this one has no counterpart in the Doc10150 data elements. In FIXM time at a position is part of the 4D point and technically is not the transmission time. The preferred approach would be to disambiguate this to avoid a semantic drift. 

#### Initial feed-back
 According to A.002, there are three times associated with a LADR message:
•	LADR Reception Time (when the LADR receive the message)
•	Alert Detection Time (MF#14 or MF#14b)
•	Transmission Time to the LADR

The time that uses the tag <fx:timeAtPosition> seems to be the time defined as bcnTranDateTime at C/S A.002 Table C.6, as it coincides with the TCA or Detect Time. 

### Resolution
TO BE DONE

---

## Time of receipt

### Requirement
ICAO Doc10150 includes the mandatory data element “Date and Time of receipt”.
This data is expected to be provided by the LADR on message reception.

### To be discussed
Some samples of LADR input data include “date and time of receipt” element which is unexpected unless this means something else (eg time of receipt by the MCC)

### Initial feed-back
According to A.002, there are three times associated with a LADR message:
•	LADR Reception Time (when the LADR receive the message)
•	Alert Detection Time (MF#14 or MF#14b)
•	Transmission Time to the LADR

The time that uses the tag <ladr:timestamp>, theory it should be the time defined as messageDateTime at C/S A.002, as the time at which the MCC sends the message to the LADR, but it seems to coincide with the time at which the message was received from the LUT. However, it is not clear to us that a tag starting with “ladr:“ the MCC has to write something, because it seems a tag oriented to be written by the LADR.

### Resolution
TO BE DONE

