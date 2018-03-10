**********************
Forms Business Service
**********************

This chapter contains the specification for the **Forms Service** on
the Digital Health Platform (DHP) in Scotland.

What you can do with this service:

1. Post an electronic form for a service user to complete

2. Allow the service user to complete the form in their own time and
   submit when ready

3. Collect the completed form

4. Include the form in the notification feed of the service user and
   determine if the user has viewed the information (optional)

Please note that at present the service is an early exemplar of one
specific form only ('About Me').

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

The Forms Service provides a means for issuing forms electronically, and
to collect the form when completed.

The form is defined by a structure of text fields which can be presented
by a TPP in whatever format is suitable.

**By using this service as CSO (Core System Owner) or as TPP (Third
Party Provider) you enter into a set of important commitments (the
'service contract').**

**In general, this requires the implementations to be consistent with
the meanings defined here and for the implementations to be backed by
safe and effective real-life service fulfilment.**

**CSOs and TPPs must ensure that they post complete and timely
information that is consistent with the definitions here. Both parties
are responsible for mapping data if necessary.**

**TPPs must ensure that they display and otherwise use information in a
manner that is consistent with the definitions here. If a record is
marked for notification then the notification should be delivered as
soon as practical.**

**TPPs must ensure that they post an accurate reflection of the will of
the service user in the completed form.**

**TPPs must ensure that no malicious code is returned via the form.**

**CSOs must ensure that they collect completed forms and act on the
service user response in a timely way.**

Information held in a Forms record
----------------------------------

+-----------------------------------+-----------------------------------+
| **Information item**              | **Description**                   |
+===================================+===================================+
| ServiceUserXRef                   | Reference to the record of the    |
|                                   | service user where this form      |
|                                   | applies.                          |
|                                   |                                   |
|                                   | The service user record uses the  |
|                                   | core system business identifier   |
|                                   | scheme (such as CHI) as well as a |
|                                   | FHIR resource id local to the DHP |
+-----------------------------------+-----------------------------------+
| Title                             | Title of the form                 |
+-----------------------------------+-----------------------------------+
| List of Fields                    | A list of the fields to be        |
|                                   | completed, for each entry:        |
|                                   |                                   |
|                                   | -  Text label of the field        |
|                                   |                                   |
|                                   | -  Text response of the service   |
|                                   |    user                           |
+-----------------------------------+-----------------------------------+
| CompletionStatus                  | In-Progress                       |
|                                   |                                   |
|                                   | Or                                |
|                                   |                                   |
|                                   | Completed                         |
+-----------------------------------+-----------------------------------+
| WhenIssued                        | Date when the form was issued     |
+-----------------------------------+-----------------------------------+
| WhenCompleted                     | Date when the form was completed  |
+-----------------------------------+-----------------------------------+
| Notify                            | Flag that determines if the form  |
|                                   | is intended for the service user  |
|                                   | notification feed and whether the |
|                                   | information has been seen by that |
|                                   | user.                             |
+-----------------------------------+-----------------------------------+

A form record is created with the In-Progress status. Both, TPPs and
CSOs can create the form.

As and when a service user stores responses, the TPP updates the form
record which will create a new version and a history of previous record
versions is retained.

When the service user marks the form as completed, then the update must
change the status to Completed. From this point onwards, no further
changes may be made.

Forms records cannot be deleted.

Technical level specification
=============================

The service is implemented via profiles defined on the HL7 FHIR API of
the platform. Access is secured via the access control engine and token
service of the platform.

It is assumed that the development team has a general understanding of
the HL7 FHIR specifications. All provisions of HL7 FHIR DSTU 2 [1]_
apply unless changes are described in this document.

**The details of the FHIR profile for the Forms Service can be found
in a dedicated chapter.**

The following is additional information to explain key aspects of the
technical implementation.

General information
-------------------

Each service user receives his or her own instance of the form.

The metadata of the resource which shows when it was created represents
when the form was originally issued.

The completion date is captured on the resource itself using the
'authored' field.

The underlying core system or its proxy integration engine needs to
collect completed forms from the DHP.

This can be done by using a history search with the relevant time period
in addition to naming the relevant platform profile for the form.

Notify and viewed flag
----------------------

The service supports a general platform feature that allows a given
piece of information to be included in the notification feed of the
service user and tracks whether the user has viewed the record. 
In this instance this applies to a new form
that was issued for completion.

.. [1]
   https://www.hl7.org/fhir/DSTU2/index.html
