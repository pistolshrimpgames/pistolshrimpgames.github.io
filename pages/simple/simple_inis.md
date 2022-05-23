---
layout: default
title: Simple .ini Documentation
parent: Simple
nav_order: 5
---

Many of these settings are in flux, have not been tested in all combinations, and are likely to break. ini files should be named the same as their corresponding sim and placed next to the sim file, in the same directory.

A global ini file which is loaded (but can be overridden, in theory) for all sims is located at `install\globals.ini`.

For certain qualifiers, like object coloring, files are read in order, meaning if two statements apply, only the last (or first? need to verify) one will win.

# Input

Input is mapped with phrases similar to their script analogs.

```
PrimaryAxisPositiveX
PrimaryAxisNegativeX
PrimaryAxisNegativeZ
PrimaryAxisPositiveZ
PrimaryAxisPositiveY
PrimaryAxisNegativeY
SecondaryAxisNegativeX
SecondaryAxisPositiveX
SecondaryAxisNegativeZ
SecondaryAxisPositiveZ
SecondaryAxisNegativeY
```

`primary button` in script currently maps directly to `PrimaryAxisPositiveY` (but gives additional values for testing released, just released, pressed, and just pressed).

To bind an input to a key or keys, list the keys that trigger that input separated by commas, e.g.

```
PrimaryAxisPositiveX,d,m,right
```

# Object Coloring

Colliders can be colored for the view by either checking what type they or their owning Collision Bodies are (`TYPE_COLOR`) or by querying one of their properties (`PROPERTY_COLOR`).

RGBA values are a number from 0 to 1, and the format is:
```
<color_declaration>,<statement>,<red>,<green>,<blue>,<alpha>
```

## TYPE_COLOR

Type color will apply if the Collision Body's is of the specified type (this means extensions will use them if applicable). 

```
TYPE_COLOR,Melee Ship (Defeated),1,0,0.25,0.75
```

## PROPERTY_COLOR

Property color will apply either if a property exists on that object, or if a simple statement evalutes to true. With just a property name, it means color an object if it _has_ the property (regardless of its value). For example, looking for the `shielded` property:

```
PROPERTY_COLOR,shielded,0.2,0.5,0.8,1.0
```

A simple statement can be tested (currently we think >, >=, <, <=, = work). For example, only color the object if `shielded` is 1:

```
PROPERTY_COLOR,shielded=1,0.2,0.5,0.8,1.0
```

To access properties beyond the top level of an object, additional `.`'s can be added to dig through properties. For example:

```
PROPERTY_COLOR,target.info.shielded=1,0.2,0.5,0.8,1.0
```

# Lighting

You have access to 3 (?) lights and an ambient light. Example:

```
LIGHT_POSITION,-1000000,1000000,-1000000
LIGHT_DIFFUSE_COLOR, 1.0f,1.0f,1.0f,0.8f
LIGHT_ATTENUATION, 0.0f, 0.0f, 0.0f
LIGHT_SPECULAR_COLOR, 1.0f, 1.0f, 1.0f

LIGHT_ID,1
LIGHT_POSITION, 1000000, 150000, 1000000
LIGHT_DIFFUSE_COLOR, 0.0f, 0.25f, 0.7f, 1.0f
LIGHT_ATTENUATION, 0.0f, 0.0f, 0.0f

LIGHT_ID,2
LIGHT_POSITION,-15, 5, -15
LIGHT_DIFFUSE_COLOR, 1.0f, 0.0f, 0.0f, 1.0f
LIGHT_ATTENUATION, 0.0f, 0.25f, 0.
```

# Camera

Example camera config for a sidescroller which uses an orthographic camera, setting the pitch to be 0 (head-on):

```
CAMERA_FEATURES,Orthographic,TargetAllPlayers,IncludeAllPlayers
CAMERA_PITCH, 0
```

## CAMERA_FEATURES

One or more of the following (comma separated, no quote marks) is required. Some are mutually exclusive or may interfere with one another.

### normalize
Support "wrapping" of playfield -- which "axes" wrap are determined by simulation "wrap_extents" components being non-zero

### targetposition
View will always center on (look at) [CAMERA_TARGET_POSITION](#camera_target_position).

### targetclientplayer
View will always center on (look at) client player position.

### targetallplayers
View will always center on (look at) average position of all players.

### orthographic
Use orthographic projection (NOTE: not used/tested a lot!).

### scaleviewsize
Scale size of "players" based on distance from camera.

### autozoom
When set, various camera values will be modified based on distance from player to camera target -- see [Autozoom Settings](#autozoom-settings) below.

### includeclientplayer
Adjust camera "zoom" to ensure client player is in view

### includeallplayers
Adjust camera "zoom" to ensure all players are in view

### zoomwrapextents
Adjust camera "zoom" so playfield fits exactly in viewport (but preserving aspect ratio, so you may have "letterboxing" in game window)

### usehardcuts
Untested/experimental

### targetplayerhorizontal
Keep camera target at same *view* x-location as player

### targetplayervertical
Keep camera target at same *view* y-location as player

### targetpointofinterest
Untested/experimental

## CAMERA_TARGET_POSITION
What camera looks at (NOTE: may be ignored depending on CAMERA_FEATURES) -- defaults to 0,0,0

## CAMERA_FOV
Field of view in degrees (for perspective projection) -- defaults to 45

## CAMERA_FOCAL_LENGTH
Distance from camera target to camera position(for perspective projection) -- defaults to 60 units

## CAMERA_PITCH
Camera pitch in degrees -- defaults to -90.
## CAMERA_FACING
Camera facing in degrees.
## CAMERA_ROLL
Camera roll in degrees.

## CAMERA_BACKGROUND_COLOR
Defaults to 0,0,0,1 (black)

## CAMERA_SIZE_FACTOR
If CAMERA_FEATURES includes "scaleviewsize", will scale view size of player -- defaults to 1/30

## Autozoom Settings

If CAMERA_FEATURES includes "autozoom", how various camera values will be modified based on distance from player to camera target.

For example:

```
CAMERA_AUTOZOOM_INNER_FOCAL_LENGTH,15
CAMERA_AUTOZOOM_OUTER_FOCAL_LENGTH,100
CAMERA_AUTOZOOM_INNER_TARGET_DISTANCE,5
CAMERA_AUTOZOOM_OUTER_TARGET_DISTANCE,30
CAMERA_AUTOZOOM_INNER_SCALE_FACTOR, 0.0222
CAMERA_AUTOZOOM_OUTER_SCALE_FACTOR, 0.0111
```

TODO: fill in the below

### CAMERA_AUTOZOOM_INNER_FOCAL_LENGTH
### CAMERA_AUTOZOOM_OUTER_FOCAL_LENGTH
### CAMERA_AUTOZOOM_INNER_TARGET_DISTANCE
### CAMERA_AUTOZOOM_OUTER_TARGET_DISTANCE
### CAMERA_AUTOZOOM_INNER_SIZE_FACTOR
### CAMERA_AUTOZOOM_OUTER_SIZE_FACTOR

## CAMERA_ASPECT_RATIO
Determines shape of camera viewport (defaults to 0, or no effect). 1, for example, would be a square.

# Meter UIs

TODO

# Backdrop

A backdrop texture file for the view can be specified with the `BACKDROP_TEXTURE_FILE` directive and specifying a png file pathed relatively starting from the `install` directory or directly (not recommended) starting from root (e.g. `C:\`). For example:

```
BACKDROP_TEXTURE_FILE=..\game\sim\test\test_sim\test_sim_backdrop.png
```