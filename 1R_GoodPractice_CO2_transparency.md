# Providing CO2 Transparency in ONE Record

## Basic Information on this document

### Objective 
The purpose of this document is to provide a Good Practice for end2end and multi-modal CO2 emission transparency in the IATA ONE Record-based data eco-system.

### Target audience
This document can be used by any party with the interest of using digital accompanying documents in ONE Record. 

### Geographical coverage
As there are no legal or operational restrictions, the solution can be used world wide.

### Creators
This document is the outcome of a ONE Record pilot project with the "Digitales Testfeld Air Cargo" by the German air cargo community. Parties/Persons involved were:

Lufthansa Cargo, Philipp Billion (in lead)

SOUVEREIGN

Fraunhofer IML, Oliver Ditz

Lufthansa Industry Solutions, Daniel Döppner

### Continous development and availability

This document is to be used and continously developed, even if the current major stakeholders should move to other topics. Thus a "handover" in Github is planned if responsibilities should shift.

### Use and reference

This Good Practice is free to access and use. 

Please mention the use of this Good practice and provide a link to the Github repository as a source.

### Publication date, version and history

Publication date, version and history should be provided by the Github version control system and not be duplicated here.


## Dependencies

### Standards applied

The ONE Record business ontology version as of APR 13, 2022 was used [Working draft Ontology of 2022APR13](https://github.com/IATA-Cargo/ONE-Record/blob/bbe86e364b04d6a6279f0ab6e9ee47e1905ec9c4/working_draft/ontology/IATA-1R-DM-Ontology.ttl).

The ONE Record API and security specification draft witout a version as of APR 13, 2022 was used (no link available yet).

### ONE Record Server Implementation used

(no ONE Record server implementation yet)

### Other Software products used

## Assumptions

One or more stakeholders are able to provide CO2 transparency on their segments of the transport, other stakeholders are interested and capable of consuming this data.

A central assumption is that there is an interest of stakeholders to recieve CO2 transparency on piece level, end2end, multi-modal.

## Process

### Solution approach

The ONE Record data model follows two principles: Piece-centricity and physics-orientation. Piece-centricity is self-explanatory, as every information is linked with the piece as the lowest available transportation object. Physics-orientation means that the linking strucutre of ONE Record follows the structures of the physical world (for more details, please refer to [ONE Record Github Repository](https://github.com/IATA-Cargo/ONE-Record).

The problem here is that the two principles are "collading" by some aspects here: The CO2-Emissions happen to the ***TransportMeans*** as trucks and planes burn fuel, not pieces or shipments. On the other hand, it would not help a data consumer to know that the aircraft with it´s piece onboard burnt a specific ammount of fuel without a brakedown to it´s piece. To close this gap, a set of data objects and process descriptions is described here to enable end-to-end, multi-modal CO2 tracking throughout the supply chain.

The vision of this approach is to give even to consumers a transparency on CO2 emissions potentially from the production site to his door, covering all movement of the piece.

Principally, the approach does not define a CO2-Emission calculation methode, but supports any methode, and even the option of providing results for different calculation methods. It also enables transparency on the calculation parameters (fuel burnt, distances, etc.), their acquisition methode (measured, calculated), so a data consumer could potentially apply his own calculation method.

### Solution in current environment

In the legacy messaging environment, end to end CO2 emission tracking on piece level isn´t possible. Generic solutions for transparency on shipment level are in place, but don´t follow a standardized approach for different modes of transportation.

# Target process

## Physical CO2 emission sources

CO2 is emitted by a ***transportMeans*** on a ***transportMovement***, because a ***transportMeans*** does not produce emissions by itself (a plane that isn´t flying), the primary attribution of CO2-Emissions is linked with the ***transportMovement***. The ***transportMovement*** can be any movement of pieces, like a Truck leg, a flight, or even a forklift-movement.

### transportMovement LO

The TransportMovement directly contains emission-relevant data: The ***distanceMeasured***, the ***distanceCalculated***, the ***fuelType***, the ***fuelAmountMeasured***, the ***fuelAmountCalculated*** and a link towards the correlating CO2-Emissions ***CO2Emissions*** (1:n link).

#### distanceMeasured and distanceCalculated

If available, the actually measured distance is provided in the ***distanceMeasured*** data field. Only if not available, the ***distanceCalculated*** data field should be populated.

#### fuelType

The ***fuelType*** data field should indicate the fuel that was consumed for this ***transportMovement***. "Kerosene", "SAF", "Renewable electric energy" are examples for possible values ***CHECK ON LIST HERE***.

#### fuelAmountMeasured and fuelAmountCalculated

If available, the actually measured fuel consumption is provided in the ***fuelAmountMeasured*** data field. Only if not available, the ***fuelAmountCalculated*** data field should be populated.

#### Other data fields

Other data fields like ***departureLocation*** and ***arrivalLocation*** could be used to verify the CO2-Emission relevant data sources. Additionally, relevant information could be added as an ***externalReference***, if only available as PDF. This could also be used for an image or a GPS-track of the geo-locational movement to provide an additional layer of information.

The ***movementType*** has a special relevance here, as it indicates wether this is a planned transport movement or an acually performed one.

### transportMeans LO

The ***transportMeans*** describes the means of transportation used to perfom for the linked transportMovement. Classical examples are a truck that performs a road leg for a transportation from the forwarder´s hub to the carrier´s origin airport, or a Boeing 777 freighter to perform a flight from Frankfurt to Rio de Janeiro. 

#### typicalFuelConsumption

The ***typicalFuelConsumption*** describes an average amount of fuel for a defined distance, e.g. 12 l / 100 km. This does not include the type of fuel, as one of the assumptions is that the consumption doesn´t depend on the type of fuel. When using this, the ***unit*** data field is quite extensively used, with a content like "l/100km".

#### typicalCO2Coefficient

The ***typicalCO2Coefficient*** describes ???

## API use

### Technical setting

### Basic API-Features used

## Results / Summary

## Additional comments
