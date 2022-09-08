# Portfolio
I'm Sam, an aspiring junior game dev from Germany.  
This temporary site is just to show some of my university and hobby game projects.

## University Projects

### Kingdom Dawn
Watch the trailer here:  
[![Trailer](http://img.youtube.com/vi/VrJTRM2MyzY/0.jpg)](http://www.youtube.com/watch?v=VrJTRM2MyzY "KingdomDawn Trailer")  

This game was conceived with my idea of trying to make a game that looks like the strategic map from Age of Wonders 3.  
I implemented the world generator and the entire game board drawing and interaction system based internally on a hex grid.  
For the treasure map look I wrote some basic shaders e.g. world space aligned textures for the river/water, that overlays the correctly aligned waves over all hexes independent of the individual hex's rotation.I think we came very close to the original vision.

For anyone interested in some technical details:
<details>
  <summary>Show</summary>
  <p>
  One simple but effective optimization I like is that instead of using Unity's Raycast system to deterine which hex the user mouses over we use a hex grid to world matrix and it's inverse to do that math.  
  Internally the hexes are structured as a 2D array so this way all it takes is transforming the mouse coords to hex grid coords with some matrix multiplication and we're done.
  </p>
  <p>
  One initially tricky aspect that had a simple (kind of) solution was creating a system for determining whether the river hex's had to be rotated and to which orientation. This is so our artist doesn't have to rotate all the hexes manually. 
  </p>
  <p>
  We used bitmasks to encode which of the hex neighbors had water on it to determine which exact kind of river/ocean tile has to be instantiated at these positions. By concatenating the bitmask to itself we can then bitshift a target bitmask over it, to determine the degree of rotation. E.g. 0b111000 encodes a hex tile whose first 3 neighbors clockwise contain water. Now if we shift this bitmask with wraparound (rotate) we can match all rotations of hex tiles with 3 consecutive water neigbors. Now our artist only had to create assets for all 14 base bitmasks and created some variations of the most used assets to introduce more variety to the look of rivers and oceans (0b1, 0b11, 0b111..., 0b1011, 0b1101...) and not the entire set that can be acquired through rotation.
  </p>
</details>

## Hobby Projects

### Unfinished Work in Progress

This game is my current work in progress. Inspired by some .io games and Mount&Blade I wanted to make a top-down game where you can put your 2D fighters into a preset formation or one of your own choosing, and easily switch between them. 

Some technical details:
<details>
  <summary>Show</summary>
  <p>
 Not many interesting details here, yet. The formations are realized by having waypoints that the individual units stick to, so long as no enemy is close enough to attack. At some specified distance the units engage. The player is also able to create their own formations. There's some basic missile logic which makes it possible to dodge throwing spears or arrows if you move out of range. This is so it could be worthwhile to have sparse formations. 
  </p>
  <p>
  Some different unit classes are in the game, fighters, wizards, archers and javelin throwers, which are just fighters with a few throwing spears that they will chuck before they're in melee range.
  </p>
</details>

### My First game: Dig Simulator

When I was 16 I programmed this masterpiece in javascript with the HTML canvas as the "engine". The game has a basic particle system and procedurally generated levels.A friend of mine did the UI/HTML parts of it. You play as a blue square trying to mine as much downward as possible, craft bombs and use them. Upgrade your vision radius, bombs and drill. Every level has an Enemy in it, if it smells more cash on you than it has, it will come for you immediately, otherwise it will mind it's own business until you come across it's tunnels.  
You can play it here: TODO INSERT LINK

For some technical details, will bore many but might amuse some:
<details>
  <summary>Make me cry</summary>
  <p>
Because at this point I had only programmed in Java before, I was immediately missing the concept of classes, so instead of using typescript for whatever reason we found this online tool that generated the javascript prototype code from typescript for you or something of the sort, which results code that is barely readable at best. ‍
 </p>
 <p>
Some technical issues I faced during development include: terrible performance. The game was playable with 15fps max and I couldn't understand how the performance could be so bad (even for Javascript back then). Turns out that I was overdrawing every frame twice: Of course the game board has to be reset (it doesn't) so I thought better update the 2D array which indexes the game blocks to a neutral color every frame. This works because the generated levels are stored in a separate 3D array, so we can just copy the relevant 2D array out of the 3D array into the buffer array. Every Frame. Twice. If you're thinking WHY? I agree. Well this was fixed, in the end only what changes from frame to frame get's drawn on the screen as is reasonable to do. For the explosion effects I thought it would be neat to have the letters of the word "boom" serve as the particles for the explosion, the end result is kind of cool.
  For this I ended up implementing some sort of list of particles that had a time to live and direction that was iterated every frame. I then ended up using the same system for animating the digging, which is just a bunch of pixels spraying from the center of a block into random directions. If you made it this far, thank you for reading!
 </p>
</details>


