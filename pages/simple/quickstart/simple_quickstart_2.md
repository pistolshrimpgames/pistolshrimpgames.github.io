---
layout: default
title: Physics Objects, Conjure, Bootstrap
parent: Quick Start
grand_parent: Simple
nav_order: 2
last_modified_date: 2022-05-23
---

A simulation with nothing physical in it and no player participation isn't terribly interesting, but this just proves you're up and running. In this section we'll cover adding a physics object, manipulating it in script, and adding some simple player controls.

# Defining Physics Objects

You can learn more about this later in detail, but physics objects are comprised of three separate components. They are a Collision Body, which contains any number of Colliders, each which have a Shape. We'll use a Box Shape, which is in the *shapes* submenu. Let's extend one of each, so our script looks like this:

![](/assets/simple/simple_collisionbody.png)

Don't worry about learning the ins and outs of these right now. Let's just get them connected to one another. Your **Test Collision Body** will use your **Test Collider**, which will use your **Test Box Shape**. Starting with the Collision Body, expand it by clicking the **+** if it's not already, and make sure the **PROPERTIES** section is visible.

![](/assets/simple/simple_collisionbody2.png)

Left click the line that says **{ colliders } = null** on the gray box, where it says **null**, and specify the **Test Collider**, under **definitions > Test Collider**.

![](/assets/simple/simple_collisionbody3.png)

After you select it, the gray box will change to yellow, indicating you've overridden it from the default selection (null).

![](/assets/simple/simple_collisionbody4.png)

Let's do the same for the Test Collider, specifying the **Test Box Shape**.

![](/assets/simple/simple_collisionbody5.png)

Lastly, we need to configure the Box Shape. The Box Shape has **extents**, which is a vector property (3 numbers, an x, y, and z). Left clicking the gray area will let you specify the values of those numbers. Let's make our box a rectangle with x being 2, y being 1, and z being 2.

![](/assets/simple/simple_collisionbody6.png)

When you are done entering numbers, hit enter to commit the values. Your Test Box Shape should look like this.

![](/assets/simple/simple_collisionbody7.png)

The Test Collision Body is ready to be Conjured and used for gameplay!

# Bootstrap & Conjuring a Component

Unlike the simulation, which uses the **(conjured)** markup to make itself immediately when the sim starts, we want to make the Test Collision Body as our player object. It will be up to a specially-named component which gets run when a player joins to do that for us. Let's extend the **base** component and name it **bootstrap** (verbatim! The name matters here).

![](/assets/simple/simple_bootstrap.png)

You will now have a very empty base component (note it has no PROPERTIES section). Simple will create one of these every time a new client - presumably a player - joins the simulation. We want to add some logic to make our Test Collision Body when that happens. Right click within it to add an INITIALIZE section.

![](/assets/simple/simple_bootstrap2.png)

Inside your INITIALIZE section, let's add a **Conjure** *operation*. Just like the Block operation that ran in our earlier UPDATE section, the **Conjure** will run once when **bootstrap** is created. Right click to add a Conjure, pulling down through the menus to specify you want a **Test Collision Body**.

![](/assets/simple/simple_bootstrap3.png)

Now you should get an instance of the Test Collision Body every time you run. Try running again, and you should see the view showing your Collision Body with its Box Shape Collider rendered as a green rectangle (hard to tell from top-down, but it's actually a prism in 3 dimensions).

![](/assets/simple/simple_bootstrap4.png)