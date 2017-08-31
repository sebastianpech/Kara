# Kara

<img src="http://imgur.com/qfHEf0E.gif" width=300 />

[![Build Status](https://travis-ci.org/sebastianpech/Kara.jl.svg?branch=master)](https://travis-ci.org/sebastianpech/Kara.jl)
[![codecov.io](http://codecov.io/github/sebastianpech/Kara.jl/coverage.svg?branch=master)](http://codecov.io/github/sebastianpech/Kara.jl?branch=master)

A package that is a port of SwissEducs [Kara](http://www.swisseduc.ch/informatik/karatojava/) (Page in German).
Kara is a concept for an easy access into the world of programming.
Kara is a tiny ladybug (or triangle) living in a forest with mushrooms, trees and leafs.
Kara can move a single mushroom, place and remove leafs and cannot move trees.
In comparison to the original Kara the interaction manly focuses on using the REPL and is lacking fancy mushrooms and bugs.

## Installation

The package is not yet contained in [METADATA.jl](https://github.com/JuliaLang/METADATA.jl), therefore to install run the following in Julia.

```jl
Pkg.clone("https://github.com/sebastianpech/Kara.jl.git")
```

## Introduction

Start using Kara.jl by opening Julia and entering `using Kara` into the REPL.
Next create a new world of size 10x10 with function bindings in global scope by entering `@World (10,10)`.

You can now use
- `move(kara)` to make a step into the direction Kara is facing,
- `turnLeft(kara)` to turn Kara left,
- `turnRight(kara)` to turn Kara right,
- `putLeaf(kara)` to place a leaf beneath Kara and
- `removeLeaf(kara)` to remove a leaf from beneath Kara

and

- `treeFront(kara)` to check if Kara stands in front of a tree,
- `treeLeft(kara)` to check if there is a tree left of Kara,
- `treeRight(kara)` to check if there is a tree right of Kara,
- `onLeaf(kara)` to check if there is a leaf beneath Kara and
- `mushroomFront(kara)` to check if Kara stands in front of a mushroom.

### Alternative methods of creating/loading a world

Kara.jl is aware of the xml syntax the original Kara uses for storing worlds.
It's possible to open a world through the GUI, after creating a new empty world, or through the command `@World [path]`.
In case one loads a world through the GUI and wants the above behaviour, the reference to Kara must be restored by:

```jl
kara = get_kara(world)
```

Kara.jl supports multiple worlds and multiple Karas. In case you want to reproduce the example, run it from within the test directory of Kara.jl e.g `~/.Julia/v0.6/Kara/test`.

```jl
# Load the world contained in example.world.
# This also creates a macro @w1 in global scope to interact with 
# the world
@World w1 "example.world"

# Create an empty world w2
@World w2 (10,2)

# Place kara in the empty world.
# place_kara() returns a reference to the placed kara.
# @w2 place_kara(1,1) is just syntactic sugar for place_kara(w2,1,1)
kara = @w2 place_kara(1,1)

# Kara is already placed in world w1, therefore we fetch it with get_kara()
# Since we cant create two kara references we use lara instead.
lara = @w1 get_kara()

# Move lara a step in world w1
@w1 move(lara)
# Alternatively:
move(w1,lara)

# Move kara a step in world w2
@w2 move(kara)

# It's even possible to allow kara from world w2 to 
# place something in world w1
@w1 putLeaf(kara)

```

### Other useful methods

- `reset!(world)`: Resets `world` to the state after loading or the last call to `store!(world)`.
- `store!(world)`: Stores the current state of `world`.
- `place_kara(world,X,Y,orientation)`: Places Kara in `world` at `X`, `Y` oriented `orientation`. Valid orientations are `:NORTH`, `:EAST`, `:SOUTH` and `:WEST`. `orientation` is optional and defaults to `:NORTH`.
- `place_mushroom(world,X,Y)`: Places a mushroom in `world` at `X`, `Y`.
- `place_tree(world,X,Y)`: Places a tree in `world` at `X`, `Y`.
- `place_leaf(world,X,Y)`: Places a leaf in `world` at `X`, `Y`.

The above used macro for interaction e.g. `@w1` basically translate `@w1 f(args...)` to `f(w1,args...)`.
Thus as all the above methods have `world` as their first argument they can also be called using the world macro.
This also works for custom methods like:

```jl
function turnAround(wo,ka)
	turnLeft(wo,ka)
	turnLeft(wo,ka)
end

@w1 turnAround(lara)
```


