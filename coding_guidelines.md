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


