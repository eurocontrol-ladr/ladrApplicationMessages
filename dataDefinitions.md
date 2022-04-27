## Data Definitions

| Model Element | Description |	
| :-  | :-------------------- |
| DistressEvent.<br>**adtActivationMethod** | `FROM ICAO DOC 10150`<br>Defined code which indicated if the activation was manual or automatic, and what parameter exceedance triggered the automatic activation, if applicable. Also incorporates cancellation message to provide information when the ADT system no longer transmits due to the activating condition no longer being fulfilled.<br><br>`DEFINITION IN CURRENT XSD SCHEMA`<br>A defined code which indicates if the activation was manual or automatic, and what parameter exceedance triggered the automatic activation, if applicable. The code also incorporates cancellation information to indicate when the ADT system no longer transmits due to the activating condition no longer being fulfilled.<br><br>`CONSOLIDATED DEFINITION`<br>TODO |
| DistressEvent.<br>**contributorCode** | `DEFINITION IN CURRENT XSD SCHEMA`<br>A code identifying [the LADR contributor ???], which enables to establish contributor domain for data validation. |
| DistressEvent.<br>**dataSource** | `DEFINITION IN CURRENT XSD SCHEMA`<br>A code enabling the identification of the source of the autonomous distress tracking data. |
| DistressEvent.<br>**flight** | `DEFINITION IN CURRENT XSD SCHEMA`<br>The flight in distress. |
| DistressEvent.<br>**identifier** | `DEFINITION IN CURRENT XSD SCHEMA`<br>The identifier of the distress event. |
| LadrAltitude.<br>**altitudeSource** | `DEFINITION IN CURRENT XSD SCHEMA`<br>The source of the altitude. |
| LadrApplicationMessageMetadata.<br>**identifier** | `DEFINITION IN CURRENT XSD SCHEMA`<br>The identifier of the message. |
| LadrApplicationMessageMetadata.<br>**receiptTime** | `DEFINITION IN CURRENT XSD SCHEMA`<br>Date and time of receipt. |
| LadrApplicationMessageMetadata.<br>**referencedMessageIdentifier** | `DEFINITION IN CURRENT XSD SCHEMA`<br>The identifier of the LADR Application Message that this message refers to. |
| LadrApplicationMessageMetadata.<br>**timestamp** | `DEFINITION IN CURRENT XSD SCHEMA`<br>The time and date that the communication was sent. |
| LadrApplicationMessageMetadata.<br>**tunv** | `DEFINITION IN CURRENT XSD SCHEMA`<br>Date and time at which the LADR system has logged the distress event upload message as invalid. |
| LadrApplicationMessageMetadata.<br>**tval** | `DEFINITION IN CURRENT XSD SCHEMA`<br>Date and time at which the LADR system has validated the distress event upload message against its schema. |
| LadrApplicationMessageMetadata.<br>**type** | `DEFINITION IN CURRENT XSD SCHEMA`<br>The type of LADR Application Message. |
| LadrApplicationMessageMetadata.<br>**validationStatus** | `DEFINITION IN CURRENT XSD SCHEMA`<br>A code indicating whether the LADR Application Message has successfully passed schema validation. |
| LadrApplicationMessage.<br>**distressEvent** | `DEFINITION IN CURRENT XSD SCHEMA`<br>The distress event that the LADR application message is about. |
| LadrApplicationMessage.<br>**metadata** | `DEFINITION IN CURRENT XSD SCHEMA`<br>Information about the LADR application message. |
| LadrLastPositionReportExtension.<br>**altitude** | `DEFINITION IN CURRENT XSD SCHEMA`<br>The vertical distance, at the last known position, of the aircraft considered as a point, measured from mean Sea level (MSL). |
| LadrLastPositionReportExtension.<br>**groundSpeed** | `DEFINITION IN CURRENT XSD SCHEMA`<br>The speed of the aircraft relative to the surface of the earth at the last known position. |
| LadrLastPositionReportExtension.<br>**heading** | `DEFINITION IN CURRENT XSD SCHEMA`<br>The direction, at the last known position, in which the longitudinal axis of the aircraft was pointed. |
| LadrLastPositionReportExtension.<br>**horizontalAccuracy** | `DEFINITION IN CURRENT XSD SCHEMA`<br>The difference between the horizontal coordinates of the aircraft measured by the ADT system and its true position referenced to the same geodetic datum expressed as a circular error at 95 percent probability. |
| LadrSurvivalCapabilitiesExtension.<br>**carriedEltHexId** | `DEFINITION IN CURRENT XSD SCHEMA`<br>The identifier of an emergency locator transmitter carried by aircraft. |
