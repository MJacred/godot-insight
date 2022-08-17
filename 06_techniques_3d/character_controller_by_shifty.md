source: https://markdownpastebin.com/?id=d9d61e67f9d64db2bd215f165b931449  
made by: [Shifty](https://github.com/Shfty)


# Shifty's Godot Character Movement Manifesto

## Preface

Before committing to building a character controller in Godot 3.2.x, it is necessary to come to terms with the following:
* Godot's physics implementation will be your opponent, rather than your ally, for the duration of this exercise
* There exist fundamental flaws in said implementation that mean a perfect system is not feasible to attain
* Common issues include:
    * Incorrect normal output from collision queries
        * https://github.com/godotengine/godot/issues/47185
    * Incorrect distance output from collision queries
    * Colliders catching on parallel edges (ex. checkerboard floor colliders)
    * Jittery collision response that scales with object size (becomes more extreme for non unit objects, ex. a 300*5*5 box)
    * Incorrect collision response between large bodies (ex. dropping a 50*2*50 box on top of another 50 * 2 * 50 box will exhibit an unexpected soft resistance before contact)
* These issues still exist in Godot 3.3
* The only fixes involve:
    * Modifying the engine
    * Creating a completely new physics engine integration

If you still want to continue with all that context in mind, buckle up, and read on...


## Physics Bodies

Rule number one: Do not use KinematicBody
* At first glace, this seems like the natural choice, since:
    A. Kinematics theoretically grant total control over your movement system
    B. Kinematics avoid having to invoke any non-deterministic RigidBody physics
* In reality, A is not the case
    * 90% of the kinematic behavior is black boxed inside move_and_slide
    * move_and_slide is heavily affected by the issues above
    * Trying to build workarounds on top of move_and_slide only cause more layers of issues
* B is still technically true, but hinges on kinematic collisions behaving correctly

Rule number two: Do not try to build your own kinematic movement system using physics queries
* This is an ambitious but natural next step after realising KinematicBody with move_and_slide is not the answer
* Attempting this from scratch will take months of full time work
* The fundamental flaws with the collision system mean that you're still subject to the core physics issues
    * In particular, incorrect results from raycast and shape trace queries will scupper any attempt at robust Movement

So, given the above, the last remaining object is to use RigidBody.
* Initially, this seems like a worst-case scenario
* Nightmarish visions of your pure, logical movement system being tainted by nasty unpredictable non-deterministic physics sim
* Worry not; as dire as it may seem, RigidBody can actually be turned into a win:
    * For one, you immediately get decent depentration for free
        * This helps mitigate most of the issues that usually plague kinematic systems by amortizing their effects across multiple frames
        * Some issues are fundamentally unavoidable, but can be worked around at a level design (see below)
    * Two, you get interaction with physics objects for free
        * No more worrying about how to bridge the gap between a physics-blind kinematic player and simulated environmental objects
    * Three, you actually have far more control than you think
        * RigidBody can operate in four modes; Rigid, Kinematic, Static, and Character
            * https://github.com/godotengine/godot/issues/46712 (has workaround, see below)
            * Rigid, Kinematic and Static operate like their respective shape classes
            * Character is of particular note: it acts like a Rigid shape, but doesn't rotate
                * Your character can push and be pushed, but won't topple over
        * RigidBody has a 'Use custom integrator' property - this is the key to the whole house of cards
            * Enabling this will disable all built-in force integration (i.e. gravity, air friction)
                * should work around: https://github.com/godotengine/godot/issues/46712
            * What remains is the collision logic, a.k.a. the automatic depenetration mentioned in point #1
            * By overriding _integrate_forces in a custom script on your RigidBody, you gain total control over its physics state via the PhysicsDirectBodyState param
                * This includes moving it, changing its velocity, applying forces, and more
                * You can fetch the PhysicsDirectSpaceState from it in order to perform global collision checks
                * This runs on the physics thread, so be careful
                    * You can crash the multi-threaded renderer if you invoke anything that touches VisualServer from here
        * In effect, combining Character mode with a custom integrator results in a 'KinematicRigidBody' of sorts
            * All the control of a kinematic from inside _integrate_forces
            * With a free layer of shielding from the underlying issues
            * And free collision response versus simulated physics objects
        * This is, by far, the most stable result i've been able to get from Godot's physics system


## Physics Shapes

You should only ever use a capsule for an upright character if you're building a Tribes game
* Capsules are good for traversal of smooth, sloped surfaces, but terrible for steps
* A common issue is launching from small steps or slopes based on a high lateral movement speed
* This is a pain to try and mitigate, so point one should be using a more predictable shape

Enter the cylinder.
* Cylinders are simple, predictable, and collide nicely with both steps and slopes by default
* It's necessary to write a routine to manually navigate steps, but the end result is worth it
    * Best method i've found so far is to perform progressive shape traces, something like this:
        * To step up:
            * Cast forward by velocity * delta_time
            * If there was a hit, cast up from the intersection point
            * If there was no hit, or a hit beyond the minimum step length cast forward from there
                - Use the remainder of the velocity that moved the shape into the step
            * If there was no hit, cast down from the endpoint
            * If there was a hit, move the player to the endpoint
                * Subtract the moved distance from their velocity to prevent stair boosting
        * To step down:
            * Cast forward
            * If there was no hit, cast down from the endpoint
            * If there was a hit, move the player to the endpoint
                * Subtract the moved distance from their velocity to prevent stair boosting
* These routines would live with the rest of your movement code inside of _integrate_forces
* Various other movement niceties can be implemented here as well
    * Using a get_rest_info shape query with a shorter, fatter cylinder for more robust ground checks
    * Manually inheriting the velocity from the RigidBody your player is standing on for total control over moving or rotating platforms
    * Applying gravity using the ground normal (or not at all) when stood on the ground to prevent slope sliding
    * And many more


## Level Design Workarounds

As mentioned above, some issues are unavoidable, but can be mitigated by instead avoiding them at design time:
* Large colliders are unstable
    * Where possible, very large colliders should be manually tessellated into smaller ones in order to avoid jittery collision response
    * This will invoke the issue below, so design-time care should be taken to place splits where their effects will be minimized
* Sets of small adjacent colliders (such as a checkered floor) cause the character to collide with their edges and pop into the air
    * Manually combine sets of small adjacent colliders into one larger collider by aggregating their vertices inside a ConvexPolygonShape
* Unstable floor results make my 'is grounded' flag jitter
    * This seems to be down to the collision margin system
    * Use a rolling window (2-3 frames should be sufficient) that folds down to a single bool using OR to de-noise the floor check
