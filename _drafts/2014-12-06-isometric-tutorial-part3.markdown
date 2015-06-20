---
layout:     post
title:      "Tutorial Part 3: Adding our main Player"
subtitle:   "Action at last"
date:       2014-12-06 12:00:00
author:     "Ldd"
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
    {name: "player_spritesheet", type:"image", src: "data/img/player.png"},

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
</p>
<p>
First of all, we need to update the constructor of our entity. The code should be easy to follow, but basically
we do the following:
<ul>
<li><b>line 6:</b> Define a non-zero velocity for our player</li>
<li><b>line 9:</b>we tell the 'camera' to follow our player</li>
<li><b>line 12:</b>we ensure that the player is updated AND collisions work outside of the viewport</li>
<li><b>line 14-19:</b>we set up basic animations for walking left, up, down or standing still</li>
</ul>
<pre class="brush: js">
init:function (x, y, settings) {
    // call the constructor
    this._super(me.Entity, 'init', [x, y , settings]);

    // set the default horizontal & vertical speed (accel vector)
    this.body.setVelocity(6, 6);
 
    // set the display to follow our position on both axis
    me.game.viewport.follow(this.pos, me.game.viewport.AXIS.BOTH);
 
    // ensure the player is updated even when outside of the viewport
    this.alwaysUpdate = true;
      
    // define a basic walking animation (using all frames)
    this.renderable.addAnimation("walk",  [2]);
    // define a basic walking down animation (using all frames)
    this.renderable.addAnimation("walkDown",  [0]);
    // define a basic walking down animation (using all frames)
    this.renderable.addAnimation("walkUp",  [4]);
    // define a standing animation (using the first frame)
    this.renderable.addAnimation("stand",  [7]);
    // set the standing animation as default
    this.renderable.setCurrentAnimation("stand");
},
</pre>
<span class="caption text-muted">Changing the init in entities.js file</span>
</p>
<p>
We need to change the update function of the player too, so 
<pre class="brush: js"> 
update : function (dt) {
    if (me.input.isKeyPressed('left')) {
        // flip the sprite on horizontal axis
        this.renderable.flipX(false);
        // update the entity velocity
        this.body.vel.x -= this.body.accel.x * me.timer.tick;
        // change to the walking animation
        if (!this.renderable.isCurrentAnimation("walk")) {
            this.renderable.setCurrentAnimation("walk");
        }
    } else if (me.input.isKeyPressed('right')) {
        // unflip the sprite
        this.renderable.flipX(true);
        // update the entity velocity
        this.body.vel.x += this.body.accel.x * me.timer.tick;
        // change to the walking animation
        if (!this.renderable.isCurrentAnimation("walk")) {
            this.renderable.setCurrentAnimation("walk");
        }
    } else if (me.input.isKeyPressed('down')) {
        // update the entity velocity
        this.body.vel.y += this.body.accel.y * me.timer.tick;
        // change to the walking animation
        if (!this.renderable.isCurrentAnimation("walkDown")) {
            this.renderable.setCurrentAnimation("walkDown");
        }
    } else if (me.input.isKeyPressed('up')) {
        // update the entity velocity
        this.body.vel.y -= this.body.accel.y * me.timer.tick;
        // change to the walking animation
        if (!this.renderable.isCurrentAnimation("walkUp")) {
            this.renderable.setCurrentAnimation("walkUp");
        }
    } else {
        this.body.vel.x = 0;
        this.body.vel.y = 0;
        // change to the standing animation
        this.renderable.setCurrentAnimation("stand");
    }
  
    // apply physics to the body (this moves the entity)
    this.body.update(dt);
 
    // handle collisions against other shapes
    me.collision.check(this);
 
    // return true if we moved or if the renderable was updated
    return (this._super(me.Entity, 'update', [dt]) || this.body.vel.x !== 0 || this.body.vel.y !== 0);
},
</pre>
<span class="caption text-muted">Changing the update function in entities.js file</span>
</p>

<p>
In game.js, we simply register our key functions.
<pre class="brush: js"> 
"loaded" : function () {
    me.state.set(me.state.MENU, new game.TitleScreen());
    me.state.set(me.state.PLAY, new game.PlayScreen());

    // add our player entity in the entity pool
    me.pool.register("mainPlayer", game.PlayerEntity);

    // enable the keyboard
    me.input.bindKey(me.input.KEY.LEFT,  "left");
    me.input.bindKey(me.input.KEY.RIGHT, "right");
    me.input.bindKey(me.input.KEY.DOWN, "down");
    me.input.bindKey(me.input.KEY.UP, "up");

    // Start the game.
    me.state.change(me.state.PLAY);
}
</pre>
<span class="caption text-muted">Changing the loaded function in game.js file</span>
</p>

<p>
Most important change of all: we need to cancel out the gravity in the world. Otherwise, our player would literally fall off the edge of the world.
Luckily for us, in melonJS, it suffices to set me.sys.gravity to 0 in order to do this.
<pre class="brush: js"> 
onResetEvent: function() {
    //set gravity to 0
    me.sys.gravity = 0;

    // load a level
    me.levelDirector.loadLevel("area01");

    // reset the score
    game.data.score = 0;

    // add our HUD to the game world
    this.HUD = new game.HUD.Container();
    me.game.world.addChild(this.HUD);
},
</pre>
<span class="caption text-muted">Changing the onResetEvent function in play.js file</span>
</p>

<p>
Now we have to make changes to our Tiled map (mostly, just add a Player object)
</p>

<p>
And now we have a character!
Click <a href="https://github.com/ldd/boilerplate/archive/v0.3.0.zip">here</a> to download everything up until here
</p>
