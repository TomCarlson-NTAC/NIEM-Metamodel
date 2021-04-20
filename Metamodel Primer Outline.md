# Model Primer Outline

**Primer needs to be standalone, but linked to other materials.**

Post here: https://github.com/NIEM/NIEM-Modeling-Formalism

## Background: How We Got Here

NIEM began as a framework for building and defining messages to facilitate exchanges of information. Part of that framework was a defined process. Part of the framework was a model that formed a base on which exchanges were defined. That base would be both carved down _and_ extended to meet the needs of each exchange. That base existed, and still exists, as a set of XML Schema documents.

As other means of exchanging information have become popular, notably JSON, the community needs have expanded to using NIEM with exchange mechanisms other than XML Schema. JSON is the current alternate means, but many more exist.

Dealing with this issue, both now and in the future, is the rationale for the Metamodel.

## Problem/Issue

This introduces a problem. How do you translate the model from XML Schema to some form of JSON, when XML Schema and JSON have similar, but definitely different, feature sets? This isn't impossible. We do have a specification and other guidance for this translation, but the translation isn't trivial and the results may not be as satisfying as possible.

The situation is shown below, the dotted line representing the incomplete translation between these two technologies.

![Two Technologies](two_technologies.svg)

Still, this conversion is manageable. When more technologies get added, the translations get out of hand, especially as the community does work in these other technologies. How does work done in JSON get translated to RDF? How accurate is that translation? Does that work get translated to XML Schema via JSON directly or via RDF?

The "N-squared" diagram is familiar to anyone who has seen many presentations about NIEM. Usually it's representing a variety of entities making an ever growing number peer-to-peer sharing agreements. The same diagram applies here, as a multitude of technologies start requiring peer-to-peer conversions between technologies.

![N-Squared Technologies](many_technologies.svg)

At the message level, NIEM provides the means to define a single centralized and standardized format for the exchange, replacing the complexity of numerous peer-to-peer agreements. Instead of peer-to-peer agreements, everyone implements towards the standard.

The same concept applies here with models and technologies. Instead of individual translations between technologies, there's one centralized and standardized "model instance." Different technologies are translated from that standard model. Now the lines are solid, as each translation can better leverage the abilities of a particular technology.

![Standardized and Centralized](model_centric.svg)


Currently, modeling concepts are embedded in a specific technology, XML Schema. We overload XML Schema concepts to include real-world concepts. We use XML Schema to both define NIEM _and_ act as the tool for validating actual messages.

## Solution

The solution is to create the modeling concepts in a conceptual format instead of embedding them in XML Schema. Instead of implying modeling concepts in XML Schema, we explicity

- explicit X has-a Y
- explicit X is-a Y
- explicit concepts like associations and roles
	
Allows for **concept**-to-technology conversions, e.g. Model -> XML Schema and Model -> JSON. This is easier.

## How It Works

"Metamodel" is a framework for building models. The metamodel itself isn't the NIEM model. It's a "neutral modeling formalism," for these models. It's a means of defining a model. It provides the means to define a model, as compared to NIEM which is a means for defining real world objects:

- NIEM: Defines real world things
- Metamodel: Defines modeling concepts

Neutral intermediate format. (Which is the intermediate format? A model instance.)

```xml
<example>Pull part of Webb's claim example for this.<example/>
```

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

Generically speaking, when you create a model from the metamodel, you get a "model instance." This is a conceptual model reflecting the objects and relationships in a subject area. This does _not_ have to be NIEM. The Metamodel could be used to create a wide variety of different model instances.

### NIEM Model Instance

When you create a specific model, that model instance name gets a prefix determined by what specific model you've created. If you create NIEM as a model, that's a "NIEM Model Instance ([[NMI]])." This is still a conceptual model. To use it for validating real-world exchanges, it would need to be instantiated into some format.

### NIEM Model Instance XML/JSON

Converting that model to a representation in a particular technology adds the technology as a suffix. If you've converted the NIEM Model Instance to XML Schema, then you have a NIEM Model Instance XML ([[NMIX]]). If you've converted it to JSON Schema, it's a "NIEM Model Instance JSON ([[NMIJ]])."

Note that a NIEM Model Instance XML ([[NMIX]]) is what we currently call "NIEM." The NIEM Model Instance abstracts that up a level, in order to separate the modeling concepts from the specific technology of XML Schema.

XML and JSON aren't the only targets for this conversion/rendering, but are the starting point for the effort.

![Terminology](terminology.svg)

## Benefits

The major benefit is enabling the use of multiple model instance formats from one "source", e.g.:

- XML Schema
- JSON/JSON-LD
- SQL
- UML (via XMI)
- RDF/OWL
- [OpenAPI](https://en.wikipedia.org/wiki/OpenAPI_Specification)
- Human readable documentation
	- Text (HTML, Markdown, RTF, PDF, CSV, etc.)
	- Diagrams

Don't need separate tool suites for each format, e.g. SSGT and Movement. Instead, you can have one tool suite that deals with models, and converters for different technologies. Converters are easier to write than tool suites.

**Add additional benefits from [[Metamodel Notes from 2020-08-25]]**

## Why Not Just Use RFD/RDFS?

NIEM models have details that aren't easily captured in RDF. Concepts like cardinality and field typing are crucial to information exchanges yet are not easily represented in RDF.

A key benefit of the Metamodel is the the NIEM Model Instance can be readily converted to RDF.

## Why Not Just Use UML?

Prior efforts defining NIEM in UML were complex and costly. While free and open source tools for using UML exist, those that explicitly supported NIEM were expensive and proprietary.

Additionally, UML tools use XMI as a format for exchanging diagrams, but implementation of XMI across tools and versions isn't as stable and reliable as needed.

A key benefit of the Metamodel is the the NIEM Model Instance can be readily converted to UML/XMI.

## Resources

**These are for my benefit right now. They may not be in the final draft.**

- [webb](https://github.com/webb)/[niem-metamodel](https://github.com/webb/niem-metamodel)
	- [iamdrscott](https://github.com/iamdrscott)/[niem-metamodel](https://github.com/iamdrscott/niem-metamodel)
- [cabralje](https://github.com/cabralje)/[niem-tools](https://github.com/cabralje/niem-tools)

