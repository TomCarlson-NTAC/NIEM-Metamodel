# Model Primer Outline

**Primer needs to be standalone, but linked to other materials.**

Post here: https://github.com/NIEM/NIEM-Modeling-Formalism

## Background: How We Got Here

NIEM began as a framework for building and defining messages to facilitate exchanges of information. Part of that framework was a defined process. Part of the framework was a model that formed a base on which exchanges were defined. That base would be both carved down _and_ extended to meet the needs of each exchange. That base existed, and still exists, as a set of XML Schema documents.

As other means of exchanging information have become popular, notably JSON, the community needs have expanded to using NIEM with exchange mechanisms other than XML Schema. JSON is the current alternate means, but many more exist.

Dealing with this issue, both now and in the future, is the rationale for the Metamodel.

## Problem/Issue

This introduces a problem. How do you translate the model from XML Schema to some form of JSON, when XML Schema and JSON have similar, but definitely different, feature sets? This isn't impossible. We do have a specification and other guidance for this translation, but the translation isn't trivial.

The situation is shown below, the dotted line representing the incomplete translation between these two technologies.

![Two Technologies](two_technologies.svg)

Still, this conversion is manageable. When more technologies get added, the translations get out of hand, especially as the community does work in these other technologies. How does work done in JSON get translated to RDF? How accurate is that translation? Does that work get translated to XML Schema via JSON or RDF?

The "N-squared" diagram is familiar to anyone who has seen many presentations about NIEM. Usually it's representing a variety of entities making an ever growing number peer-to-peer sharing agreements. The same diagram applies here, as a multitude of technologies start requiring peer-to-peer conversions between technologies.

![N-Squared Technologies](many_technologies.svg)

At the message level, NIEM provides the means to define a single centralized and standardized format for the exchange, replace the complexity of numerous peer-to-peer agreements. Instead of peer-to-peer agreements, everyone implements towards the standard.

The same concept applies here with models and technologies. Instead of individual translations between technologies, there's one centralized and standardized "model instance." Different technologies are translated from that standard model. Now the lines are solid, as each translation can better leverage the abilities of a particular technology.

![Standardized and Centralized](model_centric.svg)




Modeling concepts are embedded in a specific technology, XML Schema. We overload XML Schema concepts to include real-world concepts. This works for simpler things, but complicated real-world concepts require thought to see them while they're embedded in XML Schema

## Hazards of Embedding

**I'm not sure this is appropriate for the primer.**

How do we know something in NIEM is a code table without scouring the NDR for the information? Simply looking at the schemas can be misleading. It looks like code tables are built on the `xs:token` type, but restricted to a selection of specific enumerations.

But relying on the existence of enumerations isn't accurate, as NIEM 5.0 has some code tables that instead are built using patterns and lengths to define codes.

Relying on `xs:token` being the base type is also inaccurate, as there are other elements built on `xs:token` that aren't code tables.

The answer is that the representation term, the end of the element/type name, must end in "Code". Nothing else can be named that way.

There are multiple ways that the schema _could_ define something as a code table, but only one is the actual one true way.

Model examples from which to draw:
- niem-metamodel-master/example/extension.xml

A model instance built from the metamodel will answer that question more clearly, in the model instance. It looks like this:

```xml
<request>Does Jim Cabral have a facet example I could put here</request>
```

Forces technology-to-technology conversions, e.g. XML Schema -> JSON. These can be hard.

## Solution

Create the modeling concepts in a conceptual format instead of embedding them in XML Schema:

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


## Terminology

### Metamodel

The framework itself is the "Metamodel." It's an abstraction of what makes up a model.

### Model Instance

Generically speaking, when you create a model from the metamodel, you get a "model instance." This is a conceptual model reflecting the objects and relationships in a subject area. This does _not_ have to be NIEM. The Metamodel could be used to create a wide variety of different model instances.

### NIEM Model Instance

When you create a specific model, that model instance gets a prefix determined by what specific model you've created. If you create NIEM as a model, that's a "NIEM Model Instance ([[NMI]])." This is still a conceptual model. To use it for real-world exchanges, it need to be instantiated into some format.

### NIEM Model Instance XML/JSON

Converting that model to a representation in a particular technology adds the technology as a suffix. If you've converted the NIEM Model Instance to XML Schema, then you have a NIEM Model Instance XML ([[NMIX]]). If you've converted it to JSON, it's a "NIEM Model Instance JSON ([[NMIJ]])."

Note that a NIEM Model Instance XML ([[NMIX]]) is what we currently call "NIEM." The metamodel abstracts that up a level, in order to separate the modeling concepts from the specific technology of XML Schema.

XML and JSON aren't the only targets for this conversion/rendering, but are the starting point for the effort.

![Terminology](terminology.svg)

![Terminology (briefer)](terminology_brief.svg)


## Benefits

Multiple model formats from one "source", e.g.:

- XML Schema
- "Simple" XML Schema
- JSON/JSON-LD
- SQL
- CSV
- UML (via XMI)
- RDF/OWL
- [OpenAPI](https://en.wikipedia.org/wiki/OpenAPI_Specification)
- human readable documentation
	- Text
		- HTML
		- Markdown
		- RTF
		- PDF
	- Diagrams
		- DOT
		- Mermaid

Don't need separate tool suites for each format, e.g. SSGT and Movement. Instead, you can have one tool suite that deals with models, and converters for different technologies. Converters are easier to write than tool suites.

## Why Not Just Use RFD/RDFS?

NIEM models have details that aren't easily captured in RDF. Concepts like cardinality and field typing are crucial to information exchanges yet are not easily represented in RDF.

## Why Not Just Use UML?

Expensive proprietary tools. XMI version issues. We want free and open source tooling. Free tooling existing, but those that explicitly support NIEM tend to be the pricey ones.

The NIEM UML profile was complex. [[Jim Cabral]]: Should we even mention the profile?

Just call it "prior efforts"?

Not everyone loves UML. Metamodel will support UML, but doesn't require UML.


## Examples

Not sure how to present them. Maybe both diagrams and text, as we did with JSON normalization?

Should the examples be NIEM? Or should we start with a generic model that's simpler?

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


## Resources

- [webb](https://github.com/webb)/[niem-metamodel](https://github.com/webb/niem-metamodel)
	- [iamdrscott](https://github.com/iamdrscott)/[niem-metamodel](https://github.com/iamdrscott/niem-metamodel)
- [cabralje](https://github.com/cabralje)/[niem-tools](https://github.com/cabralje/niem-tools)

