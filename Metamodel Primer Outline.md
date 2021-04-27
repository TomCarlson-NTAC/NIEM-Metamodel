# Model Primer Outline

**Primer needs to be standalone, but linked to other materials.**

**Should we tie in the standards with a line or two?**

Post here: https://github.com/NIEM/NIEM-Modeling-Formalism

## Background: How We Got Here

NIEM began as a framework for building and defining messages to facilitate exchanges of information. Part of that framework was a defined process. Part of the framework was a model that formed a base on which exchanges were defined. That base would be both carved down _and_ extended to meet the needs of each exchange. That base existed, and still exists, as a set of XML Schema documents.

As other means of exchanging information have become popular, notably JSON<sup>[1](#json_fn)</sup>, the community needs have expanded to using NIEM with exchange mechanisms other than XML Schema. JSON is the current alternate means, but many more exist.

Dealing with this issue, both now and in the future, is the rationale for the Metamodel.

## Problem/Issue

This introduces a problem. How do you translate the model from XML Schema to some form of JSON, when XML Schema and JSON have similar, but definitely different, feature sets? This isn't impossible. We do have a specification and other guidance for this translation, but the translation isn't trivial and the results may not be as satisfying as possible.

The situation is shown below, the dotted line representing the incomplete translation between these two technologies.

![Two Technologies](two_technologies.svg)

Still, this conversion is manageable. When more technologies get added, the translations get out of hand, especially as the community does work in these other technologies. How does work done in JSON get translated to RDF<sup>[2](#rdf_fn)</sup>? How accurate is that translation? Does that work get translated to XML Schema via JSON directly or via RDF?

The "N-squared" diagram is familiar to anyone who has seen many presentations about NIEM. Usually it's representing a variety of entities making an ever growing number peer-to-peer sharing agreements. The same diagram applies here, as a multitude of technologies start requiring peer-to-peer conversions between technologies.

![N-Squared Technologies](many_technologies.svg)

At the message level, NIEM provides the means to define a single centralized and standardized format for the exchange, replacing the complexity of numerous peer-to-peer agreements. Instead of peer-to-peer agreements, everyone implements towards the standard.

The same concept applies here with models and technologies. Instead of individual translations between technologies, there's one centralized and standardized "model instance." Different technologies are translated from that standard model. Now the lines are solid, as each translation can better leverage the abilities of a particular technology.

![Standardized and Centralized](model_centric.svg)


Currently, modeling concepts are embedded in a specific technology, XML Schema. We overload XML Schema concepts to include real-world concepts. We use XML Schema to both define NIEM _and_ act as the tool for validating actual messages.

## Solution

The solution is to create the modeling concepts in a conceptual format instead of embedding them in XML Schema. Instead of implying modeling concepts in XML Schema, we explicitly define them in a Model Instance.

Allows for **concept**-to-technology conversions, e.g. Model -> XML Schema and Model -> JSON. This is easier.

## How It Works

"Metamodel" is a framework for building models. The metamodel itself isn't the NIEM model. It's a "neutral modeling formalism," for these models. It's a means of defining a model. It provides the means to define a model, as compared to NIEM which is a means for defining real world objects:

- NIEM: Defines real world things
- Metamodel: Defines modeling concepts

**Clean this up**

Neutral intermediate format. (Which is the intermediate format? A model instance.) Platform independent?

- sort of like how NIEM doesn't itself define an exchange; it's a means for you to define an exchange yourself.
- NIEM is just _one_ possible model

## What It Looks Like

Here's a snippet from NIEM, a subset of `nc:PersonEmploymentAssociation` and its type `nc:EmploymentAssociationType`. It defines how a matching XML instances document needs to look, but modeling concepts are implied.

```xml
<xs:complexType name="EmploymentAssociationType">
	<xs:annotation>
		<xs:documentation>A data type for an association between an employee and
		  an employer.</xs:documentation>
	</xs:annotation>
	<xs:complexContent>
		<xs:extension base="nc:AssociationType">
			<xs:sequence>
				<xs:element ref="nc:Employee" minOccurs="0" maxOccurs="unbounded"/>
				<xs:element ref="nc:Employer" minOccurs="0" maxOccurs="unbounded"/>
			</xs:sequence>
		</xs:extension>
	</xs:complexContent>
</xs:complexType>

<xs:element name="PersonEmploymentAssociation" type="nc:EmploymentAssociationType" nillable="true">
	<xs:annotation>
		<xs:documentation>An association between an employee and
		  an employer.</xs:documentation>
	</xs:annotation>
</xs:element>
```

Here's the matching snippet from a NIEM Model Instance subset. While it's longer, that's because it details the different objects in modeling terms. Note that it isn't XML _Schema_. It's not designed as a tool for validating exchanges. It's plain XML and only defines the Model Instance. To use as a tool for validation, you would convert this to the technology you'll be using, be it XML Schema, JSON, RDF, UML, or whatever.

```xml
<ObjectProperty structures:id="nc.PersonEmploymentAssociation">
	<Name>PersonEmploymentAssociation</Name>
	<Namespace structures:ref="nc" xsi:nil="true"/>
	<DefinitionText>An association between a person and employment
	  information.</DefinitionText>
	<Class structures:id="nc.PersonEmploymentAssociationType">
		<Name>PersonEmploymentAssociationType</Name>
		<Namespace structures:ref="nc" xsi:nil="true"/>
		<DefinitionText>A data type for an association between a person and an
		  employment.</DefinitionText>
		<ExtensionOf>
			<Class structures:ref="nc.AssociationType" xsi:nil="true"/>
			<HasObjectProperty mm:sequenceID="1" mm:minOccursQuantity="1"
			mm:maxOccursQuantity="1">
				<ObjectProperty structures:id="nc.Employer">
					<Name>Employer</Name>
					<Namespace structures:ref="nc" xsi:nil="true"/>
					<DefinitionText>A party/entity (organization or person) who
					  employs a person.</DefinitionText>
					<Class structures:ref="nc.EntityType" xsi:nil="true"/>
				</ObjectProperty>
			</HasObjectProperty>
			<HasObjectProperty mm:sequenceID="2" mm:minOccursQuantity="1"
				mm:maxOccursQuantity="1">
					<ObjectProperty structures:id="nc.Employee">
						<Name>Employee</Name>
						<Namespace structures:ref="nc" xsi:nil="true"/>
						<DefinitionText>A person who works for a business or a
						  person.</DefinitionText>
						<Class structures:ref="nc.PersonType" xsi:nil="true"/>
					</ObjectProperty>
			</HasObjectProperty>
		</ExtensionOf>
		<ContentStyleCode>HasObjectProperty</ContentStyleCode>
	</Class>
</ObjectProperty>
```

## Terminology

There are several levels to these concepts and past terminology hasn't clearly delineated them.

### Metamodel

The framework itself is the "Metamodel." It's an abstraction of what makes up a model. It's not NIEM-specific. We use it to define NIEM, but you could use it to define any number of other models. There is no "NIEM Metamodel."

The Metamodel is a crucial tool for creating and maintaining models, but is not something an ordinary NIEM user would usually care about.

### Model Instance

Generically speaking, when you create a model from the Metamodel, you get a "model instance." This is a conceptual model reflecting the objects and relationships in a subject area. This does _not_ have to be NIEM. The Metamodel could be used to create a wide variety of different Model Instances. Of course, our main interest is in NIEM so we want to create a Model Instance reflecting NIEM.

### NIEM Model Instance

When you create a specific model, that name for that model instance gets a prefix determined by what specific model you've created. If you create NIEM as a model, that's a "NIEM Model Instance ([[NMI]])." This is still a conceptual model. To use it for validating real-world exchanges, it would need to be instantiated into some format.

Up until this point, the Metamodel and Model Instances are mainly behind-the-scenes tools. What community members eventually need are versions of NIEM they can use to define exchanges and to which they can implement. The NIEM Model Instance is what a user would use to define an exchange, in terms of creating both subsets and new content. It's what would underlie the tooling. It's the platform _independent_ version of NIEM.

To actually implement an exchange, platform _dependent_ versions are needed.

### NIEM Model Instance XML/JSON

Transforming the NIEM Model Instance to a representation in a particular technology adds the technology as a suffix. If you've converted the NIEM Model Instance to XML Schema, then you have a NIEM Model Instance in XML ([[NMIX]]). If you've converted it to JSON Schema, it's a "NIEM Model Instance in JSON ([[NMIJ]])."

Note that a NIEM Model Instance in XML ([[NMIX]]) is what we currently call "NIEM." The NIEM Model Instance abstracts NIEM up a level, in order to separate the modeling concepts from the specific technology of XML Schema.

XML and JSON aren't the only targets for this conversion/rendering, but are the starting point for the effort.

![Terminology](terminology.svg)

While creating platform dependent versions of NIEM for validation purposes is a major outcome of the Metamodel and NIEM Model Instance, some instances may have entirely different purposes, often as a means of viewing a model.

## Benefits

The major benefit is enabling the use of multiple model instance formats and views from one "source." The NIEM Model Instance could be transformed into any of these example formats:

- XML Schema
- JSON/JSON-LD<sup>[3](#json-ld_fn)</sup>
- SQL<sup>[4](#sql_fn)</sup>
- UML<sup>[5](#uml_fn)</sup> (via XMI<sup>[6](#xmi_fn)</sup>)
- RDF/OWL<sup>[7](#owl_fn)</sup>
- OpenAPI<sup>[8](#openapi_fn)</sup>
- Protobuf<sup>[9](#protobuf_fn)</sup>
- Human readable documentation
	- Text (HTML<sup>[10](#html_fn)</sup>, Markdown<sup>[11](#markdown_fn)</sup>, DOCX<sup>[12](#docx_fn)</sup>, RTF<sup>[13](#rtf_fn)</sup>, PDF<sup>[14](#pdf_fn)</sup>, CSV<sup>[15](#csv_fn)</sup>, etc.)
	- Diagrams (Graphviz/DOT<sup>[16](#dot_fn)</sup>)

Don't need separate tool suites for each format, e.g. SSGT and Movement. Instead, you can have one tool suite that deals with models, and converters for different technologies. Converters are easier to write than tool suites.

**Add additional benefits from [[Metamodel Notes from 2020-08-25]]**

## Why Not Just Use RFD/RDFS<sup>[17](#rdfs_fn)</sup>?

NIEM models have details that aren't easily captured in RDF. Concepts like cardinality and field typing are crucial to information exchanges yet are not easily represented in RDF.

A key benefit of the Metamodel is the the NIEM Model Instance can be readily converted to RDF.

## Why Not Just Use UML?

Prior efforts defining NIEM in UML were complex and costly. While free and open source tools for using UML exist, those that explicitly supported NIEM were expensive and proprietary.

Additionally, UML tools use XMI as a format for exchanging diagrams, but implementation of XMI across tools and versions isn't as stable and reliable as needed.

A key benefit of the Metamodel is the the NIEM Model Instance can be readily converted to UML/XMI.

___

- <a name="json_fn">1</a>. [JavaScript Object Notation (JSON)](https://en.wikipedia.org/wiki/JSON)
- <a name="rdf_fn">2</a>. [Resource Description Framework (RDF)](https://en.wikipedia.org/wiki/Resource_Description_Framework)
- <a name="json-ld_fn">3</a>. [JavaScript Object Notation for Linked Data (JSON-LD)](https://en.wikipedia.org/wiki/JSON-LD)
- <a name="sql_fn">4</a>. [Structured Query Language (SQL)](https://en.wikipedia.org/wiki/SQL)
- <a name="uml_fn">5</a>. [Unified Modeling Language (UML)](https://en.wikipedia.org/wiki/Unified_Modeling_Language)
- <a name="xmi_fn">6</a>. [XML Metadata Interchange (XMI)](https://en.wikipedia.org/wiki/XML_Metadata_Interchange)
- <a name="owl_fn">7</a>. [Web Ontology Language (OWL)](https://en.wikipedia.org/wiki/Web_Ontology_Language)
- <a name="openapi_fn">8</a>. [OpenAPI](https://en.wikipedia.org/wiki/OpenAPI_Specification)
- <a name="protobuf_fn">9</a>. [Protocol Buffer](https://en.wikipedia.org/wiki/Protocol_Buffers)
- <a name="html_fn">10</a>. [HyperText Markup Language (HTML)](https://en.wikipedia.org/wiki/HTML)
- <a name="markdown_fn">11</a>. [Markdown](https://en.wikipedia.org/wiki/Markdown)
- <a name="docx_fn">12</a>. [Office Open XML (DOCX)](https://en.wikipedia.org/wiki/Office_Open_XML)
- <a name="rtf_fn">13</a>. [Rich Text Format (RTF)](https://en.wikipedia.org/wiki/Rich_Text_Format)
- <a name="pdf_fn">14</a>. [Portable Document Format (PDF)](https://en.wikipedia.org/wiki/PDF)
- <a name="csv_fn">15</a>. [Comma-Separated Values (CSV)](https://en.wikipedia.org/wiki/Comma-separated_values)
- <a name="dot_fn">16</a>. [DOT (graph description language)](https://en.wikipedia.org/wiki/DOT_(graph_description_language))
- <a name="rdfs_fn">17</a>. [RDF Schema (Resource Description Framework Schema)](https://en.wikipedia.org/wiki/RDF_Schema)