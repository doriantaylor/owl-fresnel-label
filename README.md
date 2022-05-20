<div class="section" about="#" typeof="owl:Ontology">

## Fresnel Labeling Extension

  - Author  
    [<span property="foaf:name">Dorian
    Taylor</span>](http://doriantaylor.com/person/dorian-taylor#me)
  - Created  
    June 3, 2014
  - Updated  
    January 11, 2020
  - Namespace URI  
    <https://vocab.methodandstructure.com/fresnel-label#>
  - Preferred Namespace Prefix  
    flx

This document specifies additional extensions to the Fresnel vocabulary.

<div class="section">

### Value Ordering

The Fresnel vocabulary provides a useful mechanism for encoding the
desired behaviour for rendering the presence, sequence, and additional
styling accoutrements to the properties associated with a given RDF
resource.

However, neither the core nor extended Fresnel vocabularies explicitly
depict a method for specifying the sequence of property *values*. A
resource can have multiple values for a given property, which, in
addition to resources and blank nodes, can contain languages, datatypes,
or XML content.

It is worth noting that datatype and cardinality constraints in OWL,
while convenient as such, are *not* supposed to be interpreted as
integrity constraints, and are not reliable predictors of actual
content. Therefore it is important for rendering systems to be prepared
to handle unexpected values in RDF properties.

If we are faced with the problem of selecting *exactly one* property
value to display, for instance, as a label, how would we choose it? The
`fresnel:alternateProperties` property can give us a short list, but
past that we're left on our own. We're in the same situation when we're
trying to sort multiple property values any other way than a naïve
lexical sort.

<div class="section">

#### The Algorithm

We must first consider sorting literal values and then sorting
resources.

Literal values in RDF have three different dimensions, although only one
of the latter two can be active at once:

  - lexical content
  - language
  - datatype

There is also a fourth quasi-dimension, which is the specific property
the literal rode in on.

you have the property which was given by the property description, and
then you have the property which was actually selected, which could be
an arbitrarily long path of subproperty/equivalent-property relations

Lexical content can also be interpreted as a number or number-like
object based on the datatype, however this ought to be amenable to being
overridden.

`xsd:dateTime` literals, for instance, should have their time zones
normalized before being compared.

There is also a “smart” sorting algorithm (à la macOS Finder) that
breaks up strings into arrays and treats runs of digits as numbers,
comparing them numerically to each other but lexically to strings.

there is the issue of sorting plain/language/datatype values wholesale

There is the issue of showing and hiding values when there is more than
one of them; we can just sort the list and take the first one, but we
need to be able to instruct the property description that all subsequent
results after the first one should be hidden.

there is the issue of subproperties and equivalent properties, ie do we
penalize topological distance from the asserted property and if so how
much

`fresnel:alternateProperties` and `fresnel:mergeProperties` have
different implications. The former implies testing for the presence of
one property and then another until you find the one you want; the
latter implies pooling all the properties together.

`fresnel:allProperties` also has no blanket policy and will have to be
sorted, but in this case we will be sorting the *properties* themselves,
not their *values*.

-----

resource values have their own lexical content but also tend to be
associated with some sort of label or other

resources also have types, and it is conceivable to want to sort
resource values by their own type

a property may mix literal and resource values together

</div>

</div>

<div class="section">

### Reverse Properties

Short of FSL or SPARQL selectors, there is no denotation of *reverse*
properties in plain Fresnel.

A common scenario for reverse properties is when the subject is in a
list, collection, or other blank node-identified entity, and we are
looking for a link to a *resource*. We need to be able to eg trace up to
the head of the list and see what property ultimately connected the
subject to the reverse relation.

</div>

</div>

<div class="section">

## Classes

<div class="section">

### Value Ordering

<div id="SortingPolicy" class="section" about="[flx:SortingPolicy]" typeof="owl:Class">

#### SortingPolicy

This class encapsulates a reusable sorting policy which can be applied
to multiple property descriptions.

  - Subclass of:  
    [sh:PropertyShape](https://www.w3.org/TR/shacl/#property-shapes)

[Back to Top](https://vocab.methodandstructure.com/fresnel-label#)

</div>

<div id="SortPropertyList" class="section" about="[flx:SortPropertyList]" typeof="owl:Class">

#### SortPropertyList

A SortPropertyList is just an ordinary list with some additional
constraints related to sorting properties.

  - Subclass of:  
    [rdf:List](http://www.w3.org/1999/02/22-rdf-syntax-ns#List)
  - Property restrictions:  
    [rdf:first](https://www.w3.org/TR/rdf-schema/#ch_first)
    <span rel="owl:allValuesFrom"><span about="_:uo1" rel="owl:unionOf">∈
    [rdf:Property](https://www.w3.org/TR/rdf-schema/#ch_property) ∪
    <span about="_:p1" rel="rdf:rest">[sh:PropertyShape](https://www.w3.org/TR/shacl/#property-shapes)<span about="_:p2" rel="rdf:rest" resource="rdf:nil"></span></span></span></span>
    [rdf:rest](https://www.w3.org/TR/rdf-schema/#ch_rest) ∈
    [flx:SortPropertyList](https://vocab.methodandstructure.com/fresnel-label#SortPropertyList)

</div>

</div>

<div class="section">

### Formatting

<div id="Disposition" class="section" about="[flx:Disposition]" typeof="owl:Class">

#### Disposition

A `Disposition` marks out the way a term is rendered on a canvas.

  - Subclass of:  
    [fresnel:PropertyValueStyle](https://www.w3.org/2005/04/fresnel-info/manual/#displayingValues)

</div>

</div>

</div>

<div class="section">

## Properties

<div class="section">

### Value Ordering

<div id="descending" class="section" about="[flx:descending]" typeof="owl:DatatypeProperty">

#### descending

Specify that the sort should be in descending order.

  - Domain:  
    [flx:SortingPolicy](https://vocab.methodandstructure.com/fresnel-label#SortingPolicy)
  - Range:  
    [xsd:boolean](http://www.w3.org/2001/XMLSchema#boolean)

[Back to Top](https://vocab.methodandstructure.com/fresnel-label#)

</div>

<div id="longestFirst" class="section" about="[flx:longestFirst]" typeof="owl:DatatypeProperty">

#### longestFirst

When this flag is set to true, if two strings are being compared where
one (starting from the beginning) is an exact substring of the other,
the *longer* string will precede the shorter one.

In a conventional sort, when two strings that start out identical but
one is longer, the longer string will be placed *after* the shorter one.
This flag signals the opposite behaviour.

  - Domain:  
    [flx:SortingPolicy](https://vocab.methodandstructure.com/fresnel-label#SortingPolicy)
  - Range:  
    [xsd:boolean](http://www.w3.org/2001/XMLSchema#boolean)

[Back to Top](https://vocab.methodandstructure.com/fresnel-label#)

</div>

<div id="sortValuesBy" class="section" about="[flx:sortValuesBy]" typeof="owl:ObjectProperty">

#### sortValuesBy

  - Domain:  
    [fresnel:PropertyDescription](https://www.w3.org/2005/04/fresnel-info/manual/#sublenses)
  - Range:  
    <span about="_:uo2" rel="owl:unionOf" resource="_:q1">
    [rdf:Property](https://www.w3.org/TR/rdf-schema/#ch_property)
    <span about="_:q1" rel="rdf:rest" resource="_:q2">∪</span>
    [sh:PropertyShape](https://www.w3.org/TR/shacl/#constraints-section)
    <span about="_:q2" rel="rdf:rest" resource="_:q3">∪</span>
    [flx:SortPropertyList](https://vocab.methodandstructure.com/fresnel-label#SortPropertyList)
    <span about="_:q3" rel="rdf:rest" resource="rdf:nil"></span> </span>

</div>

</div>

<div class="section">

### Reverse Properties

<div id="reverse" class="section" about="[flx:reverse]" typeof="owl:DatatypeProperty">

#### reverse

This flag signifies when a property description is intended to represent
a *reverse* relationship.

  - Domain:  
    [fresnel:PropertyDescription](https://www.w3.org/2005/04/fresnel-info/manual/#sublenses)
  - Range:  
    [xsd:boolean](http://www.w3.org/2001/XMLSchema#boolean)

[Back to Top](https://vocab.methodandstructure.com/fresnel-label#)

</div>

</div>

<div class="section">

### Lens Selection

<div id="priority" class="section" about="[flx:priority]" typeof="owl:DatatypeProperty">

#### priority

Ascribe an arbitrary tiebreaking priority to a lens.

This property is analogous to XSLT, which specifies a `priority=`
attribute for overriding ambiguities in template selection.

  - Domain:  
    [fresnel:Lens](https://www.w3.org/2005/04/fresnel-info/manual/#lenscore)
  - Range:  
    [xsd:decimal](http://www.w3.org/2001/XMLSchema#decimal)

</div>

</div>

<div class="section">

### Formatting

<div id="disposition" class="section" about="[flx:disposition]" typeof="owl:ObjectProperty owl:FunctionalProperty">

#### disposition

Ascribe a `flx:Disposition` to the given format.

  - Domain:  
    [fresnel:Format](https://www.w3.org/2005/04/fresnel-info/manual/#stylecore)
  - Range:  
    [flx:Disposition](https://vocab.methodandstructure.com/fresnel-label#Disposition)

</div>

</div>

</div>

<div class="section">

## Individuals

<div class="section">

### Reverse Properties

<div id="allReverseProperties" class="section" about="[flx:allReverseProperties]" typeof="fresnel:PropertySet">

#### allReverseProperties

This property set, analogous to fresnel:allProperties, designates all
*reverse* properties from a given subject.

[Back to Top](https://vocab.methodandstructure.com/fresnel-label#)

</div>

</div>

<div class="section">

### Dispositions

<div id="block" class="section" about="[flx:block]" typeof="flx:Disposition">

#### block

Display each value as a generic block.

</div>

<div id="paragraph" class="section" about="[flx:paragraph]" typeof="flx:Disposition">

#### paragraph

Display each value as a paragraph.

</div>

<div id="section" class="section" about="[flx:section]" typeof="flx:Disposition">

#### section

Display each value as a section.

</div>

<div id="inline" class="section" about="[flx:inline]" typeof="flx:Disposition">

#### inline

Display each value inline.

</div>

<div id="hidden" class="section" about="[flx:hidden]" typeof="flx:Disposition">

#### hidden

Render the value in the markup but do not display it.

</div>

</div>

</div>
