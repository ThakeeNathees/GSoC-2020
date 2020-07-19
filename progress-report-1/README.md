## GDscript Documentation Generation System

- Project : GDscript Documentation Generation System
- Student : Thakee Nathees ([ThakeeNathees](https://github.com/ThakeeNathees/))
- Mentors: George Marques ([vnen](https://github.com/vnen)) and Ankit Priyarup ([ankitpriyarup](https://github.com/ankitpriyarup))
- Repositories:
    - Core implementation : https://github.com/ThakeeNathees/godot/tree/GDSctipt-Documentation
    - UI implementation : https://github.com/ThakeeNathees/godot/tree/GDScript-DocUI
    
### About The Project

One of the most requested language feature that GDScript lacks of is the documentation generation like "javadoc" in Java, "XML comments" in C# 
and "doc string" in Python, etc. The Godot in-engine documentation is very useful as we could browse and search the whole API offline, without 
leaving the editor. But the documentations were hard coded to the engine and for those who makes plugins and libraries doesn't have any way 
to use that feature and It's harder for their users to understand how to use their  plugin/library, how to initialize it and what's the cleanup 
process. So they either have to read the source code or to search help through the internet.

My original proposal was to imlpement this with the new annotation system (which will be added in the new GDScript 2.0) but after discussing with 
the Godot devs and contributers  (https://github.com/godotengine/godot-proposals/issues/993) the plan was changed to implement it with comments. 
A comment starts with double hash symbol "##" considered as doc comment and it should be immediately above the method/property it documenting. 
Here is a working example.

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

The documentation for the above piece of code will generated and updated as the script is being written in the help window, it can be found by it's class name 
(if it doesn't have a class name it's path).

### Overview and Progress

The idea is when the script compiles, collect the description, brief_description, tutorials, links, etc. from the doc comments (comments starts with "##") 
and map the comments to the field it documenting. The name, data type, return type, setter, getter, default value, value of a const, etc. these information 
will extracted from the parse tree and with that It'll build a ClassDoc instance and update the editor help. It's the core part of the project and it's 
almost completed as of now. The other part of the project is the UI implementation. Visual Studio like C# XML comment autocompletion supports were implemented
and I'm currently focusing on generating an optional XML file of a documentation which could be exported to HTML by the user if needed.

![](https://github.com/ThakeeNathees/GSoC-2020/blob/master/progress-report-1/doc.jpg)

### What's Next

Currently the first part of the project is almost done and I'm working on the UI features and yet these are in my todo: With the documentation property of 
a GDScript, the code editor can show a brief description when a property/method is hovered and a description when auto-completing for the property/method. 
The description of an exported var could also be used as a tooltip. And an XML version of the documentation could also be generated for the plugin/library 
and it could be exported to HTML by the designer for his/her website. 
