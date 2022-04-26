## Definitions

| Model Element | Description |	
| :-  | :-------------------- |
| DistressEvent.<br>**adtActivationMethod** | `DESCRIPTION`<br>A defined code which indicates if the activation was manual or automatic, and what parameter exceedance triggered the automatic activation, if applicable. The code also incorporates cancellation information to indicate when the ADT system no longer transmits due to the activating condition no longer being fulfilled. |
| DistressEvent.<br>**contributorCode** | `DESCRIPTION`<br>A code identifying [the LADR contributor ???], which enables to establish contributor domain for data validation. |
| DistressEvent.<br>**dataSource** | `DESCRIPTION`<br>A code enabling the identification of the source of the autonomous distress tracking data. |
| DistressEvent.<br>**flight** | `DESCRIPTION`<br>The flight in distress. |
| DistressEvent.<br>**identifier** | `DESCRIPTION`<br>The identifier of the distress event. |
| LadrAltitude.<br>**altitudeSource** | `DESCRIPTION`<br>The source of the altitude. |
| LadrApplicationMessageMetadata.<br>**identifier** | `DESCRIPTION`<br>The identifier of the message. |
| LadrApplicationMessageMetadata.<br>**receiptTime** | `DESCRIPTION`<br>Date and time of receipt. |
| LadrApplicationMessageMetadata.<br>**referencedMessageIdentifier** | `DESCRIPTION`<br>The identifier of the LADR Application Message that this message refers to. |
| LadrApplicationMessageMetadata.<br>**timestamp** | `DESCRIPTION`<br>The time and date that the communication was sent. |
| LadrApplicationMessageMetadata.<br>**tunv** | `DESCRIPTION`<br>Date and time at which the LADR system has logged the distress event upload message as invalid. |
| LadrApplicationMessageMetadata.<br>**tval** | `DESCRIPTION`<br>Date and time at which the LADR system has validated the distress event upload message against its schema. |
| LadrApplicationMessageMetadata.<br>**type** | `DESCRIPTION`<br>The type of LADR Application Message. |
| LadrApplicationMessageMetadata.<br>**validationStatus** | `DESCRIPTION`<br>A code indicating whether the LADR Application Message has successfully passed schema validation. |
| LadrApplicationMessage.<br>**distressEvent** | `DESCRIPTION`<br>The distress event that the LADR application message is about. |
| LadrApplicationMessage.<br>**metadata** | `DESCRIPTION`<br>Information about the LADR application message. |
| LadrLastPositionReportExtension.<br>**altitude** | `DESCRIPTION`<br>The vertical distance, at the last known position, of the aircraft considered as a point, measured from mean Sea level (MSL). |
| LadrLastPositionReportExtension.<br>**groundSpeed** | `DESCRIPTION`<br>The speed of the aircraft relative to the surface of the earth at the last known position. |
| LadrLastPositionReportExtension.<br>**heading** | `DESCRIPTION`<br>The direction, at the last known position, in which the longitudinal axis of the aircraft was pointed. |
| LadrLastPositionReportExtension.<br>**horizontalAccuracy** | `DESCRIPTION`<br>The difference between the horizontal coordinates of the aircraft measured by the ADT system and its true position referenced to the same geodetic datum expressed as a circular error at 95 percent probability. |
| LadrSurvivalCapabilitiesExtension.<br>**carriedEltHexId** | `DESCRIPTION`<br>The identifier of an emergency locator transmitter carried by aircraft. |
