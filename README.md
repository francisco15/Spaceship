# Mini-project description - Spaceship

Asteroids is a relatively simple game by today's standards, but was still immensely popular during its time. In the game, the player controls a spaceship via four buttons: two buttons that rotate the spaceship clockwise or counterclockwise (independent of its current velocity), a thrust button that accelerates the ship in its forward direction and a fire button that shoots missiles. Large asteroids spawn randomly on the screen with random velocities. The player's goal is to destroy these asteroids before they strike the player's ship. In the arcade version, a large rock hit by a missile split into several fast moving small asteroids that themselves must be destroyed. Occasionally, a flying saucer also crosses the screen and attempts to destroy the player's spaceship.

## Mini-project development process

For this mini-project, you will implement a working spaceship plus add a single asteroid and a single missile. 

## Phase one - Spaceship

In this phase, you will implement the control scheme for the spaceship.This includes a complete Spaceship class and the appropriate keyboard handlers to control the spaceship. Your spaceship should behave as follows:

* The left and right arrows should control the orientation of your spaceship. While the left arrow is held down, your spaceship should turn counter-clockwise. While the right arrow is down, your spaceship should turn clockwise. When neither key is down, your ship should maintain its orientation. You will need to pick some reasonable angular velocity at which your ship should turn.

* The up arrow should control the thrusters of your spaceship. The thrusters should be on when the up arrow is down and off when it is up. When the thrusters are on, you should draw the ship with thrust flames. When the thrusters are off, you should draw the ship without thrust flames.

* When thrusting, the ship should accelerate in the direction of its forward vector. This vector can be computed from the orientation/angle of the ship using the provided helper function 𝚊𝚗𝚐𝚕𝚎_𝚝𝚘_𝚟𝚎𝚌𝚝𝚘𝚛. You will need to experiment with scaling each component of this acceleration vector to generate a reasonable acceleration.

* Remember that while the ship accelerates in its forward direction, but the ship always moves in the direction of its velocity vector. Being able to accelerate in a direction different than the direction that you are moving is a hallmark of Asteroids.

* Your ship should always experience some amount of friction. (Yeah, we know, "Why is there friction in the vacuum of space?". Just trust us there is in this game.) This choice means that the velocity should always be multiplied by a constant factor less than one to slow the ship down. It will then come to a stop eventually after you stop the thrusters.

1. Modify the draw method for the Ship class to draw the ship image (without thrust flames) instead of a circle. This method should incorporate the ship's position and angle. Note that the angle should be in radians, not degrees. Since a call to the ship's draw method already exists in the draw handler, you should now see the ship image. Experiment with different positions and angles for the ship.

2. Implement an initial version of the update method for the ship. This version should update the position of the ship based on its velocity. Since a call to the update method also already exists in the draw handler, the ship should move in response to different initial velocities.

3. Modify the update method for the ship to increment its angle by its angular velocity.

4. Make your ship turn in response to the left/right arrow keys. Add keydown and keyup handlers that check the left and right arrow keys. Add methods to the Ship class to increment and decrement the angular velocity by a fixed amount. Call these methods in the keyboard handlers appropriately and verify that you can turn your ship as you expect.

5. Add a method to the Ship class to turn the thrusters on/off (you can make it take a Boolean argument which is True or False to decide if they should be on or off). Modify the keyboard handler to call this method in response to the thrust key being pressed/released.

6. Modify the ship's draw method to draw the thrust image when it is on. (The ship image is tiled and contains both images of the ship.)

7. Modify the ship's thrust method to play the thrust sound when the thrust is on. Rewind the sound when the thrust turns off.

8. Add code to the ship's update method to use the given helper function 𝚊𝚗𝚐𝚕𝚎_𝚝𝚘_𝚟𝚎𝚌𝚝𝚘𝚛 to compute the forward vector pointing in the direction the ship is facing based on the ship's angle.

9. Next, add code to the ship's update method to accelerate the ship in the direction of this forward vector when the ship is thrusting. You will need to update the velocity vector by a small fraction of the forward acceleration vector so that the ship does not accelerate too fast.

10. Then, modify the ship's update method such that the ship's position wraps around the screen when it goes off the edge (use modular arithmetic!).

11. Up to this point, your ship will never slow down. Finally, add friction to the ship's update method as shown in the "Acceleration and Friction" video by multiplying each component of the velocity by a number slightly less than 1 during each update.

## Phase two - Rocks

To implement rocks, we will use the provided Sprite class. Note that the update method for the sprite will be very similar to the update method for the ship. The primary difference is that the ship's velocity and rotation are controlled by keys, whereas sprites have these set randomly when they are created. Rocks should screen wrap in the same manner as the ship.

In the template, the global variable 𝚊_𝚛𝚘𝚌𝚔 is created at the start with zero velocity. Instead, we want to create version of 𝚊_𝚛𝚘𝚌𝚔 once every second in the timer handler. Next week, we will add multiple rocks. This week, the ship will not die if it hits a rock. We'll add that next week. To implement rocks, we suggest the following:

1. Complete the Sprite class by modifying the draw handler to draw the actual image and the update handler to make the sprite move and rotate. Rocks do not accelerate or experience friction, so the sprite update method should be simpler than the ship update method. Test this by giving 𝚊_𝚛𝚘𝚌𝚔 different starting parameters and ensuring it behaves as you expect.

2. Implement the timer handler 𝚛𝚘𝚌𝚔_𝚜𝚙𝚊𝚠𝚗𝚎𝚛. In particular, set 𝚊_𝚛𝚘𝚌𝚔 to be a new rock on every tick. (Don't forget to declare 𝚊_𝚛𝚘𝚌𝚔 as a global in the timer handler.) Choose a velocity, position, and angular velocity randomly for the rock. You will want to tweak the ranges of these random numbers, as that will affect how fun the game is to play. Make sure you generated rocks that spin in both directions and, likewise, move in all directions.

## Phase three - Missiles

To implement missiles, we will use the same sprite class as for rocks. Missiles will always have a zero angular velocity. They will also have a lifespan (they should disappear after a certain amount of time or you will eventually have missiles all over the place), but we will ignore that this week. Also, for now, we will only allow a single missile and it will not yet blow up rocks.

Your missile should be created when you press the spacebar, not on a timer like rocks. They should screen wrap just as the ship and rocks do. Otherwise, the process is very similar:

1. Add a 𝚜𝚑𝚘𝚘𝚝 method to your ship class. This should spawn a new missile (for now just replace the old missile in 𝚊_𝚖𝚒𝚜𝚜𝚒𝚕𝚎). The missile's initial position should be the tip of your ship's "cannon". Its velocity should be the sum of the ship's velocity and a multiple of the ship's forward vector.

2. Modify the keydown handler to call this shoot method when the spacebar is pressed.

3. Make sure that the missile sound is passed to the sprite initializer so that the shooting sound is played whenever you shoot a missile.

## Phase four - User interface

Our user interface for RiceRocks simply shows the number of lives remaining and the score. This week neither of those elements ever change, but they will next week. Add code to the draw event handler to draw these on the canvas. Use the 𝚕𝚒𝚟𝚎𝚜 and 𝚜𝚌𝚘𝚛𝚎 global variables as the current lives remaining and score. 
