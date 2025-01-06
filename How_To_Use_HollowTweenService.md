# How To Use HollowTweenService
 A scripting tutorial for Kopykat's scripting API. 
 
# Introduction
 This tutorial will teach you how Kopykat's `HollowTweenService` library works, and why we use it.

<br>

# What You'll Learn

- Creating dynamic tweens with `HollowTweenService`.

- Using arithmetic to edit Tween properties with `HollowTweenType`.

- Resetting Tweens smoothly with `HollowTweenType`.

<br>
#### Section A.1
 # Creating Tweens with HollowTweenService
> **WAIT** First things first, why are we using HollowTweenService instead of TweenService?
 
`HollowTweenService` is a library that works alongside `TweenService`. It allows us to create `Tween`s that can be *written* to.

In Roblox, the `Tween` instance is read-only. This means we can't change it after it has been created. This means that if we're only using Roblox's `TweenService`, it impossible to define a single Tween for many instances. 
   
 Instead of creating `Tweens`, the `HollowTweenService` library creates `HollowTweenType`s. These are roughly identical to regular `Tween`s, except that they cannot be played. To play a `HollowTweenType`, we have to first convert it to a `Tween`.

Got that? Don't worry, it's simpler than it sound!

<br>
#### Section A.2.
> **WAIT** How do I make a HollowTweenType in a `script`?

Now that you know what a `HollowTweenType` is, let's create one. First, let's create a new `script` in the same directory as `HollowTweenService`. For illustrative purposes, a script like this would require the library like so:

``` lua
-- ExampleScript1.lua

local HollowTweenService = require(script.Parent.HollowTweenService)
```

Great! The library can now be used. We can create a new HollowTween using the function `HollowTweenService:create()`. This function takes three parameters:
<br>
> **DOCUMENTATION** 

| Parameter      | Data Type | Description |  
| ------------- | ------------- | ------------- |
fullChildName | `string?` | An optional property that defines the instance relative to the target you wish to access. This could be a string such as **UIStroke** or **Parent.Frame.TextButton**.
tweenInfo | `TweenInfo` | The TweenInfo that will be used in the actual Tween.
propertyTable | `dictionary` | The Instance properties that will change with the Tween.
delay | `number?` | An optional property that defines a scriptable delay.

<br>

So, given this information, we know that at the bare minimum, our `HollowTween` needs a `TweenInfo` object and a `PropertyTable` object. Let's do just that!

``` lua
-- ExampleScript1.lua

local HollowTweenService = require(script.Parent.HollowTweenService)

-- First create the TweenInfo.
local tweenInfo = TweenInfo.new(10, Enum.EasingStyle.Sine)

-- Now we define the property table. This contains the property Size.
local propertyTable = {Size=Vector3.new(2, 2, 2)}

-- Now we can finally create the new HollowTween.
local myHollowTween = HollowTweenService:create(nil, tweenInfo, propertyTable)
```
<br>
Okay, we have a `HollowTween`, but how do we play it? We can play this `HollowTween` by converting it to a normal `Tween` using `HollowTweenService:toTween()`.

This function takes two parameters:

> **DOCUMENTATION** 

| Parameter      | Data Type | Description |  
| ------------- | ------------- | ------------- |
rootInstance | `Instance` | The instance that the `HollowTweenType` will eventually be changing.
baseHollowTween | `HollowTweenType` | The `HollowTweenType` that will be used to define the new Tween.

<br>
Based on this function definition, we know we'll need to input an instance, and we'll need to input a target instance, and a `HollowTweenType`.
Let's say we want to play the `HollowTweenType` with the Baseplate as the target instance. We can do that like so:

``` lua
-- ExampleScript1.lua (Continuing)...

-- First, we'll define our target instance.
local baseplate = workspace.Baseplate

-- Now we create a new Tween using the HollowTweenType we made before.
local newTween = HollowTweenService:toTween(baseplate, myHollowTween)

-- Lastly, let's play this new Tween!
newTween:play()
```

With that, the baseplate will shrink into a 2x2x2 cube in 10 seconds. Congratulations, you made your first `HollowTweenType`, and then applied it!

#### Section B.1.

 # Using arithmetic inside a HollowTween

 > **WAIT** How can I write to the values of a `Tween` using a `HollowTweenType`?

 It was previously mentioned that you could write to HollowTweens. You could do this manually, but HollowTweens come with several features to automatically write this information.

 One way we can do this is by multiplying, adding, subtracting, or dividing within the property table. We can do this by setting a property's value to a table, where the first value is a mathematical operator, and the second value is the operand. Here's an example:
``` lua
-- ExampleScript2.lua

local HollowTweenService = require(script.Parent.HollowTweenService)

-- First create the TweenInfo.
local tweenInfo = TweenInfo.new(10, Enum.EasingStyle.Sine)

-- Now we define the property table. This contains the property 
-- 'Size'. The table within tells the HollowTween that this animation 
-- divides the size by 100.
local propertyTable = {Size={"/", 100} }

-- Now we can finally create the new HollowTween.
local myHollowTween = HollowTweenService:create(nil, tweenInfo, propertyTable)

-- Now we create a new Tween using the HollowTweenType we made before.
local newTween = HollowTweenService:toTween(workspace.Baseplate, myHollowTween)

-- Lastly, let's play this new Tween!
newTween:play()
```

Wonderful! The baseplate will now resize to a hundreth of its original size. This is just one of four operators:
> **DOCUMENTATION** 

| Operator      | Description |  
| ------------- | ------------- | 
 `+` | Adds the operand onto the original value. 
 `-` | Subtracts the operand from the original value. 
 `/` | Divides the operand by the original value. 
 `*` | Multiplies the operand by the original value. 

<br>

#### Section C.1.

 # How to smoothly reset a Tween.

 As a special feature of HollowTweenService, we can undo all changes made with `Tween`s. We can do this by setting the property we want to reset in the properties table to `"?"`. Here's an example property table that does this:
 ```lua
 local propertyTable = {Size="?"}
 ```

 If we were to play a HollowTween using this property table, it would reset the instance back to its original state.

> <span style="color : ORANGE"> **WARNING** <span>  <span style="color : LightGray"> A property table that resets a `Tween` **must** be converted to a `Tween` before the instance is altered, else it **will not work**. <span>  

<br>

#### Section D.1.

 # Accessing child and parent instances in a Tween

You may remember that the first parameter of the `HollowTweenService:create()` command 
has been set to `nil` in the the previous example scripts.
This property, referred to as `fullChildName`, lets us access the parent and
child instances of the instance that is being Tweened.
``` lua
-- ExampleScript3.lua

local HollowTweenService = require(script.Parent.HollowTweenService)

-- First, let's define the name of the child instance. 
local childName = "Texture"

-- Next, let's create the TweenInfo.
local tweenInfo = TweenInfo.new(10, Enum.EasingStyle.Sine)

-- Now we define the property table. This contains the property 
-- 'Transparency'. The table within tells the HollowTween that 
-- this animation divides the size by 100.
local propertyTable = {Transparency=1}

-- Now we can finally create the new HollowTween.
local myHollowTween = HollowTweenService:create(childName, tweenInfo, propertyTable)

-- Now we create a new Tween using the HollowTweenType we made.
local newTween = HollowTweenService:toTween(workspace.Baseplate, myHollowTween)

-- Lastly, let's play this new Tween!
newTween:play()
```

 If the Baseplate contains an Instance named `Texture`, then you should 
now see that texture slowly disappear!
We can also use this same process to access several children. 
Here's an example of Tweening the Baseplate texture from the workspace:
```lua
local childName = "Baseplate.Texture"
local myHollowTween = HollowTweenService:create(childName, tweenInfo, propertyTable)
local newTween = HollowTweenService:toTween(workspace, myHollowTween)
```
>**NOTE** The full child name can also access object properties, like `Character`, `PrimaryPart`, or `Parent`.

> <span style="color : ORANGE"> **WARNING** <span>  <span style="color : LightGray"> Attempting to access a parent or child instance that **doesn't exist** will result in an **error**. <span>  


<br>

> **WAIT** Why is this feature necessary? Why wouldn't I access the child or parent directly? 

 There are certain cases when this is simply not possible. A great example is `GuiActionService`. The point of `GuiActionService` is to let us define animations for GUI once, and never again. These animations are intended to target several different instances at once.

Because we can't know what instance we will be targetting, it is impossible to Tween the child or parent instances without creating another event specifically for them.
 
By adding references to the instance's children before we create the Tween, we can access the same children for many instances without ever creating redundant code.
