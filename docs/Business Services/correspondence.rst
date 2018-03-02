===============================
Correspondence Business Service
===============================
This document contains the specification for the **Correspondence
Service** on the Digital Health Platform (DHP) in Scotland.

What you can do with this service:

1. Post notification of correspondence for distribution across all
   online channels that are connected to the platform

2. Communicate where the attachments of the correspondence can be
   retrieved electronically

3. Deposit the attachments as file on the platform itself (optional)

4. Include the correspondence in the notification feed of the service
   user and determine if the user has viewed the information (optional)

Please note that this is not a general file storage service or document
management service. Core systems hold the original documents.

Contact for enquiries and suggested changes: <email of the platform
technical lead and/or platform service desk – must be able to fulfil any
enquiries, if need be via delegation>

Content of this document 
-------------------------

+-----------------------------------+-----------------------------------+
| Business level specification      | Read first in order to learn what |
|                                   | the platform offers and the       |
|                                   | commitment required for           |
|                                   | successful use. Contains          |
|                                   | important concepts and            |
|                                   | definitions.                      |
+===================================+===================================+
|                                   |                                   |
+-----------------------------------+-----------------------------------+
| Technical level specification     | Overview that explains how the    |
|                                   | service was implemented           |
|                                   | technically. Starting point to    |
|                                   | achieve a successful integration. |
+-----------------------------------+-----------------------------------+

Additional reference information
--------------------------------

[1] DHP Introduction: A summary of the platform architecture that
provides the general framework for this service. It is assumed that you
are familiar with this.

[2] Additional Technical Services: A technical reference that describes
features which are available across a number of business services.
Selected aspects of this business service rely on these features.

Business level specification
============================

The Correspondence Service provides a means for a core system owner or
third party provider to send correspondence in electronic form to the
service user.

It is a mechanism to support correspondence packages which provide its
contents via URLs or as binary files in a variety of formats.

**By using this service as CSO (Core System Owner) or as TPP (Third
Party Provider) you enter into a set of important commitments (the
'service contract'). **

**In general, this requires the implementations to be consistent with
the meanings defined here and for the implementations to be backed by
safe and effective real-life service fulfilment.**

**CSOs and TPPs must ensure that they post complete and timely
information that is consistent with the definitions here. **

**CSOs and TPPs are responsible for ensuring that binary files that are
posted as attachments are free from malware.**

**CSOs and TPPs are responsible for ensuring that URLs which refer to
external systems are reputable and accessible over the internet.**

**CSOs and TPPs are responsible for mapping data if necessary and must
ensure that their binary files only contain appropriate metadata. **

**CSOs and TPPs must ensure that they display and otherwise use
information in a manner that is consistent with the definitions here. If
a record is marked for notification then the notification should be
delivered as soon as practical.**

**CSOs and TPPs must ensure that staff or the service user is given a
means to access an attachment that is part of the correspondence.**

Information held in a Correspondence record
-------------------------------------------

+-----------------------------------+-----------------------------------+
| **Information item**\  [2]_       | **Description**                   |
+===================================+===================================+
| ServiceUserXRef                   | Reference to the record of the    |
|                                   | service user where this           |
|                                   | correspondence applies.           |
|                                   |                                   |
|                                   | The service user record uses the  |
|                                   | core system business identifier   |
|                                   | scheme (such as CHI) as well as a |
|                                   | FHIR resource id local to the DHP |
+-----------------------------------+-----------------------------------+
| Created                           | Datetime when the correspondence  |
|                                   | was created                       |
+-----------------------------------+-----------------------------------+
| Description                       | Short plain text summary          |
+-----------------------------------+-----------------------------------+
| List of attachments               | The attachments that make up the  |
|                                   | overall correspondence package    |
|                                   |                                   |
|                                   | For each entry                    |
|                                   |                                   |
|                                   | -  Plain text title               |
|                                   |                                   |
|                                   | -  One of the following           |
|                                   |    attachment options:            |
|                                   |                                   |
|                                   |    -  File binary                 |
|                                   |                                   |
|                                   |    -  URL to location of an       |
|                                   |       external document (which    |
|                                   |       can be a webpage, or path   |
|                                   |       to a file)                  |
|                                   |                                   |
|                                   | -  Datetime when the attachment   |
|                                   |    was created originally in the  |
|                                   |    core system (optional)         |
+-----------------------------------+-----------------------------------+
| Notify                            | Flag that determines if the       |
|                                   | appointment is intended for the   |
|                                   | service user notification feed    |
|                                   | and whether the information has   |
|                                   | been seen by that user.           |
+-----------------------------------+-----------------------------------+

Records can be created and deleted by CSOs and TPPs.

Deletion of records should only happen to correct a data quality issue
or if a record was sent to the wrong person. Deleting records makes them
invisible for normal operational purposes but the history is retained as
audit record.

Correspondence records cannot be updated, but follow-up correspondence
can always be issued by creating a new record.

CSOs and TPPs can retrieve records for viewing by service users or staff
using core systems.

If an attachment is added in the form of a URL, then the posting party
must ensure that the URL can be resolved transparently. This means the
URL needs to point to an open public system, or the underlying system
needs to accept security tokens that were issued by the DHP to the user.

The party that provides a front end for viewing correspondence must
ensure that users have a path to access the attachments conveniently.
Depending on the available technology this may take different forms. For
example, a web application could display a PDF attachment in the default
PDF viewer of the device, whilst a digital assistant on a voice-only
device may read out key information and explain how to view the full
document on a different device.

Technical level specification
=============================

The service is implemented via profiles defined on the HL7 FHIR API of
the platform. Access is secured via the access control engine and token
service of the platform. See reference [2] for details.

The applicable FHIR Profiles can be found at: <URL of the online
documentation – in the short term we can share Word documents>

It is assumed that the development team has a general understanding of
the HL7 FHIR specifications. All provisions of HL7 FHIR DSTU 2 [3]_
apply unless changes are described in this document.

The following is additional information to explain key aspects of the
technical implementation.

Notify and viewed flag
----------------------

The service supports a general platform feature that allows a given
piece of information to be included in the notification feed of the
service user and tracks whether the user has viewed the record (see
reference [2] for details). In this instance this applies to a package
of correspondence that was created.

.. [1]
   A mechanism to link correspondence to an appointment is under
   development.

.. [2]
   A mechanism to link correspondence to an appointment is under
   development.

.. [3]
   https://www.hl7.org/fhir/DSTU2/index.html
