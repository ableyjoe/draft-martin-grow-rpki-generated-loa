---
title: "Generating a Letter of Agency to reflect RPKI Validity"
category: info

docname: draft-martin-grow-rpki-generated-loa
submissiontype: IETF
v: 3
keyword:
 - RPKI
 - LOA
 - routing
venue:
  group: "Global Routing Operations"
  type: "Working Group"
  mail: "grow@ietf.org"
  github: "ableyjoe/draft-martin-grow-rpki-generated-loa"

author:
 -
    fullname: Algin Martin
    organization: Cloudflare
    email: algin@cloudflare.com
 -
    fullname: Joe Abley
    organization: Cloudflare
    email: jabley@cloudflare.com

normative:

informative:

--- abstract

Letters of Agency (LOA) are commonly used to authorise network
providers to accept and propagate routing information received from
others. The Resource Public Key Infrastructure (RPKI) can be used
to perform a similar function, with the advantage that RPKI-signed
objects can be validated automatically and in a more robust manner
than manual processing of documents. However, some network operators
have established processes that expect and require LOAs to be
exchanged, despite their limitations. This document proposes a
format for constructing a LOA in the case where RPKI validation
is available, with the goal of enabling a transition to a future
where LOAs are no longer needed.

--- middle

# Introduction

Some organisations use IP addresses that have been assigned or
allocated for their use and that are independent of any particular
network service provider. For such address space to be reachable
on the global Internet, routing information must be proagated from
one or more originating autonomous systems and must be received by
other adjacent or further distant autonomous systems in order for
traffic directed at those addresses to be sent in the right direction.

It is best current practice for network operators not to accept
routing information from adjacent networks indiscriminately, but
rather to filter routing information to ensure that Internet traffic
is not routed inappropriately.  Network operators are expected to
accept routing information selectively, filtering and allowing only
route advertisements that they have reason to believe are legitimate.

In the case of routing information that is being received directly
from a customer network, or is being originated by the network
provider on the customer's behalf, such legitimacy has often been
determined by the customer issuing a Letter of Agency (LOA), a
document that provides authorisation for one operator to act on
behalf of another. Such letters are commonly exchanged in the form
of electronic documents exchanged over insecure mechanisms like
e-mail with no integrity protection or strong authentication. For
more discussion about LOAs, see {{loa}}. Some limitations of this
practice are discussed briefly in {{security}}.

The RPKI provides an opportunity to provide equivalent authorisation
to a Letter of Agency in a form that is independently verifiable
and cryptographically strong. However, many network operators have
established workflows that include LOAs, and at the time of writing
many customers are not practised in the management of RPKI-signed
objects.

This document describes a template for constructing a Letter of
Agency in the case where RPKI verification of a request to exchange
routing informationi is possible. We propose that network operators
accept such documents, with corresponding crypytographic validation
of associated signed objects, in place of a conventional LOA.

# Terminology

{::boilerplate bcp14}

# RPKI Authorisation of Routing Data {#authorisation}

Suppose a customer requests that a network provide connectivity for
their addresses, represented by an IP prefix. The provider is
requested to either originate or accept the prefix from the customer's
network using the Border Gateway Protocol (BGP) {{!RFC4271}}. The
prefix will propagate from the provider's network with the AS_PATH
attribute "... P+ C*" where P is the provider's Autonomous System
Number (ASN), C is the customer's ASN, + denotes one or more
repetitions and * denotes zero or more repetitions.

For example, a customer with ASN C who advertises prefix A to the
provider's ASN P might intend that the prefix propagates to adjacent
networks of the provider with the AS_PATH "... P C". A different
customer who directs the provider to originate prefix B on their
behalf might intend that the prefix propagates to adjacent networks
of the provider with the AS_PATH "... P".

The RPKI can be used to sign and publish objects that reflect these
intentions. The two object types used for this purpose are the
Route Origin Authorization (ROA) and Autonomous System
Provider Authorization (ASPA) objects.

## Route Origin Authorization (ROA) Objects

ROAs are described in {{!RFC9582}}. A valid ROA provides authorisation for
a particular set of prefixes to be originated from a particular ASN.

In the examples above, a signed ROA might authorise prefix A to be
originated from ASN C, and another signed ROA might authorise prefix
B to be originated from ASN P.

## Autonomous System Proider Authorization (ASPA) Objects

ASPAs are described in {{!I-D.ietf-sidrops-aspa-profile}}. A valid ASPA
provides authorization for a particular prefix to propagate along
particular edges in a graph of connected autonomous systems.

In the examples above, a signed ASPA might authorise prefix A to
exhibit the AS_PATH "... P C".

# Bring Your Own IP (BYOIP) Considerations

Originating routes on behalf of customers has long been a common
function of network operators. At the time of writing it is also
common for other types of provider, e.g. those that provide services
such as content distribution or cloud compute. Such service providers
sometimes use the phrase "Bring Your Own IP" (BYOIP) to describe
the practice of using customer-supplied addresses to make services
available on behalf of those customers.

The provider may have many distinct and unrelated customers. A
customer who authorises the provider to attach their BYOIP prefixes
to their account does not normally authorise the use of those
prefixes by other customers. In order to ensure that a particular
BYOIP prefix is attached to an account that is authorised to use
it, a provider requires further authorisation from the customer.

{{?RFC9323}} provides a profile for RPKI Signed Checklists (RSCs) which
can be used by a resource holder as an attestation.

In the case of the examples in {authorisation}, the customer
responsible for BYOIP prefix A might sign an attestation that
authorises the use of that prefix with their account and make that
attestation available to the provider. The attestion might be
signed with the ROA or the ASPA associated with prefix A.
Similarly, the customer responsible for BYOIP prefix B might
sign a checklist authorising using the ROA associated with prefix B.

Authorisation of particular BYOIP prefixes to be attached to
particular provider accounts is necessarily provider-specific. Other
mechanisms are possible.

# Constructing a LOA based on RPKI Validation

## Overview

A LOA constructed according to this specification is referred to
as an RPKI Letter of Agency (RPKI LOA).

An RPKI LOA is intended to be a human-readable representation of
validation that can be performed by automated systems. Consequently,
there is no benefit in an RPKI LOA being machine-readable. Automatic
verification of authorisation to originate or propagate a prefix
SHOULD use RPKI-signed objects and perform signature validation
directly and SHOULD NOT attempt to process the contents of an RPKI
LOA.

This document specifies an ordering and a list of details that MUST
be included in a LOA for it to be consistent with this specification,
but provides few normative requirements for presentation or precise
wording. This is intended to provide sufficient consistency for the
LOA to be clear and familiar while leaving flexibility for providers
to be able to include additional information and adopt a style that
suits their particular circumstances.

This section describes a number of sections, all of which MUST be
included in an RPKI LOA. Some sections specify text to be included
verbatim, while other sections describe the information to be
included without prescribing particular text.

Where this specification requires particular text to be included
verbatim, that text MUST be included in English. RPKI LOAs MAY also
include translations of that normative text in other languages.
All other parts of an RPKI ROA MAY use any languages or scripts.

Each of the following sections MUST be separated so that they can
easily be distinguished from each other. Each section MAY include
a section heading.

## Introduction

This section MUST begin with the following text, included verbatim:

"This is an RPKI LOA that conforms to (this document)."

This section MAY also include other introductory information, such as
useful external references or information about the resource holder.

## Provenance and Validity

The name and contact details of the person or organisation issuing
the RPKI LOA should be included.

The date at which the RPKI LOA was prepared should be included.

## Route Origination and Service Provider Authorisation

This section MUST begin with the following text, included verbatim:

"The following route originations have been authorised by the
publication of RPKI-signed ROA and/or ASPA objects. Relying parties
should perform their own validation of these objects in order to
confirm the details provided in this RPKI LOA."

All prefixes that are the subject of the RPKI LOA MUST be included.

Each prefix MUST be clearly annotated with the origin ASN and also
the provider ASN in the case where the origin AS is not the same
as the provider AS.

This section MAY include references tools that a reader might use
in order to confirm the authorisations described in the document.
For example invocation syntax and example output from command-line
tools might be included in order to make it easier for a reader to
confirm the information described.

# Security Considerations {#security}

LOAs do not provide strong authorisation because it is not
straightforward to authenticate their contents. Anecdotally, LOAs
are often accepted by unqualified staff without systematic or
thorough review. It is not clear that the exchange of LOAs provides
useful legal protection to any party in the event that a related
subsequent complaint was made.

The form of LOA described in this document does not in itself provide
stronger authorisation; however, the contents do provide a means
by which the recipient of a LOA can obtain a measure of authentication
which is cryptographically strong. The recipient of an RPKI LOA who
reads and follows the directions within is able to obtain more
confidence about the authorised use of IP addresses described within
it than with a conventional LOA.

# IANA Considerations

This document has no IANA actions.

--- back

# Acknowledgments

Dirk-Jan van Helmond helped with the authors' tremendously poor
Dutch in {{nederlands}}. Note that any residual crimes against language
are the fault of the authors alone.

These fine and wonderful other people also helped with this document
(your name goes here).

# Letter of Agency {#loa}

A Letter of Agency (LOA) is a document used in telecommunications
to authorise a provider to act on behalf of a customer. LOAs were
originally specified for use in the United States and their use is
specified in the Code of Federal Regulations Title 47 / Chapter 1
/ Subchapter B / Part 64 / Subpart K, "S 64.1130 Letter of agency
form and content".

LOAs were subsequently adopted more informally for use between
Internet providers in order to document and authorise the use of
IP addresses administered by a customer to be made reachable by an
Internet Service Provider. The acronym LOA is sometimes taken to
mean "Letter of Authorisation" despite its original meaning.

# Example RPKI LOA

## Customer originates a prefix to a transit provider

INTRODUCTION

This is an RPKI LOA that confirms to (this document).

PROVENANCE AND VALIDITY

This document was produced by Cloudflare on behalf of a customer at
2024-10-13 15:00 UTC. For more information about this document, please
contact Cloudflare as follows:

~~~
  Cloudflare, Inc
  101 Townsend Street
  San Francisco
  CA 94107
  USA
~~~

  rpki@cloudflare.com

ROUTE ORIGIN AND SERVICE PROVIDER AUTHORISATION

The following route originations have been authorised by the
publication of RPKI-signed ROA and/or ASPA objects. Relying parties
should perform their own validation of these objects in order to
confirm the details provided in this RPKI LOA.

~~~
  PREFIX                  ORIGIN AS       PROVIDER AS
  199.212.90.0/24         9327            13335
  199.212.91.0/24         9327            13335
~~~

Route origin authorisation can be verified by reference to published
signed RPKI objects, as illustrated by the following:

~~~
  https://rpki-validator.ripe.net/ui/199.212.90.0%2F24/9327
  https://rpki-validator.ripe.net/ui/199.212.91.0%2F24/9327
~~~

## Customer directs provider to originate a BYOIP prefix in Dutch {#nederlands}

INTRODUCTIE

This is an RPKI LOA that confirms to (this document).

Dit is een RPKI LOA die voldoet aan (dit document). Dit document
is in het Nederlands geschreven en is bedoeld voor een publiek dat
Nederlands spreekt.  Het is van belang dat kernzaken van dit document
ook in het Engels geschreven zijn.

HERKOMST EN GELDIGHEID

Dit document is in opdracht van een klant door Cloudflare geproduceerd
om 2024-10-13 15:00 UTC. Voor meer informatie over dit document
kunt u contact opnemen met Cloudflare via:

~~~
  Cloudflare, Inc
  101 Townsend Street
  San Francisco
  CA 94107
  USA

  rpki@cloudflare.com
~~~

OORSPRONG VAN ROUTES

The following route originations have been authorised by the
publication of RPKI-signed ROA and/or ASPA objects. Relying parties
should perform their own validation of these objects in order to
confirm the details provided in this RPKI LOA.

De volgende route-oorsprongen zijn geautoriseerd door de publicatie
van RPKI-ondertekende ROA en/of ASPA-objecten. Afhankelijke partijen
moeten hun eigen validatie van deze objecten uitvoeren om de details
in deze RPKI LOA te bevestigen.

~~~
  PREFIX                  OORSPRONG AS
  199.212.92.0/24         13335
  199.212.93.0/24         13335
~~~

Route-oorsprong autorisatie kan worden geverifieerd middels verwijzing
naar gepubliceerde ondertekende RPKI-objecten, zoals geïllustreerd
middels:

~~~
  https://rpki-validator.ripe.net/ui/199.212.92.0%2F24/13335
  https://rpki-validator.ripe.net/ui/199.212.93.0%2F24/13335
~~~

