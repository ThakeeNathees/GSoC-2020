## GDscript Documentation Generation System

- Project : GDscript Documentation Generation System
- Student : Thakee Nathees ([ThakeeNathees](https://github.com/ThakeeNathees/))
- Mentors: George Marques ([vnen](https://github.com/vnen)) and Ankit Priyarup ([ankitpriyarup](https://github.com/ankitpriyarup))
- Repositories:
    - Core implementation : https://github.com/ThakeeNathees/godot/tree/GDSctipt-Documentation
    - UI implementation : https://github.com/ThakeeNathees/godot/tree/GDScript-DocUI
    
### About The Project

One of the most requested language feature that GDScript lacks is being able to generate documentation for your code, using a system similar to "javadoc" in Java, "XML comments" in C# and "doc string" in Python, etc. The Godot in-engine documentation is very useful as we can browse and search the whole API offline, without leaving the editor. But this documentation is hardcoded in the editor binary, and users who make plugins and libraries don't have any way to use that feature. In-editor documentation would be useful for their users to understand how to use their plugin/library, how to initialize it and what's the cleanup process. So they either have to read the source code or to search help through the Internet.

My original proposal was to implement this with the new annotation system (which is added in Godot 4.0 with the new GDScript 2.0), but after discussing with the Godot devs and contributors (https://github.com/godotengine/godot-proposals/issues/993) the plan was changed to implement it with comments. A comment that starts with double hash symbol "##" is considered a doc comment and it should be immediately above the method/property it documents.

Here is a working example:

```gdscript
## A Math utility library for efficient matrix computations.
##
## @desc:
##     A utility math library implemented with SIMD feature
##     for efficient conputations with complex matrices.
##
class_name Math

## The maximum value a gdscript integer can hold
const INT_MAX = 0x7fffffffffffffff

## Adds two integers and returns the result.
## [color=yellow]Warning:[/color]
##     An integer overflow occurs when a + b > [member INT_MAX]
func add(a:int, b:int) -> int:
	return a + b
```

The documentation for the above piece of code will be generated as the script is being written, and automatically updated in the Help window. User scripts documentation can be found by their class_name if defined, otherwise the script's path.

### Overview and Progress

The idea is that when the script compiles, we collect the description, brief_description, links, constants, etc. from the doc comments (comments that start with "##") and map the comments to the field that they are documenting. All the relevant information (the name, data type, return type, setter, getter, default value, value of a constant, etc.) will be extracted from the parse tree and used to build a ClassDoc instance (like used for the Godot API) and update the editor help. It's the core part of the project and it's almost completed as of now. The other part of the project is the UI implementation. Visual Studio-like C# XML comment autocompletion support has also been implemented, and I'm currently focusing on generating an optional XML file for each documentation page, which could be exported to HTML by the user if needed.

![](https://github.com/ThakeeNathees/GSoC-2020/blob/master/progress-report-1/doc.jpg)

### What's Next

Currently the first part of the project is almost done and I'm working on the UI features. The following features are planned:

- Based on documented properties of a GDScript, the code editor can show a brief description when a property/method is hovered and a description when auto-completing for the property/method.

- he description of an exported variable could also be used as a tooltip in the Inspector

- An XML/JSON version of the documentation could also be generated for the plugin/library and it could be exported to HTML by the author for their website.
