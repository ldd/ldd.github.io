---
layout:     post
title:      "Tutorial Part 1: Setting up the Tiled Map"
subtitle:   "Creating the map for the game"
date:       2014-12-06 12:00:00
author:     "Ldd"
---

<i>I'll assume that you have followed the Tiled tutorial and you feel comfortable using Tiled. Feel free to skip this section if you are using the default taco.tmx file</i>

<p>The first thing that we will do is create a new map. <b>(File > New...)</b>
<br>We will use a map with 20x20 tiles, each having a width of 96px and a height of 64px. Make sure to specify <b>isometric</b> as your orientation and <b>base64 (uncompressed)</b> as the layer format.
</p>
<img class="center-block" src="{{ site.baseurl }}/img/create-map.png" alt="New map window in Tiled">
<span class="caption text-muted">Creating a  new map</span>

<p>
Great, we have a map! Now we need to add our tileset. <b>( Map > New Tileset...)</b><br>
from the ressources that you have downloaded, select the spritesheet.png file. Make sure that the Tile width and height match our map.
</p>
<img class="center-block" src="{{ site.baseurl }}/img/add-tileset.png" alt="Adding a tileset in Tiled">
<span class="caption text-muted">Adding a tileset</span>

<p>
Now we are ready to build! Do your best and fill the map. You only have two possible tiles so it shouldn't be too bad.
<img class="center-block" src="{{ site.baseurl }}/img/add-tiles.png" alt="Adding tiles to the map in Tiled">
<span class="caption text-muted">Adding tiles to our map</span>
</p>

<p>
Feel free to define a background colour for our level <b>(Map > Map Properties)</b><br>
<b>Tip:</b> There's a [...] button that is hard to see that will let you use a colour picker to the right of the <text>Background Color</text> property that appears if you select that property.
</p>
<img class="center-block" src="{{ site.baseurl }}/img/change-color.png" alt="Changing the map's background">
<span class="caption text-muted">Adding tiles to our map</span>

<p>
Those are all the changes we want for now. Save the file <b>(File > Save)</b><br>
To save the file properly, remember to save it as area01.tmx in  <b>data/map/</b>
</p>

<p>
Click <a href="https://github.com/ldd/boilerplate/archive/v0.1.0.zip">here</a> to download everything up until here
</p>
