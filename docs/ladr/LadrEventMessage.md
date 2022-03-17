# LADR Event Message

```mermaid
    sequenceDiagram
    autonumber
    actor Contributor
    participant LADR
    actor User
    Contributor->>LADR: LADR Event Upload Message
    LADR->>Contributor: LADR Event Upload Validation Message
    LADR->> User: LADR Event Notification Message
    rect rgb(255, 255, 0)    
    LADR->>User: LADR Event Message 
    end
    User->>LADR: Event Notification Acknowledgment Message       
    User->>LADR: Event Validation Message
```

## XML schema and samples

- Go to [Schema definition](https://github.com/hlepori/test_ladr/tree/main/schemas/ladrEventMessage) on Github

- Go to [XML examples](https://github.com/hlepori/test_ladr/tree/main/samples) on Github

## UML description

```mermaid
classDiagram
class LadrMessage	
<<LADR_Application>> LadrMessage	
LadrMessage : + timestamp [1] Time	
class LadrMessageType	
<<LADR_Application>> LadrMessageType	
LadrMessageType : LADR_EVENT_MESSAGE	
LadrMessageType <-- LadrMessage : +type [1]	
LadrMessage : + uniqueMessageIdentifier [1] UniversallyUniqueIdentifier	
LadrMessage : + receiptTime [1] Time	
class Flight	
<<FIXM_Core>> Flight	
class AircraftOperator	
<<FIXM_Core>> AircraftOperator	
AircraftOperator : + designatorIcao [1] AircraftOperatorDesignator	
Flight --> AircraftOperator : +operator [1]	
class Aircraft	
<<FIXM_Core>> Aircraft	
Aircraft : + aircraftAddress [0..1] AircraftAddress	
class FlightCapabilities	
<<FIXM_Core>> FlightCapabilities	
class CommunicationCapabilities	
<<FIXM_Core>> CommunicationCapabilities	
CommunicationCapabilities : + otherCommunicationCapabilities [0..1] CharacterString	
CommunicationCapabilities : + selectiveCallingCode [0..1] SelectiveCallingCode	
FlightCapabilities --> CommunicationCapabilities : +communication [0..1]	
class SurvivalCapabilities	
<<FIXM_Core>> SurvivalCapabilities	
class SurvivalCapabilitiesExtension	
<<FIXM_Core>> SurvivalCapabilitiesExtension	
class LadrSurvivalCapabilitiesExtension	
<<LADR_Application>> LadrSurvivalCapabilitiesExtension	
LadrSurvivalCapabilitiesExtension : + carriedEltHexId [0..*] CharacterString	
SurvivalCapabilitiesExtension<|-- LadrSurvivalCapabilitiesExtension	
SurvivalCapabilities --> SurvivalCapabilitiesExtension : +extension [0..*]	
FlightCapabilities --> SurvivalCapabilities : +survival [0..1]	
Aircraft --> FlightCapabilities : +capabilities [0..1]	
Aircraft : + registration [0..1] AircraftRegistrationList	
Flight --> Aircraft : +aircraft [0..1]	
class FlightEmergency	
<<FIXM_Core>> FlightEmergency	
class LastContact	
<<FIXM_Core>> LastContact	
class LastPositionReport	
<<FIXM_Core>> LastPositionReport	
class SignificantPointChoice	
<<FIXM_Core>> SignificantPointChoice	
class GeographicalPosition	
<<FIXM_Core>> GeographicalPosition	
GeographicalPosition : + pos [1] LatLongPos	
SignificantPointChoice --> GeographicalPosition : +position [1]	
LastPositionReport --> SignificantPointChoice : +position [1]	
class LastPositionReportExtension	
<<FIXM_Core>> LastPositionReportExtension	
class LadrLastPositionReportExtension	
<<LADR_Application>> LadrLastPositionReportExtension	
LadrLastPositionReportExtension : + altitude [1] Altitude	
class Bearing	
<<FIXM_Core>> Bearing	
class ZeroBearingType	
<<FIXM_Core>> ZeroBearingType	
ZeroBearingType : TRUE_NORTH	
ZeroBearingType : MAGNETIC_NORTH	
Bearing --> ZeroBearingType : +zeroBearingType [1]	
LadrLastPositionReportExtension --> Bearing : +heading [0..1]	
class LadrAltitudeSource	
<<LADR_Application>> LadrAltitudeSource	
LadrAltitudeSource : BARO	
LadrAltitudeSource : GNSS	
LadrLastPositionReportExtension --> LadrAltitudeSource : +altitudeSource [1]	
LadrLastPositionReportExtension : + groundSpeed [1] GroundSpeed	
class Distance	
<<FIXM_Core>> Distance	
LadrLastPositionReportExtension --> Distance : +horizontalAccuracy [0..1]	
LastPositionReportExtension<|-- LadrLastPositionReportExtension	
LastPositionReport --> LastPositionReportExtension : +extension [1..*]	
LastContact --> LastPositionReport : +position [1]	
FlightEmergency --> LastContact : +lastContact [1]	
Flight --> FlightEmergency : +emergency [1]	
class FlightIdentification	
<<FIXM_Core>> FlightIdentification	
FlightIdentification : + aircraftIdentification [0..1] AircraftIdentification	
Flight --> FlightIdentification : +flightIdentification [0..1]	
class RouteTrajectoryGroupContainer	
<<FIXM_Core>> RouteTrajectoryGroupContainer	
class RouteTrajectoryGroup	
<<FIXM_Core>> RouteTrajectoryGroup	
class RouteTrajectoryElement	
<<FIXM_Core>> RouteTrajectoryElement	
class TrajectoryPoint4D	
<<FIXM_Core>> TrajectoryPoint4D	
class GeographicalPosition	
<<FIXM_Core>> GeographicalPosition	
GeographicalPosition : + pos [1] LatLongPos	
TrajectoryPoint4D --> GeographicalPosition : +position [1]	
class TrajectoryPointProperty	
<<FIXM_Core>> TrajectoryPointProperty	
TrajectoryPointProperty : + description [0..1] CharacterString	
TrajectoryPoint4D --> TrajectoryPointProperty : +pointProperty [0..*]	
RouteTrajectoryElement --> TrajectoryPoint4D : +point4D [1]	
RouteTrajectoryGroup --> RouteTrajectoryElement : +element [1..*]	
RouteTrajectoryGroupContainer --> RouteTrajectoryGroup : +current [1]	
Flight --> RouteTrajectoryGroupContainer : +routeTrajectoryGroup [1]	
LadrMessage --> Flight : +flight [1]	
class LadrEvent	
<<LADR_Application>> LadrEvent	
class PersonOrOrganization	
<<FIXM_Core>> PersonOrOrganization	
PersonOrOrganization : + identifier [1] CharacterString	
LadrEvent --> PersonOrOrganization : +contributorCode [1]	
LadrEvent : + dataSource [1] CharacterString	
LadrEvent : + adtActivationMethod [1] CharacterString	
LadrEvent : + identifier [1] UniversallyUniqueIdentifier	
class LadrDistressValidationCode	
<<LADR_Application>> LadrDistressValidationCode	
LadrDistressValidationCode : FALSE_ACTIVATION	
LadrDistressValidationCode : GENUINE	
LadrDistressValidationCode : UNDETERMINED	
LadrEvent --> LadrDistressValidationCode : +distressValidationCode [1]	
LadrEvent : + activationCodeInterpretation [0..1] CharacterString	
LadrEvent : + eventTransittingFIRTime [0..1] Time	
LadrEvent : + eventValidatedTime [0..1] Time	
LadrEvent : + eventIssuedTime [0..1] Time	
LadrMessage --> LadrEvent : +ladrEvent [1]	
```
