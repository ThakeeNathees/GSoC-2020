
![5f21bb0114081243850484](https://user-images.githubusercontent.com/41085900/90886842-d2596c80-e3d0-11ea-9216-f750c0b95cfb.png)

## Google Summer of Code - 2020 Final Evaluation

I worked on documentation system for the language called GDScript for [Godot Engine](https://github.com/godotengine/godot) this summer as a GSoC student.

### Introduction
The more and more plugins and libraries developed in the recent years the need for the feature has increased. Yet there are numbers of wonderful plugins where the users are required to go through the internet tutorials to understand what it does and how to use it, meanwhile the engine's core documentations are available to view in the engine offline.

```
“Documentation is a love letter that you write to your future self.”
— Damian Conway
```

The project is to implement documentation system to GDScript similar to "javadoc" in Java, "XML comments" in C# and "doc string" in Python, etc. It'll be possible to view the documentation for the scripts in the engine's doc window and also supports to generate them as XML/JSON files. In addition the project aims to implement some essential UI support like doc comment autocompletion, docs generator [wizard](https://en.wikipedia.org/wiki/Wizard_(software)), script editor symbol description on hover, tool tip for exported variables etc.

### How it works?
The typical [compilation pipeline](https://en.wikibooks.org/wiki/How_to_Write_a_Compiler/The_Compilation_Pipeline) is scanning/tokenizing, parsing, semantic analysis, optimization and code generation.  The member fields are constructed at the parsing phase, but the comments aren't survive after the tokenizing phase since they are ignored after that. To use those comments, it'll collect them when tokenizing. When parsing a member it'll check if any comment exists immediately above the definition of the member value, except for enum value description, which should be at the same line. The doc comments could also be multiple lines. Class doc contains special tags to separate different section. These will parsed and applied to the member at the code generation phase.

A doc comment example:
```gdscript
extends Node2D
class_name MyClass

## A brief description about the script
## 
## @desc:
## 	the description about the script, what it can do
## 	how to use it, the cleanup process and more.
##
## @tutorial(Tutorial1): http://my_page.com/the/tutorial/1/
## @tutorial(Tutorial2): http://my_page.com/the/tutorial/2/


## description of the member variable.
@onready var child = $child

## the description can also be used as
## the tooltip for the exported variable.
@export var a_var:int

## description of direction enum values.
enum Direction {
	UP    = 0, ## direction up
	DOWN  = 1, ## direction down 
	LEFT  = 2, ## direction left
	RIGHT = 3, ## direction right
}
```
The type information and the reduced default values could be extracted at the semantic analysis phase. At the code generation phase It'll build a GDScript instance which is the target binary version of the language. A property of type `DocData::ClassDoc` (the engine's documentation representation) have added to the GDScript and it'll construct the documentation once it compiles with all the information gathered from the previous phases. Each time the engine starts it'll cache the doc property of the scripts to the editor help window and  each time a script updated it'll update the help window with the new documentation.

### Work done
Godot [PR workflow](https://docs.godotengine.org/en/stable/community/contributing/pr_workflow.htm) is that, it favor commits that bring the codebase from one functional state to another functional state without having intermediate commits, hence I worked on a single commit. Each time I made a change I had to [amend/rebase](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History). I've split my work into two main tasks as core implementation and UI implementation.
- https://github.com/godotengine/godot/pull/39359 : my initial PR for the core doc comment implementation but after the new [GDScript 2.0](https://github.com/godotengine/godot/pull/40598) (the refactor of the GDScript parser with more features and optimizations) I abandoned the PR and needed to re-implement everything for the new parser.
- https://github.com/godotengine/godot/pull/41095 : it was the refactored version of my doc comment PR for the new parser which updated with typed signal parameters, enum types and [annotations](https://github.com/godotengine/godot-proposals/issues/828).
- https://github.com/godotengine/godot/pull/39849 : the UI implementation PR which has 3 features with it's own commits.
  - script editor doc comment auto completion : basic auto completion support, auto pair symbol, indentation support, etc.
  - XML generation: the backend to generate XML files for a documented script.
  - Export docs window: A wizard to export all the script of a given plugin/collection of scripts as XML/JSON files.
- https://github.com/ThakeeNathees/godot/tree/TextEdit-hover-hint: the working branch for description hint of hovered symbol in the script editor
- https://github.com/godotengine/godot-docs/pull/3892: the documentation for the documentation comments. It contains the guide and usage of the doc comments for the [Godot online docs](https://docs.godotengine.org/en/stable/).

### GSoC Experience
Before this summer of code, I have never worked with such a large scale codebase. This is the first time I'm contributing to an opensource. And It gave me the opportunity to collaborate with the international domain experts. As I've started to contribute, my knowledge on git which was restricted by the basic add-commit-push cycle have improved considerably. I had the chance to polish my knowledge on code reading skills, software development and project management throughout the summer. Also I Learnt a lot of new things like, the importance of/how to use build systems, test frameworks, documentations etc.

I always had a keen interest to learn about compilers, compiler internals and how they function. But I had a limited knowledge on compiler design before participating on GSoC. Along this summer of code I was able to understand the GDScript compiler from the maintainer of the GDScript and my mentor [George Marques](https://github.com/vnen). A special thanks to him for the guidance, also I want to thank both my mentors (@vnen, @ankitpriyarup) and the community for their help and support.

### What's next?
I'm interested in contributing more to the Godot engine and other opensource projects. I'll continue to contribute after the GSoC ends, specially to the GDScript language. There is always something new to learn from an opensource from which I'll improve my knowledge and skills.
