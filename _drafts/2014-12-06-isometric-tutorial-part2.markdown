---
layout:     post
title:      "Tutorial Part 2: Loading our Tiled Map"
subtitle:   "melonJS to the rescue!"
date:       2014-12-06 12:00:00
author:     "Ldd"
---

<i>I'll assume that you have followed the melonJS tutorial or that at least you know how to handle the "cross-origin request" issue</i>

<p>
The first change we will do is telling melonJS what to load and where that is located. To do so, we edit the resources.js file.
{% highlight javascript %}
game.resources = [

    /* Graphics.
     * @example
     */
    {name: "spritesheet", type:"image", src: "data/img/map/spritesheet.png"},

    /* Texture Atlases
     * @example
     * {name: "texture", type: "json", src: "data/img/example_tps.json"},
     */

    /* Maps.
      */
    {name: "area01", type: "tmx", src: "data/map/area01.tmx"},
    
    //more commented out code goes here
];
{% endhighlight %}
<span class="caption text-muted">Changing the resources.js file</span>
As you can see, if you followed the melonJS platformer tutorial, we simply declare the source of our files.
</p>

<p>
Next, we tell melonJS to actually use our level in game.js
{% highlight javascript %}
game.PlayScreen = me.ScreenObject.extend({
    /**
     *  action to perform on state change
     */
    onResetEvent: function() {

        // load a level
        me.levelDirector.loadLevel("area01");//only change needed

        // reset the score
        game.data.score = 0;

        // add our HUD to the game world
        this.HUD = new game.HUD.Container();
        me.game.world.addChild(this.HUD);
    },

    /**
     *  action to perform when leaving this screen (state change)
     */
    onDestroyEvent: function() {
        // remove the HUD from the game world
        me.game.world.removeChild(this.HUD);
    }
});
{% endhighlight %}
<span class="caption text-muted">Changing the game.js file</span>
</p>
<p>
Believe it or not, that is all that is needed to get an isometric game running.
Click <a href="https://github.com/ldd/boilerplate/archive/v0.2.0.zip">here</a> to download everything up until here
</p>
