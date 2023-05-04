# Providing CO2 Transparency in ONE Record

## Basic Information on this document

### Objective 
The purpose of this document is to provide a Good Practice for end2end and multi-modal Greenhouse Gas emission transparency in the IATA ONE Record-based data eco-system. This document is supposed to suggest a first implementation guide for closing the bridge between the "Greenhouse Gas Logistics Emissions Data Model" of the Smart Freight Center and ONE Record. At a later stage, the Smart Freight Center should be able to integrate and maintain their data model in the context of a modular integration in ONE Record independently from this approach.

### Target audience
This document can be used by any party with the interest of using digital accompanying documents in ONE Record. 

### Geographical coverage
As there are no legal or operational restrictions, the solution can be used world wide.

### Creators
This document is the outcome of a ONE Record pilot project with the "Digitales Testfeld Air Cargo" by the German air cargo community. Parties/Persons involved were:

Lufthansa Cargo, Dr. Philipp Billion

Souvereign, Moritz Tölke

Frankfurt University of Applied Sciences, Niclas Scheiber

Fraunhofer IML, Oliver Ditz

Lufthansa Industry Solutions, Daniel Döppner

### Continous development and availability

Subsequently, the responsibility for this topic/document should be handed over to the Smart Freight Center. This document is to be used and continously developed, even if the current major stakeholders should move to other topics. Thus a "handover" in Github is planned if responsibilities should shift.

### Use and reference

This Good Practice is free to access and use. If you use it, please refer to this document explicitly plus provide a link to the Github repository as source. This will ensure know-how-transfer and transparency.

### Publication date, version and history

Publication date, version and history should be provided by the Github version control system and not be duplicated here.

## Dependencies

### Standards applied

UPDATE REQUIRED!! The ONE Record business ontology version as of APR 13, 2022 was used [Working draft Ontology of 2022APR13](https://github.com/IATA-Cargo/ONE-Record/tree/api_2.0.0-dev/working_draft/API).

The ONE Record API and security specification draft of version 2.0 as of MAY 2nd, 2023 was used: [API and Security specification v2.0 Release Candidate](https://github.com/IATA-Cargo/ONE-Record/tree/api_2.0.0-dev/working_draft/API).

### ONE Record Server Implementation used

(no ONE Record server implementation yet)

### Other Software products used

(none so far)

## Assumptions

One or more stakeholders are able to provide greenhouse gas emission transparency on their segments of the transport, other stakeholders are interested and capable of consuming this data. A central assumption is that there is an interest of stakeholders to recieve CO2 transparency on piece level, end2end, multi-modal.

## Solution approach

The ONE Record data model follows two principles: Piece-centricity and physics-orientation. Piece-centricity is self-explanatory, as every information is linked with the piece as the lowest available transportation object. Physics-orientation means that the linking strucutre of ONE Record follows the structures of the physical world (for more details, please refer to [ONE Record Github Repository](https://github.com/IATA-Cargo/ONE-Record).

The problem here is that the two principles are "collading" by some aspects here: The CO2-Emissions happen to ***Activities*** as trucks and planes burn fuel during the transportation process, not pieces or shipments by themselfes. On the other hand, it would not help a data consumer to know that the aircraft with it´s piece onboard burnt a specific ammount of fuel without a brakedown to it´s piece. To close this gap, a set of data objects and process descriptions is described here to enable end-to-end, multi-modal CO2 tracking throughout the supply chain.

The vision of this approach is to give even to consumers a transparency on CO2 emissions potentially from the production site to his door, covering all movement of the piece.

Principally, the approach does not define a GHG-emission calculation methode, but supports any methode, and even the option of providing results for different calculation methods. It also enables transparency on the calculation parameters (fuel burnt, distances, etc.), their acquisition methode (measured, calculated), so a data consumer could potentially apply his own calculation method.

## Solution in current environment

In the legacy messaging environment, end to end CO2 emission tracking on piece level isn´t possible. Generic solutions for transparency on shipment level are in place, but don´t follow a standardized approach for different modes of transportation.


## GLEC Data Model and ONE Record Data Model Terminology

To avoid a new, unaligned approach to CO2-measurement, the GLEC Data Model is the basis for GHG-emission transparency in ONE Record. The terminology of the GLEC Framework and ONE Record is quite similar, but not completely identical. Differences in the terminology are minor enough to be accepted. In the following good practice, the terms will be used according to the ONE Record definition, but with remarks to identify significant differences.

### "Shipment"

GLEC Terminology: A shipment is an identifiable collection of one or more freight items to be transported together from the original shipper to the ultimate consignee. A shipment may be transported in one or a multiple number of consignments. A shipment can be aggregated or disaggregated to different consignments according to the requirements of the means of transportation on any one element of the transport chain, e.g., single bulk units and packages can be aggregated on a pallet and such pallet can be handed over as a unit for aggregation in a container, which in turn is treated as a consignment in a vehicle.

ONE Record: A shipment is physical freight by a shipper transported under one contract by a carrier, so this definition does not fit here. Instead, this definition fits best with the piece-definition of ONE Record and it's ability to reflect the physical world with the piece-in-piece-concept. But there is one limitation: Pure loading devices like containers or pallets are usually defined as LoadingUnits in ONE Record.

Conclusion: A "shipment": in the GLEC Terminology is either a single piece in ONE Record or a single piece plus a loadingUnit. 

### "Consignment"

GLEC Terminology: A consignment separately identifiable amount of freight transported from one consignor to one consignee via one or more modes of transport.

ONE Record: No use of the term "consignment", the definition indicates a use like the ONE Record term "shipment".

Conclusion: A consignment in the GLEC terminology can be matched with a shipment in ONE Record, either a master AWB (if concluded between a forwarder and a carrier), or a house AWB (if concluded between a shipper and a forwarder).

### "Transport chain element (TCE)"

GLEC Terminology: A transport chain element (TCE) is a section of a transport chain within which the freight is carried by a single vehicle or transits through a single hub.

ONE Record: No use of term "Transport chain element (TCE)", but the same function is covered by the "TransportMovement LO", with the limtation to a single leg. But as a transportMovement LO is always a single leg, there is no "transit through a single hub" in ONE Record like in the GLEC Terminology.

Conclusion: Same terminological approach, but the granularity is higher in the ONE Record use of the "TransportMovement LO". This should not lead to significant problems, as data on a higher granularity can be aggregated. 

### "Transport operation category (TOC)"

GLEC Terminology: A transport operation category (TOC) is a group of transport operations that share similar characteristics.

ONE Record: ONE Record uses activities, and subsets of activities can be clustered according to their characteristics.

Conclusion: Transport operations can be matched without problems with a variety of transport related Activities in ONE Record.  

### "Hub operation category (HOC)"

GLEC Terminology: A hub operation category (HOC) is a group of hub operations that share similar characteristics.

ONE Record: ONE Record uses activities, and a subset of those activities are performed in the hub. There is no clustering into "hub operations" so far.

Conclusion: Hub operation can be matched without problems with hub related Activities in ONE Record.


# Data use and target process

Principally, the measurement of GHG Emissions in ONE Record follows the GLEC approach. The following guideline is supposed to line out the practical application within the ONE Record environment.

In ONE Record, climate relevant emissions are performed within the framework of ***activities*** , because an aircraft, a truck or a forklift do not produce emissions by themselfes (e.g. a plane that isn´t flying), thus the primary attribution of CO2-Emissions is linked with the ***activity***. Principally, any movement of pieces, like a Truck leg, a flight, or even a forklift-movement can be an activity, but also summarized activities like "warehousing" or "checks" can be used here to share GHG Emission data. On top, several activities can be combined into a service. That complete service can also provide CO2-Emission calculations.

While all climate data exchange around ***activities*** serves for submitting the data basis for climate impact calculation between different stakeholders of the supply chain, the end user relevant climate impact data on piece level aims to fulfill the estimation of the actual impact of the transport on the climate for the shipper of the piece.

The following table illustrates a matching between the GLEC GHG Data Model and the ONE Record data model v3.0:




## transportMovement LO

The TransportMovement directly contains emission-relevant data: The ***distanceMeasured***, the ***distanceCalculated***, the ***fuelType***, the ***fuelAmountMeasured***, the ***fuelAmountCalculated*** and a link towards the correlating CO2-Emissions ***CO2Emissions*** (1:n link).

### Data fields: distanceMeasured and distanceCalculated

If available, the actually measured distance is provided in the ***distanceMeasured*** data field. Only if not available, the ***distanceCalculated*** data field should be populated.

### Data field: fuelType

The ***fuelType*** data field should indicate the fuel that was consumed for this ***transportMovement***. "Kerosene", "SAF", "Renewable electric energy" are examples for possible values ***no standardized list, list by ISO expected; Moritz: standard-liste Referenz***. 
1:n
***Energy Carrier***
***FeedStock***
***FeedStockShare***
Data exchange guidance Table 6 (Mail vom 29.4.2022)

### Data fields: fuelAmountMeasured and fuelAmountCalculated

If available, the actually measured fuel consumption is provided in the ***fuelAmountMeasured*** data field. Only if not available, the ***fuelAmountCalculated*** data field should be populated.

### Data field: totalLoadedWeight

TBD: the total transportation weight is required for climate relevant emission monitoring. Question: How do we deal with this? 

Option 1: Assume shipments are not split, so it is the sum of all totalGrossWeighs of the Shipments. Pro: Realistic and exact, con: doesn´t work if shipments are split

Option 2: Sum the pieces on truck. Pro: easy to calculate; con: is not correct (total gross weight is usually higher with additional loading equipment), Piece info often missing

Option 3: create a new measured value totalLoadedWeight; most accurate, but also available?

## payloadDistance LO

The ***payloadDistance*** LO describes the relevant factor for the climateImpact calculation on a truck.

### Data field: payloadDistanceResult (value)

The payloadDistanceResult is a most relevant parameter for the estimation of the climate impact of this transportMovement. It is usually calculated by multiplying the weight and the distance of the transportMovement. Possible units are kilogram-kilometre ("kgkm"), tonne-kilometre ("tkm"), kilometre-tonne ("kmt") and "ton-mile", which is in the US: 1 ton-mile * ( 0.907185 t / short ton) * ( 1.609344 km / mile ) = 1.460 tkm.

### ISOTransparencyLevel (int)

This parameter shows the level of parameters to be included. 

TBD here, e.g. is there a level including the deadhead legs? Etc.

### FuelConsumptionParameter

This indicator can be either "measured" or "calculated". It describes the calculation basis for the calculation result, and not the data basis, which can be found in the the ***fuelAmountMeasured*** and ***fuelAmountCalculated*** of the transportMovement (TBD: Obsolete due to the ISO Levels bringing clear indicators here?).

### DistanceParameter

This indicator can be either "measured" or "calculated". It describes the calculation basis for the calculation result, and not the data basis, which can be found in the the ***distanceMeasured*** and ***distanceCalculated*** of the transportMovement (TBD: Obsolete due to the ISO Levels bringing clear indicators here?)

### CO2CoefficiencyFactor

**tbd** required?

### Other data fields

Other data fields like ***departureLocation*** and ***arrivalLocation*** could be used to verify the CO2-Emission relevant data sources. Additionally, relevant information could be added as an ***externalReference***, if only available as PDF. This could also be used for an image or a GPS-track of the geo-locational movement to provide an additional layer of information.

The ***movementType*** has a special relevance here, as it indicates wether this is a planned transport movement or an already  performed one.

## transportMeans LO

The ***transportMeans*** describes the means of transportation used to perfom for the linked transportMovement. Classical examples are a truck that performs a road leg for a transportation from the forwarder´s hub to the carrier´s origin airport, or a Boeing 777 freighter to perform a flight from Frankfurt to Rio de Janeiro. 

### Data field: typicalFuelConsumption

The ***typicalFuelConsumption*** describes an average amount of fuel for a defined distance, e.g. 12 l / 100 km. This does not include the type of fuel, as one of the assumptions is that the consumption doesn´t depend on the type of fuel. When using this, the ***unit*** data field is quite extensively used, with a content like "l/100km".

### Data field: typicalCO2Coefficient

The ***typicalCO2Coefficient*** describes ??? required?

### Data field: 

## piece LO

The Piece is the central unit of the ONE Record data model, and thus climate impact should be calculated and published on this level. If no detailed piece information is available, the total gross weight of the shipment is evenly distributed amoungst the pieces of the shipment. The total number of pieces should also be known. If the weights of individual pieces are known, they must be taken into account.


### Data field: grossWeight

The data field ***grossWeight*** within the piece LO describes the weight of the piece, and thus is to be used for the impact calculation

### Data field: skeletonBy

The data field ***skeletonBy*** aims for providing information, if and by whom a piece skeleton was created. Skeleton pieces are placeholders, if the owning party does not provide piece information (usually the Shipper). In that case, the totalGrossWeight of the shipment is evently distributed over the number of pieces, and thus pieces skeletons are created with a generic UPID. If the field is filled with content, piece skeletons were created, if left blank, piece information is available. Piece skeletons can be created by any party. Once piece skeletons are used, they are to be used along the supply chain for any piece-level purpose, instead of creating new piece skeletons by downflow parties.


## ClimateEffect LO

The ***climateEffect*** LO is the Logistics Object documenting the effective climate impact of the transportation of the piece. Each stakeholder should quantify the effect for his own part of the transportation chain, meaning the carrier should provide information for all legs under the MAWB contract, including flight legs, RFS, etc., the forwarder should provide all transportation legs under his control (usually the HAWB), including the carriers legs, etc. To clearly indicate these "embedded" emissions, a climateEffect can contain "embedded" climateEffects.

### Data field: CO2equivalentWTW

**tbd**

### Data field: CO2equivalentTTW

**tbd**

### Data field: MethodName

This field contains the name of the method applied.

### Data field: MethodVersion

This field contains the version of the method calculation, if available.

### Data field: MethodLink

This data field contains a URL to more details on the calculation method applied.

### Data field: Verification

**tbd**

### Data field: Accreditation

**tbd**

### Data field: TransportActivity

### Data field:  includedClimateEffects

This data field contains linked climateEffect calculation of embedded transport activites (see remarks above) 


# API use

### Technical setting

### Basic API-Features used

## Results / Summary

## Additional comments / FAQs

### How do we deal with missing piece information?

Principally, the ONE Record data model is based on the piece. Thus the ***climateImpact*** LO is linked to the piece, never the shipment. Thus we seem to have a problem, if e.g. the weights of each piece are missing, as this is a relevant factor for climate Impact calculation. 

But even if *detailed* piece information are not available, the number of pieces is usually available. In that case, the ***totalGrossWeight*** of the shipment is divided over the number of pieces. Meaning that it is assumed that all pieces have the same weight. This procedure is called the "use of piece skeletons". But this approach is only to be applied, if there´s no piece information available. If piece information are available, they must be taken into account for the climate impact calculation.

If a consumer wants to consume the ***climateImpact*** on shipment level, it is required to sum up the ***climateImpact*** of all pieces within the shipment. Providing the climate impact on shipment level is not possible within ONE Record.
