******************************************
Health & Care Information Business Service
******************************************
This chapter contains the specification for the **Health & Care Information 
Service** on the Digital Health Platform (DHP) in Scotland.

What you can do with this service:

1. Provide service users with a summary view of their health and care record 
   based on information held in public service systems

2. Retrieve this information in a structured form

3. Obtain the date when the summary record was last changed

Please note that the current version of the service supports slots for 
allergies and medications records that are held in the Scottish Emergency Care 
Summary database. Further developments would add more sources and types 
of information.

Content of this chapter 

+-----------------------------------+-----------------------------------+
| Business level specification      | Read first in order to learn what |
|                                   | the platform offers and the       |
|                                   | commitment required for           |
|                                   | successful use. Contains          |
|                                   | important concepts and            |
|                                   | definitions.                      |
+-----------------------------------+-----------------------------------+
| Technical level specification     | Overview that explains how the    |
|                                   | service was implemented           |
|                                   | technically. Starting point to    |
|                                   | achieve a successful integration. |
+-----------------------------------+-----------------------------------+

It is assumed that you have read the introduction chapter and are 
familiar with the platform architecture that provides the general 
framework for this service.

Business level specification
============================

The Health & Care Information Service provides a means to give
service users online view-only access to a summary record based on 
information held in Scottish public sectore databases.

The service will only show information from connected systems. It is
possible that the person has additional records in other systems. 
It is also possible that there is in-flight information that has not yet 
been received.

**By using this service as TPP (Third Party Provider) you enter into a
set of important commitments (the 'service contract').**

**In general, this requires the implementations to be consistent with
the meanings defined here.**

**The record on the platform will be updated within 48h of a change
in the submitting core system.**

**Records will not be transferred if the patient decided that he
or she withdraws consent for their record to be shared.**

**The record represents a snapshot valid at the time recorded. The
snapshot will only be updated when an actual change happened.**

**TPPs must ensure that they display and otherwise use information in a
manner that is consistent with the definitions here. The TPP is
responsible for mapping data if necessary.**

Information held in an Health & Care Information summary record
---------------------------------------------------------------

+-----------------------------------+-----------------------------------+
| **Information item**              | **Description**                   |
+===================================+===================================+
| ServiceUserXRef                   | Reference to the record of the    |
|                                   | service user where this summary       |
|                                   | record applies.                   |
|                                   |                                   |
|                                   | The service user record uses the  |
|                                   | core system business identifier   |
|                                   | scheme (CHI) as well as a FHIR    |
|                                   | resource id local to the DHP      |
+-----------------------------------+-----------------------------------+
| List of allergies                 | The known allergies of the user   |
|                                   |                                   |
|                                   | For each entry                    |
|                                   |                                   |
|                                   | -  Date when the allergy was      |
|                                   |    recorded first                 |
|                                   |                                   |
|                                   | -  Substance that causes the      |
|                                   |    allergy in plain text          |
|                                   |                                   |
|                                   | -  Substance that causes the      |
|                                   |    allergy in READ coding         |
|                                   |                                   |
|                                   | -  Additional free text notes     |
|                                   |                                   |
|                                   | The list will have no entries if  |
|                                   | there are no known allergies      |
+-----------------------------------+-----------------------------------+
| List of medication                | The prescribed medication of the  |
|                                   | user                              |
|                                   |                                   |
|                                   | For each entry                    |
|                                   |                                   |
|                                   | -  Date when the medication       |
|                                   |    started                        |
|                                   |                                   |
|                                   | -  the drug identifier in plain   |
|                                   |    text                           |
|                                   |                                   |
|                                   | -  the drug identifier in READ    |
|                                   |    coding                         |
|                                   |                                   |
|                                   | -  dosage (amount and frequency)  |
|                                   |    in plain text                  |
|                                   |                                   |
|                                   | The list will have no entries if  |
|                                   | no medication is prescribed       |
+-----------------------------------+-----------------------------------+
| LastUpdated                       | Datetime at which the snapshot    |
|                                   | record was created                |
+-----------------------------------+-----------------------------------+

The underlying core system or its proxy integration engine will
create and update records on the platform where they exist and the
transfer is permitted.

Updates are reflected in form of a new version of the record. A history
of previous record versions is retained.

TPPs can only read records.

Technical level specification
=============================

The service is implemented via profiles defined on the HL7 FHIR API of
the platform. Access is secured via the access control engine and token
service of the platform. 

It is assumed that the development team has a general understanding of
the HL7 FHIR specifications. All provisions of HL7 FHIR DSTU 2 [1]_
apply unless changes are described in this document.

**The details of the FHIR profile for the Health & Care Information Service 
can be found in a dedicated chapter.**

The following is additional information to explain key aspects of the
technical implementation.

Retrieving the correct records
------------------------------

The snapshot of the record comes in form of a Composition resource.
The Composition refers out to a separate resource for the Patient that
the summary applies to. 
The record comes as a list of sections which are like slots holding 
different parts of the record as *contained* resources. 

The correct summary can be retrieved by searching with the relevant platform profile
and relevant patient subject reference. 
Note that it is possible that a given person does not have a record on the platform.

In the current version of the service, there are section slots for:

1. a MedicationStatement resource based on medications recorded in the ECS

2. an AllergyIntolerance resource based on allergies recorded in the ECS

The meta data on the Composition resource provides the information on
when the record was last updated.

.. [1]
   https://www.hl7.org/fhir/DSTU2/index.html
