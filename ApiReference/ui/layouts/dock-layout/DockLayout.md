---
nav-title: "Class ui/layouts/dock-layout.DockLayout"
title: "Class ui/layouts/dock-layout.DockLayout"
description: "Class ui/layouts/dock-layout.DockLayout"
---
## Class: "ui/layouts/dock-layout".DockLayout  
_Inherits:_ _Object_  
A Layout that arranges its children at its outer edges, and allows its last child to take up the remaining space. 

##### Static Properties
 - **dockProperty** - _Object_.    
  Represents the observable property backing the dock property.
 - **stretchLastChildProperty** - _Object_.    
  Represents the observable property backing the stretchLastChild property of each DockLayout instance.

##### Static Functions
 - **getDock(** view _Object_ **)** _String_  
     Gets the value of the Left property from a given View.
   - **view** - _Object_
   - _**return**_ - _String_
 - **setDock(** view _Object_, value _String_ **)**  
     Sets the value of the Left property from a given View.
   - **view** - _Object_
   - **value** - _String_

##### Instance Properties
 - **stretchLastChild** - _Boolean_.    
  Gets or sets a value that indicates whether the last child element within a DockLayout stretches to fill the remaining available space.
The default value is true.