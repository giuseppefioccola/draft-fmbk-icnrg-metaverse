---
title: Information-Centric Metaverse
abbrev: Metaverse
docname: draft-fmbk-icnrg-metaverse-latest
date: {DATE}
category: info

ipr: trust200902
area: Transport
workgroup:
keyword: Internet-Draft
submissionType: IETF

stand_alone: yes
pi:
  rfcedstyle: yes
  toc: yes
  tocdepth: 6
  tocindent: yes
  sortrefs: yes
  symrefs: yes
  strict: yes
  comments: yes
  compact: yes
  inline: yes
  text-list-symbols: -o*+

author:
  -
    ins: G. Fioccola
    name: Giuseppe Fioccola
    org: Huawei Technologies
    street: Palazzo Verrocchio, Centro Direzionale Milano 2
    city: Segrate (Milan)
    code: 20054
    country: Italy
    email: giuseppe.fioccola@huawei.com
  -
    ins: P. Mendes
    name: Paulo Mendes
    org: Airbus
    city: Taufkirchen
    code: 82024
    country: Germany
    email: paulo.mendes@airbus.com
  -
    ins: J. Burke
    name: Jeff Burke
    org: UCLA REMAP
    street: 102 East Melnitz Hall
    city: Los Angeles
    code: CA  90095
    country: USA
    email: jburke@remap.ucla.edu
  -
    ins: D. Kutscher
    name: Dirk Kutscher
    org: HKUST(GZ)
    city: Guangzhou
    country: China
    email: ietf@dkutscher.net


normative:

informative:

  IEEE-P2874:
     target: https://standards.ieee.org/ieee/2874/10375/
     title: IEEE SA P2874 Standard for Spatial Web Protocol, Architecture and Governance

  socialVR-measurements: DOI.10.1145/3517745.3561417

--- abstract

This document aims to explore the new challenges for the transport network
brought by the development of Metaverse.
It discusses the Metaverse as an Information-Centric Network (ICN).

--- middle

# Introduction

"Metaverse" is a place-holder for a range of new technologies and
experiences. Central to most working
definitions is the notion of shared, interoperable, and
persistent eXtended Reality (XR). Whereas most initial prototypes and
blueprints suggest leveraging or extending existing Internet and Web
protocols, there are performance
and scalability gaps between the vision and current technology, 
for example as reported by {{socialVR-measurements}}.

Some of the observed performance problems seem to stem from
fundamental gaps in today's Internet and Web technologies. For example, CDNs and similar techniques are used to address
the lack of scalable and robust multi-destination
communication, but have limited utility for bidirectional, low-latency interaction.    The ability to leverage computing in the network with
the required level of flexibility and trustworthiness is critical to
offloading services and enabling the ultra-low latencies that some
Metaverse applications claim to require, but challenging in the current architecture. Finally, the host- and connection-based semantics of IP are conceptually misaligned with the view from "inside" metaverse applications, where computing devices have little reason to be visible unless explicitly required by the application. 

Different remedies are being proposed to meet some of these challenges, e.g., providing more (costly)
deterministic communication services through resource reservation and
scheduling on the Internet, requiring/enabling the network to
understand application requirements and to provide corresponding QoS,
extended overlay infrastructure for reducing latencies for CDN-like
distribution etc, and data-centric application frameworks.

Alternatively, one might also take a more principled approach and
not take current design and deployment models as a given. Rather, we can
take the Metaverse as a broad candidate proposal for a future web
infrastructure that enables fine-granular 3D content exchange, rich
interaction between physical and virtual infrastructure, access to
static data and dynamic computation results for individual users as
well as for large user groups without the limitations of today's
platform- and overlay-based system -- i.e., conceive the Metaverse and
the future web as a fundamentally information-centric system.

This document addresses three aspects:

1. the documentation of requirements and observed gaps with the current
   technology stack;
2. a discussion of the applicability of different networking and
   distributed computing technologies; and
3. an initial discussion of a more fundamental and comprehensive
   re-design of a future web that provides useful services for
   Metaverse systems and beyond.


# Requirements and Gaps

{{?I-D.han-iccrg-arvr-transport-problem}} started to analyze the requirements
of Virtual Reality (VR) and Augmented Reality (AR) to networking from a
transport protocol perspective.
Some of the requirements are:

* low-latency and high-speed transport for dynamically generated content, to reach services in one-hop
  and for real-time user interactions;
* intelligent control and SLA real-time monitoring to convey the traffic and manage
  network resources and source/route re-selection;
* decentralization and Edge Services by positioning the data close to the user;
* security models implemented at the data object level;  
* effective operation when access to infrastructure is limited or unavailable; <!-- JB: I added this as it is a limitation to the current web that gets exacerbated in Metaverse applications, and has significant implications for disaster resilience, mobile/vehicular networking, etc. --> and
* reducing data sizes through resolution changes, compression, and
  more efficient encodings. <!-- Just-in-time content delivery decisions -->

{{socialVR-measurements}} performed measurements with five popular
social VR platforms. The experimental results revealed that all these
platforms face fundamental technical challenges considering the claims
that are often associated with the Metaverse. One issue was poor
scalability with respect to the number of users in one session:
throughput, end-to-end latency, and on-device computation resource
utilization increase almost linearly with the number of users. Other
issues include noticeable load and reduced achievable video rendering
frame rates and considerable network utilization even with smaller
numbers of users.

<!--The following paragraph is not quite right, but I think this type of point needs to be made as well, to really explain the benefits of rethinking the next generation web. -->
While it is less easily quantified, the impacts of network architecture complexity and resulting deployment expense on the difficulty of authoring (and collaborative editing) of next generation content that mixes media and code must also be taken into account.  Iteratively creating and publishing an interactive 3D application that performs well on the global internet, works offline, and is reasonably secure is a challenging task, which limits innovation and appears to drive prospective creators towards a small number of centralized platforms and services that homogenize features to the detriment of application variety. 


# Current and Emerging Mainstream Approaches

Different IETF technologies have been proposed to address some of the
above-mentioned issues and to provide a better service for Metaverse
applications. Below we list a few of them and will discuss
them in more detail in a future version of this document. For example,
on the network and transport layer, there are elaborate solutions
for dealing with bandwidth limitations, network congestion, lossy transport
protocols, and the ever growing size of video data, to address
the above requirements, for instance:

* MPTCP{{?RFC8684}} and MPQUIC{{?I-D.ietf-quic-multipath}} are the
  expansions of TCP{{?RFC9293}} and QUIC{{?RFC9000}} in order to
  dispatch packets over multiple paths to maximize throughput.

* Dynamic Adaptive Streaming over HTTP (DASH) aim to improve the
  viewport quality of immersive videos by refining tile
  delivery. But the client-driven nature of DASH introduces less control
  on the server side. <!-- JB: I'm not sure that the last sentence is applicable here as NDN-style ICN is receiver-driven? -->

* Media over QUIC (MoQ) ({{?I-D.ietf-moq-requirements}}) and
  extensions such as QuicR ({{?I-D.jennings-moq-proto}}) use similar
  concepts and delivery mechanisms to those used by CDN and named
  objects.  There are fundamental characteristics that QuicR provides
  for ultra low latency delivery, by leveraging the characteristics of
  QUIC protocol. <!-- This is a good reference - do we want to speak about the limitations?  e.g., session-based approach? -->

* The APplication-aware Networking (APN) aims to develop a framework
  to enable fine-granularity network service provisioning (traffic
  operations) within the network domain(s) that supports APN
  ({{?I-D.li-apn-framework}}). APN aims to use the ability to apply
  policies to traffic flows entering into the infrastructure.  In
  modern networks, where things such as deterministic networking and
  networking slicing are required, there is a requirement for more
  functionality than QoS can provide.

* The Computing-Aware Traffic Steering (CATS) aims to analyze the
  problem on the edge node, which makes a decision based on the
  metrics of interest, and then steers the traffic to a node that
  serves a service instance. Indeed, for AR/VR services, the
  performance experienced by the end users depends on both network
  metrics such as bandwidth and latency, and compute metrics such as
  processing, storage capabilities, and capacity.

In all of these approaches, the Metaverse is considered as an overlay
application with corresponding infrastructure dependencies, but this
potentially increases the current gaps (and resulting costs and
technical complexity) between distributed applications and the
underlying network architecture.

In the 3D hypermedia space, one proposal for a new "Spatial Web"
Framework is the HyperSpace Transaction Protocol (HSTP), as described
by {{IEEE-P2874}}, intended to "enable interoperable, semantically
compatible connections between connected hardware (e.g. autonomous
drones, sensors, smart devices, robots) and software (e.g. services,
platforms, applications, artificial intelligence systems)". The
specification, which is not accessible publically, is supposed to
include
<!-- JB: I think I have their book; I will try to add to this -->
1. a spatial range query format and response
language for requesting data about objects within a dimensional range
(spatial, temperature, pressure, motion) and their content.
2. a semantic data ontology schema for describing objects, relations,
and actions in a standardized way
3. a verifiable credentialing and certification method for
permissioning create, retrieve, update, and delete (CRUD) access to
devices, locations, users, and data; and
4. a human and machine-readable contracting language that enables the
expression and automated execution of legal, financial and physical
activities.

We cannot review the technical specification, but the feature
description seems to suggest an application layer protocol that would
enable more expressiveness and functionality in the "web" (i.e.,
application and presentation) layer, however based on the assumption
of existing networking technology and overlay approaches. 


# Opportunities and Challenges for Information-Centric Metaverse

Considering the gaps and perceived requirements from applications and
proposed application layer protocols, we can reason about a holistic
design that can address the aforementioned problems *and* provide a
more useful foundations for future hypermedia communication.

Information-Centric Networking (ICN) introduces named information objects, 
e.g. media contents, as the central concept as opposed to a physical computer,
or node ({{?RFC7927}}). In ICN approaches, the principal paradigm is not
host-to-host communication as in the current Internet architecture. Security is intrinsic to the data objects, rather than the channels that carry them, and secured data is forwaded based on names. 
The increasing demand for highly scalable and efficient distribution of content
has motivated the development of architectures that focus on information objects,
their properties, and receiver interest in the network to achieve efficient and
reliable distribution of such objects.

Therefore, we can conceive the Metaverse as an information-centric
system where most applications participate in granular 3D content
exchange, context-aware integration with the physical world, and other
Metaverse-relevant services.  There is strong semantic alignment between the network architecture and the application domain, in this case: We argue that the Metaverse is
inherently an information-centric concept that will become synonymous with the
network itself.  

Many applications and application concepts already work with data-oriented paradigms. Mapping them to a
host-centric network model creates complexity, security, and robustness issues, which can
be addressed with an ICN-oriented approach.

The overlay approach to deal with real-time interactive media adds significant
complexity. It is needed a fine-grained, hierarchical media exchange for low-latency
interactive communication that enables scalable multi-destination distribution,
and in-network replication and transformation that exposes object hierarchy for
fine grained access and security.  Mapping security between application data objects that are manipulated in a virtual environment independently of where they are stored requires a data-centric security paradigm even in the current network.  

Additionally, we propose that the Metaverse is an extension of the Web into immersive XR modalities
that are often aligned with physical space,  <!--Not sure about introducing physical space here, as we don't use it in the next point. Maybe a second point --> leveraging ICN concepts provides
support for decentralized publishing, content interoperability and co-existence,
based on general building blocks and not within separated application silos as
today’s initial prototypes.

In the following, we discuss issues with today's technologies, ICN
support that could be leveraged, and research opportunities for the
ICN community for four topics:

1. Scalable multimedia communication;
2. Interaction with applications;
3. In-network computing; and
4. Interoperability with existing infrastructure.

## Scalable multimedia communication

### Issues today

* Low-latency live streaming is not easy and not efficient in the Internet today. CDN-based DASH incurs high latencies.
* Current trends: blending of real-time interactive (WebRTC with RTP)
  and streaming (DASH), for example: Amazon Twitch, Meta Rush, IETF Media-over-QUIC, QuicR
* What is needed:
  * fine-granular media distribution that supports both interactive and streaming;
  * scalable multi-destination distribution, i.e., some kind of in-network replication;
  * ability to leverage wireless broadcast such as 5G Broadcast; 
  * approaches that function under limited connectivity and disconnected operation; and
  * support for hetergeneous devices and edge networks, i.e.,
    different quality layers, possibly dynamic transcoding.
	
### ICN Support

ICN is generally supporting most of these requirements:
* multi-destination distribution can be achieved through automatic in-network
  replication and interest aggregation, further supported by
  opportunistic caching.
* ICN provides a uniform interface for unicast and "multicast" (i.e.,
  there is no difference for consumers).
* IP-Multicast issues (inter-domain, routing scalability) do not apply
* Wireless broadcast could be leveraged where available, without
  requiring application-aware in edge routers.
* Receiver-driven operation conducive to supporting different quality
  levels, like in DASH today: receiver has all the knowledge directly
  (current performance) and can make timely decisions.
* ICN approaches enable local and intermittently connected name-based behavior more easier, as they do not rely on infrastructure for many name-based operations. <!--JB: refine-->
* Consumer mobility is an efficient operation in ICN (a non-operation).

### Research Opportunities

* Based on ICN's basic capabilities, actual systems would need specific
  approaches for quality adaptation, e.g.,
  * use of layered coding; and
  * role of in-network transcoding.
* In order to achieve low-latency and QoS, more work is needed for
  * fine-tuning with respect to interest aggregation, caching; and for
  * prioritizing "flows", e.g., audio over video.
* Sender mobility has seen some proposals in research that need to be validated.
* More experiments are needed with large-scale interactive multimedia communication
  and low-latency transport.
* Actual application development and deployment is needed to gradually
  develop best practices, software stacks, and re-usable application
  components.
  * For initial deployments, some kind of overlay topology over the
    current Internet might be needed.
  * Whereas the technology (different faces and underlay protocols) is
    essentially ready, there are other issues the deployment and
    efficient operation of such overlays (shortest path communication,
    routing, reliability).

## Interaction with applications

### Issues today

Many applications, not only the Metaverse, work inherently with
data-oriented paradigms when they are accessing named data, objects
etc. Mapping this to a connection-based communication model creates
complexities and robustness issues and would not result in good
abstractions for systems.

In Metaverse applications, data can potentially be shared efficiently
between nodes and within one node/process. Connection-based
communication models make it hard/impossible to do so.

Application layer data structures in VR (3D models, scene
descriptions) are based on (and should be secured within) object hierarchies, connection-based
systems may not be able to take advantage of it.

### ICN Support

ICN generally enables direct data-oriented communication: just names
and objects so that location, storage contexts (files etc.) become
less relevant. In addition ICN provides

* name-based APIs to applications;
* support for object collections through manifests;
* security models that revolve around objects rather than hosts; 
* reduced "hidden complexity" in infrastructure, such as cloud services configuration to support a specific applicaiton; 
* additional "middleware" such as dataset synchronization; and
* data-sharing is generally supported, which has benefits beyond
  networking, e.g., zero-copy sharing in processes etc.

### Research Opportunities

* Work should be expanded on the development of information-centric
  hypermedia concepts, i.e., linking from object collections to other
  objects/collections.
* Mappings between data-centric network objects and current hierarchical formats being considered as standards for Metaverse applications, such as USD <!--ref--> and glTF <!--ref--> should be explored. 
* Manifest technologies such as FLIC should be extended to support
  dynamically created objects.
* Concepts for dealing with "mutable objects" (or mutable
  "information") should be developed, i.e., how to deal with updates
  without giving up data immutability.
* The relationship between application-layer data-oriented operation
  and network-layer needs to be explored further, e.g.,
  * Would there be any differences?
  * Would it be needed to think about robust namespace mappings schemes?
* Concepts and mechanisms for privacy, selective attention, content
  filtering, and autonomous interactions, as well as ownership and
  control on the publishing side are needed.
* In general more applications should be developed to enable more experiments.

## In-Network Computing

In-network computing can support Metaverse systems in different ways:

* transcoding (to support heterogeneous receiver groups and networks better);
* coding for better communication robustness & efficiency;
* rendering for offloading clients and servers;
* compression and decompression on various levels, including semantic communication;
* ad insertion; and
* potentially for future decomposed Metaverse systems.

### Issues today

* In-network computing today is typically limited to coarse-grained
  CDN-style computing, include Multi-Access Edge Computing.
* Current trust and security frameworks require TLS connection
  termination, i.e., represent an overlay approach, which is not
  conducive to low latency communication.
* Dynamic, just-in-time, instantiation of computing function on
  application-agnostic platforms is not available.

### ICN Support

* The named-data approach is generally useful for distributed computing (NFN,
  RICE, CFN).
* ICN's security model makes it possible to do on-path computing / data transformation securely
* better performance under limited connectivity or disconnected operation; 
* discovery of functions and request forwarding can be punted on
  regular ICN mechanisms (name-based forwarding).

### Research Opportunities

* Robust distributed computing interaction models (RMI, Dataflow,
  REST) should be further developed and tested.
* Specific approaches such as in-network media transcoding should be developed.
* In general, more experiments for different types of applications are needed.

## Interoperability with existing infrastructure

In addition, the interoperability aspects also need to be investigated,
and, for example, Hybrid Information-Centric Networking (hICN), which
implements information-networking functionalities into IPv6
({{?I-D.muscariello-intarea-hicn}}, could provide a solution.

It would be theoretically possible to leverage the solutions mentioned in the
previous section in order to reach the above ICN oriented capabilities. But a
systemic approach would be highly desirable in the longer term.

# Security Considerations

TBD

# IANA Considerations

This document makes no request of IANA.

# Contributors

TBD

# Acknowledgements

TBD
