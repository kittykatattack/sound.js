Sound for Games
===============

"Sound for Games" is micro-library that lets you load, play and generate sound effects and music for
games and interactive applications. It's very small: less than 800
lines of code and no dependencies.

At its heart it's composed of
just two, short, independent functions: `makeSound` and `soundEffect`. The `makeSound` function helps you
load and play sound files (mp3, wav, ogg, and webm). The `soundEffect`
function helps you generate a wide range of sound and music effects
from pure code.
These two functions are completely modular and free of dependencies, so
you can
copy and paste whichever parts of them you need into your own
projects. In fact, you can create all the sound an music for your
project in less than
150 lines of code by just using the `soundEffect` function on its own. 

If you need it, there's also a `sounds` object with a helpful
`load` method that makes it easy to load and access sound files.
You'll find all the code in the `sound.js` file. Take a look at the
`index.html` file for a working example of all the features. All this
code is completely unlicensed and free for you to use however you
like.

Installation
-------------

Just link to the `sound.js` file with a `<script>` tag:
```
<script src="sound.js"></script>
```
Or just copy/paste whichever functions you need from the `sound.js`
file into your own project code.

Loading sound files
-------------------

The best way to load sound files is to use the `sounds` object's
`load` method. List the each sound file's path and name in the `sound.load`
method's array. The assign a callback function to `sounds.whenLoaded`.
```
//Load the sounds
sounds.load([
  "sounds/shoot.wav", 
  "sounds/music.wav",
  "sounds/bounce.mp3"
]);

//Assign the callback function that should run
//when the sounds have loaded
sounds.whenLoaded = setup;
```
After the sounds have loaded, the `setup` function will run. Use the
`setup` function to initialize your sounds.
```
function setup() {
  //Initialize sounds here
}
```
Of course, you can give this function any name you like, it doesn't
have to be called `setup`.

Initializing loaded sounds
--------------------------

After a sound file is loaded using the `sounds.load` method, you can access
it as a property of the `sounds` object like this:
```
sounds["sounds/music.wav"]
```
Create variable references to all the sounds you want to use so that
they're easier to work with:
```
var shoot = sounds["sounds/shoot.wav"],
    music = sounds["sounds/music.wav"],
    bounce = sounds["sounds/bounce.mp3"];
```
You now have three sound objects, `shoot`, `music`, and `bounce` that
you can play and control.

Playing and controlling loaded sounds
-------------------------------------

You can play, pause, loop and restart sounds as well as set their volume and speaker
pan settings,  Here's how:
```
  //Play the sound
  music.play();

  //Make the sound loop
  music.loop = true;

  //Set the volume 
  //1 is full volume, 2 is double volume, 0.5 is half volume, etc.
  music.volume = 0.7;  

  //Pause the sound. To resume paused sounds call `play` again
  music.pause();

  //Play from a specific time (in seconds)
  music.playFrom(10)

  //Set the pan
  //-1 is the full left speaker, 0 is the middle, 1 is the full right
  //speaker
  music.pan = -0.8;
```
### Change the playback rate
Use `playbackRate` to change the speed at which the sound plays back.
A value of 0.5 will make the sound play at half speed. A value of 2
will make it play at double speed. 1 is normal speed.
```
music.playbackRate = 0.5;
```
Changing a sound's playback rate doesn't affect its pitch.

### Add echo

Use the `setEcho` method to set the sound's optional echo effect.
`setEcho` takes three arguments: `delay`, `feedback` and `filter`.
```
bounce.setEcho(0.2, 0.3, 1000);
```
`delay` and `feedback` are times, in seconds. `delay`determines how much
time will pass before the sound repeats. `feedback` determines the how
strong each repetition is - the higher the `feedback` value, the
longer the echo will last. `filter` is an optional value in Hertz that
filters out frequencies above that value. It changes the echo's tone
with each repetition for a more organic effect. Set `filter` to `0` to
disable it. If you ever need to switch off a sound's echo effect, set
the sound's
`echo` property to `false`
```
bounce.echo = false;
```
### Add reverb

Set a sound's reverb effect using `setReverb`. `setReverb` takes 3
arguments: `duration`, `decay`, and `reverse`. 
```
music.setReverb(2, 2, false);
```
`duration` and `decay` are times, in seconds, that determine how
pronounced the reverb effect is. Give them large values to simulate a
large space, and small values to simulate a small space. The third
argument `reverse` is a Boolean (a true/false value) that determines whether the
reverb effect should be reversed. Set it to `false` for normal reverb, or
to `true` for a spooky reverse reverb. Set the sound's `reverb`
property to `false` if you need to disable the reverb effect later.

This is how work with sound files. But "Sound for Games" also lets you
generate your own
music and sound effects from scratch.

Generating sound effects and music
----------------------------------

Use the versatile `soundEffect` function to create an almost limitless
variety of sound effects using thirteen low-level parameters. Here's a model
for using it, including a description of what each parameter does
```
soundEffect(
  frequencyValue,      //The sound's fequency pitch in Hertz
  attack,              //The time, in seconds, to fade the sound in
  decay,               //The time, in seconds, to fade the sound out
  type,                //waveform type: "sine", "triangle", "square", "sawtooth"
  volumeValue,         //The sound's maximum volume
  panValue,            //The speaker pan. left: -1, middle: 0, right: 1
  wait,                //The time, in seconds, to wait before playing the sound
  pitchBendAmount,     //The number of Hz to bend the sound's pitch down
  reverse,             //If `reverse` is true the pitch will bend up
  randomValue,         //A range, in Hz, within which to randomize the pitch
  dissonance,          //A value in Hz. Creates 2 dissonant frequencies above and below the target pitch
  echo,                //An array: [delayTimeInSeconds, feedbackTimeInSeconds, filterValueInHz]
  reverb               //An array: [durationInSeconds, decayRateInSeconds, reverse]
);

```
(Note: `reverb` for sound effects is currently **experimental** due to
browser quirks. In the future it might stabilize but, until then use
it at your peril!)
The strategy for using the `soundEffect` function is to tinker with
all these parameters and come up with your own custom library of sound
effects for games. Think of it as a big sound board with thirteen colourful
flashing dials you can play with. And think of yourself as a mad
scientist and that thirteen is your lucky number!

###Shoot sound

Here's an
example of how to use the `soundEffect` function to create a typical laser shoot sound.
```
function shootSound() {
  soundEffect(
    1046.5,           //frequency
    0,                //attack
    0.3,              //decay
    "sawtooth",       //waveform
    1,                //Volume
    -0.8,             //pan
    0,                //wait before playing
    1200,             //pitch bend amount
    false,            //reverse bend
    0,                //random pitch range
    25,               //dissonance
    [0.2, 0.2, 2000], //echo array: [delay, feedback, filter]
    undefined         //reverb array: [duration, decay, reverse?]
  );
}
```
The "sawtooth" waveform setting gives the sound a biting harshness.
The `pitchBendAmount` is 1200, which means the sound's pitch drops
1200 Hz. from start to finish. That makes it sound like every laser
from every science fiction movie you've ever seen. The `dissonance` value of 25 means that
two extra overtones are added to the sound, 25 Hz above and below the
main frequency. Those extra overtones add an edgy complexity to the
tone.

Because the `soundEffect` function is wrapped in a custom `shootSound`
function, you can play the effect at any time in your application code
like this:
```
shootSound();
```
It will play immediately.

###Jump sound

Let's look at another example. Here's a `jumpSound` function that
produces a typical platform game character jumping sound.
```
function jumpSound() {
  soundEffect(
    523.25,       //frequency
    0.05,         //attack
    0.2,          //decay
    "sine",       //waveform
    3,            //volume
    0.8,          //pan
    0,            //wait before playing
    600,          //pitch bend amount
    true,         //reverse
    100,          //random pitch range
    0,            //dissonance
    undefined,    //echo array: [delay, feedback, filter]
    undefined     //reverb array: [duration, decay, reverse?]
  );
}

```
The `jumpSound` has an `attack` value of 0.05, which means there's a
very quick fade-in to the sound. It's so quick that you can't really hear it, but it
softens the start of sound a bit. The `reverse` value is `true` which means
that the pitch bends up instead down. (This makes sense because
jumping characters jump upwards.) The `randomValue` is 100. That means
the pitch will randomize within a range 100 Hz above and below the target frequency, so
that the sound's pitch is slightly different every time. This adds organic interest to
the sound and makes the game world feel alive.

###Explosion sound

You can create a radically different `explosionSound` effect just by
tweaking these same parameters.
```
function explosionSound() {
  soundEffect(
    16,          //frequency
    0,           //attack
    1,           //decay
    "sawtooth",  //waveform
    1,           //volume
    0,           //pan
    0,           //wait before playing
    0,           //pitch bend amount
    false,       //reverse
    0,           //random pitch range
    50,          //dissonance
    undefined,   //echo array: [delay, feedback, filter]
    undefined    //reverb array: [duration, decay, reverse?]
  );
}

```
This creates a low frequency rumble. The starting point for the
explosion sound is to set the `frequency` value extremely low: 16 Hz. It also
has a harsh "sawtooth" waveform. But what makes it really work is the
`dissonance` value of 50. This adds two overtones above and below the
target frequency; they interfere with each other and the main sound.

###Making music

But it's not just for sound effects! You can use the `soundEffect`
function to create musical notes, and play them at set intervals.
Here's a function called `bonusSound` which plays three notes (D, A
and high D) in a
rising pitch sequence. It's typical of the kind of musical motif you might
hear when a game character scores some bonus points, like picking up
stars or coins. (When you hear this sound you might have a flashback to 1985!)
```
function bonusSound() {
  //D
  soundEffect(587.33, 0, 0.2, "square", 1, 0, 0);
  //A
  soundEffect(880, 0, 0.2, "square", 1, 0, 0.1);
  //High D
  soundEffect(1174.66, 0, 0.3, "square", 1, 0, 0.2);
}
```
The key to making it work is the last argument: the `wait` value. The
first sound's `wait` value is 0, which means the sound will play
immediately. The second sound's `wait` value is 0.1, which means it
will play after a delay of 100 milliseconds. The last sound's `wait`
value is 0.2, which will make it play in 200 milliseconds. This
means that all three notes play in sequence with a 100 millisecond
gap between them.

With just a little more work you could use the `wait` parameter to build
a simple music sequencer, and build your own mini-library of musical sound effects
just for playing notes. If you need it, here's how to convert
frequencies in Hertz to real note values:

[Note values of frequencies in Hertz](http://www.phy.mtu.edu/~suits/notefreqs.html) 

Have fun!
