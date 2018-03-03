***************************************
Emergency Care Summary Business Service
***************************************
This chapter contains the specification for the **Emergency Care
Summary Service** on the Digital Health Platform (DHP) in Scotland.

What you can do with this service:

1. Provide service users with a view of their prescribed medication and
   allergies as known to their GP

2. Retrieve this information in plain text

3. Retrieve substance and drug identification as clinical READ code

4. Obtain the date when the record was last changed

Please note that this service is currently aimed at TPPs only and
reflects records held in the Scottish Emergency Care Summary database.

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

The Emergency Care Summary (ECS) Service provides a means to give
service users online view-only access to a record by the same name that
is held in a national NHS Scotland database.

The service will only show information held in GP records. It is
possible that the person has additional prescribed medication (eg
hospital) and that there is in-flight information that is not yet known
to the GP.

**By using this service as TPP (Third Party Provider) you enter into a
set of important commitments (the 'service contract').**

**In general, this requires the implementations to be consistent with
the meanings defined here.**

**The ECS record on the platform will be updated within 48h of a change
made by the GP in the practice system.**

**Records will not be transferred if the patient told their GP that he
or she withdraws consent for their ECS to be shared.**

**The record represents a snapshot valid at the time recorded. The
snapshot will only be updated when an actual change happened.**

**TPPs must ensure that they display and otherwise use information in a
manner that is consistent with the definitions here. The TPP is
responsible for mapping data if necessary.**

Information held in an ECS record
---------------------------------

+-----------------------------------+-----------------------------------+
| **Information item**              | **Description**                   |
+===================================+===================================+
| ServiceUserXRef                   | Reference to the record of the    |
|                                   | service user where this ECS       |
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

The underlying core system (ECS and its proxy integration engine) will
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

The following is additional information to explain key aspects of the
technical implementation.

Retrieving the correct records
------------------------------

The snapshot of the ECS record comes in form of a mostly self-contained
Composition resource, which refers out to separate resources for
Patient, Custodian and Author.

The meta data on the Composition resource provides the information on
when the record was last updated.

The relevant (for this ECS service) composition will be identified by
looking at the fields for type, custodian and author. This will show a
'Medical Record' from National Service Scotland from a device that is
the ECS system or its proxy integration engine.

The composition will also have a unique business identifier in a coding
scheme that includes the 'ECS' keyword in a defined position.

The above can be used to determine if a service user has an ECS record
on the platform and then retrieve it.

.. [1]
   https://www.hl7.org/fhir/DSTU2/index.html
