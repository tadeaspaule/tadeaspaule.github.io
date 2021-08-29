+++
title = "HC update #1 : landmass generation"
date = 2021-08-29
description = "First update on HC progress! Briefly about how I generate landmasses for the 2D map"
[taxonomies]
tags = ["proj-hc", "programming", "rust", "gamedev", "doing", "procedural-generation"]
+++

First HC update! I'm working on map generation now, and wanted to write a bit about how I've done landmass generation. I know this has been done a million times already, but I went in blind and didn't look up any common approaches since I found the process of figuring out how to make it work fun, reinventing the wheel or not.

### showcase

First, here are some generated landmasses:

{{ imagerow(width=180, paths=["/hc/screenshots/gen-landmass/normal-0.png","/hc/screenshots/gen-landmass/normal-1.png", "/hc/screenshots/gen-landmass/normal-2.png"]) }}

I like them! Each has character, and will hopefully give rise to intersting storytelling; a mythical monster dwelling in the giant lake of the world on the left, merchant empires operating in the large inland see of the middle world, or an insular cult occupying the small island of the one on the right.

### generation algorithm

Here is how the landmasses are generated:

1. The map is filled with `Ocean` terrain
2. Until enough of the map is covered in land, do:
    1. Pick a random `Ocean` tile that's not too close to the map's edge
    2. "Flood out" some number of land tiles from it

Pretty simple, right? It gets interesting when you start tweaking some of the parameters. Here are the most important ones:

- `land_total` - how much landmass do we want? I set this to some random fraction of the total number of tiles, within some sensible range (vague words here since I'm still changing this. Let's say it's between 0.2 and 0.8)
- `points` - how many flood origin points (step #2.1) to pick. This also affects step #2.2, as the amount of flooding from each point is simply `land_total / points`
- `expand_chance` - how likely is it to "flood" to a neighboring tile

### flooding?

A brief look at how "flooding" works:

1. create a list `all_pos`, initially containing the flooding's origin point
2. create a list `pos`, initially also containing the origin point
3. create a new list, `new_pos`, initially empty
4. go through elements in `pos`, and look at each element's neighboring tiles
    1. if the tile is not in `all_pos`, `pos`, or `new_pos`, add it to `new_pos` with chance `expand_chance`
5. add all elements in `new_pos` to `all_pos`
6. clear `pos` and add all elements in `new_pos` to it
7. clear `new_pos`
8. repeat steps 4-7 until `all_pos` contains the desired number of elements

### parameter influence

Lastly, a bit about how each of the mentioned parameters affects the resulting map:

- `land_total` - self-explanatory. Don't want the number to be too high otherwise it's just one large blob with little variety, but also not too little so there's land on which things can happen
- `points` - more points make the landmasses more "wiggly" and fragmented... Words are hard, pictures below
- `expand_chance` - if this is just set to `1` (aka `100%`), the flood just produces a relatively uniform "block", with no interesting edges. Again, pictures below

{{ imagerow(width=180, desc="High `points` number" paths=["/hc/screenshots/gen-landmass/manypoints-0.png","/hc/screenshots/gen-landmass/manypoints-1.png", "/hc/screenshots/gen-landmass/manypoints-2.png"]) }}


{{ imagerow(width=180, desc="`points == 1`" paths=["/hc/screenshots/gen-landmass/singlepoint-0.png","/hc/screenshots/gen-landmass/singlepoint-1.png", "/hc/screenshots/gen-landmass/singlepoint-2.png"]) }}

{{ imagerow(width=180, desc="`points == 1` and high `expand_chance`" paths=["/hc/screenshots/gen-landmass/manypoints-highexpand-0.png","/hc/screenshots/gen-landmass/manypoints-highexpand-1.png", "/hc/screenshots/gen-landmass/manypoints-highexpand-2.png"]) }}

For the showcase landmasses shown at the beginning, I used the following values:

- `land_total = area * rand(30%, 60%)`
- `points = 20`
- `expand_chance = 30%`

That's it for now! At the time of writing I'm well on my way to getting various terrains (think biomes) sensibly placed on the generated landmasses, so next update will be about that and hopefully soon.

