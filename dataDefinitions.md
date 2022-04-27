## Data Definitions

| Model Element | Description |	
| :-  | :-------------------- |
| DistressEvent.<br>**adtActivationMethod** | `FROM ICAO DOC 10150`<br>Defined code which indicated if the activation was manual or automatic, and what parameter exceedance triggered the automatic activation, if applicable. Also incorporates cancellation message to provide information when the ADT system no longer transmits due to the activating condition no longer being fulfilled.<br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>A defined code which indicates if the activation was manual or automatic, and what parameter exceedance triggered the automatic activation, if applicable. The code also incorporates cancellation information to indicate when the ADT system no longer transmits due to the activating condition no longer being fulfilled.<br><br>`CONSOLIDATED DEFINITION`<br>TODO |
| DistressEvent.<br>**contributorCode** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>A code identifying [the LADR contributor ???], which enables to establish contributor domain for data validation. |
| DistressEvent.<br>**dataSource** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>A code enabling the identification of the source of the autonomous distress tracking data. |
| DistressEvent.<br>**identifier** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>The identifier of the distress event. |
| LadrAltitude.<br>**altitudeSource** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>The source of the altitude. |
| LadrApplicationMessageMetadata.<br>**identifier** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>The identifier of the message. |
| LadrApplicationMessageMetadata.<br>**receiptTime** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>Date and time of receipt. |
| LadrApplicationMessageMetadata.<br>**referencedMessageIdentifier** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>The identifier of the LADR Application Message that this message refers to. |
| LadrApplicationMessageMetadata.<br>**timestamp** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>The time and date that the communication was sent. |
| LadrApplicationMessageMetadata.<br>**tunv** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>Date and time at which the LADR system has logged the distress event upload message as invalid. |
| LadrApplicationMessageMetadata.<br>**tval** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>Date and time at which the LADR system has validated the distress event upload message against its schema. |
| LadrApplicationMessageMetadata.<br>**type** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>The type of LADR Application Message. |
| LadrApplicationMessageMetadata.<br>**validationStatus** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>A code indicating whether the LADR Application Message has successfully passed schema validation. |
| LadrLastPositionReportExtension.<br>**altitude** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>The vertical distance, at the last known position, of the aircraft considered as a point, measured from mean Sea level (MSL). |
| LadrLastPositionReportExtension.<br>**groundSpeed** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>The speed of the aircraft relative to the surface of the earth at the last known position. |
| LadrLastPositionReportExtension.<br>**heading** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>The direction, at the last known position, in which the longitudinal axis of the aircraft was pointed. |
| LadrLastPositionReportExtension.<br>**horizontalAccuracy** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>The difference between the horizontal coordinates of the aircraft measured by the ADT system and its true position referenced to the same geodetic datum expressed as a circular error at 95 percent probability. |
| LadrSurvivalCapabilitiesExtension.<br>**carriedEltHexId** | `FROM ICAO DOC 10150`<br><br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>The identifier of an emergency locator transmitter carried by aircraft. |
