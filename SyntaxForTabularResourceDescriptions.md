# Introduction
## Schema development spreadsheets


# Resource types


## Software 

A packaged set of instructions that can be executed by a machine to perform one or more functions. Distribution might be as source code, an executable file, an installer package, as a deployable container (e.g. Docker container) or as a web application that can be invoked via URL. In the ECRR, software resources are distinguished from Service resourcs in that when sofware is invoked via URL (Web application), the user enters an environment created by the software; when a service is invoked, the user waits for response from service and uses that in the environment from which the service was invoked.  

Software can be registered at different granularity. For example a web application (e.g. GeoNetwork Opensource) can be registered as a generic software instance, with distribution linked to the source code, or as a specific geonetwork instance that hosts a particular metadata catalog, with a distribution linked to the online web application for that catalog.   A Jupyter notebook can be registered as software with a link to the .ipynb file in a GitHub, but might also be registered as an instance that is accessible in a particular onling Hub.   

For representation of properties desired for software in the ECRR using schema.org, software resources should be typed as sdo:SoftwareSourceCode. 

## Interface or API

A set of operations, messages used to invoke the operations, and inputs necessary to execute the operations, along with an expected output content and format. An Interface/API should have an associated specification document.  A Service resource must implement at least one interface. See discussion of Service, Interchange format, Interface/API, below

## Interchange format

A serialization scheme (data format) for communication between agents. Most general values are for generic formats specified by typical MIME types, but includes much more granular identification of schemes/Data Types/Data Specification that are specific to particular applications. These generally implement some underlying information model (schema and vocabulary) that enables semantic as well as syntactic interoperability.  An Interchange Format SHOULD have an associated specification that documents its provisions and usage. Registered interchange formats are intended to provide identifiers for data serialization formats that work with specific applications. Links to registered formats would be via URI values for the inputFormat and outputFormat properties on an Interface/API, Software, or Service resource.  In situations where no specification resource is available to document the format, it is sufficient to simply identify the format as an application specific format. This is the case for various commercial applications that use proprietary formats, as well as for scientific model input and output formats.

### Input and Output format encoding

Service, Software, and Interface/API resources have properties to specify the interchange format for input and output. In the case that the format is only specified at the generic syntax level, a MIME type for the file format is specified here. Formats that specify particular vocabularies, xml schema, or other requirements and enable a higher level of interoperability should in general have entries in the EC registry, and are linked via their registry URI to the resource that uses them. In unusual cases for which a MIME type,  EC Interchange Format identifier, or external identifier is not available, a short free text label for the format should be provided, and details elucidated in the resource description. 

## Service, Interchange format, Interface/API

The elements of web service that are important for the ECRR are 
1. what are the resources that the service operates on.
1. what are the representations of those resources used for input and output on the service
1. what operations are available to act on the resources.  

As an example, imagine a simple service that reports the current temperature from a thermometer. The resource offered is a temperature reading, let's say the representation is a JSON object the has two keys: 'ID'-- the themometer's identifier and 'temp'--the current temperature as a decimal number in degrees Fahrenheit. The service runs on the HTTP protocol, accessing the service via the URL for that thermometer, and the only operation available is 'GET'-- i.e. tell me the current state of the offered resource.

A more complex service might provide temperatures from a collection of thermometers. Each thermometer could be considered a separate resource accessed by the service. Further functionality (and complexity) could be added by offering not only the current temperature, but the average temperature over the last 24 hours. The service now provides many offerings. Different service protocols have different mechanisms to request these various offerings. The most widely used current approach would be to implement different resource paths in the HTTP URI used to access the service (in software practice these are commonly called 'routes'). Something like http://mytemps.net/thermID to get the current temperature from thermometer thermID, and http://mytemps.net/thermID/avg to get the 24 hour average. 

Another layer of complexity is added when query parameters can be added to the service request to modify how the resource state is reported.  In our temperatures service, one could imagine added parameters to specify whether the temperature should be reported in degrees Celsius or Fahrenheit, or to specify the interval over which the average should be reported (last hour, last day, last week...), or to specify the format for the response representation. In our http service implementation this might look like this: http://mytemps.net/thermID?u=celsius, or http://mytemps.net/thermID/avg?interval=week&f=json, using standard URL parameter encoding to ask for the temperature in degrees celsius or the average for the last week in the JSON output format.

This last aspect gets into an important issue for interoperability on the web--there are many ways to serialize the same information so that it can be sent over a wire as a bitstream. In our example, we started with a JSON representation, but the same information could be transmitted as comma delimited text, or in an XML encoding, a set of RDF triples encoded in Turtle, or even a graphic image of a thermometer in a tiff file. The client receiving the information from the service must know how to interpret the bitstream that is returned from the service -- i.e. what is the interchange format.

The point of this example is to help clarify the language used to describe these resources in the EarthCube Resource Registry. In the example, the functionality accessible at http://mytemps.net is the service-- it is a particular instance of some software application that is running on that server. The Interface or API is a specification of the resources available (temperature readings), the operations that can be invoked (GET), the mechanism for invoking those operations, and the interchange formats used to communicate any information required as input by the service, or transmitted from the service to the requesting client.  The service implements an interface. Communication across the interface uses some interchange format.

### Formats and Interchange formats or profiles

There is a continuum of complexity in the definition of file formats.  Let's consider files that are serialized as ASCII text. If I know a file is ASCII text, I can open the file using the simplest of text editors, e.g. something like Notepad.exe in Windows. This is the simplest kind of file format, and enables the most basic level of interoperability (communication) -- the computer can interpret the bits to reconstruct what was serialized. The file will open and show me a bunch of characters. If the characters form words with punctuation marks, then information can be transmitted from the originator of the file to me. That's the next level of interoperability -- syntax. If those words are in a language I understand and are arranged in a way I can interpret their meaning, then we have achieved some communication; this is the semantic level of interoperability. If this ASCII text file was a representation of some resource I found in a catalog, knowing that the **format** is 'text/plain; charset=us-ascii' would tell me I could open the file, but not necessarily that I could use it-- for that I would need the additional information that the content is in English. From the point of view of the registry, we could say that 'text/plain; charset=us-ascii, lan = en' is an **interchange format** useful to me for communicating textual information with my colleagues. 

Moving from the text realm to the data realm, let's imagine our text file actually contains data. To keep it simple, we'll say that data are in a table, with rows that are about particular entities and columns containing values that specify properties of the entity in the corresponding row. The basic format is still 'text/plain; charset=us-ascii', but if I want to extract the data from the file, more information is necessary. What is the column delimiter, what is the row delimiter, where are the column headings in the file, are there comments...  This is a common enough problem that there are stardard labels (MIME types) to identify some typical ways to serialize tabular data-- e.g. text/csv, text/tab-separated-values (leaving off the charset specification...). Having this more specific format information would assure me that the file should open in my software that reads these standard serialization formats, and I'll be able to look at the contents; we have syntactic interoperability.  If I actually want to use the data, I have to be able to understand what the column headings mean, and how the property values are specified--things like units of measure, or the meaning of terminology used. In order to be confident that this tabular data will work with my software application, I need to know these semantic details. In the ECRR, this would be represented as a registered interchange format, data specification profile, or data type (as construed by the RDA Data Type registry WG).  Metadata describing the distributions available for a dataset can assert that the distribution conforms to some interchange format (identified by a URI), and metadata describing some software application could specify (via URI) the interchange formats that can be used as input and are generated as output. Catalog search clients could then match applications and data that will work together. 

This is obviously only the tip of the iceberg problem of matching data with applications in a distributed information system like EarthCube, but we are at a point where these linkages can be engineered for relatively simple cases. 

### Linking Services, API, input and output formats in the Registry

Service resources in the ECRR represent service instances. A service instance must implement some interface, and that interface utilizes some input and output formats for communication.  In many cases, a particular service instance is a one-of implementation for some specific application, and there is no registered resource for either the interface or the input or output interchange format. In these cases, all information describing the service operation must be included in the Service registration.  In cases where there are specifications (with identifiers) for the interface or input or output interchange format, these can be linked either to a Specification in the ECRR, or if there is a resolvable URI that can be used as the link target. 

The recommended approach is to 1) register each service instance, with the particular endpoint base URL for that instance; 2) Register the API(s) that the service instance implements. The API will link to a variety of potentialAction objects that specify the details of the resources (objects) that are operated on, the actions that can be invoked, a template string for invoking the action, any parameters required by the template, required input formats (typically for HTTP POST or PUT requests), and output formats offered by the service. Content negotiation using HTTP headers should be documented in the text description for the action. 3) If a specification document is available that documents the service, the service instances and the API should both be linked to the specification via the 'Conforms to' property.

Some APIs are generic enough that service instances will implement Potential actions that are more specific than the API specifiction. For example and Open Geospatial Consortium Web Feature Service (OGC WFS) getFeature action can operate on a wide variety of object types (the feature type in OGC terms). Thus, Potential Actions that restrict the generic actions in the API specification need to be specified on the service instance. *[is a property on Potential action needed to indicate the generic action that it restricts?]*

If the input and output formats for communication with a service are not specified in detail, with only a MIME type, the standard MIME type label (e.g. text/xml, text/csv) can be used. 

### Potential Actions

The potential action object is used to describe the offerings available from a service. The design is based on the schema.org PotentialAction object. The action is associated with one or more Interface/API or Service resources (see discussion above). Along with the standard Name and Description properties, the object includes a link to the Interface/API or Service resources that implement the action. Input and Output format fields specify the interchange profile for getting data in and out of the action endpoint. See the discussion of Format specification. It is not unusual for various input or output formats to be available; in these cases, the Target template should include parameters for specifying what formats will be used. Currently the Action model can not represent format requests based on HTTP headers for content negotiation. The Action Object identifies the resource that a Service or API acts on. Output from the Action will typically be a representation of the Action object serialized via the specified OutputFormat. 

The *HTTP method* property is one of GET, PUT, POST, DELETE. Since the current PotentialAction model only provides for a URI template to specify requests, only GET and DELETE are currently supported. For services that do not operate via HTTP, this property should be NULL. The Operations property allows free text labels for service capabilities beyond the basic CRUD functionality of HTTP. 

The *Target template* field should contain a template string following the IETF RFC6570 URI template specification. By convention, {BaseURL} should be used as the parameter representing the *Base URL* value specified in a service instance registry description.  Each parameter that will be substituted in the template should be enclosed in braces ('{...}'). 

The *Template parameter* field provides definitions for populating parameters in the *Target template*.  In the long run, a separate 'PropertyValueSpecification' object is warranted, following the pattern used in the schema.org Actions implementation.  For the time being, a lot of information is packed into this field, using some invented syntax to distinguish various aspects:

- parameter names from the template are prefixed with two hyphens and enclosed in braces. e.g. 
```
--{typeNames}
```
- parameter values are strings by default. If a different data type is  expected, it is specified by suffix on the parameter name, with a colon (':'). e.g.  
```
{maxlatitude}:double
```
- any explanation of the parameter value follows the parameter name (and data type if present), separated by a space and terminated by a period ('.'). e.g. 

```
--{dataTypeID}:integer Identifies data type, list delimiter is '|'.
```
- If there is a controlled vocabulary for the parameter, the values are specified by a pipe ('|') delimited list enclosed in parenthesis. e.g. 
```
--{format} output file format options. (json|xml)
```
- If the parameter is populated with code values instead of terms, the term (or explanation) associed with a value follows each value separated by a colon (':') e.g.  
```
--{dataseries}  string identifying NWIS data categories. (uv:current data |
dv:daily values | gwlevels:groundwater levels | peak:peak stream flow at gage
| inventory:site information | measurements:streamflow data for site)
```
- For some particularly complex templates, parameters are specified in groups. The group names are the parameters in the template, but instead of using the {? var1, var2, var3} notation, the template is like ?{group1}&{group2}&group3. Each group consists of a set of parameters enclosed in parenthesis ('(  )'), with each parameter in the group enclosed in braces ('{  }'), following conventions for the template parameters.  In some particulary complex. APIs, the parameter value specification is a term enclosed in angle brackets ('<  >'), in which case the user should refer to the service documentation to understand the constraints of parameter values.  e.g.

```
--{channel-options} ({net} (<network>), {sta} (<station>), {loc}
(<location>), {cha} (<channel>))
```
in this example, *channel-options* is the group. The group name does not actually appear in a request. The parenthesis following the space after the group name enclose the parameters in teh group, each parameter specification separated by a comma. The range specification for the parameter, enclosed in parenthesis, is a term enclosed in brackets (e.g. <network>, <station>) indicating that the value for the parameter has constraints that are not specified here in the registry.  

Obviously this parameter specification scheme will need to be a work in progress. Hopefully we can move these conventions forward in teh community for better machine-actionable description of service requests.

## Semantic Resources

The essential requirement for communication between agents is a shared conceptualization of the things that are being communicated about, and a shared set of labels (words) that map to the elements of that conceptualization.  In the case of people talking to each other must speak the same language. In many cases that is insufficient-- the pragmatics of human communication are specific to communities, and sometimes communication between members of different communities using the same language fails because of different conventions and practices in the use of that language.

Information engineers attempt to address these challenges using a variety of semantic resources with varying level of complexity and formality. These can be considered in a spectrum. Note the term word here is used in the general sense of [lexical item](https://en.wikipedia.org/wiki/Lexical_item). 

- Controlled Vocabulary -- A list of words, typically scoped as the value range for some property of an entity. A typical application is to construct pick lists in user interfaces. Defintions are not included, but the meaning of the words is assumed to be understood sufficiently by the community of users who will use the list.
- Glossary -- a collection of words with defintions. Some glossaries restrict an entry to have a single definition, in which case the entries in the glossary can be considered [concepts](https://en.wikipedia.org/wiki/Concept). If multiple definitions are allowed, the glossary is a mapping from a word to 1 or more concepts, useful for people evaluating the usage of the word in some context, but not for machine applications.
- Thesaurus -- a collection of words with a set of semantically fuzzy relationships suchs as broader, narrower, related, see also, use instead, synonym, antonym... Typically a thesaurus does not include definitions, and meaning is infered based on the asserted relationships.
- Conceptual model -- an explicit, symbolic representation of a knowledge domain that includes a collection of the kinds of things supposed to exist in the domain, a collection of relationships between those things, and a collection of logical rules that determine possible configurations of things in the domain. Various schemes are used to represent conceptual models, both graphical (UML, concept maps) and linguistic (OWL, KIF, RDFS)
- Vocabulary -- In information engineering usage in the Semantic Web community, this term is often applied to resources that are representations of conceptual models more formal than a Thesaurus or Glossary, but less formal than an ontology, typically consisting of a collection of entities and properties, in which domain,  range, and cardinality constraints might be only loosely defined (or not defined). The names of the entities and properties constitute the vocabulary. Examples: Dublin Core, VOID, FOAF, DCAT 
- Ontology -- In information engineering usage: a [knowledge representation](https://en.wikipedia.org/wiki/Knowledge_representation_and_reasoning) using formal logic, typically based on some specific [description logic](https://en.wikipedia.org/wiki/Description_logic). 

In the ECRR, various semantic resources are registered as a means of associating interchange formats and dataset schema with a semantic foundation.  Use of compatible conceptualizations, which might be represented by one or more of these semantic resources, is an essential prerequisite for harmonization of any data, and for most kinds of interoperability. 

## Repository

A storage system in which objects may be stored for subsequent access or retrieval (generalize from Kahn and Wilensky, 1995, http://www.cnri.reston.va.us/k-w.html)

## Specification

A document that describes the technical characteristics of an artifact or practice, possibly including a description of what it should do, or an explicit set of requirements that it must satisfy (http://en.wikipedia.org/wiki/Specification).  Software, Interface/API and Interchange format resoruces may have an associated specification linked via the 'Conforms to' property.  Specifications may be hierarchical, e.g. a profile specification might inherit provisions from a parent specification, and add additional constraints to further restrict usage of the artifact or practice.


----------
# Identifiers 

Items registered in the EC Resource Registry are assigned unique identifiers in the scope of the registry. The final technology for generating http URIs for the registered resources and resolving those identifiers on the web. To develop the test descriptions, we will assign unique integers to all test descriptions in the RegistryID column. 

# Schema.org encoding 

## General considerations

### Cross references to existing registry entries

Because we don't have URIs for EC registry items (yet), cross referencs should use the name of the target resource, with a fragment identifier prefix, e.g. 

```
#FDSN Web Service Specification Commonalities version 1.2
```
These should be updated with http URIs when the registry is deployed

### Labeled links:

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

### Nil reasons

- Inapplicable (http://www.opengis.net/def/nil/OGC/0/inapplicable). Property does not apply in the current description
- Missing (http://www.opengis.net/def/nil/OGC/0/missing). Value exists but is not provide
- Template (http://www.opengis.net/def/nil/OGC/0/template) - Value will be available at a later date.
- Unknown (http://www.opengis.net/def/nil/OGC/0/unknown). Value exists but is unknown
- Withheld (http://www.opengis.net/def/nil/OGC/0/withheld). Value withheld for security or privacy concerns


## Fields common to all resources

Fields are listed alphabetically here. 

### Citation
The citation for this resource. Recommend following ESIP (RDA?) guidelines for resource citation. UI implementer's should use ESIP guidelines for data and software citation. Value is mandatory, but nilable. Use appropriate nil value URI.

```
dc:'Bibliographic Citation' max 1 rdfs:Literal
```



### Dates

Explicit field name not needed..."	tuples of {dateType, Date}, where date type indicates events in history of resouce, e.g. created, updated, published, superseded..  Date should use one of the xml schema date formats (date, dateTime, gYear, gMonthYear)	"All ECResources can use:

* sdo:dateCreated max 1
* sdo:dateModified some
* sdo:datePublished some
* sdo:expires some

Date or DateTime"	"Mandatory to use appropriately: 

On creation use dateCreated
On update create or update dateModified

If the resource has died (i.e., no longer accessible or useful), use expires

If a process is used to verify resource entry before it is made publicly visible use datePublished"	On any date use today	flag development activities with status

### Dependencies

Names of resources that must be accessible and usable in order for the resource to realize its intended function	"ro:""depends on"" some (ecrro:'ECResource' or URL or Text)

Several types:
- use of standards-- for specifications, use the 'dependency' relationship to link to standards that are used in part. If the specification is a profile, use 'profileOf'; this implies that conformance to the specification implies conformance to some base standard, as opposed to just importing some elements from a different spec (the distinctions can get fuzzy, the intension of the designer should determine).
- software library or modules used, e.g. NetCDF library (C libraries, JAVA JARs), or software modules available from repositories like pip, conda, for python, R, node.js environments.  
- stand alone desktop (or mobile) application used, e.g. Geoportal
- Web application/site, e.g. gitHub
- Generic dependency, e.g. database, storage system, web browser
- code interpreter, e.g. Python, R
 
Mandatory if applicable		dependencies that user has to access are a function of the distribution (e.g. installer, source code, docker container).  The description could point to the URL of a document containing the list of dependencies.



### description
Free text description of the resource including a text description of provenance if that is available and important to the registering agent. Value is mandatory.
```
sdo:description some Text
```

### Expected lifetime
Qualitative specification of how long the resource is expected to remain accessible, valid and usable for intended function. Clearly this will be a guestimate or asipration, but is intended to help users searching the registry asses whether they should use the resource.  Value is optional.  
 Based on the test descriptions, the starting controlled vocabulary is: 
- '> 5 yr'
- '> 1 yr'
- unknown
- long term
- not applicable

```
ecrro:expectedLifetime  Range: some Text
```


### External identifier
Unique identifer string for the resource, ideally a resolvable and globally-unique URI. Value is optional.

```
sdo:identifier some (sdo:PropertyValue or Text or sdo:URL)
```
		
### Intended audience/target user

Term to specify the type of agent intended to utilize the resource. Value is optional.

```
sdo:audience some ECAudience
```

The proposed enumeration of ECAudience is:
- data producers
- data users
- data facilities & repositories
- technologists
- data intermediary
- developers
- scientists
- infomaticists
- members of the public
- all
		

### keywords
Index terms to guide discovery. Value is Optional
```
sdo:keywords some Text
```
Multiple keywords in the Text value should be a comma delimited list. Thus no commas are allowed in individual keywords.

### License
Legal or security constraints that restrict access, usage or sharing of the resource. If a URL is available to access the license text, provide that, otherwise some text explanation of usage constratins should be provided.  Value is mandatory.

```
sdo:license some (URL or Text)
```

### maturity/status	

Maturity assessment category (e.g. ESIP software assessment).  Indicates the degree to which the resource has been tested and validated for accuracy and conformance to applicable specifications.	Value is optional.

"ecrro:'has maturity state' some 'EC resource maturity'

EC resource maturity enumeration items have URI's; the plan is to register the controlled vocabulary in the ESIP Community Ontology Registry (COR). Enumeration values are currently:
- 'unknown'
- 'planning'
- 'in development >1 yr out' 
- 'in development <1 yr out' 
- 'alpha or rough draft' 
- 'beta or complete draft' 
- 'production ready'
- 'in production' 
- 'superceded'
- 'not accessible'"	

Default value is  'unknown'

### Name
Text label for the resource. The name should be informative and uniquely identify the resource for human users. Value is mandatory

```
sdo:name some Text
```
		
### primary publication
Identifier for publication that specifies the component, e.g. DOI or a standard academic citation.	Value is optional.


### Related resources
List of URL or citations for other resources of interest related to the resource	"sdo:isRelatedto some (ecrro:'ECResource' or Text or URL)

Do we care what the relationship is?  If so then role codes and such come into play"	Optional 		have to declare the resource being described to be an sdo:Product or and sdo:Service; have to figure out sdo encoding, use? additional property?? No inverse relationships needed

### Responsible Party

Explict field name not needed...	property to assign authorship or other contribution relationships to the resource. At least one of the available set of fields is mandatory.  If the identified agent is a person, use ORCID to identify if possible, if the agent is an organization, use either FundRef ID's or ROR's if possible. 
```
Any/all of:
[sdo:contributor | sdo:editor | sdo:creator | sdo:publisher] 
        some sdo:Person or sdo:Organization"
```

### Science domain/community	
Discipline categories to classify intended or actual users; 

Use AGU sections list? No - not granular enough
SWO:topics? Need to add all of the Earth Science topicsbepress:heirarchy?

This is a question of level of granularity "	Use Ruth's extensions to the SWO:topics for Earth Science - publish it			Discipline agnostic should be a term

### Stewardship
Agent (i.e., individual, individuals, organization, or a position/role) that is responsible for operation, availability, and maintainence of the resource. Please use Orcids, FundRef Ids, or ROR's if at all possible. A value is Mandatory. The default value will be whomever is logged in/currently authorized to insert the record.
```
"eccro:'stewarded by' some (sdo:Person or sdo:Organization)
```


	
### Type

The type of resource this is. Value is mandatory, and must be from the ECResourceType vocabulary. Other types can be declared using the JSON-LD @Type key. 
Current ECResourceType vocabulary: 
- Software
- Interface_API
- Interchange format
- Repository
- Service
- Semantic Resource
- Specification
- Catalog or registry
- Platform
- Use Case
- Bundled Object


```
ecrro:ECResourceType max 1 ecrro:ECResourceTypes
```


### URL to user-readable page
URL that locates a human-readable website or web page with documentation about the resource. Value is optional.

sdo:url some (sdo:URL or CreativeWork)

### usage
categorization of the number of users (people, projects, applications...) for the resource. Values are optional. Represented with two fields: 
1. general usage specifies the approximate totla number of users
1. disciplinary usage indicates whether usage is primarily by technologists or scientists

```
ecrro:'has usage' max 1 ecrro:'EC general usage' 
and
ecrro:'has usage' max 2 ecrro:'EC disciplinary usage'
```
'EC general usage' controlled vocabulary:       
- not applicable:  use for resources that are in development or not released for public usage
- low usage: Usage mostly limited to a single project or lab and close collaborators
- some usage: Usage by multiple entities
- wide usage: Used by many independent entities
- unknown: Default if no value specified

### version
Identifies the particular version of a resource described by the Registry record. Value is optional.

```
sdo:version some Text
```
		
## Format

There are a variety of properties that describe format in different context: 1) representation format for a semantic resource; 2) base format for an interchange format; 3) input format (Service, API, Software); 4) output format (Service, API, Software). 

These can all implemented using sdo:encodingFormat in different contexts. The range of sdo:encodingFormat is Text or URL. In the test descriptions in many cases the formats are specified at the level of a standard MIME type, but for some resource we have identifiers for interchange formats registered in the EC registry. These registered formats are typically more proscriptive than the MIME type definition, but are based on a MIME type. For example ouput from an FDSN station data service is encoded in XML using a particular xml schmea. The Specific output format is identified by the XML schema namespace URI, but this output would also conform to the MIME type application/xml. In the format values in the registry this hiearachical relationship is indicated using the angle bracket '>' so the output format in the FDSN station example would be represented by 

```
application/xml > http://www.fdsn.org/xml/station/1 
```


## Resource specific encoding

In this section, guidance is provided for encoding details specific to each resource type.

### Interface/API

#### Communication protocol
#### Input format
#### Output format
#### Operations
#### Offering

This field provides a list of the resources that can be accessed via the service. In a RESTful Web API implemented on HTTP, each resource offered will have a unique identifer, typically a path (route) anchored in the service base URI, and the operations available are the basic HTTP CRUD operations PUT, GET, POST, and DELETE. Query parameters appended to the resource URI are used to modify the representation returned, for example by subsetting, filtering, regridding, resampling. There is a great deal of variability in the actual implementations of services invoked on the Web; in many cases resources and operations are specified by query parameters in service requests. 

Operations and interchange formats might vary for different resources offered by the service. , also need to include url template for each offering. Note the description of WebAPI offerings needs to be updated in light of discussions at https://github.com/earthcubearchitecture-project418/p419dcatservices.  sourceProfiles are scoped to WebApi specification offerings, and to individual offerings on a service instance if they are different from the sourceProfiles in the WebAPI specification (profile on instance should be consistent with profile on WebAPI specification).

#### Conforms to

For an interchange format or interface, this field is a link to the specification document that defines the format.  Ideally the specification should be registered as a specification resource in the registry, but any resolvable URI is valid. 


Conforms to


