---
layout:     post
title:      "Tutorial Part 3: Adding our main Player"
subtitle:   "Action at last"
date:       2014-12-06 12:00:00
author:     "Ldd"
header-img: "img/post-bg-04.jpg"
highlight-code: true
---

<p>
Let's add a character now. The first change we will do is to add our character sheet to the resources.js file.
<pre class="brush: js">
game.resources = [

    /* Graphics.
     * @example
     */
    // our level tileset 
    {name: "spritesheet", type:"image", src: "data/img/map/spritesheet.png"},
    //the main player spritesheet
    {name: "player_spritesheet", type:"image", src: "data/img/map/player.png"},

    /* Maps.
      */
    {name: "area01", type: "tmx", src: "data/map/area01.tmx"},
    
    //more commented out code goes here
];
</pre>
<span class="caption text-muted">Changing the resources.js file</span>
</p>

<p>
Next, we need to actually create a character. We do so by modifying our player in js/entities/entities.js
<pre class="brush: js">
init: function(x, y, settings) {
    // call the constructor
    this._super(me.Entity, 'init', [x, y, settings]);

    // set the default horizontal & vertical speed (accel vector)
    this.body.setVelocity(3, 15);

    // set the display to follow our position on both axis
    me.game.viewport.follow(this.pos, me.game.viewport.AXIS.BOTH);

    // ensure the player is updated even when outside of the viewport
    this.alwaysUpdate = true;
     
    // define a basic walking animation (using all frames)
    this.renderable.addAnimation("walk",  [0, 1, 2, 3, 4, 5, 6, 7]);
    // define a standing animation (using the first frame)
    this.renderable.addAnimation("stand",  [0]);
    // set the standing animation as default
    this.renderable.setCurrentAnimation("stand");
},
</pre>
<span class="caption text-muted">Changing the resources.js file</span>
Keep in mind that this is ...
</p>
<p>
<pre class="brush: js"> 
update: function(dt) {

    if (me.input.isKeyPressed('left')) {
        // flip the sprite on horizontal axis
        this.flipX(true);
        // update the entity velocity
        this.body.vel.x -= this.body.accel.x * me.timer.tick;
        // change to the walking animation
        if (!this.renderable.isCurrentAnimation("walk")) {
            this.renderable.setCurrentAnimation("walk");
        }
    } else if (me.input.isKeyPressed('right')) {
        // unflip the sprite
        this.flipX(false);
        // update the entity velocity
        this.body.vel.x += this.body.accel.x * me.timer.tick;
        // change to the walking animation
        if (!this.renderable.isCurrentAnimation("walk")) {
            this.renderable.setCurrentAnimation("walk");
        }
    } else {
        this.body.vel.x = 0;
        // change to the standing animation
        this.renderable.setCurrentAnimation("stand");
    }
 
    if (me.input.isKeyPressed('jump')) {
        // make sure we are not already jumping or falling
        if (!this.body.jumping && !this.body.falling) {
            // set current vel to the maximum defined value
            // gravity will then do the rest
            this.body.vel.y = -this.body.maxVel.y * me.timer.tick;
            // set the jumping flag
            this.body.jumping = true;
        }

    }

    // apply physics to the body (this moves the entity)
    this.body.update(dt);

    // handle collisions against other shapes
    me.collision.check(this);

    // return true if we moved or if the renderable was updated
    return (this._super(me.Entity, 'update', [dt]) || this.body.vel.x !== 0 || this.body.vel.y !== 0);
}
</pre>
<span class="caption text-muted">Changing the resources.js file</span>
</p>
<p>
And now we have a character!
Click <a href="https://github.com/ldd/boilerplate/archive/v0.3.0.zip">here</a> to download everything up until here
</p>
