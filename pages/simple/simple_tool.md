---
layout: default
title: Simple Tool UI Overview
parent: Simple
nav_order: 4
last_modified_date: 2022-05-23
---

# Tool Layout

![](/assets/simple/simple_tool_diagram.png)

1. **[Toolbar](#toolbar)**: Actions for running the viewer, debugging a running simulation, and [configuring multiplayer](#multiplayer-config).
2. **Simulation / File Explorer**: All included simulations (sims) and their script (ai) files are listed here. Double-clicking an ai file will open it in the _Script Editor_.
3. **Output Log**: Errors from the tool/runtime and user-specified prints.
4. **[Script Editor](#script-editor)**: Script editor, shows the list of open scripts in tabs.
5. **Breakpoint Debugger**: Shows information when using breakpoint debugging.

# Toolbar

![](/assets/simple/simple_tool_diagram2.png)

1. **Viewer Selector**: Lets you pick a runtime to use as the simulation viewer (only _viewer.exe_ should be selected).
2. **Start / Restart Simulation**: Starts the simulation and runs the selected viewer. Also starts/joins a multiplayer game depending on configuration. See [Multiplayer Config](#multiplayer-config).
3. **Debugger Controls**: When running a simulation and using breakpoints, controls the flow of execution. See [Debugger Controls](#debugger-controls).
4. **Multiplayer Config**: Settings for configuring/hosting/joining multiplayer games. See [Multiplayer Config](#multiplayer-config).

## Multiplayer Config

This UI is very much a work in progress and likely to change, but here are steps to follow to get multiplayer games running.
> _Note_: `ID`'s must contain no spaces.

### Running a Server

To play any form of multiplayer, you must utilize the `trivial_signaling_server.exe`, included with Simple. If you are playing locally, simply run it.

To be a _peering_ server, allowing play between players over the network, someone must run that executable and have a publicly accessible IP/domain with port 10000 open. 

### Local Play, Single Player

To play locally with just one viewer.

![](/assets/simple/simple_tool_multiplayer1.png)
- **ID**: `Any string`
- **Invite**: `0`
- **Join**: `<blank>`
- **Server**: `localhost`

### Local Play, Multiplayer

To play locally, but with multiple viewers (e.g. for testing multiplayer behavior). `Server` must be `localhost`.

![](/assets/simple/simple_tool_multiplayer2.png)

- **ID**: `Any string`
- **Invite**: `<num_players>` (e.g. 2)
- **Join**: `<blank>`
- **Server**: `localhost`

### Network Play, Hosting

To play over the network, hosting for other players to join. They will join using your `ID`, and you must all be using the same `Server`.

![](/assets/simple/simple_tool_multiplayer3.png)

- **ID**: `<YourID>`
- **Invite**: `0`
- **Join**: `<blank>`
- **Server**: `<peering_server>`

### Network Play, Joining

To play over the network, joining another player's game. Your `ID` must be unique, and you must all be using the same `Server`.

![](/assets/simple/simple_tool_multiplayer4.png)

- **ID**: `<YourID>`
- **Invite**: `0`
- **Join**: `<JoinID>`
- **Server**: `<peering_server>`

## Debugger Controls

TODO: Explain me!

# Script Editor

![](/assets/simple/simple_script_ed2.png)

The script editor is where you will spend the majority of your time creating in simple, authoring script logic to drive your sims. The window displays the active script and includes tabs for all other opened scripts so you can swap between them (Hotkey: `Ctrl + Tab`).

You may also creation additional script editor windows by dragging the righthand or bottom handle of the window out (maximum 4 windows in a grid), splitting the screen. To pick which window receives a script when opening it from the Simulation Explorer, just click that window first to give it focus, indicated by a subtle change around the border of the panel.

![](/assets/simple/simple_script_ed.png)

## Script Editor Controls

Rough list of all the controls you can use while editing scripts:
> Note: Undo/Redo currently are not functional.

- **Single Left Click**: _Modify_
    - **In the window**: Select a line.
    - **On an editable section** (gray box): Edit value.
    - **On a _flow_ line**: Modify a _flow arrow_.
- **Double Left Click**
    - **On a component/property/namespace**: Rename.
- **Left Click Drag**: Move selected lines (can move between panes with a split screen editor).
- **Right Click**: _Add/Markup_
    - **In the window**: Extend a component. Add a namespace.
    - **Within a component**: Add a SECTION to component.
        - **On a Block operation**: Add a PROPERTIES section to the Block. 
    - **Within PROPERTIES**: Add a property.
    - **Within a SECTION**: Add an operation.
    - **On a component/property/operation**: Modify markup.
- **Delete**:
    - **On an component/operation/property**: Remove the selected object(s).
    - **On an inherited property**: Remove property override and restore to default.
- **Ctrl+C, Ctrl+X, Ctrl+V**: Copy/cut/paste selected components, properties, namespaces.
    > _Note_: Cutting and pasting _always_ makes a copy of a component, so references to the cut components/properties not in scope during the cut/paste will break. Use left click drag to move if this is not what you want.
- **Ctrl+D**: Enable/disable selected operation(s).
- **Ctrl+L**: Toggle line numbers.
- **Enter**: Commit number entry (when modifying name/number fields).