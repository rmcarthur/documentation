.. _v1:

Open States v1 API
==================

.. warning:: API v1 is deprecated, please check out `API v3 <https://docs.openstates.org/en/latest/api/v3/>`_ or `API v2 <https://docs.openstates.org/en/latest/api/v2/>`_ both of which are actively supported.

Basics
------

-  All API calls are URLs in the form ``https://openstates.org/api/v1/METHOD/``
-  Responses are `JSON <http://json.org>`_ unless otherwise specified.
-  If an error occurs the response will be a plain text error message
   with an appropriate HTTP error code (404 if object is not found, 401
   if authentication fails, etc.).
-  To use the API you must `register for an API key <https://openstates.org/api/register/>`_.
-  Once activated, pass your API key via the ``apikey`` query paramter or the ``X-API-KEY`` header.
-  All changes to the API will be announced on the `Open States Discourse
   <https://discourse.openstates.org>`_. It is recommended you subscribe if you're using the API.
-  For Python users, there's an official
   `pyopenstates <https://docs.openstates.org/projects/pyopenstates/en/latest/>`__ package
   available.

Deprecation
-----------

As of November 2018 we are beginning the process of removing lesser-used portions of this API.  New applications should use API v2 or v3.

None of these were used by more than 1% of API users, and their removal will help us hopefully extend the life of the existing API.

**The Event & Committee endpoints are no longer supported.**  Committees are available in API v2, and events are not currently collected as part of Open States.

Additionally, **the boundary endpoint will no longer be something we provide**. This data changes rarely and is best served from a static resource instead of an HTTP query.  We are exploring options to provide a suitable resource for this in the future.

Additionally, the following rarely-used parameters are no longer supported in v1, similar functionality is available in v2:

Bill Search:
    * sponsor_id
    * subject
    * type
    * bill_id__in
    * sort=signed, sort=passed_upper, sort=passed_lower

Legislator Search:
    * first_name
    * last_name
    * active=false
    * party
    * term


If you were using the fields= parameter to control which data was returned you'll find that it is no longer always respected.  To ease caching we have altered the behavior, this was designed to be done in a backwards-compatible way: we now return more fields than we used to by default, and only in select cases is it necessary to add fields= to a request to obtain fields that would otherwise be omitted.

Additionally, the extra fields prefixed with + are scheduled to be removed.  They were never guaranteed, and we presume this won't affect any users.


Deprecation of Identifiers
--------------------------

Old Open States IDs (resembling AKL000001 or NYB00012345) will continue to work for entities created before 2019, but going forward will not be available.  In most cases this shouldn't be an issue, since the bill detail endpoint supports the <state>/<session>/<identifier> format.  Right now we have no plans to bring v1 IDs back.


Data Types
----------

Open States provides data about six core data types.

:ref:`metadata`
    Details on what data is available, including terms, sessions, and state-specific names for things.
:ref:`bills`
    Details on bills & resolutions, including actions & votes.
:ref:`legislators`
    Details on legislators, including contact details.
:ref:`districts`
    Details on districts and their boundaries.

.. toctree::
   :maxdepth: 2
   :hidden:

   metadata
   bills
   legislators
   districts
   categorization

Methods
-------

=================================   ==================================================  =====================================================================================
Method                               URL pattern                                         Description
=================================   ==================================================  =====================================================================================
:ref:`metadata-overview`             /metadata/                                          Get list of all states with data available and basic metadata about their status.
:ref:`state-metadata`                /metadata/`state`/                                  Get detailed metadata for a particular state.
:ref:`bill-search`                   /bills/                                             Search bills by (almost) any of their attributes, or full text.
:ref:`bill-detail`                   /bills/`state`/`session`/`bill_id`/                 Get full detail for bill, including any actions, votes, etc.
:ref:`legislator-search`             /legislators/                                       Search legislators by their attributes.
:ref:`legislator-detail`             /legislators/`leg_id`/                              Get full detail for a legislator, including all roles.
:ref:`legislator-geo`                /legislators/geo/?lat=latitude&long=longitude       Lookup all legislators that serve districts containing a given point.
:ref:`district-search`               /districts/`state`/[`chamber`/]                     List districts for state (and optionally filtered by chamber).
=================================   ==================================================  =====================================================================================
