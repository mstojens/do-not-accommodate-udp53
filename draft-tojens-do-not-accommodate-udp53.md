---
title: "Discourage Accommodation for Classic DNS over UDP"
category: info

docname: draft-tojens-do-not-accommodate-udp53-latest
submissiontype: IETF
v: 3
# area: INT
# workgroup: dnsop
keyword:
 - Classic DNS
 - DNS over TCP
 - DNS over UDP

author:
 -
    fullname: Tommy Jensen
    organization: Microsoft
    email: tojens.ietf@gmail.com

normative:

informative:


--- abstract

Protocols that rely on Classic DNS have to account for considerations that only
apply to UDP, such as message fragmentation. However, DNS implementations are
already required to support both TCP and UDP, and using TCP would alleviate
these considerations. This document specifies that new protocols with a 
dependency on Classic DNS do not need to account for the limitations of Classic
DNS over UDP and can instead expect implementations to use Classic DNS over TCP.


--- middle

# Introduction

Many uses of the DNS require message sizes larger than common path MTUs. This
poses problems for Classic DNS over UDP by requiring peers to handle message
fragmentation. This is usually addressed by requiring peers to set the trucation
bit and repeat the query using Classic DNS over TCP, which wastes a round trip,
introducing delay in the network traffic that depends on the DNS data retrieval.

However, there are also worse implications, such as requiring IoT devices to do
polling {{Section 3.1 of ?RFC9726}}, additional recommendations for avoiding IP
fragmentation {{?RFC9715}} placing transport-like burdens on DNS implementors,
and normative requirements that exist only to deter the attack described in the
DNS threat analysis {{!RFC3833}}, such as having to consciously control source
port selection when initializing a DNS resolver with priming queries
{{?RFC9609}}.

These complications can be avoided by assuming Classic DNS over TCP is the only
form of Classic DNS that new protocols need to accommodate. The Implementation
Requirements for Classic DNS over TCP {{!RFC7766}} already require
DNS implementations to support Classic DNS over TCP wherever they support
Classic DNS over UDP and to treat TCP as a first-class transport rather than
only a fallback option from UDP. Therefore, it is safe to require protocols to
assume Classic DNS is always available over TCP without some specific scenario
limitations being defined.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Terminology

This document reuses terms defined in DNS Terminology {{!RFC9499}}, including
"Classic DNS".

# Requirements for new protocols

This document does not directly define new normative requirements for DNS
implementations. Instead, it defines requirements that apply to new documents
produced by the IETF. 

Authors of new IETF documents SHOULD NOT add normative requirements that are
only necessary to accommodate Classic DNS over UDP that are not necessary to
accommodate Classic DNS over TCP without sufficient explaination for why UDP is
a preferable transport to TCP (which would deviate from the Implementation
Requirements for Classic DNS over TCP {{!RFC7766}}).

Authors of new IETF documents SHOULD direct implementations to use Classic
DNS over TCP first by default instead of Classic DNS over UDP if the document
defines any usage of Classic DNS that is expected to need message truncation.
This will avoid a wasted round trip to switch from UDP to TCP. This requirement
does not in any way affect preference between Classic DNS and encrypted DNS.


# Security Considerations

Discouraging protocols from accommodating Classic DNS over UDP provides a
non-zero security benefit by defending against the attack described in the DNS
threat analysis {{Section 2.2 of ?RFC3833}}.

Increasing usage of Classic DNS using TCP will increase maintained state, but
this is not concerning because encrypted DNS protocols already require
connection maintenance and usage of Classic DNS over TCP is already common due
to increasing message sizes.


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

Many thanks for constructive feedback on this document to NULL.
