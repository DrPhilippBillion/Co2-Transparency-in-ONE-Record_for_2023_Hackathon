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

## Solution approach

The ONE Record data model follows two principles: Piece-centricity and physics-orientation. Piece-centricity is self-explanatory, as every information is linked with the piece as the lowest available transportation object. Physics-orientation means that the linking strucutre of ONE Record follows the structures of the physical world (for more details, please refer to [ONE Record Github Repository](https://github.com/IATA-Cargo/ONE-Record).

The problem here is that the two principles are "collading" by some aspects here: The CO2-Emissions happen to the ***TransportMeans*** as trucks and planes burn fuel, not pieces or shipments. On the other hand, it would not help a data consumer to know that the aircraft with it´s piece onboard burnt a specific ammount of fuel without a brakedown to it´s piece. To close this gap, a set of data objects and process descriptions is described here to enable end-to-end, multi-modal CO2 tracking throughout the supply chain.

The vision of this approach is to give even to consumers a transparency on CO2 emissions potentially from the production site to his door, covering all movement of the piece.

Principally, the approach does not define a CO2-Emission calculation methode, but supports any methode, and even the option of providing results for different calculation methods. It also enables transparency on the calculation parameters (fuel burnt, distances, etc.), their acquisition methode (measured, calculated), so a data consumer could potentially apply his own calculation method.

## Solution in current environment

In the legacy messaging environment, end to end CO2 emission tracking on piece level isn´t possible. Generic solutions for transparency on shipment level are in place, but don´t follow a standardized approach for different modes of transportation.

# Data use and target process

Climate relevant emissions are performed by a ***transportMeans*** on a ***transportMovement***, because a ***transportMeans*** does not produce emissions by itself (e.g. a plane that isn´t flying), the primary attribution of CO2-Emissions is linked with the ***transportMovement***. The ***transportMovement*** can be any movement of pieces, like a Truck leg, a flight, or even a forklift-movement. 

While all climate data exchange around ***transportMeans*** and ***transportMovement*** serves for submitting the data basis for climate impact calculation between different stakeholders of the supply chain, the following climate impact data on piece level serves to fulfill the estimation of the actual impact of the transport on the climate.



|   	|Physical object with climate impacting emissions|Climate impacting attributions on piece level|
|---	|---	|---	|
|Focus LOs  	|***transportMeans***, ***TransportMovement***, ***payloadDistance***|***piece***, ***climateEffect***|
|Example|RFS Truck from AMS to CDG caused 12t  
|Purpose|Provide basic data for climate impact calculation on piece level|Provide transparency on climate impact of transport on piece level|
|Provider of data	|Operators (Airline, Trucking Company, etc.)|"Supply chain orchestrators" (can be airlines, forwarders, booking platforms)|
|Target Group / Data consumers |"Supply chain orchestrators" (can be airlines, forwarders, booking platforms)|Shippers, end customers, etc.|

## transportMovement LO

The TransportMovement directly contains emission-relevant data: The ***distanceMeasured***, the ***distanceCalculated***, the ***fuelType***, the ***fuelAmountMeasured***, the ***fuelAmountCalculated*** and a link towards the correlating CO2-Emissions ***CO2Emissions*** (1:n link).

### Data fields: distanceMeasured and distanceCalculated

If available, the actually measured distance is provided in the ***distanceMeasured*** data field. Only if not available, the ***distanceCalculated*** data field should be populated.

### Data field: fuelType

The ***fuelType*** data field should indicate the fuel that was consumed for this ***transportMovement***. "Kerosene", "SAF", "Renewable electric energy" are examples for possible values ***CHECK ON LIST HERE***.

### Data fields: fuelAmountMeasured and fuelAmountCalculated

If available, the actually measured fuel consumption is provided in the ***fuelAmountMeasured*** data field. Only if not available, the ***fuelAmountCalculated*** data field should be populated.

### Data field: totalLoadedWeight

TBD: the total transportation weight is required for climate relevant emission monitoring. Question: How do we deal with this? 

Option 1: Assume shipments are not split, so it is the sum of all totalGrossWeighs of the Shipments. Pro: Realistic and exact, con: doesn´t work if shipments are split

Option 2: Sum the pieces on truck. Pro: easy to calculate; con: is not correct (total gross weight is usually higher with additional loading equipment), Piece info often missing

Option 3: create a new measured value totalLoadedWeight; most accurate, but also available?

## payloadDistance LO

### Data field: payloadDistanceResult (value)

The payloadDistanceResult is a most relevant parameter for the estimation of the climate impact of this transportMovement. It is usually calculated by multiplying the weight and the distance of the transportMovement. Possible units are kilogram-kilometre ("kgkm"), tonne-kilometre ("tkm"), kilometre-tonne ("kmt") and "ton-mile", which is in the US: 1 ton-mile * ( 0.907185 t / short ton) * ( 1.609344 km / mile ) = 1.460 tkm.

### ISOTransparencyLevel (int)

This parameter shows the level of parameters to be included. 

TBD here, e.g. is there a level including the deadhead legs? Etc.

### FuelConsumptionParameter

This indicator can be either "measured" or "calculated" (TBD: Obsolete due to the ISO Levels bringing clear indicators here?)

### DistanceParameter

This indicator can be either "measured" or "calculated" (TBD: Obsolete due to the ISO Levels bringing clear indicators here?)


### Other data fields

Other data fields like ***departureLocation*** and ***arrivalLocation*** could be used to verify the CO2-Emission relevant data sources. Additionally, relevant information could be added as an ***externalReference***, if only available as PDF. This could also be used for an image or a GPS-track of the geo-locational movement to provide an additional layer of information.

The ***movementType*** has a special relevance here, as it indicates wether this is a planned transport movement or an already  performed one.

## transportMeans LO

The ***transportMeans*** describes the means of transportation used to perfom for the linked transportMovement. Classical examples are a truck that performs a road leg for a transportation from the forwarder´s hub to the carrier´s origin airport, or a Boeing 777 freighter to perform a flight from Frankfurt to Rio de Janeiro. 

### Data field: typicalFuelConsumption

The ***typicalFuelConsumption*** describes an average amount of fuel for a defined distance, e.g. 12 l / 100 km. This does not include the type of fuel, as one of the assumptions is that the consumption doesn´t depend on the type of fuel. When using this, the ***unit*** data field is quite extensively used, with a content like "l/100km".

### Data field: typicalCO2Coefficient

The ***typicalCO2Coefficient*** describes ???

## tbd

The purpose of this Logistics Objects is to share actual emissions for a Transport Movement. It´s main purpose is share this data between operator and other interested parties 

### Data field: 

## piece LO

The Piece is the central unit of the ONE Record data model, and thus climate impact should be calculated and published on this level. If no detailed piece information is available, the total gross weight of the shipment is evenly distributed amoungst the pieces of the shipment. The total number of pieces should also be known. If the weights of individual pieces are known, they must be taken into account.

The pieces LO only serves to provide base data, not effective 

## ClimateEffect LO

# API use

### Technical setting

### Basic API-Features used

## Results / Summary

## Additional comments

Calculation on piece level: indicator, if calculation was perfomed on piece-level or not

Ebene: typicalCO2Coefficient
Type: 
  calculation approach
    Rundlauf: 
    Intensitätswert

CO2Coefficient / CO2E IntensityFactor

Nicht CO2, aber climate relevant
TTW
- if meeasured timeframe: 
