# How To Use GuiActionService
 A scripting tutorial for Kopykat's scripting API. 



# Prerequisites
This tutorial requires `HollowTweenService`.


# Introduction
 This tutorial will teach you how to add new GUI animations using `HollowTweenService` and `GuiActionService`. These libraries have been created to simplify the creation of Tweens for many instances.

<br>

# What You'll Learn

- Creating GUI subclasses, `SubclassEventType`, for `GuiActionService`.

- Using conditional formatting with a `SubclassEventType`.

<br>
#### Section A.1.
# Creating our first GUI subclass.

Here's a summary of what we will be doing in this section:

1. We create a table containing one or more `GuiEventDeclarationType`s.
2. We then convert this table into a `SubclassEventsType` with the `GuiActionService:newSubclass()` function.
3. For every GUI instance we want to animate, we set the `GuiSubclassName` attribute to the name of our new subclass.
4. We initiate our GUI with GuiActionService:initScreenGui(). Once this has been ran, new GUI and currently existing GUI will automatically be animated using matching, registered subclasses.

First, we're gonna need GUI to work with. For illustrative purposes, here is some example code
to create a new TextButton on the screen:
```lua
-- ExampleScript1.lua
local newScreenGui = Instance.new("ScreenGui")
newScreenGui.Parent = game.Players.LocalPlayer.PlayerGui
local newButton = Instance.new("TextButton")
newButton.Parent = newScreenGui
```

Okay! We have some GUI we can begin animating.

To begin, we must first create a table containing at least one `GuiEventDeclarationType`.
> **WAIT** What is a GuiEventDeclarationType?

> **DOCUMENTATION** *GuiEventDeclarationType*

| Property      | Data Type | Description |  
| ------------- | ------------- | ------------- |
eventName | `String` | The name of the RBXScriptSignal or event. This could be something like `MouseEnter` or `MouseButton1Click`.
eventData | `{name : string, value : any, inverted : boolean}` | An optional table that assists with conditional formatting for the `GetPropertyChangedSignal` event.
sound | `Sound?` or `String?` | An optional property that tells GuiActionService to play a sound when the event fires. This can either be a Sound instance or a Sound id.
tweens | `{HollowTweenType}?` | A table that contains every animation that will play during the event. These are defined using `HollowTweenType`s.
methods | `{(targetInstance : GuiObject) -> nil}?` | An optional table containing functions that will play during the event.

By reading this table, we know that at minimum we will require an **event name**, and a **table of `HollowTweenType`s**. So let's do just that!

```lua
-- ExampleScript1.lua (Continuing)...

local GuiActionService = require(script.Parent.GuiActionService)

-- First, I'll make a variable for my new text color.
local white = Color3.new(1, 1, 1)

-- Next, I'll make a table containing every HollowTweenType. There's a single tween here, which sets TextColor to white in 1 second.
local myTweens = {
    HollowTweenService:create(nil, TweenInfo.new(1), {TextColor3=white}),
}

-- This is our first event. The MouseEnter event name means that when the mouse hovers over the GUI element, the event will fire. When the event fires, every HollowTweenType in the 'tweens' table will play.
local mouseEnterEvent =  {
    eventName = "MouseEnter",
    tweens = myTweens
}

-- Finally, we can define the event table for our new button events.
local awesomeButtonEvents = {mouseEnterEvent}
```

Wonderful! We're halfway through. Next, we will need to register these events as a new subclass. Here's how I would register a new subclass, which I'll name `awesomeButton`. This will use the events we created earlier for `myAwesomeButton`.

```lua
-- ExampleScript1.lua (Continuing)...

-- First, we define the new subclass.
local awesomeSubclass = GuiActionService:newSubclass("awesomeButton", myAwesomeButton)

-- Next, we register the subclass into GuiActionService.
GuiActionService:registerSubclass(awesomeSubclass)

```

 There's only a few more steps. Now that we have awesomeButton registered as a subclass, we're going to need to link our button to the subclass. We can link the button by setting the `GuiSubclassName` attribute of the button.

 After we set the attribute name, we initiate our `ScreenGui` from before. Once it has intiated, all GUI elements with a `GuiSubclassName` equal to `awesomeButton` will have animated text color when the mouse hovers over it.

 Here's how we would complete our script to do this:
 ```lua
-- ExampleScript1.lua (Continuing)...

-- First, we link the button we made at the beginning to awesomeButton.
newButton:SetAttribute("GuiSubclassName", "awesomeButton")

-- Now we finally initiate the screen gui we made at the beginning!
GuiActionService:initScreenGui(newScreenGui)

-- The end!
 ```

Congratulations! You made your first animated button using GuiActionService! Now when you hover over the button, its text color will change.
> **NOTE** You can set the `GuiSubclassName` attribute in the Explorer before starting the game.

> **NOTE** You cannot change the `GuiSubclassName` of an instance after it has been created.

#### Section A.2.
# All Event Names

Before we move on, here is a list of all supported event names.

> **DOCUMENTATION** *GuiEventDeclarationType.eventName*

| Property      | Description |  
| ------------- | ------------- |
`MouseButton1Down` | Fires when the mouse button is pressed on a Gui element.
`MouseButton1Up` | Fires when the mouse button is lifted on a Gui element.
`MouseButton1Click` | Fires when the mouse button clicks a Gui element.
`MouseEnter` | Fires when the mouse hovers over a Gui element.
`MouseLeave` | Fires when the mouse stops hovering over a Gui element.
`MouseMoved` | Fires when a mouse moves over a Gui element.
`PropertyChangedSignal` | Fires when a property defined in the `GuiEventDeclarationType`'s `eventData` has changed.


<br>

#### Section B.1.
# Conditional Formatting

There's one extra feature of GuiActionService. We can detect when a specific property has changed, and even detect whether that property is equal to another value!

To do this, we first need to set the `eventName` of the `GuiEventDeclarationType` to `PropertyChangedSignal`. 

Then, we must setup the `eventData` of the same `GuiEventDeclarationType`.

<br>

> **WAIT** How do I define event data?

eventData is a very simple object, it has three properties. They have been listed below.

> **DOCUMENTATION** *GuiEventDeclarationType.eventData*

| Property      | Data Type | Description |  
| ------------- | ------------- | ------------- |
name | `string` | The name of the property you will be detecting.
value | `any?` or `{any}?` | An optional value or table of values to compare to the Instance's property value. If `eventData.value` is defined, then the event will only fire if the target Instance's property equals any value in `eventData.value`. If this value is set to `nil`, then the value will be ignored, and then the event will fire everytime the property changes. 
inverted | boolean? | An optional option. If set to `true`, the value comparison will change. Instead, the event will only fire if all values of `eventData.value` **do not** equal the Instance's property value.

<br>
Given this table, we now know that we can play Tweens during property changes.

> **NOTE** By using the `inverted` property, we can safely undo changes made using the `PropertyChangedSignal` event. If the Instance's property value no longer equals any of the written values, then we can Tween it back to normal by setting properties to `"?"` in our `HollowTweenTypes`.

In the example below, I am setting the `newTextBox.TextColor3` to the color red when `newTextBox.Text` equals `red`.
```lua
-- ExampleScript2.lua (Continuing)...

-- Let's define our GUI just like before, but with a TextBox instead.
local newScreenGui = Instance.new("ScreenGui")
newScreenGui.Parent = game.Players.LocalPlayer.PlayerGui

local newTextBox = Instance.new("TextBox")
newTextBox.Parent = newScreenGui

-- Next, let's require the library.
local GuiActionService = require(script.Parent.GuiActionService)

-- First, I'll make a variable for my new text color.
local red = Color3.new(1, 0, 0)

-- Next, I'll make a table containing every HollowTweenType. There's a single tween here, which sets TextColor to red in 1 second.
local myTweens = {
    HollowTweenService:create(nil, TweenInfo.new(1), {TextColor3=red}),
}

-- Then I define my event data. This tells GuiActionService that the text
-- must first equal "red" before the event can fire.
local myEventData = {
    name = "Text", 
    value = "red"
}

-- Finally, we put everything together. The event name has been
-- set to PropertyChangedSignal so that the text change can
-- be detected.
local textChangedEvent = {
	eventName = "PropertyChangedSignal",
    eventData = myEventData
	tweens = myTweens
},

-- This code has been shortened. But it is the same process as before
-- First, set the GuiSubclassName attribute to awesomeBox to link the text box.
-- Then, we finally initiate our UI using a table of events. 
newTextBox:SetAttribute("GuiSubclassName", "awesomeBox")
GuiActionService:initScreenGui({textChangedEvent})
```

Now, when we write the word 'red' into this textbox, it will turn red!
