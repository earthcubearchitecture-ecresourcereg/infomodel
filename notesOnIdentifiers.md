
# Identifiers for EarthCube


## Requirements:

In constructing the EarthCube registry, many resources need to be described and cross referenced, but in many cases these do not have existing indentifiers that can be resolved on the web to understand what the identifier represents. The registry requires a mechanism to assign identifiers for these resources that can be used for linking them in the context of the registry and to other resources on the web.

Ideally organizations participating in EarthCube will generate and maintain identifiers for products in their purview, but we can not assume this will be the case.  

1. low cost
2. reliability; resolution on the web need to be reliable and persistent for the lifetime of the resources. 
3. portable stewardship. As EC evolves, different organizations might operate the infrastructure
4. secure; only authorized individuals should be able to mint new identifiers or update existing redirects.


## Options:

### DOI 
DOI is an acronym for "digital object identifier", meaning a "digital identifier of an object". A DOI name is an identifier (not a location) of an entity on digital networks. The DOI system provides for persistent and actionable identification and interoperable exchange of managed information on digital networks [Introduction to DOI](https://www.doi.org/doi_handbook/1_Introduction.html)(#1.6.1). The design intention is to provide identifiers for use within the digital network in the digital supply (and rights) chain of all content, supporting the highest level of automation, trust, and accuracy. [See case study](https://www.doi.org/topics/150628_DOI_Case_Study_Paskin.pdf).  The DOI system is standardized as ISO 26324. The cost of registering new DOI names depends on the services using a DOI which are provided by a Registration Agency. Each Registration Agency is free to offer its own business model in compliance with overall DOI policies. Individual Registration Agencies adopt appropriate rules for their community and application. The DOI system implements the Handle System® (a general-purpose global name service enabling secure name resolution over the Internet) and the indecs Framework (a generic ontology-based contextual data model structure). The DOI name has two components, the prefix and the suffix, which together form the DOI name, separated by the "/" character. The portion following the "/" separator character, the suffix, may be an existing identifier, or any unique string chosen by the registrant. The portion preceding the "/" character (the prefix) denotes a unique naming authority. There is no limitation on the length of a DOI name.


### ARK

[IETF RFC specification](https://tools.ietf.org/id/draft-kunze-ark-21.html) 

The ARK (Archival Resource Key) naming scheme is designed to facilitate the high-quality and persistent identification of information objects.  A Name Assigning Authority (NAA) is an organization that creates (or delegates creation of) long-term associations between identifiers and information objects. An NAA does not so much create an identifier as create an association. In the ARK an NAA is represented by the Name Assigning Authority Number (NAAN). The ARK namespace reserved for an NAA is the set of names bearing its particular NAAN. For example, all strings beginning with "ark:12025/" are under control of the NAA registered under 12025. Each NAA is free to assign names from its namespace (or delegate assignment) according to its own policies. These policies must be documented in a manner similar to the declarations required for URN Namespace registration [[RFC2611]](https://tools.ietf.org/html/rfc2611)]. A typical technique is to hierarchically partition a namespace into subnamespaces using prefixes that guarantee non-collision of names in different partition.  Prefix-based partition management is an important responsibility of the NAA.

In order to derive an actionable identifier (these days, a URL) from an ARK, a hostport (hostname or hostname plus port combination) for a working Name Mapping Authority (NMA) must be found. This is refered to as a Name Mapping Authority Hostport (NMAH). The ARK scheme depends on having some method to locate the correct active NMAH given an NAAN.

In 2018 the California Digital Library (CDL) and DuraSpace announced a collaboration aimed at building an open, international community around Archival Resource Keys (ARKs) and their use as persistent identifiers in the open scholarly ecosystem.

As with any identifier scheme, persistence requires a redirectable reference to content in stable storage. EZID operates on a cost-recovery basis and can be used to manage your namespace, which includes minting and resolving ARKs (and other identifiers), as well as maintaining metadata. (http://n2t.net/e/ark_ids.html) 


### Handle system 
The Handle System is a general-purpose global name service that allows secured name resolution and administration over networks such as the Internet.  The Handle System manages handles, which are unique names for digital objects and other Internet resources (https://www.ietf.org/rfc/rfc3650.txt). The handle system provides an  operational connection between a handle and the entity it names that is maintained within the Handle System. It is probably best to view the Handle System as a name-attribute binding service with a specific protocol for securely creating, updating, maintaining, and accessing a distributed database. It has a self-contained administrative framework  that de-couples the ownership of each handle from the system administrators and allows access control to be defined for each handle value. The name-to-value bindings may also be secured, both via signatures to verify the data and via challenge response to verify the transmission of the data, allowing handles to be used in trust management applications.  The Global Handle Registry is a unique Local Handle Service which stores information on the prefixes (also known as naming authorities) within the Handle System and can be queried to find out where specific handles are stored on other Local Handle Services within this distributed system. (see also https://en.wikipedia.org/wiki/Handle_System)
Every handle consists of two parts: its naming authority, otherwise known as its prefix, and a unique local name under the naming authority, otherwise known as its suffix:

```
 <Handle> ::= <Handle Naming Authority> "/" <Handle Local Name> 
```

The naming authority and local name are separated by the ASCII character "/".  The collection of local names under a naming authority defines the local handle namespace for that naming authority.  Any local name must be unique under its local namespace. Naming authorities are defined in a hierarchical fashion resembling a tree structure.  Each node and leaf of the tree is given a label that corresponds to a naming authority segment.  The parent node notifies the parent naming authority of its child nodes.  Handles may consist of any printable characters from the Universal Character Set (UCS-2) of ISO/IEC 10646, which is the exact character set defined by Unicode v3.0. Handles can be used natively, or expressed as Uniform Resource Identifiers (URIs) through a namespace within the info URI scheme (http://www.rfc-editor.org/rfc/rfc4452.txt); for example, 20.1000/100 may be written as the URI, info:hdl/20.1000/100. Handles may also be expressed as HTTP URIs through the use of a HTTP proxy server,[12] such as: http://hdl.handle.net/10.1000/182.

Local Handle Services are intended to be hosted by organizations with administrative responsibility for handles under certain naming authorities.  A Local Handle Service may be responsible for any number of local handle namespaces, each identified by a unique naming authority.  The Local Handle Service and its responsible set of local handle namespaces must be registered with the Global Handle Registry. Local Handle service requires operation of Handle software (https://www.handle.net/download_hnr.html) on a server under control of the local naming authority.

CNRI requires a one-time $50 Registration Fee (for each new prefix allotted), and an Annual Service Fee of $50 per prefix, payable to CNRI. If you wish, you may choose to prepay the Annual Service Fee for up to five years.  CNRI provides specifications and the source code for reference implementations for the servers and protocols used in the system under a royalty-free "Public License", similar to an open source license.

# Discussion
Since DOI is a special case of the Handle system, it will not be discussed separately here, in particular because its stewardship model is more restrictive than we would like for EarthCube. 

Both the Handle System and ARK are based on the idea of distributed, hierarchical name management that allows naming authorities to delegate name assignment to subdomains. Both have a business model that incurs some cost to support persistent resolvable http URIs, but also instills confidence that they will persist. 

Use of the handle system would require EarthCube to operate a server for a local name authority, running the CNRI software.   For ARKs, there would need to be a similar 'resolver' that maps the ARK to a URL to get the resource. Various open source resolver projects exist (essentialy implementing what PURL does), or you can pay EZID to run your resolver at http://n2t.net.   ARK has some conventions that might not work so well with standard URL parsers, and requirements for Dublin core type metadata on registered items.  

At this point, I think the expedient course would be for EarthCube to register for an NAAN (nameing authority number) with ARK. In the REgistry spreadsheets, we can assign names that can be used to generate ARKs or handles. 
