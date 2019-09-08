## Cross references to existing registry entries

Because we don't have URIs for EC registry items (yet), cross referencs should use the name of the target resource, with a fragment identifier prefix, e.g. 

```
#FDSN Web Service Specification Commonalities version 1.2
```
These should be updated with http URIs when the registry is deployed


## Dependencies

Several types:
- use of standards-- for specifications, use the 'dependency' relationship to link to standards that are used in part. If the specification is a profile, use 'profileOf'; this implies that conformance to the specification implies conformance to some base standard, as opposed to just importing some elements from a different spec (the distinctions can get fuzzy, the intension of the designer should determine).
- software library or modules used, e.g. NetCDF library (C libraries, JAVA JARs), or software modules available from repositories like pip, conda, for python, R, node.js environments.  
- stand alone desktop (or mobile) application used, e.g. Geoportal
- Web application/site, e.g. gitHub
- Generic dependency, e.g. database, storage system, web browser
- code interpreter, e.g. Python, R

## ConformsTo

for an interchange format, this field is a link to the specification document that defines the format.

## Scope

## Semantic Resources

The essential requirement for communication between agents is a shared conceptualization of the things that are being communicated about, and a shared set of labels (words) that map to the elements of that conceptualization.  In the case of people talking to each other must speak the same language. In many cases that is insufficient-- the pragmatics of human communication are specific to communities, and sometimes communication between members of different communities using the same language fails because of different conventions and practices in the use of that language.

Information engineers attempt to address these challenges using a variety of semantic resources with varying level of complexity and formality. These can be considered in a spectrum. Note the term word here is used in the general sense of [lexical item](https://en.wikipedia.org/wiki/Lexical_item). 

- Controlled Vocabulary -- A list of words, typically scoped as the value range for some property of an entity. A typical application is to construct pick lists in user interfaces. Defintions are not included, but the meaning of the words is assumed to be understood sufficiently by the community of users who will use the list.
- Glossary -- a collection of words with defintions. Some glossaries restrict an entry to have a single definition, in which case the entries in the glossary can be considered [concepts](https://en.wikipedia.org/wiki/Concept). If multiple definitions are allowed, the glossary is a mapping from a word to 1 or more concepts, useful for people evaluating the usage of the word in some context, but not for machine applications.
- Thesaurus -- a collection of words with a set of semantically fuzzy relationships suchs as broader, narrower, related, see also, use instead, synonym, antonym... Typically a thesaurus does not include definitions, and meaning is infered based on the asserted relationships.
- Conceptual model -- an explicit, symbolic representation of a knowledge domain that includes a collection of the kinds of things supposed to exist in the domain, a collection of relationships between those things, and a collection of logical rules that determine possible configurations of things in the domain. Various schemes are used to represent conceptual models, both graphical (UML, concept maps) and linguistic (OWL, KIF, RDFS)
- Vocabulary -- In information engineering usage: In the Semantic Web community, this term is applied to resources that are representations of conceptual models more formal than a Thesaurus or Glossary, but less formal than an ontology, typically consisting of a collection of entities and properties, in which domain,  range, and cardinality constraints might be only loosely defined (or not defined). Examples: Dublin Core, VOID, FOAF, DCAT 
- Ontology -- In information engineering usage: a [knowledge representation](https://en.wikipedia.org/wiki/Knowledge_representation_and_reasoning) using formal logic, typically based on some specific [description logic](https://en.wikipedia.org/wiki/Description_logic). 
- 

In the ECRR, various semantic resources are registered as a means of associating interchange formats and dataset schema with a semantic foundation.  Use of compatible conceptualizations, which might be represented by one or more of these semantic resources, is an essential prerequisite for harmonization of any data, and for most kinds of interoperability. 

## Labeled links:

One of the challenges of the standard approaches to linked data is the inclusion of opaque (or translucent...) URIs representing various entities. Some sore of label is almost always available when by dereferencing a URI. In a catalog environment in which a search result might include many URIs for linked resources, a search client application is faces with a choice-resolve/drill into URIs to obtain something useful for the user to understand, or present the bare URI and let the user guess.  We are advocating that links to resources that users might find useful should be labeled with some text to help the user-- basically the html anchor concept. The challenge is how to implement this in the schema.org environment we are working in.

For a simple related resource assocation, we are proposing to use sdo:isRelatedTo :

```

	"isRelatedTo": [
		{
			"@type": "Product",
			"name": "libmseed software library to support reading and writing miniSEED data",
			"url": "https://seiscode.iris.washington.edu/projects/libmseed"
		}
	],
```
The domain and range of sdo:isRelatedTo are sdo:Product and sdo:Service, so to use this property in a way that will validate with the structured data validation tool, the representation of any registry entry must declare among its '@Type' either sdo:Product or sdo:Service (or a subclass), whichever is more appropriate. 

use sdo:Thing to get name and URL

## Service, InterchangeFormat, Interface_API

The elements of web service that are important for the ECRR are 
1. what are the resources that the service operates on.
1. what are the representations of those resources used for input and output on the service
1. what operations are available to act on the resources.  

As an example, imagine a simple service that reports the current temperature from a thermometer. The resource offered is a temperature reading, let's say the representation is a JSON object the has two keys: 'ID'-- the themometer's identifier and 'temp'--the current temperature as a decimal number in degrees fahrenheit. The service runs on the HTTP protocol, accessing the service via the URL for that thermometer, and the only operation available is 'GET'-- i.e. tell me the current state of the offered resource.

A more complex service might provide temperatures from a collection of thermometers. Each thermometer could be considered a separate resource accessed by the service. Further functionality (and complexity) could be added by offering not only the current temperature, but the average temperature over the last 24 hours. The service now provides many offerings. Different service protocols have different mechanisms to request these various offerings. The most widely used current approach would be to implement different resource paths in the URL used to access the service (in sofware practice these are commonly called 'routes'). Something like http://mytemps.net/thermID to get the current temperature from thermometer thermID, and http://mytemps.net/thermID/avg to get the 24 hour average. 

Another layer of complexity is added when parameters can be added to the service request to modify how the resource state is reported.  In our temperatures service, one could imagine added parameters to specify whether the temperature should be reported in degrees Celsius or Fahrenheit, or to specify the interval over which the average should be reported (last hour, last day, last week...), or to specify the format for the response representation. In our http service implementation this might look like this: http://mytemps.net/thermID?u=celsius, or http://mytemps.net/thermID/avg?interval=week, using standard URL parameter encoding to ask for the temperature in degrees celsius or the average for the last week.

This last aspect gets into an important issue for interoperability on the web--there are many ways to serialize the same information so that it can be sent over a wire as a bitstream. In our example, we started with a JSON representation, but the same information could be transmitted as comma delimited text, or in an XML encoding, a set of RDF triples encoded in Turtle, or even a graphic image of a thermometer in a tiff file. The client receiving the information from the service must know how to interpret the bitstream that is returned from the service -- i.e. what is the interchange format.

The point of this example is to help clarify the language used to describe these resources in the EarthCube Resource Registry. In the example, the functionality accessible at http://mytemps.net is the service-- it is a particular instance of some software application that is running on that server. The Interface or API is a specification of the resources available, the operations that can be invoked, the mechanism for invoking those operations, and the interchange formats used to communicate any information required as input by the service, or transmitted from the service to the requesting client.  The service implements an interface. Communication across the interface uses some interchange format.

### Formats and Interchange formats or profiles

There is a continuum of complexity in the definition of file formats.  Let's consider files that are serialized as ASCII text. If I know a file is ASCII text, I can open the file using the simplest of text editors, e.g. something like notepad in Windows. This is the simplest kind of file format, and enables the most basic level of interoperability (communication) -- the computer can interpret the bits to reconstruct what was serialized. The file will open and show me a bunch of characters. If the characters form words with punctuation marks, then information can be transmitted from the originator of the file to me. That's the next level of interoperability -- syntax. If those words are in a language I understand and are arranged in a way I can interpret their meaning, then we have achieved some communication; this is the semantic level of interoperability. If this ASCII text file was a representation of some resource I found in a catalog, knowing that the **format** is 'text/plain; charset=us-ascii' would tell me I could open the file, but not necessarily that I could use it-- for that I would need the additional information that the content is in English. From the point of view of the registry, we could say that 'text/plain; charset=us-ascii, lan = en' is an **interchange format** useful to me for communicating textual information with my colleagues. 

Moving from the text realm to the data realm, let's imagine our text file actually contains data. To keep it simple, we'll say that data are in a table, with rows that are about particular entities and columns containing values that specify properties of the entity in the corresponding row. The basic format is still 'text/plain; charset=us-ascii', but if I want to extract the data from the file, more information is necessary. What is the column delimiter, what is the row delimiter, where are the column headings in the file, are there comments...  This is a common enough problem that there are stardard labels (MIME types) to identify some typical ways to serialize tabular data-- e.g. text/csv, text/tab-separated-values (leaving off the charset specification...). Having this more specific format information would assure me that the file should open in my software that reads these standard serialization formats, and I'll be able to look at the contents; we have syntactic interoperability.  If I actually want to use the data, I have to be able to understand what the column headings mean, and how the property values are specified--things like units of measure, or the meaning of terminology used. In order to be confident that this tabular data will work with my software application, I need to know these semantic details. In the ECRR, this would be represented as a registered interchange format (or profile).  Metadata describing the distributions available for a dataset can assert that the distribution conforms to some interchange format (identified by a URI), and metadata describing some software application could specify (via URI) the interchange formats that can be used as input. Catalog search clients could then match applications and data that will work together. 

This is obviously only the tip of the iceberg problem of matching data with applications in a distributed information system like EarthCube, but we are at a point where these linkages can be engineered for relatively simple cases. 


Service resources in the ECRR represent service instances. In many cases, a particular service instance is a one-of implementation for some specific application. In such cases, there is unlikely to be much documentation specifying the resources accessed by the service 
