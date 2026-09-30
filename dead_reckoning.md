# Dead Reckoning: Knowing Where You Are Without Looking

## So, what is it?

Dead reckoning is basically figuring out where you are by remembering where you started and keeping track of every move you've made since. No looking around, no asking for directions. Just "I started here, went this far, turned this much, so I must be about *there*."

You know when you walk through your room in the dark with your eyes closed? You still kinda know where the bed is because you count your steps and remember your turns. Same thing.

For a robot, it means estimating where it is *right now* by adding up all its movements from a known starting point, without checking the outside world. And it's not a new trick. Sailors were doing it centuries ago, using their speed, direction, and time to guess where their ship was in the middle of the ocean.

## How does it actually work?

The robot keeps a running note of its pose: where it is (x, y) and which way it's facing (θ). Every tiny fraction of a second, it checks how much it moved and updates that note.

Say it's a two-wheeled robot. Each wheel has an encoder that counts how much it turned. Both wheels turned the same? Went straight. One turned more than the other? It turned. From that, the robot works out how far it went forward and how much its heading changed, then shifts its position accordingly. Do that hundreds of times a second and boom, you've got a live estimate of where the robot is.

And it's not just wheels. An IMU (measures acceleration and rotation) can do the job too, by adding up its readings over time. Some robots even just trust the speed commands they sent to the motors and assume those happened.

## How is it used in real life?

Honestly, almost never on its own. Dead reckoning is the fast, smooth, always-on part of navigation. Then other stuff like GPS, LiDAR, cameras, or a compass comes in every so often to say "hey, you're actually over here." A filter (like a Kalman filter) blends the two together. Dead reckoning covers the gaps, and the corrections keep it honest.

## The good stuff

- **Cheap and simple.** Encoders and an IMU don't cost much, and the math is light.
- **Fast.** It updates as fast as your sensors do, so the robot always has an answer.
- **Self-contained.** No satellites, no beacons, no infrastructure. Works indoors, underground, underwater.
- **Smooth.** No sudden jumps in position, which controllers love.

## The not-so-good stuff

Here's the catch: every measurement is a little bit wrong. Wheels slip, floors are bumpy, sensors are noisy and biased. And since every new position is built on top of the last one, those tiny errors just keep stacking up and never go away. That's called **drift**.

The worst offender is heading error. If the robot thinks it's facing 0° but it's really at 1°, it ends up about 1.75 meters off sideways after 100 meters. A tiny angle mistake turns into a big position mistake, because distance multiplies it. So the longer you rely on dead reckoning alone, the less you can trust it.

## Where you'll see it

- **Mobile robots and robot vacuums**, tracking their path between landmarks
- **Cars**, like when your GPS dot keeps moving inside a tunnel. The car is dead reckoning from wheel speed and turning.
- **Submarines and underwater vehicles**, where GPS signals can't reach
- **Aircraft and ships**, as a backup when other navigation systems fail
- **Drones**, holding steady between GPS updates
- **Phones and wearables**, counting your steps and tracking movement indoors

## The takeaway

Think of dead reckoning as that one friend who's super confident and always quick to answer, but slowly gets more and more lost. You wouldn't trust them on a long trip alone, but give them a map and let them check in now and then, and they're really useful. The whole trick in robotics is knowing when to let it run and when to correct it.
