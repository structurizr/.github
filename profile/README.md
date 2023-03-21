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

## tl;dr

Structurizr lets you create a __single model__ with __multiple views__, which can be __rendered with a number of tools__.
If you just want to create some software architecture diagrams quickly:

1. [Read the 5 minute introduction to the C4 model](https://www.infoq.com/articles/C4-architecture-model/)
2. [Start with the Structurizr DSL](https://structurizr.com/dsl)

## Authoring tools

In Structurizr terminology, a "workspace" is a wrapper for two things:

1. Model: your software architecture model, consisting of elements and relationships.
2. Views: views onto the model, which will ultimately be rendered as diagrams.

Workspaces are described using an open JSON data format (an OpenAPI 3.0 definition; see [structurizr/json](https://github.com/structurizr/json)),
which allows model authoring to be decoupled from diagram rendering.

There are a number of tools for creating a Structurizr compatible workspace.

- [structurizr/dsl](https://github.com/structurizr/dsl): Mentioned on the [ThoughtWorks Tech Radar - Techniques - Diagrams as code](https://www.thoughtworks.com/radar/techniques/diagrams-as-code), the Structurizr DSL provides a way to author a Structurizr workspace using a straightforward text-based domain specific language.
- [structurizr/java](https://github.com/structurizr/java): A Java library to create Structurizr workspaces using JVM languages.

There are also a number of ports of `structurizr/java`, including:

- [structurizr/dotnet](https://github.com/structurizr/dotnet)
- [aldosolorzano/structurizr-clj](https://github.com/aldosolorzano/structurizr-clj)
- [Midnighter/structurizr-python](https://github.com/Midnighter/structurizr-python)
- [structurizr-php/structurizr-php](https://github.com/structurizr-php/structurizr-php)
- [ChristianEder/structurizr-typescript](https://github.com/ChristianEder/structurizr-typescript)
- [goadesign/model](https://github.com/goadesign/model)

The Structurizr DSL is the recommended authoring tool for most teams,
with the code-based tools being useful for teams who want to use code to help build their software architecture model
(e.g. component discovery via static analysis, parsing distributed log files, etc) to create data-driven software architecture diagrams.
It's also possible to use a hybrid approach, with [DSL and code](https://github.com/structurizr/dsl/tree/master/docs/cookbook/dsl-and-code).

## Rendering tools

There are number of tools that can be used to render diagrams in a Structurizr workspace, each offering a different set of features and integration options.

- [Structurizr Lite/on-premises/cloud service](https://structurizr.com): A browser-based diagram and documentation rendering tool with interactive diagrams, "double-click to zoom", etc.
- [c4viz](https://github.com/pmorch/c4viz): A browser-based diagram renderer, with diagram navigation and "click to zoom".
- [Structurizr Site Generatr](https://github.com/avisi-cloud/structurizr-site-generatr): Generates a HTML microsite with diagrams, documentation, and a UI to explore the model. 
- [Kroki](https://kroki.io): Kroki generates diagrams from a number of text-based formats, including the Structurizr DSL.
- [Git for Confluence | Markdown, PlantUML, Graphviz, Mermaid](https://marketplace.atlassian.com/apps/1211675/git-for-confluence-markdown-plantuml-graphviz-mermaid): A Confluence plugin that will render a specific diagram from a Structurizr DSL file stored in your git repo.
- [structurizr/export](https://github.com/structurizr/export): A collection of Java classes to generate diagrams as PlantUML, Mermaid, DOT, and WebSequenceDiagrams. An export to Ilograph is also available.
- [structurizr/cli](https://github.com/structurizr/cli): A command line utility for Structurizr, designed to be used in conjunction with the Structurizr DSL, supporting the ability to push a workspace to the Structurizr cloud service/on-premises installation, and export views to PlantUML, Mermaid, DOT, WebSequenceDiagrams, etc formats.

All of the above diagram rendering tools use an automatic layout algorithm, with the exception of the Structurizr cloud service/on-premises installation/Lite, which provides both automatic and manual layout facilities.

## Custom tooling

Workspaces are described using an open JSON data format (see [structurizr/json](https://github.com/structurizr/json)),
so it's relatively straightforward to build your own custom tooling to consume that data;
perhaps for rendering views with your own diagramming tool, or to integrate the data with your internal dashboards and service catalogs.

![Structurizr tooling overview](profile/images/structurizr-overview.jpg)

Although JSON is an easy data format to work with, using one of the code-based authoring tools (see above) will provide a quicker starting point. For example, you can load a JSON workspace definition using [structurizr/java](https://github.com/structurizr/java) as follows, with the resulting `Workspace` object providing an easy way to navigate/manipulate/translate/export the data:

```
public static void main(String[] args) throws Exception {
    Workspace workspace = WorkspaceUtils.loadWorkspaceFromJson(new File("workspace.json"));
}
```

## Getting started

The separation of workspace authoring from diagram rendering makes it possible to use the Structurizr tooling in a number of ways:

- Author a workspace with [structurizr/dsl](https://github.com/structurizr/dsl), render diagrams using [Structurizr Lite](https://structurizr.com/help/lite).
- Author a workspace with [structurizr/dsl](https://github.com/structurizr/dsl), render diagrams using the [Structurizr cloud service](https://structurizr.com/help/cloud-service) or [on-premises installation](https://structurizr.com/help/on-premises), via the [structurizr/cli push command](https://github.com/structurizr/cli/blob/master/docs/push.md).
- Author a workspace with [structurizr/java](https://github.com/structurizr/java), render diagrams using the [Structurizr cloud service](https://structurizr.com/help/cloud-service) or [on-premises installation](https://structurizr.com/help/on-premises).
- Author a workspace with [structurizr/dsl](https://github.com/structurizr/dsl), export diagrams using the [structurizr/cli export command](https://github.com/structurizr/cli/blob/master/docs/export.md) for rendering with PlantUML, Mermaid, D2, WebSequenceDiagrams, etc.
- etc

The easiest way get started is:

- Use the [Structurizr DSL demo page](https://structurizr.com/dsl) to try the DSL and a number of diagram rendering options, without installing any tooling.
- Use the DSL in conjunction with Structurizr Lite; see [Getting started with Structurizr Lite](https://dev.to/simonbrown/getting-started-with-structurizr-lite-27d0).

## Documentation

Since there are a number of ways to use the Structurizr tooling, there are a number of places to find documentation.

- [Structurizr - Help](https://structurizr.com/help): documentation about the Structurizr browser-based diagrams/documentation/decisions renderer.
- [Structurizr Lite](https://structurizr.com/share/76352/documentation): installation and configuration.
- [Structurizr on-premises](https://structurizr.com/share/18571/documentation): installation and configuration.
- [Structurizr CLI](https://github.com/structurizr/cli#readme): installation and usage.
- Structurizr DSL: [language reference](https://github.com/structurizr/dsl/blob/master/docs/language-reference.md) and [cookbook](https://github.com/structurizr/dsl/tree/master/docs/cookbook).
- [Structurizr exporters](https://github.com/structurizr/export#readme): configuration and usage of the PlantUML, C4-PlantUML, Mermaid, DOT, WebSequenceDiagrams, and Ilograph exporters. 
