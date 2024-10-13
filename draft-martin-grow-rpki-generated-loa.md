---
title: "Generating a Letter of Agency to reflect RPKI Validity"
category: info

docname: draft-martin-grow-rpki-generated-loa-latest
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
    email: algin@amazon.com
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
format for constructing an LOA in the case where RPKI validation
is available, with the goal of enabling a transition to a future
where LOAs are no longer needed.

--- middle

# Introduction

Some organisations use IP addresses that have been assigned or
allocated for their use and that are independent of any particular
network service provider. For such address space to be reachable
on the global Internet, routing information must be proagated from
the originating autonomous system and must be received by other
adjacent or further distant autonomous systems in order for traffic
directed at those addresses to be sent in the right direction.

Originating routes on behalf of customers has long been a common
function of network operators. At the time of writing it is also
common for other types of provider, e.g. those that provide services
such as content distribution or cloud compute. Such service providers
sometimes use the phrase "Bring Your Own IP" (BYOIP) to describe
the practice of using customer-supplied addresses to make services
available on behalf of those customers.

It is best current practice for network operators not to accept
routing information from adjacent networks indiscriminately, but
rather to filter routing information to ensure that Internet traffic
is not routed inappropriately, e.g. as a result of an operational
accident or a deliberate attempt to interfere. Network operators
are expected to accept routing information selectively, filtering
and allowing only route advertisements that they have reason to
believe are legitimate. In the case of routing information that is
being received directly from a customer network, or is being
originated by the network provider on the customer's behalf, such
legitimacy has often been determined by the customer issuing a
Letter of Agency, a document that provides authorisation for one
operator to act on behalf of another. Such letters are commonly
exchanged in the form of electronic documents exchanged over insecure
mechanisms like e-mail with no integrity protection or strong
authentication.

The RPKI provides an opportunity to provide equivalent authorisation
to a Letter of Agency in a form that is independently verifiable
and cryptographically strong. However, many network operators have
established workflows that include a requirement for a Letter of
Agency {loa} to be received in order to authorise the propagation
of a route on the Internet.

This document describes a template for constructing a Letter of
Agency in the case where RPKI verification of a request to exchange
routing informationi is possible. We propose that network operators
accept such documents, with corresponding crypytographic validation
of associated signed objects, in place of a conventional LOA.

# The Trouble with LOAs

# RPKI-Based Authorisation of Prefix Origination

# Constructing a LOA based on RPKI Validation

# Security Considerations

LOAs do not provide strong authorisation because it is not
straightforward to authenticate their contents. Anecdotally, LOAs
are often accepted by unqualified staff without systematic or
thorough review. It is not clear that the exchange of LOAs provides
useful legal protection to any party in the event that a related
subsequent complaint was made.

The form of LOA described in this document does not in itself provide
stronger authorisation; however, the contents do provide a means
by which the recipient of a LOA can obtain a measure of authentication
which is cryptographically strong. The recipient of a LOA of the
type described in this document who reads and follows the directions
within is able to obtain more confidence about the authorised use
of IP addresses described within it than with a conventional LOA.

# IANA Considerations

This document has no IANA actions.

--- back

# Acknowledgments

These fine and wonderful people helped with this document.

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

