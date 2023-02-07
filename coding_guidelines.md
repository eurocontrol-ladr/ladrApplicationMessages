# Coding guidelines

## Introduction
TODO

## Date/Time Specification
> Note: The following piece of guidance will eventually  be available on the FIXM User Manual (scheduled for Q1 2023).

FIXM requires times to be expressed in UTC. A constraint is therefore placed on the FIXM classes used to represent date/time values that
imposes the use of the trailing character `Z` to indicate UTC, in line with the W3C XSD specification.

Sub-second precision is considered unneeded for most aviation data, but other fields such as message timestamps 
can still benefit from higher precision date/times.

`FIXM Core 4.2.0` defines only one type for representing date/time values, named `Time`, which employs unrestricted sub-second precision and 
is used for typing indifferently aviation-related times and message timestamps. It is recommended to encode aviation-related times with whole 
second precision only, or with trailing characters `.000Z`. 

```xml
<!--xmlns:fx="http://www.fixm.aero/flight/4.2"-->	
<!-- whole second precision only -->
<fx:timeAtPosition>2021-06-24T16:20:25Z</fx:timeAtPosition>
```
OR 

```xml
<!--xmlns:fx="http://www.fixm.aero/flight/4.2"-->	
<!-- with trailing characters .000Z -->
<fx:timeAtPosition>2021-06-24T16:20:25.000Z</fx:timeAtPosition>
```

Message timestamps can use higher precision, as needed.

```xml
<ladr:timestamp>2021-06-24T16:20:32.132Z</ladr:timestamp>
```

---

> **Note to implementers:** The mapping of the XSD type `dateTime` to native structures in various development contexts is not always 1-1 
> and may exhibit a wide variety of difficulties depending on the tooling and runtime context. In particular, the trailing character `Z` indicating 
> UTC may actually be stripped/omitted, leading to FIXM times being interpreted as local times instead of UTC times by some applications. 
> FIXM implementers are therefore invited to crosscheck that their systems correctly interpret FIXM times as UTC time.


## Aircraft Identification vs Aircraft Registration vs Aircraft Address

ICAO Doc 10150 identifies a number of fields exchanged in order to enabling the identification of an aircraft in distress. These fields include the `Aircraft Identification`, the `Aircraft Registration` and the `Aircraft Address`.

### Aircraft Identification

The `Aircraft Identification`, abbreviated ACID, is defined by ICAO as *A group of letters, figures or a combination thereof which is either identical to, or the coded equivalent of, the aircraft call sign to be used in air-ground communications, and which is used to identify the aircraft in ground-ground air traffic services communications.* The `Aircraft Identification` is NOT an identifier of an aircraft. It is an identifier of a flight, i.e. the operation of an aircraft from A to B. 

Examples of ACID: 
- ACID = `MAS370`, pronounced as `Malaysia Airlines Three-Seven-Zero` (= the callsign)
- ACID = `AFR447`, pronounced as `Air France Four-Four-Seven` (= the callsign)
- ACID = `BAW1234`, pronounced as `Speedbird One-Two-Three-Four` (= the callsign)

### Aircraft Registration

The `Aircraft Registration` is *a unique, alphanumeric string that identifies a civil aircraft and consists of the Aircraft Nationality or Common Mark and an additional alphanumeric string assigned by the state of registry or common mark registering authority.* The `Aircraft Registration` is commonly called tail number, and is visible on the airframe. 

Examples of Aircraft Registration: 
- Aircraft Registration = `9MMRO` for the aircraft operated during flight `MAS370`

![Aircraft Registration 9MMRO](./MH370_aircraft_reg.png)

### Aircraft Address

The `Aircraft Address` is *A unique combination of twenty-four bits available for assignment to an aircraft for the purpose of air-ground communications, navigation and surveillance.* The `Aircraft Address` is hardcoded in the aircraft and is commonly called Mode S Address.

Examples of Aircraft Address: 
- Aircraft Address = `4010DA`

### Overview

![Overview](./aircraft_id_reg_address_from_AIRM.png)



