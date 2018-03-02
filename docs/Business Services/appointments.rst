=============================
Appointments Business Service
=============================
This document contains the specification for the **Appointments
Service** on the Digital Health Platform (DHP) in Scotland.

What you can do with this service:

1. Post notification of appointments for distribution across all online
   channels that are connected to the platform

2. Communicate timing, location and additional free text information

3. Allow service users to respond in order to accept, decline or request
   a reschedule (optional)

4. Include the appointment in the notification feed of the service user
   and determine if the user has viewed the information (optional)

Please note that this is not an appointment booking service. Core
systems perform booking and send appointment notifications.

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

The Appointments Service provides a means for maintaining notifications
of appointments and service user responses in the Person Held File of
the platform.

The service provides the ability to record the citizen’s response to an
appointment notification and these responses are relayed back to the
core system where appropriate action can be taken.

**By using this service as CSO (Core System Owner) or as TPP (Third
Party Provider) you enter into a set of important commitments (the
'service contract'). **

**In general, this requires the implementations to be consistent with
the meanings defined here and for the implementations to be backed by
safe and effective real-life service fulfilment.**

**CSOs and TPPs must ensure that they post complete and timely
information that is consistent with the definitions here. Both parties
are responsible for mapping data if necessary. **

**TPPs must ensure that they display and otherwise use information in a
manner that is consistent with the definitions here (eg only offer
response actions if that is supported in the case at hand). If a record
is marked for notification then the notification should be delivered as
soon as practical.**

**TPPs must ensure that they post an accurate reflection of the will of
the service user in a response record.**

**CSOs must ensure that they collect responses and act on service user
requests in a timely way. A turnaround of less than 24h is preferred. **

**The appointment state reflects a shared record between the core system
and the service user. Both CSO and TPP are responsible for maintaining
updates to the appointment record such that it is an accurate reflection
of the dialogue between core system and service user.**

Information held in an Appointment record
-----------------------------------------

+-----------------------------------+-----------------------------------+
| **Information item**\  [2]_       | **Description**                   |
+===================================+===================================+
| ServiceUserXRef                   | Reference to the record of the    |
|                                   | service user where this           |
|                                   | appointment applies.              |
|                                   |                                   |
|                                   | The service user record uses the  |
|                                   | core system business identifier   |
|                                   | scheme (such as CHI) as well as a |
|                                   | FHIR resource id local to the DHP |
+-----------------------------------+-----------------------------------+
| AppointmentID                     | Identifier of the appointment in  |
|                                   | the core system to allow for      |
|                                   | correlation of records            |
+-----------------------------------+-----------------------------------+
| CoreSystem                        | Optional information about the    |
|                                   | core system behind the            |
|                                   | appointment. Allows messages to   |
|                                   | be returned to the correct        |
|                                   | underlying core system            |
+-----------------------------------+-----------------------------------+
| AppointmentState                  | State of the appointment          |
|                                   | (described further below)         |
+-----------------------------------+-----------------------------------+
| Timing                            | Date and time information that    |
|                                   | defines when the appointment      |
|                                   | starts and when it ends (or how   |
|                                   | long it takes)                    |
+-----------------------------------+-----------------------------------+
| Location                          | Optional information about the    |
|                                   | establishment the patient must    |
|                                   | attend                            |
+-----------------------------------+-----------------------------------+
| Description                       | Additional plain English          |
|                                   | description, such as the type of  |
|                                   | clinic or procedure that was      |
|                                   | scheduled                         |
+-----------------------------------+-----------------------------------+
| Comments                          | Any additional comments (eg       |
|                                   | reason for cancellation or phone  |
|                                   | contact number)                   |
+-----------------------------------+-----------------------------------+
| ViewOnly                          | Flag that determines if a service |
|                                   | user response is supported        |
+-----------------------------------+-----------------------------------+
| Notify                            | Flag that determines if the       |
|                                   | appointment is intended for the   |
|                                   | service user notification feed    |
|                                   | and whether the information has   |
|                                   | been seen by that user.           |
+-----------------------------------+-----------------------------------+

Records can be updated, in which case a new version of the record is
created and a history of previous record versions is retained. Updates
can be flagged to generate a new entry in the service user notification
feed.

Records can be deleted, in which case they are invisible for normal
operational purposes but the history is retained as audit record.

A CSO may choose to maintain multiple entries that relate to the same
appointment in different states.

For example, an appointment that was originally posted can be followed
by a second entry that shows the appointment as cancelled.

It would be left to the service user to understand the linkage and
manage the entries separately.

Note that there is an important distinction between a silent delete of a
record (eg if a data quality issue sent a record to the wrong person)
and a cancellation that is still made visible to the service user.

Appointment states
------------------

+-----------------------------------+-----------------------------------+
| **State**                         | **Description**                   |
+===================================+===================================+
| Unacknowledged                    | Initial state when a new          |
|                                   | appointment entry arrives (could  |
|                                   | be entirely new or an update)     |
|                                   |                                   |
|                                   | No Service user response yet      |
+-----------------------------------+-----------------------------------+
| Accepted                          | Service user accepted the         |
|                                   | appointment.                      |
|                                   |                                   |
|                                   | Acceptance message sent to the    |
|                                   | core scheduling system            |
+-----------------------------------+-----------------------------------+
| Declined                          | Service user declined the         |
|                                   | appointment, did not ask for      |
|                                   | alternative date.                 |
|                                   |                                   |
|                                   | Declined message sent to the core |
|                                   | scheduling system                 |
+-----------------------------------+-----------------------------------+
| RescheduleRequested               | Service user declined the         |
|                                   | appointment, asked for            |
|                                   | alternative date.                 |
|                                   |                                   |
|                                   | Reschedule message sent to the    |
|                                   | core scheduling system            |
+-----------------------------------+-----------------------------------+
| Cancelled                         | Core service cancelled the        |
|                                   | appointment.                      |
+-----------------------------------+-----------------------------------+

 'Readonly' appointments
------------------------

The appointment record includes a ViewOnly flag which a CSO can use as
follows:

-  If the flag is left clear, then service users are given the option to
   respond electronically. A TPP should make actions available on the
   front end to capture these responses (accept, decline, reschedule)

-  If the flag is set, then a service user response is not supported.
   The information is intended for display only and a TPP must not make
   response actions available on the front end.

In both cases, a CSO may use notes in the appointment comment field to
explain other ways in which a service user can get in touch (eg
telephone number).

Information held in an appointment response record
--------------------------------------------------

+-----------------------------------+-----------------------------------+
| **Information item**              | **Description**                   |
+===================================+===================================+
| AppointmentXRef                   | Reference to the original         |
|                                   | Appointment Notification record   |
+-----------------------------------+-----------------------------------+
| ServiceUserXRef                   | Reference to the record of the    |
|                                   | service user where this           |
|                                   | appointment applies.              |
|                                   |                                   |
|                                   | The service user record uses the  |
|                                   | core system business identifier   |
|                                   | scheme (such as CHI) as well as a |
|                                   | FHIR resource id local to the DHP |
+-----------------------------------+-----------------------------------+
| ResponseAction                    | The response selected by the      |
|                                   | service user (described further   |
|                                   | below)                            |
+-----------------------------------+-----------------------------------+
| Comments                          | Free text comments by the service |
|                                   | user, for example preferred times |
|                                   | for a rescheduled appointment     |
+-----------------------------------+-----------------------------------+

+-----------------------+-----------------------+-----------------------+
| **Supported response  | **Description**       | **Action is available |
| actions**             |                       | if overall state is   |
|                       |                       | one of**              |
+=======================+=======================+=======================+
| Accept                | Accept appointment    | Unacknowledged        |
+-----------------------+-----------------------+-----------------------+
| Decline               | An appointment is no  | Unacknowledged,       |
|                       | longer required       | Accepted              |
+-----------------------+-----------------------+-----------------------+
| Reschedule            | Request a new         | Unacknowledged,       |
|                       | date/time             | Accepted              |
+-----------------------+-----------------------+-----------------------+

If a service user responds more than once to an appointment, for
example, after an update is received to a reschedule request, then a new
response record needs to be created each time.

Response records cannot be updated or deleted.

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

Setting up reference information
--------------------------------

The service allows optional information on appointment participants to
be provided in the form of contained resources.

This may be used in particular to describe the Location for attending
the appointment or the underlying appointment system (Device).

In addition, appointments will be identified by a system+value pair.

All information that defines such reference records needs to be
registered with the platform operator so that potential conflicts can be
managed. This will also prepare future developments where such records
become explicit resources in the Person Held File.

Mapping of states
-----------------

In order to record the status of an appointment correctly, there are two
fields on the profile that work in combination. The following describes
how the business states are mapped to FHIR.

+-----------------------+-----------------------+-----------------------+
| **Business State**    | **Appointment.status* | **Appointment.partici |
|                       | *                     | pant.status**         |
+=======================+=======================+=======================+
| Unacknowledged        | Pending               | Needs-action          |
+-----------------------+-----------------------+-----------------------+
| Accepted              | Booked                | Accepted              |
+-----------------------+-----------------------+-----------------------+
| Declined              | Pending               | Declined              |
+-----------------------+-----------------------+-----------------------+
| RescheduleRequested   | Pending               | Tentative             |
+-----------------------+-----------------------+-----------------------+
| Cancelled             | Cancelled             | (any value)           |
+-----------------------+-----------------------+-----------------------+

Collecting new service user responses
-------------------------------------

The underlying appointment system or its proxy integration engine needs
to collect relevant service user responses.

This can be done by using a history search with the relevant time period
and information known about the service user identity and/or appointment
system, in addition to naming the relevant platform profile (see
reference [2]).

Notify and viewed flag
----------------------

The service supports a general platform feature that allows a given
piece of information to be included in the notification feed of the
service user and tracks whether the user has viewed the record (see
reference [2] for details). In this instance this applies to an
appointment record that was created or updated.

.. [1]
   A mechanism to attach supporting documents to an appointment is under
   development.

.. [2]
   A mechanism to attach supporting documents to an appointment is under
   development.

.. [3]
   https://www.hl7.org/fhir/DSTU2/index.html
