# Structurizr - "models as code"

There has been a trend over the past few years towards text-based tooling and "diagrams as code",
with the most popular examples including [PlantUML](http://plantuml.com),
[WebSequenceDiagrams](https://www.websequencediagrams.com), and
[Mermaid](https://mermaid-js.github.io/mermaid/).
With these tools, the diagram source is provided as text using a proprietary domain-specific language,
which the tool then visualises, typically with an automatic layout algorithm.
These tools generally have a low barrier to entry, and the source text is easily version controlled.
Also, it's relatively straightforward to automate the use of these tools in order to generate diagrams
and documentation during your build process.

However, each diagram needs to be defined separately, typically in a separate text file.
If you have the same element on two diagrams, and you want to change the name of that element,
you need to make sure that you change the name everywhere it's used.
The global search and replace features in most developer tooling does make this less of a problem, but it's just one way that a collection of diagrams can easily become inconsistent if not managed properly.
To solve this problem, we can create a __single model__, and visualise __multiple views__ of it.

Structurizr is a collection of tooling to create software architecture diagrams and documentation based upon the
[C4 model for visualising software architecture](https://c4model.com).
Structurizr was started in 2014 by [Simon Brown](https://simonbrown.je) (the creator of the C4 model),
and has grown into a community of tooling, much of which is open source.

## Some basics

In Structurizr terminology, a "workspace" is a wrapper for two things:

1. Model: your software architecture model, consisting of elements and relationships.
2. Views: views onto the model, which will ultimately be rendered as diagrams.

Workspaces are described using an open JSON data format (an OpenAPI 3.0 definition; see [structurizr/json](https://github.com/structurizr/json/)),
which decouples model authoring from diagram rendering.
