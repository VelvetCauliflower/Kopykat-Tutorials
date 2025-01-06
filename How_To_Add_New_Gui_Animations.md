# How To Add New GUI Animations To Kopykat

 A scripting tutorial for editing Kopykat.
 
# Prerequisites

You will need to read about `GuiActionService` prior to starting this tutorial.

# Introduction

GUI animations are loaded into the GUI automatically in Kopykat. These are loaded from a `ModuleScript` named GuiAnimations.

By adding new `GuiActionService.GuiEventDeclarationType` objects to this ModuleScript, you can automatically add new GUI animations to Kopykat. 

<br>

# What You'll Learn

- The location of the GUI animations module.

- The general format of the `GuiAnimations` dictionary.

- Recommended practice for sorting the GUI animations.

<br>

#### Section A.1

# Where the GUI animations are stored.

GUI animations are stored in `Client.storage.gui_animations.GuiAnimations`. This Module script returns a dictionary that defines every GUI animation in the game as a `GuiActionService.GuiEventDeclarationType`. 

<br>

#### Section A.1

# The General Format

The index of each GUI animation is its subclass name. 
To better explain this, here is the current GuiAnimations file:
```lua
local GuiAnimations : {GuiActionService.GuiEventDeclarationType} = {

	defaultButton = require(Events.defaultButton),

	defaultRisingFrame = require(Events.defaultRisingFrame),

}
```

`defaultButton` and `defaultRisingFrame` are the subclass names of each `GuiActionService.GuiEventDeclarationType`. 

Therefore, if we wanted to make a button use the `defaultButton` subclass, we would set the button's `GuiSubclassName` attribute to `defaultButton`.

<br>

#### Section B.2

# Sorting Files

When creating a new `GuiActionService.GuiEventDeclarationType`, it's recommended that you create a new ModuleScript inside `Client.storage.gui_animations.generic` to define it. This reduces clutter, and ensures that the new code is easy to find.

 If you're creating several animations for a specific GUI, then they should be grouped together in a new folder inside `Client.storage.gui_animations`.

