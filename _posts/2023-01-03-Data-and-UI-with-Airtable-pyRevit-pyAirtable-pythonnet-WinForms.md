---
title: "Data and UI Tests with Airtable, pyRevit, pyAirtable, pythonnet, and WinForms"
date: 2023-01-03
comments_id: 3
---

## Background

I'm currently working on a planning project where a client provided about 2400 rows of data on rooms in nine existing buildings.  In order to manage and make usable this data, we extracted it from the original CSV / Excel files, and imported it as a series of linked tables in [Airtable](airtable.com). We were then able to process the data and add additional categorization information on top of the client-provided data.

The client also provided DWG files for each floor of the existing buildings, fortunately with a closed boundary line for the interior wall face of each room on its own layer, as well as text labels of each room on a different layer. I built a Dynamo script to import the CAD lines as Revit room boundaries, and a separate Dynamo script to import the label data as a new room, with the correct data assigned.  

I then created a series of [pyRevit](https://www.notion.so/pyrevitlabs/pyRevit-bd907d6292ed4ce997c46e84b6ef67a0) scripts to import the processed data from the Airtable extracted as new CSV files, which would match up each room in each building between Revit and CSV rows, and import all of the data into our customized Revit shared parameters.  

## The Problem(s)

### Problem 1: Data Flow
The Airtable > CSV > pyRevit > Revit workflow works great for when we make updates in Airtable, but if for whatever reason we want to make an update in the Revit model, then we would need to manually update the Airtable to match, or else  export data from Revit to CSV and run an importer to Airtable.  If there happen to be changes made in both locations, however, either of these process may result in newer data being overwritten by older data. To me, both the manual and bulk updata processes are ripe for errors, so we've mostly just stuck to making updates only in Airtable.

We're starting though to bring in team members that only make updates in Revit, or who won't have access to the Airtable, so its high time to implement a solution.

#### Problem 1 Solution: pyAirtable

In browsing through the [Airtable API docs](https://airtable.com/developers/web/api/introduction), I noticed a few Python libraries.  One of them, [pyAirtable](https://github.com/gtalarico/pyairtable), happens to be Gui Talarico, who I've known about from his work on various other AEC tools/plugins/applications, so I decided to start giving it a try. I was able to get an inital pyRevit tool that would connect to the Base through the pyAirtable/Airtable API, then compare each room in the open Revit model to the base data and print any discrepancies out to the pyRevit console.

### Problem 2: UI Library compatibility

The next step in getting a more customized updater is to build a user interface that will allow the user to select which rooms from the list of discrepancies are going to be updated, and which direction to push the data, to Revit or to Airtable. (Ideally, you'd also be able to select individual parameters to update, but we'll see how far we get...) While pyRevit does have some built-in forms capabilites, pyAirtable is a Python 3 library, and setting the shebang line in a pyRevit script to Python 3 happens to break a some of native pyRevit IronPython (Python 2) libraries.  And since pyRevit is using an embedded version of CPython 3, its not possible to import Tkinter, like I've done before for other UI tools.

In browsing through some other pyRevit files, I had noticed that `System.Windows.Forms` was used to accomplish some UI tasks. I wasn't familiar with using this, and whenever I'd try to get those methods to work in IDLE (my go-to test bed for understanding and testing basic Python and library functionality), I couldn't figure out how to get the libraries to import. 

#### Problem 2 Solution: 

The key was that these scripts were using `import clr` which is already configured in the embedded Python versions in these applications, but wasn't a default Python 3 library.  To get a testable library setup for use with vanilla Python 3 / IDLE, I needed to install [pythonnet](https://github.com/pythonnet/pythonnet) via `pip install pythonnet`, which then allowed the `import clr` and subsequent `clr.AddReference(...)` statements to work in my IDLE environment. It appears that clr / pythonnet works in both IronPyton 2 and embedded CPython 3, so I'm now able to start putting together a UI.

### Next Steps: Develop UI, Learn WinForms

I think I finally have the pieces in place to be able to both develop in pyRevit as well as test in IDLE, so the next steps are to learn WinForms enough to put together some basic UI elements. Not knowing exactly how System.Windows.Forms really worked, I found the Forms videos in this series of YouTube tutorials by [Sigma Coding](https://www.youtube.com/playlist?list=PLcFcktZ0wnNnz07eWc7N5ao1dyiXoV-ib) to be very helpful. It also pointed me toward the [Microsoft .Net Documentation](https://learn.microsoft.com/en-us/dotnet/api/system.windows.forms?view=netframework-4.8) that I had been missing.

For today, I've got a pop-up window working now from a pyRevit button which now prints to the discrepancy data from the Airtable comparison to a WinForms window in stead of the pyRevit console, so thats a promising start for me.  

![firstUItest](https://user-images.githubusercontent.com/101344143/210449728-19fc4330-7578-419d-85fd-88198dc31480.PNG)

