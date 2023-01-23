---
title: Testing Extensible Storage in RevitPythonShell (RPS)
date: 2023-01-11
comments_id: 4
---

I tend to put most of my development time into working on scripts and applications in pyRevit, as I'm most comfortable in Python.  One benefit of using Python with the Revit API for me, as a relative novice, is that I can use [RevitPythonShell](https://github.com/architecture-building-systems/revitpythonshell) (RPS) as an interactive shell, which features autocomplete for the loaded Revit API libraries and hints for the arguments the API methods are expecting. This comes in very handy as it allow me to test Python code for the Revit API and get immediate feedback if anything is wrong in my code, as well as see what other types of methods or properties are available.

Today, I was thinking about how to store some arbitrary configuration settings for a pyRevit script I'm working on directly in the Revit projects, rather than in some .csv or .pickle file on the network. I had heard about using [Extensible Storage](https://www.revitapidocs.com/2022/79486a74-376c-9555-c873-45d5a750f051.htm) before, so I decided to give it a try in RPS to make sure I understood how to both store and later retrieve data from the model.  I found [this blog post](https://archi-lab.net/what-why-and-how-of-the-extensible-storage/) by Konrad Sobon on the archi-lab website about his initial foray into using Extensible Storage, and used it as a guide (in C#) for how to go about doing something similar in Python. He does a great job explaining it, but I thought a little simpler version for Python might be handy. (Also useful is Jeremy Tammik's 2011 post on [Extensibe Storage](https://thebuildingcoder.typepad.com/blog/2011/04/extensible-storage.html)) So: below is a (cleaned up) version of the RPS shell commands and responses for the inital definition and storage of the data, and then the separate retrieval.  

(Note: I've removed some of the lines where RPS returned the object being modified, for clarity. The `>>>` on each line is the RPS prompt, so don't just copy paste this into a new file)

### Create Schema and Store Data
Extensible Storage data must be attached to a Revit element, so since its eventually going to be related to project-wide settings, I'm adding the entity to the Project Information object.

```
IronPython 2.7.7 (2.7.7.0) on .NET 4.0.30319.42000 (64-bit)
Type "help", "copyright", "credits" or "license" for more information.
>>> import System
>>> ES = ExtensibleStorage # Rename already loaded library for convenience
>>>
>>> guid = System.Guid.NewGuid()
>>> sb = ES.SchemaBuilder(guid)
>>> sb.SetReadAccessLevel(ES.AccessLevel.Public)
>>> sb.SetWriteAccessLevel(ES.AccessLevel.Public)
>>> sb.SetSchemaName("TestSchema_Example")
>>> sb.SetDocumentation("An example of a schema to store arbitrary data")
>>> sb.AddSimpleField("Example_Field", str)
>>> schema = sb.Finish()
>>>
>>> entity = ES.Entity(schema)
>>> field = schema.GetField("Example_Field")
>>> entity.Set(field, "Test value to be stored in a field")
>>> 
>>> pi = doc.ProjectInformation #Revit object to attach data to
>>> t = Transaction(doc, "Add entity and data to project information object")
>>> t.Start()
﻿Autodesk.Revit.DB.TransactionStatus.Started
>>> 
>>> pi.SetEntity(entity)
>>> t.Commit()
﻿Autodesk.Revit.DB.TransactionStatus.Committed
>>> 

```

### Retrieve Data
Just to make sure I wasn't reusing already created Python objects, I closed and reopened RPS for a new interactive shell session.
```
IronPython 2.7.7 (2.7.7.0) on .NET 4.0.30319.42000 (64-bit)
Type "help", "copyright", "credits" or "license" for more information.
>>> pi = doc.ProjectInformation
>>> schemas = ExtensibleStorage.Schema.ListSchemas()
>>> len(schemas)
﻿22
>>> schema = [s for s in schemas if s.SchemaName == "TestSchema_Example"][0]
>>> schema
﻿<Autodesk.Revit.DB.ExtensibleStorage.Schema object at 0x00000000000005F1 [Autodesk.Revit.DB.ExtensibleStorage.Schema]>
>>> 
>>> entity = pi.GetEntity(schema)
>>> field_value = entity.Get[str]("Example_Field")
>>> field_value
﻿'Test value to be stored in a field'
>>> 
```
### Next Steps
Next step is to see about using fields to see if I can store lists or dictoaries using the Array and Map data types  - see [the Building Coder's post about Extensible Storage](https://thebuildingcoder.typepad.com/blog/2011/04/extensible-storage.html). It would also be awesome if I could somehow pickle my actual objects and store them as a blob or some other binary object, but I kind of doubt thats possible at the moment.

Thanks for stopping by!<br>
Dan
