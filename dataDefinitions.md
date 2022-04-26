## Definitions

| Model Element | Description |	
| :-  | :-------------------- |
| DistressEvent.**adtActivationMethod** | `DESCRIPTION`<br>A defined code which indicates if the activation was manual or automatic, and what parameter exceedance triggered the automatic activation, if applicable. The code also incorporates cancellation information to indicate when the ADT system no longer transmits due to the activating condition no longer being fulfilled. |
| DistressEvent.**contributorCode** | `DESCRIPTION`<br>A code identifying [the LADR contributor ???], which enables to establish contributor domain for data validation. |
| DistressEvent.**dataSource** | `DESCRIPTION`<br>A code enabling the identification of the source of the autonomous distress tracking data. |
| DistressEvent.**flight** | `DESCRIPTION`<br>The flight in distress. |
| DistressEvent.**identifier** | `DESCRIPTION`<br>The identifier of the distress event. |
| LadrAltitude.**altitudeSource** | `DESCRIPTION`<br>The source of the altitude. |
| LadrApplicationMessageMetadata.**identifier** | `DESCRIPTION`<br>The identifier of the message. |
| LadrApplicationMessageMetadata.**receiptTime** | `DESCRIPTION`<br>Date and time of receipt. |
| LadrApplicationMessageMetadata.**referencedMessageIdentifier** | `DESCRIPTION`<br>The identifier of the LADR Application Message that this message refers to. |
| LadrApplicationMessageMetadata.**timestamp** | `DESCRIPTION`<br>The time and date that the communication was sent. |
| LadrApplicationMessageMetadata.**tunv** | `DESCRIPTION`<br>Date and time at which the LADR system has logged the distress event upload message as invalid. |
| LadrApplicationMessageMetadata.**tval** | `DESCRIPTION`<br>Date and time at which the LADR system has validated the distress event upload message against its schema. |
| LadrApplicationMessageMetadata.**type** | `DESCRIPTION`<br>The type of LADR Application Message. |
| LadrApplicationMessageMetadata.**validationStatus** | `DESCRIPTION`<br>A code indicating whether the LADR Application Message has successfully passed schema validation. |
| LadrApplicationMessage.**distressEvent** | `DESCRIPTION`<br>The distress event that the LADR application message is about. |
| LadrApplicationMessage.**metadata** | `DESCRIPTION`<br>Information about the LADR application message. |
| LadrLastPositionReportExtension.**altitude** | `DESCRIPTION`<br>The vertical distance, at the last known position, of the aircraft considered as a point, measured from mean Sea level (MSL). |
| LadrLastPositionReportExtension.**groundSpeed** | `DESCRIPTION`<br>The speed of the aircraft relative to the surface of the earth at the last known position. |
| LadrLastPositionReportExtension.**heading** | `DESCRIPTION`<br>The direction, at the last known position, in which the longitudinal axis of the aircraft was pointed. |
| LadrLastPositionReportExtension.**horizontalAccuracy** | `DESCRIPTION`<br>The difference between the horizontal coordinates of the aircraft measured by the ADT system and its true position referenced to the same geodetic datum expressed as a circular error at 95 percent probability. |
| LadrSurvivalCapabilitiesExtension.**carriedEltHexId** | `DESCRIPTION`<br>The identifier of an emergency locator transmitter carried by aircraft. |
