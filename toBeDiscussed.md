# Topics to be discussed

The topics listed include a record of discussions and potential changes to the message schemas. These changes will be implemented at a later stage. Once implemented Eurocontrol will notify. As a consequence the current schemas do not reflect the potential changes mentioned.

## Message identifier

### Requirement

ICAO doc 10150 does not require the messages sent to or from LADR to have an identifier. However, it may be useful that the exchanged LADR messages have one, for tracing purposes, and/or so that a response message can indicate the message to which it is responding.

Note: The ICAO FF-ICE concept has a requirement along these lines.

### At which level should message identification be captured?

Message identification is on a borderline between the information layer and the technical infrastructure layer, and different aspects need to be considered for dealing with the question, such as the following:

- Messaging protocols come with their native model for serialised messages which commonly defines message identification structures. See for instance AMQP 1.0 message model at http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-messaging-v1.0-os.html#section-message-format . Message identification may be therefore supported natively by the selected messaging protocol that will support the exchanges. On the other hand, messaging protocols may define an internal logic whereby the information being sent is actually split in pieces and exchanged using multiple serialised messages. In this case, the message identification scheme of the protocol protocol may not be fit for purpose for identifiying an application message.
- The type of message exchange pattern being considered. If a synchronous R/R pattern is considered, the reply is always de facto correlated with the request. In this case, there may be no need to specify a means to cross-reference application messages.
- ...


### To be discussed

> Assuming the message identification is supported at the level of the LADR Application Message... 

In some prototype's samples, some LADR messages had a message number, expressed as an unsigned integer. It is understood that this field corresponds to the field MESSAGE NUMBER as standardised in the MCC standard interface. As LADR will centralise DistressEventUplaodMessages sent from multiple accredited LADR contributors, the risk exists of potential collisions between message identifiers sent by different sources (for instance two distinct LADR contributors using the same value "12345" for identifying to two distinct messages sent to the LADR). 

To mitigate the risk, the use of UUIDs may be considered for identifying LADR messages. UUIDs are 128 bits values enabling distributed systems to uniquely identify something without significant central coordination and with confidence that the same identifier will never be unintentionally used by anyone for anything else. A UUID is meant to be universally unique, i.e. the probably of a duplicate identifier being generated within the IT universe is negligible for the foreseeable future. UUIDs are defined in IETF RFC 4122 and ISO/IEC 9834-8, the latter specifying robust UUID generation procedures.

The ISO/IEC 9834 is already used in an Aviation/ATM context, with AIXM and FIXM using UUID v4.0 for uniquely identifying their respective features of interest. The FF-ICE message identifiers are also ISO UUID v4.

An example of UUID v4 reads as follows: `282123e9-958a-4e99-a007-a0d838a148f0`

Off-the-shelf libraries exist that support the generation of UUID v4. Online UUID v4 generation services also exist that can be leveraged (an example can be found at https://www.uuidgenerator.net/version4).

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

**28/04/2022: LADR FIXM message development – meeting #2:**
- Security of the off-the-shelf libraries dealing with UUID generation need to be carefully assessed
- Usual programming languages (C#, .Net, Java, ...) have these libraries
- There could be other message identification schemes that could be envisaged such a message id involving e.g. "MCC name + message number + timestamp"
- For now, keep UUID as the way forward. Action on LADR contributors to crosscheck the feasibility of the proposal, and to propose alternatives should the UUID prove unsatisfactory.

**Envisaged changes to the schemas following meeting on 28/04**
- no change

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

In principle, EUROCONTROL will foresee a process to connect contributors to the LADR.

How is the contributor code managed? Is it provided by ICAO, the LADR or by the contributors? When provided by the LADR it could be part of the connection process to the LADR. But it could also be a list of codes provided by ICAO (similar to Doc 8585). From a LADR point of view it needs to be an identifier of the contributing organisation  enabling to lookup specific data associated to the contributor such as ADT activitation methods.

### Resolution

**28/04/2022: LADR FIXM message development – meeting #2:**
- the contributor codes could be assigned by ICAO once accreditation is confirmed - similar to Doc 8585 approach

**Envisaged changes to the schemas following meeting on 28/04**
- no change

---

## Data source

### Requirement (from ICAO Doc 10150)

|Field|Format|LADR functionality|Example|
|:-|:-|:-|:-|
|Data source|TTTTTT|Enable identification of the source of data (manufacturer, type of ADT)| GCP-01 |

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

**28/04/2022: LADR FIXM message development – meeting #2:**
- consider renaming the property to `typeOfAdt` - possible values: `ELT-DT`, `...` 
- if `ELT-DT` is provided, then the hexID shall perhaps be provided => could be a business rule checked by LADR - TBC
- Ian will check with manufacturers what other values could be exchanged.
- Would there be a need to exchange as well the manufacturer of the ELT-DT?

**Envisaged changes to the schemas following meeting on 28/04**
- Property DistressEvent.dataSource to be renamed to `DistressEvent.typeOfAdt`
- Datatype DataSourceType to be renamed to `TypeOfAdtChoiceType`, and modelled as a choice between a set of predefined enumerated values (ELT-DT, or possibly other values => ref action on Ian) or free text.

```xml
<xs:complexType name="DistressEventType">
  <!-- ... -->
  <xs:element name="typeOfAdt" type="ladr:TypeOfAdtChoiceType"/>	
  <!-- ... -->
</xs:complexType>

<xs:complexType name="TypeOfAdtChoiceType">
  <xs:choice>
    <xs:element name="type" type="ladr:TypeOfAdtType"/>
    <xs:element name="other" type="xs:string"/>
  </xs:choice>
<xs:complexType>
	
<xs:simpleType name="TypeOfAdtType">
  <xs:restriction base="xs:string">
    <xs:enumeration value="ELT-DT"/>
    <xs:enumeration value="..."/>
  </xs:restriction>
</xs:simpleType>
```

An example of resulting XML encoding would look like this:

```xml
<ladr:distressEvent>
  <ladr:typeOfAdt>
    <ladr:type>ELT-DT</ladr:type>
  </ladr:typeOfAdt>
  <!-- ... -->	
```

**17/05/2022: LADR FIXM message development – meeting #3:**
- proposal to add a “VersionNumber” field.

**Envisaged changes to the schemas following meeting on 17/05**
- add data element to capture VersionNumber of ADT type in `Distress Event Upload Message`
- example `TypeOfAdt` : GCP-01, GCP-02 and `TypeAdtVersion` : 001 (sequence)

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

What would be the right pattern for this field: TTTTTT, NNNNNN, a mix... ? Any need for predefined values? This then should/could be a codelist(s) to be defined for harmonisation purposes?

### Resolution

28/04/2022: LADR FIXM message development – meeting #2:
- probably formed of six numbers
- no need to standardise. Look-up tables would be made available by contributors to enable the interpretation of the code.

**Envisaged changes to the schemas following meeting on 28/04**
- The pattern of type `AdtActivationMethodType` to be changed to `([0-9]){6}` 

```xml
<xs:simpleType name="AdtActivationMethodType">
  <xs:restriction base="xs:string">
    <xs:pattern value="([0-9]){6}"/>
  </xs:restriction>
</xs:simpleType>
```

**17/05/2022: LADR FIXM message development – meeting #3:**
- continued discussion on need to standardise meanings of activation codes
- further build up resolution as part of message sample creation work

**Envisaged changes to the schemas following meeting on 17/05**
- No change

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

**28/04/2022: LADR FIXM message development – meeting #2:**
- what is wanted is the position of the aircraft and the time at which the aircraft was at the position
- this means that the fixm property can be rightfully reused

**Envisaged changes to the schemas following meeting on 28/04**
- The restricted subset of FIXM CORE defined for the DistressEventUploadMessage will include FIXM CORE property `fx:LastPositionReportType.timeAtPosition` 

---

## Time of receipt

### Requirement
ICAO Doc10150 includes the mandatory data element “Date and Time of receipt”.
This data is expected to be provided by the LADR on message reception.

### To be discussed
Some samples of LADR input data include “date and time of receipt” element which is unexpected unless this means something else (eg time of receipt by the MCC)

#### Initial feed-back
According to A.002, there are three times associated with a LADR message:
•	LADR Reception Time (when the LADR receive the message)
•	Alert Detection Time (MF#14 or MF#14b)
•	Transmission Time to the LADR

The time that uses the tag <ladr:timestamp>, theory it should be the time defined as messageDateTime at C/S A.002, as the time at which the MCC sends the message to the LADR, but it seems to coincide with the time at which the message was received from the LUT. However, it is not clear to us that a tag starting with “ladr:“ the MCC has to write something, because it seems a tag oriented to be written by the LADR.

### Resolution

**28/04/2022: LADR FIXM message development – meeting #2:**
- this is the time the LADR picked this info up
- no need to exchange the info on input to LADR => will not be part of the the DistressEventUploadMessage

**Envisaged changes to the schemas following meeting on 28/04**
- No change needed. This data element is already absent from the DistressEventUploadMessage schemas
 
---

## Device Identifier: 

### To be discussed
An alphanumeric text string pertaining to the specific ADT device activated.  For ELT(DT)s this would be a hexadecimal entry of length 15 or 23 characters (FGB and SGB respectively), but could be limited to 15 characters if preferable.  Not only is this a critical element in the C/S arena, it is the identifier often used by RCCs and links to registration databases with additional owner/beacon information.  

### Clarification: 
Difference with “data source” and “Emergency locator transmitter (ELT) Hex ID” from Doc10150 ?

### Resolution:

**28/04/2022: LADR FIXM message development – meeting #2:**
- allow property `carriedELDHexId` to be either 15 or 23 characters
- From a LADR perspective, there is for now no need to get two fields covering both legacy ELT and new ELT-DT. Only the hexId of the ELT-DT that has generated the position info is wanted.

**Envisaged changes to the schemas following meeting on 28/04**
- The pattern of type `LadrEltHexIdType` is to be changed to `([A-Z]|[0-9]){15}|([A-Z]|[0-9]){23}` 

```xml
<xs:simpleType name="LadrEltHexIdType">
  <xs:restriction base="xs:string">
    <xs:pattern value="([A-Z]|[0-9]){15}|([A-Z]|[0-9]){23}"/>
  </xs:restriction>
</xs:simpleType>
```
---

## Cancelation of distress:

### To be discussed:
Overall approach to be discussed: data element? / part of upload message? / needed in the beginning?

### Resolution:

28/04/2022: LADR FIXM message development – meeting #2:
- The cancellation of distress by a LADR Contributor is expected to be an aspect covered by the `ADT Activation Method`. 

**Envisaged changes to the schemas following meeting on 28/04**
- no change

---

## Data completeness handling: 

### To be discussed: 
Confirmation of the approach to handle mandatory versus optional data elements in relation to LADR validation and logging:
all mandatory fields need to be provided and cannot be nilled.

### Resolution:

**28/04/2022: LADR FIXM message development – meeting #2:**
- There is a possibility that the data declared mandatory is actually not available because the beacon goes off. A schema that is too restrictive would make it impossible fot the LADR message to be sent. 
- Current way forward: elements can not be nilled. This may be reconsidered as appropriate if the point above needs attention. Should nillability be restored, use cases for it should be carefully described and mutually agreed on.

**Envisaged changes to the schemas following meeting on 28/04**
- no change

---

## Same contributor sending same position multiple times: 

### To be discussed: 
Is it possible that the LADR will receive the same ADT position information (sent by one aircraft) multiple times from MCCs belonging to the same distress tracking organisation? Eg when the distress event is picked up by more than one MCC?

The LADR assumes:
- that the UUID of the messages sent by the different MCCs of the same distress tracking organisation will be different;
- that all messages will be kept.

### Resolution:

**17/05/2022: LADR FIXM message development – meeting #3:**
- the group confirmed the eventuality that this can happen.
- the group confirmed the LADR assumptions stated above.

**Envisaged changes to the schemas following meeting on 17/05**
- no change

---

## Upload message requiring operational validation information: 

### To be discussed: 
When an AOC is sending LADR upload messages could it include operational validation information? Should the information than be foreseen on upload to LADR?

The LADR assumes that the sending of the distres event data and the operational validation of the distress event will be done using two distinct messages: the `Distress Event Upload Message` and the `Distress Event Validation Message`. This means that no operational validation info will be provided as part of the `Distress Event Upload Message`.

### Resolution:

**17/05/2022: LADR FIXM message development – meeting #3:**
- the group confirmed the current approach in terms of separation of this information between event data uploading and subsequent operational validation.
- the group confirmed that capturing "confirmation of genuine distress event" information is not part of the `Distress Event Upload Message`

**Envisaged changes to the schemas following meeting on 17/05**
- no change

---

## Encoding optional data: 

### To be discussed: 
The encoding of optional data needs to be verified. Possible clarifications need to be discussed.

Fields:
- horizontalAccuracy;
- groundSpeed;
- other?

### Resolution:

**13/06/2022: LADR FIXM message development – meeting #4:**
- the group discussed the approach in providing horizontal accuracy as a distance measure accompanied with a UOM;
- it was confirmed that the schema of the upload message caters for the capability to encode the information;
- the location on gitHUB with additional relevant encoding information was shared with the group:
- https://github.com/eurocontrol-ladr/ladrApplicationMessages/tree/main/schemas/distressEventUploadMessage/base
- contributors where asked to encode some additional samples accordingly;
- regarding groundSpeed a similar discussion occured with a similar clarification and resolution outlook.

**Envisaged changes to the schemas following meeting on 13/06**
- NONE

---

## Precision of date time values

The current schemas allow for date time to be exchanged with a precision down to milliseconds. In all XML samples received so far, data times were provided with a precision limited to whole seconds, for both message timestamps and ‘times at position’. Examples:

```xml
<ladr:timestamp>2021-06-24T16:20:32Z</ladr:timestamp>
<fx:timeAtPosition>2021-06-24T16:20:25Z</fx:timeAtPosition>
```

The topic of date time precisions was discussed recently within the FIXM project. It was concluded that:
- For aviation times, precision should be limited to whole seconds;
- For message timestamps, milliseconds should still allowed, because this precisions can still be usefull e.g. in computer logs
These rules on date time precision will actually be enforced in the future FIXM version.

Proposal for LADR: apply the same rules.

### Resolution:

**13/06/2022: LADR FIXM message development – meeting #4:**
- the group rejected the proposal and confirmed that there is no need for a data capture rule for message timestamps to be set to milliseconds;
- the current contributor practice as per the samples will be continued; in the event of issues on the LADR side this will be discussed later.

**Envisaged changes to the schemas following meeting on 13/06**
- NONE

## Precision of lat/long values

Similarly, which precision (number of digit) would be right for latitude/longitude
- minimum 3 digits (would be worth up to 110m) ?
- maximum 5 digits (would be worth up to 1.1m) ?

Examples from received samples:

```xml
<fb:pos>39.449 -77.010</fb:pos>
<fb:pos>38.99896 -76.85370</fb:pos>
<fb:pos>12.041 100.0211</fb:pos>
```
### Resolution:

**13/06/2022: LADR FIXM message development – meeting #4:**
- the group confirmed that there is no need for any data capture rule;
- the precision shall be sufficient in correlation to the horizontal accuracy.

**Envisaged changes to the schemas following meeting on 13/06**
- NONE

## horizontalAccuracy UOM preference

Should horizontal accuracy be calculated to a preferred UOM (M, KM)?

### Resolution:

**11/07/2022: LADR FIXM message development – meeting #5:**
- TBD

**Envisaged changes to the schemas following meeting on 11/07**
- TBD

## Dilution of precision (DOP)

Is dilution DOP information needed in relation to accuracy data?
Accuracy in satellite based positioning is variable. DOP is a factor that quantifies error propagation using equations bringing into account horizontal, vertical, time, geometrical and position variability factors.

DOP is a classification:

DOP <1	-> Ideal: Highest possible confidence level to be used for applications demanding the highest possible precision at all times.
DOP 1-2	-> Excellent: At this confidence level, positional measurements are considered accurate enough to meet all but the most sensitive applications.
DO 2-5	-> Good: Represents a level that marks the minimum appropriate for making accurate decisions. Positional measurements could be used to make reliable in-route navigation suggestions to the user.
DOP 5-10 -> Moderate: Positional measurements could be used for calculations, but the fix quality could still be improved. A more open view of the sky is recommended.
DOP 10-20 -> Fair: Represents a low confidence level. Positional measurements should be discarded or used only to indicate a very rough estimate of the current location.
DOP >20 -> Poor: At this level, measurements are inaccurate by as much as 300 meters with a 6-meter accurate device (50 DOP × 6 meters) and should be discarded.

### Resolution:

**11/07/2022: LADR FIXM message development – meeting #5:**
- TBD

**Envisaged changes to the schemas following meeting on 11/07**
- TBD
