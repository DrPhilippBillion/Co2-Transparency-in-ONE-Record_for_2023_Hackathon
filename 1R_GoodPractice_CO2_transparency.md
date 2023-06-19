# Providing CO2 Transparency in ONE Record for 2023 ONE Record Hackathon hosted by LH Cargo

## Basic Information on this document

### Objective 
The purpose of this document is to provide a Guideline for end2end and multi-modal Greenhouse Gas emission transparency in the IATA ONE Record-based data eco-system for the 2023 ONE Record Hackathon hosted by LH Cargo. This document is supposed to suggest a first implementation guide for closing the bridge between the "Greenhouse Gas Logistics Emissions Data Model" of the Smart Freight Center and ONE Record. At a later stage, this draft approach is to be replaced by an aligned industry approach.

### Target audience
This document can be used by any party with the interest of using digital accompanying documents in ONE Record. 

### Geographical coverage
As there are no legal or operational restrictions, the solution can be used world wide.

### Creators
This document is the outcome of a ONE Record pilot project with the "Digitales Testfeld Air Cargo" by the German air cargo community. Parties/Persons involved were:

Lufthansa Cargo, Dr. Philipp Billion

Frankfurt University of Applied Sciences, Niclas Scheiber

Souvereign, Moritz Tölke

Fraunhofer IML, Oliver Ditz

Lufthansa Industry Solutions, Daniel Döppner

### Continous development and availability

As this is only a document to "close the gap", until a systematic and aligned integration by CO2-experts is available, this should not be the basis for long-term handling of this topic.

### Use and reference

This Good Practice is free to access and use. If you use it, please refer to this document explicitly plus provide a link to the Github repository as source. This will ensure know-how-transfer and transparency.

### Publication date, version and history

Publication date, version and history should be provided by the Github version control system and not be duplicated here.

## Dependencies

### Standards applied

The ONE Record business ontology version 3.0 as of APR 13, 2022 was used.

The ONE Record API and security specification draft of version 2.0 as of JUNE 15th, 2023 (https://ddoeppner.github.io/ONE-Record/).

The data fields and basic architecture of the data model are based on the "Data exchange of GHG Logistics Emissions - Guidance" by SmartFreightCenter, as of MARCH 2023.

## Assumptions

One or more stakeholders are able to provide greenhouse gas emission transparency on their segments of the transport, other stakeholders are interested and capable of consuming this data. A central assumption is that there is an interest of stakeholders to recieve CO2 transparency on piece level, end2end, multi-modal.

## Solution approach

Following the principleas of the SmartFreightCenter-approach, an integration draft was created, but with some limitations: According to the piece-centricity approach of ONE Record, climate effects are to be provided on piece-level. That is a narrowed down approach to the SmartFreightCenter-approach, where emissions can also be provided on shipment level.

The following diagram shows the correlation between the existing and the new data for climate impact transparency in ONE Record:

![Data Model Diagram](GLEC Framework in 1R - V0.1[41].jpg "Climate Impact data in ONE Record")

## Solution in legacy environment

In the legacy messaging environment, end to end CO2 emission tracking on piece level isn´t possible. Generic solutions for transparency on shipment level are in place, but don´t follow a standardized approach for different modes of transportation.


## GLEC Data Model and ONE Record Data Model Terminology

To avoid a new, unaligned approach to CO2-measurement, the GLEC Data Model is the basis for GHG-emission transparency in ONE Record. The terminology of the GLEC Framework and ONE Record is quite similar, but not completely identical. Differences in the terminology are minor enough to be accepted. In the following good practice, the terms will be used according to the ONE Record definition, but with remarks to identify significant differences.

### "TransportMovement"

The TransportMovement equals the TOC (TransportOperationsCategory) in GLEC framework and includes all shared parameters for a given transport or flight used for emission calculation. This includes standard ONE Record properties, such as distanceMeasured and distanceCalculated, and new properties such as loadFactor and embedded EnergyFeedstock information.


### "TransportChainElement"

The TransportChainElement equals TCE in the GLEC framework and includes the shared emission parameters for a particular piece, a group of pieces, a shipment or a customer (not defined at this point in time) on a specific TransportMovement. A TransportMovement (as TOC) can be the base for many TCEs of many customers.


### "ClimateEffect"

The ClimateEffect was designed to satisfy the piece-centricity of ONE Record, as this element holds the calculated emissions of exactly ONE piece on exactly ONE TransportMovement through a TransportChainElement (TCE). A TCE can have many ClimateEffects for all Pieces covered, and a Piece can have many ClimateEffect. A ClimateEffect must have a connection to one Piece and one TCE.

## Data use and target process

In ONE Record, climate relevant emissions are performed within the framework of ***LogisticsActivities*** , because an aircraft, a truck or a forklift do not produce emissions by themselfes (e.g. a plane that isn´t flying), thus the primary attribution of CO2-Emissions is linked with the ***LogisticsActivities***. Principally, any movement of pieces, like a Truck leg, a flight, or even a forklift-movement can be an activity, but also summarized activities like "warehousing" or "checks" can be used here to share GHG Emission data.

While all climate data exchange around ***LogisticsActivities*** serves for submitting the data basis for climate impact calculation between different stakeholders of the supply chain, the end user relevant climate impact data on piece level aims to fulfill the estimation of the actual impact of the transport on the climate for the shipper of the piece. While a linking of any activity in ONE Record with the parameters of co2 emissions is possible, our example only shows the implementation for the ***transportMovement***.

Important hint: The definitions of the additional, CO2 emission-related data fields, can be found in the "Data exchange of GHG Logistics Emissions - Guidance" by SmartFreightCenter, as of MARCH 2023 and will not be repeated here.

### transportMovement 

The TransportMovement directly contains emission-relevant data like ***distanceMeasured***, ***distanceCalculated***,  ***loadFactor***, ***loadFactorRemarks***, an indicator for "empty" flights ***positioningMovement*** and links towards the data objects ***ClimateEffect***, ***TransportChainElement***, and has the embedded ***EnergyFeedstock***.

### transportMeans

The ***transportMeans*** describes the means of transportation used to perfom for the linked transportMovement. Classical examples are a truck that performs a road leg for a transportation from the forwarder´s hub to the carrier´s origin airport, or a Boeing 777 freighter to perform a flight from Frankfurt to Rio de Janeiro. For road transport, the ***emissionClass*** is specifically relevant.

### energyFeedstock

The Energy feedstock reflects the energy used to perform the transportMovement. As this is in a 1:n relationship with the transportMovement, it can reflect shares like 30% kerosene and 70% SAF.

### TransportChainElement

The TransportChainElement reflects the parameters for the climate effect calculation as described in the "Data exchange of GHG Logistics Emissions - Guidance" by SmartFreightCenter. It is linked with the ***activity*** and the ***climateEffect***.

### piece

The Piece is the central unit of the ONE Record data model, and thus climate impact should be calculated and published on this level. If no detailed piece information is available, the total gross weight of the shipment is evenly distributed amoungst the pieces of the shipment. The total number of pieces should also be known. If the weights of individual pieces are known, they must be taken into account.

### climateEffect

The ***climateEffect*** is the object documenting the effective climate impact of the transportation of the piece. Each stakeholder should quantify the effect for his own part of the transportation chain, meaning the carrier should provide information for all legs under the MAWB contract, including flight legs, RFS, etc., the forwarder should provide all transportation legs under his control (usually the HAWB), including the carriers legs, etc. It is linked with the piece and the ***TransportChainElement***, that contains all parameters for the calculation.

## API use

No specific requirements here.

## FAQ

### How do we deal with missing piece information?

Principally, the ONE Record data model is based on the piece. Thus the ***climateImpact*** LO is linked to the piece, never the shipment. Thus we seem to have a problem, if e.g. the weights of each piece are missing, as this is a relevant factor for climate Impact calculation. 

But even if *detailed* piece information are not available, the number of pieces is usually available. In that case, the ***totalGrossWeight*** of the shipment is divided over the number of pieces. Meaning that it is assumed that all pieces have the same weight. This procedure is called the "use of piece skeletons". But this approach is only to be applied, if there´s no piece information available. If piece information are available, they must be taken into account for the climate impact calculation.

If a consumer wants to consume the ***climateImpact*** on shipment level, it is required to sum up the ***climateImpact*** of all pieces within the shipment. Providing the climate impact on shipment level is not possible within ONE Record.
