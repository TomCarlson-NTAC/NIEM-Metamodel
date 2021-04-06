# Model Primer Outline

**Primer needs to be standalone, but linked to other materials.**

Post here: https://github.com/NIEM/NIEM-Modeling-Formalism

## Background: How We Got Here

Start with the NIEM story. Messages, XML Schema, but what if you don't want XML Schema. Do it in 
other formalisms. N-squared diagram. Intermediate format.

NIEM began as a framework for building and defining messages to facilitate exchanges of information. Part of that framework was a defined process. Part of the framework was a model that formed a base on which exchanges were defined. That base would be both carved down _and_ extended to meet the needs of each exchange.

That base existed as a set of XML Schema documents.

As other means of exchanging information became popular, notably JSON, the community needs expanded to using NIEM with exchange mechanisms other than XML Schema.





N-squared for exchanges => standardized and centralized

N-squared for models => standardized and centralized

## Problem/Issue

Modeling concepts are embedded in a specific technology, XML Schema. We overload XML Schema concepts to include real-world concepts. This works for simpler things, but complicated real-world concepts require thought to see them while they're embedded in SML Schema

Forces technology-to-technology conversions, e.g. XML Schema -> JSON. These can be hard.

## Solution

Create the modeling concepts in a conceptual format instead of embedding them in XML Schema:

- explicit X has-a Y
- explicit X is-a Y
- explicit concepts like associations and roles
	
Allows for **concept**-to-technology conversions, e.g. Model -> XML Schema and Model -> JSON. This is easier.

## How It Works

"Metamodel" is a framework (_better term?_) for building models.

- The metamodel itself isn't the NIEM model. "Neutral modeling formalism" (formalism/language?) for these models. (Although _technically_ is **a** model itself. **cut this**) It's a means of defining a model. It provides a way to s...
- NIEM: defining real world things
- Metamodel: defining modeling concepts

Neutral intermediate format.

```xml
<example/>

```


- sort of like how NIEM doesn't itself define an exchange; it's a means for you to define an exchange yourself.
- NIEM is just _one_ possible model

## Why Not Just Use RFD/RDFS?

NIEM models have details that aren't easily captured. cardinality and field typing.

## Terminology

### Metamodel

The framework itself is the "metamodel." It's an abstraction of what models are made up of.

### Model Instance

Generically speaking, when you create a model from the metamodel, you get a "model instance." This is a conceptual model 

### NIEM Model Instance

When you create a specific model, that model instance gets a prefix determined by what specific model you've created. If you create the NIEM as a model, that's a "NIEM Model Instance ([[NMI]])."

### NIEM Model Instance XML/JSON

When you then convert (render?) that model to a representation in a particular technology, then it get's the technology added as a suffix. If you've converted the NIEM Model Instance to XML Schema, then you have a NIEM Model Instance XML ([[NMIX]]). If you've converted it to JSON, it's a "NIEM Model Instance JSON ([[NMIJ]])."

Note that a NIEM Model Instance XML ([[NMIX]]) is what we currently call "NIEM." The metamodel abstracts that up a level, in order to separate the modeling concepts from the specific technology of XML Schema.

XML and JSON aren't the only targets for this conversion/rendering, but are the starting point for the effort.

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

## Examples

Not sure how to present them. Maybe both diagrams and text, as we did with JSON normalization?

Should the examples be NIEM? Or should we start with a generic model that's simpler?

## Resources

- [webb](https://github.com/webb)/[niem-metamodel](https://github.com/webb/niem-metamodel)
	- [iamdrscott](https://github.com/iamdrscott)/[niem-metamodel](https://github.com/iamdrscott/niem-metamodel)
- [cabralje](https://github.com/cabralje)/[niem-tools](https://github.com/cabralje/niem-tools)

