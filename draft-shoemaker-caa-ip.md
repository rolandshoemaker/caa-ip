---
title: "Certification Authority Authorization (CAA) Validation for IP Addresses"
abbrev: CAA-IP
docname: draft-shoemaker-caa-ip-latest
category: std

ipr: trust200902
area: General

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: R. B. Shoemaker
    name: Roland Bracewell Shoemaker
    org: ISRG
    email: roland@letsencrypt.org

normative:
  RFC1034:
  RFC2119:
  RFC2317:
  RFC2818:
  RFC6844:

--- abstract

The Certification Authority Authorization (CAA) RFC specifies a method for users
to restrict which Certificate Authorities (CAs) are authorized to issue certificates
for their DNS domain names. This document extends that specification to provide
a method for holders of IP addresses to do the same.

--- middle

# Introduction

asd

# Terminology

In this document, the key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL
NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" are to be
interpreted as described in BCP 14, RFC 2119 {{RFC2119}} and indicate
requirement levels for compliant ACME-Wildcard implementations.

# Certification Authority Processing For IP Addresses

Before issuing a certificate containing a IP address a compliant CA MUST check
for the relevant CAA Resource Record set. If such a record set exists a CA MUST
NOT issue a certificate unless the records in the set are consistent with the
request and the policies of the CA.

A certificate request MAY specify more than one IP address in which case CAs
MUST verify the CA Resource Record set for all the IP addresses specified in
the request.

As defined in RFC 2818 {{RFC2818}} IP addresses in certificates must match
exactly with the requests URI so CAA records with the "issuewild" tag MUST not
be considered part of the relevant Resource Record set.

Unlike the mechanism defined in RFC 6844 {{RFC 6844}} this mechanism doesn't
involve climbing the DNS tree and only requires querying a single DNS name. The
relevant Resource Record set for a given IP address is found by querying the
reverse mapping zone for the IP for CAA records.

Given a certificate request containing the IPv6 address "2001:db8::1" the relevant
query for the reverse mapping within the IP6.ARPA {{!RFC3596}} would be:

~~~~~~~~~~
1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.b.d.0.1.0.0.2.ip6.arpa. IN CAA
~~~~~~~~~~

And for a request containing the IPv4 address "192.0.2.1" the relevant query for
the reverse mapping within the IN-ADDR.ARPA {{!RFC1034}} would be:

~~~~~~~~~~
1.2.0.192.in-addr.arpa. IN CAA
~~~~~~~~~~

When doing queries CAs SHOULD either use a resolver that chases CNAME records or
manually chase CNAMEs themselves in order to allow for zone delegations {{!RFC2317}}.

# Security Considerations

## Use of DNSSEC

# IANA Considerations

## Certification Authority Restriction Properties

Change the contents of the Meaning column for the "issue" Tag to say
"Authorization Entry by Domain or IP address" and add "RFCXXXX" to the
References column.

# Acknowledgments

