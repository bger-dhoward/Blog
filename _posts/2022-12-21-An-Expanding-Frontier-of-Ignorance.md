---
title: "An Expanding Frontier of Ignorance"
date: 2022-12-21
comments_id: 2
---

> You might ask why we cannot teach physics by just giving the basic laws on page one and then showing how they work in all possible circumstances, as we do in Euclidean geometry, where we state the axioms and then make all sorts of deductions. (So, not satisfied to learn physics in four years, you want to learn it in four minutes?) We cannot do it in this way for two reasons. First, we do not yet _know_ all the basic laws: ***there is an expanding frontier of ignorance***. Second, the correct statement of the laws of physics involves some very unfamiliar ideas which require advanced mathematics for their description. Therefore, one needs a considerable amount of preparatory training even to learn what the words mean. No, it is not possible to do it that way. We can only do it piece by piece. 
> 
> <sub>Richard Feynman - Ch. 1 Atoms in Motion - The Feynman Lectures on Physics </sub> [^1]

## Introduction
This is an initial post to give a very brief and incomplete summary of the things I've worked on over the past year or so. My intent is to give individual posts that describe some of these projects in more detail.

### An incomplete listing of computation design projects:
- Graphic Program Generator - pyRevit
  - Initial Healthcare version - reads from CSV format used by our healthcare planners, converts to simple 2D Revit filled regions.
  - Aggregate version for University client - uses same CSV format, but with some nomenclature changes for client.  Used to visualize existing facilites
  - Existing Room-by-Room - for University client, placing each existing room (of varying size) on same line, with row wrapping
  - 3D Healthcare version - Uses 3D generic model family in Revit, with built in clustering. 
- Project Data website - Python, Flask, SQLite - **_Experimental_**
  - Processes exported Deltek data into a SQLite database to track updates to project forecasts
  - Lists projects to locally hosted Flask website, with custom visuals to show week-by-week project forcast hours per phase.
  - Allows for data fields to be added to each project, edited, and displayed all in browser.
- Randomized Modular Curtainwall Facades - Dynamo for Revit, Python
  - Based on Integer Partitions
- Build Tabular Programs - Python, Excel - **_Experimental_**
  - A Python tool to create more or less standardized Excel tabular program files for use in presentations out of more structured data sources (ie CSV or databases)
- Standardized Office Data Collection - pyRevit, SQLite - **_Experimental_**
  - A pyRevit tool to test the collection of standardized project data to a central database.
  - Gross Area Plan data
  - Net Room area data
- Cable Tray Clearance Generator - Dynamo for Revit
  - A tool to make up for the lack of built in clearance models for cable trays elements in Revit.
- Center Rooms and Tags - Dynamo for Revit
  - Dynamo script to center all tagged rooms in a view, then center the tags on the room elements.
- Planning models from existing CAD drawings
- Import color scheme to Revit from file
- Import data to rooms from CSV file
- Compare + Push/Pull data between Revit and Airtable
- Revit Model / Template auditing tools - mostly experiments
  - 3D views per workset - Dynamo
  - Export DWG Links data
  - Count Line Types

## Some experiences:
- Autodesk University 2022 in New Orleans
  - (to do: link to AU website video playlist)

### Thanks for stopping by!
Dan

[^1]: I like this quote because it speaks to how, with every new subject we come in contact with, the amount of things that we know that we don't know is always expanding. I'm learning new things, and going to try to use this bloggy thing to help organize, document, and share what I'm working on and learning.
