---
layout: default
title: Language Reference
parent: Simple
nav_order: 5
last_modified_date: 2022-06-01
---

This is an extremely high-level overview of the Simple script language, mostly to help explain what different words (e.g. markup) mean and not necessarily how to use them all together. Simple script is comprised of a few high-level parts, so just to familiarize yourself with those big ones:
- **[Components](#components)**: Definitions for entities with properties. Can be Conjured to create them. Executes script in Sections.
- **[Properties](#properties)**: Storage for Components. Can be typed as values (numbers, vectors, and orientations) or Components.
- **[Operations](#operations)**: Script commands that Components run.

Components and Properties have **Flavors**, indicated by their color, which dictate their basic behavior.

Components, Properties, and Operations may optionally use **Markup**, indicated by words in parentheses, which may change their behavior but does not change the basic behavior indicated by their flavor (for Components and Properties).

# Components

Components are definitions for entities. They - and their properties - do not exist until Conjured.

Once conjured, a component with a `health` property will die when its `health` is 0 or less by the end of the frame of execution. Only the `base` component has no `health` (can be used like a function).

## Component Flavors

- **Persistent** (green): Once conjured, component will exist normally.
- **Immediate** (orange): Once conjured, component will exist only as long as another component keeps a [needed](#property-markup) property referencing it.

## Component Markup

- **(conjured)**: An instance of the Component is automatically conjured when an a simulation containing it runs. Effectively a singleton.
    - **(captured)**: Only available if component is marked with `conjurer is` and `captured by`. An instance of this component is automatically conjured from the `conjurer is`'s **{ extra captures }** property every time a component of the type that conjures it is conjured. See [Captured Components](#captured-components).
- **(published)**: Component appears in menus outside of its own ai file. No effect at runtime.
- **(conjurer is ...)**: Indicates the Component's `conjurer` should be a certain type of Component.
    - **(conjurer is \<on-demand\>)**: The Component will be conjured on demand, e.g. by attempts to access its properties, and it will never exist more than once. Can be used on an Immediate component to create collections of constant data since the user can attempt to access the definition in the interface and its properties.
- **(captured by ...)**: The Component can be [captured](#captured-components). Declaring `<any type>` means you don't care about the order of execution. A more basic type will insert its execution into that order of the script.

## Component Sections

Components may have any of the following sections which add storage and/or script logic.

When determining order of execution for component extensions, we execute from the most basic to the most extended for initialize and update, and in reverse of that for finalize.

- **PROPERTIES**: Storage for the component's properties. Performs calculations when conjured before the INITIALIZE section runs.
- **INITIALIZE**: Executes once, when the component is conjured.
- **UPDATE**: Executes every frame the component is alive.
- **FINALIZE**: Executes once, when the component is dying.

## Captured Components

A component which is marked to be `(captured by)` another component will affix itself to any valid component when assigned to a `(captured)` property in it.

When captured, that component stops executing in parallel with its captor and becomes a serialized part of the captor's execution. This has a few important effects:
- It has live property access to its captor - and vice-versa, the capturing component can access its captive - without needing to use the Synchronize command.
- The order of execution can be controlled by the `(captured by)` setting.

Instances of captured components may **only be captured by one instance** of a component. This means you cannot, say, Conjure a component and then later attempt to capture it in another property. You may only propertly capture by:
- Using Assign to place a component definition into a `(captured)` property.
- Using the `from` markup for Conjure to tell the component what property it should be placed into.
- Using the `(captured)` markup on the component definition.

# Properties

Properties are either part of a component because they were manually added (colored boxes) or have been inherited from a more basic component it extends (colored text with white backdrop).

Properties can be read between components but are only allowed to be set by the owning component or a [Synchronize](#synchronize) operation. When reading a property that is not owned by you (i.e. in a different component), you will only see the start-of-frame value of the property, and not any changes made during the frame.

## Property Types

- **value**: A floating point number.
    - **rng**: A number which, when Evaluated, will return a random value between `0.0` and `1.0`. Right click a **value** and select _rng_ to make it an rng, and vice-versa.
- **vector**: A collection of 3 numbers (x, y, z).
- **orientation**: A collection of 3 orientation values (tilt, facing, roll) in degrees. Also have forward, up, and right vectors which can be read from - changing based on their angle values - or set - modifying their angle values.
- **component**: A reference to a component. Can be set to null. Is automatically set to null if it is referencing a conjured component that dies.

### Collections

All properties may be marked to be a collection, indicated by `{ curly braces }`, meaning multiples of any matching property may be stored in it. Value-type collections (including vector and orientation) always initialize with one member. Component-type collections may be empty, which is equivalent to null.

Evaluating or Assigning a `{ collection }` to a number will yield the size of the collection.

#### Collection Indexer

When casting a `{ collection }` to a single type (e.g. `Assign property = { collection }`), the first member of the collection is always returned. Sometimes you will want to _index_ in to the collection and get a member in the middle or end of the collection. This is achieved by using the _indexer_, which is a two step process.

The indexer starts at 1, meaning the first member of the collection. To get a different member, set the `indexer` to be the depth in the collection you want. For example, the last member in the collection would use an `indexer` equal to the size of the collection.

Once assigning the `indexer`'s value, if you cast an object to the `{ collection } [ indexer ]`, you will get the member at that index. For example, getting the last member of a collection:

![](/assets/simple/simple_indexer.png)

Or you could iterate through a collection by index.
> _Note_: repeat Filter/Evaluate's `<index>` starts at `0`, while `indexer` starts at `1`.

![](/assets/simple/simple_indexer2.png)

Specifying an index out of range for the `indexer` (e.g. a negative number, or one larger than the collection) shows a warning at runtime and makes any attempts to assign to it fail.

## Property Flavors

- **immediate** (orange): The property will initialize to the specified value every frame. Any changes made to it during execution will be discarded.
    - **synchronized** (blue): The property is like a _pointer_; the initialization specifies what it is pointing at, and further Assign operations modify what is stored there. Required to change properties in a `value` collection. If valid at runtime, permits live access (as if you had run a Synchronize operation) if the runtime thinks you should have it.
- **persistent** (green): The property will initialize to the specified value but remember any changes made to it during script execution.
    - **needed** (black): The property will _need_ an [immediate](#component-flavors) component, persisting it as long as it is has health.

## Property Markup

- **(published)**: Property appears in menus outside of the owning component's ai file. Has no effect at runtime.
- **(conjured)**: The component assigned to this property _must_ be conjured. If given a definition, it will conjure it.
- **(unconjured)**: The component assigned to this property _must_ be a definition. If given a conjured component, it will point to its definition. _Note_: Currently it will not immediately be a definition if you Assign a conjured component and then immediately query it. It will be a definition on the next frame.
- **change type** (for inherited properties): Allows you to specify a more specialized version of property in an extension, implying a cast. Only affects the Simple menus, has no effect at runtime.
- **failable** (shown as `=?`): If the assignment fails, do not print a warning in the log.

## Complex Property Initialization

Property initialization is limited to a single _Assign_ line, meaning you can only add one operator and do no more than what it and a single Assign's interface allows. Sometimes, you will want more involved initialization, like adding multiple members to a collection, setting separate parts of a vector, or even conditionally initializing properties differently depending on some logic. Complex property intiliazation is available when you want this.

![](/assets/simple/simple_property_init.png)

A complex property initialization block is represented by a `...` on the right-hand side of a property initialization. It happens when you right click and select `add initialization` and can be removed by right clicking and selecting `remove initialization`.

Within an initialization, a scoped `<property>` property is available, representing the property being initialized. No operations are allowed which could trigger other, script side effects (i.e. Synchronize, Conjure), and no Assign is allowed of anything but the scoped `<property>` or properties added to a Block's PROPERTIES section. All other commands (Filter, Evaluate) are available, and all the same rules apply for operations.

Complex initializations obey the same rules of the [property flavors](#property-flavors), so an _immediate_ property would execute its initialization every frame, for example.

An important reason to use complex property initializations is to ensure the start-of-frame property is initialized correctly, so outside observers will see a good start value.

![](/assets/simple/simple_property_init2.png)

In the above example:
- **Not Ideal**: Collection is populated in INITIALIZE. Requires a persistent property that is going to change (but only once). Collection will be empty to outside observers until until the next frame.
- **Better**: Collection is populated in a property initialization. Collection will be 'correct' on the first frame. No real script (INITIALIZE) script required.
- **Ideal**: Collection is marked as _immediate_. The collection is effectively a constant and behaves like one, without the need for a persistent property.

# Operations

Operations are script commands which run in property SECTIONS and in [complex property initialization](#complex-property-initialization). They have a few types of [markup](#operation-markup). as well as some specific to various operations.

The behavior for operations with indents (everything but Assign) is: "if the operation succeeds, enter the indented block".

## Evaluate

Evaluate a number (for _repeat_) or if a statement is true.

### Evaluate Markup

- **repeat**: Repeat the script in the block as many times as the Evaluate calls for (e.g. `Evaluate 3` means run 3 times). Adds a scoped `< index >` property to which starts at 0 and counts up for each iteration.

## Assign

Assign a property to be something.

### Assign Markup

- **failable** (shown as `=?`): If the assignment fails, do not print a warning in the log.

## Filter

Filter a collection for members which pass an evaluation and return a scoped `< subset >` if any pass, optionally sorting them by some numerical basis. To query a property in each member of the collection, a scoped `<- candidate` property is added to the `filter by` and `sort by`.

### Filter Markup

- **repeat**: Repeat the script in the block once for every member in the `< subset >`. Adds a scoped `< member >` which is the current item in the `< subset >` on each iteration and a scoped `< index >` property which starts at 0 and counts up for each iteration.

## Block

Creates an indented Block, which can optionally include some text (no effect on runtime, just helpful for organization). Block can optionally add a PROPERTIES section to it, allowing the creation of Block-scoped properties.

## Conjure

Conjure a component, creating a new instance of its definition.

### Conjure Markup

- **from**: Conjures the component from a specific property, assigning it to that property. Required when trying to conjure a [captured component](#captured-components).
- **make immediate/persistent/neutral**: By default (neutral), use the flavor specified by the component. Otherwise, Conjure the component as if it were the specified flavor (immediate/persistent). The color of the component name will change to match. Gray is neutral.

## Synchronize

Synchronize has a lot of hidden complexity which will not be detailed here, but it should be used to gain access to setting properties in another component. A good rule of thumb is to only use Synchronize for that explicit purpose and to only use it infrequently (e.g. in INITIALIZE, FINALIZE, or during infrequent occasions - like a missile collision).

Synchronize makes a value _live_, meaning you will see changes made to the property during the current frame of execution. To achieve this, Synchronize forces the target component to execute before the calling component. This creates serialization and diminishes performance. 

## Operation Markup

### else

`else` will make an operation execute only if the prior operation (Evaluate, Filter, Synchronize) was false.

### Printing

When an operation has a print, it gets an icon on the righthand side of the line indicating it.
> _Note_: Prints are always in order within a single component's execution, but if many components print on the same frame, it will not necessarily be representative of the actual execution order (which is all in parallel!).

- **no print** (default): Don't print anything.
- **print to host**: Print to the Output log each time this executes.
- **print to target**: Print to the view in the top left corner each time this executes.

> _Note_: The tool is not very defensive about log flooding right now. If you are experiencing tool hangs when running, make sure you don't have tons of prints to host.

Different operations have slightly different behavior when printing:
- **Evaluate**: For a number, print the number. For an expression or an object, print `0` (false/null) or `1` (true/non-null).
- **Assign**: Print the result of the assignment.
- **Filter**: Print the resulting `< subset >`.
- **Block**: Print the text of the Block.
- **Conjure**: (rarely used) Print as long as this object is alive.
- **Synchronize**: Print if the Synchronize succeeded.

## Flow Out

Not referred to by name in the interface, but the green arrows within every script block indicate _flow_. By default, they point down, meaning "go to the next line". They can be manipulated by clicking on lines to the left of the indent, allowing you to _go out_ as many indentations as you want.

It can be used when trying to break out of a loop.

![](/assets/simple/simple_flow_out.png)

It can be used to author things like an `OR` statement.

![](/assets/simple/simple_flow_out2.png)

Placing a flow arrow to the leftmost position beyond the edge of a component means "end execution this frame".

![](/assets/simple/simple_flow_out3.png)

# Namespaces

Namespaces can be used to organize components, grouping them together in script but also in submenus when looking at the definitions list.

Namespace subcategories can be created by nesting namespaces within one another or by using a `->` in the text of the namespace, indicating it belongs to a subcategory.

![](/assets/simple/simple_namespaces.png)

In this example, both `Marble 1` and `Marble 2` will appear in the menus under `balls > marbles`.