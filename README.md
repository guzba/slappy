# Slappy - 3d sound api for nim.

Big thanks to [yglukhov's sound](https://github.com/yglukhov/sound) library!

This library provides higher level interface to the [OpenAL](https://github.com/treeform/openal) library.

Slappy provides the standard features of:
* 3d positions of sounds.
* 3d position of listener.
* Doppler shift.
* Acoustic attenuation.

Slappy also provides some extra features such as:
* A much more nim-like api.
* `.wav` and `.ogg` loading.
* Sound priority. (in progress)
* Max number of the same sound played. (in progress)
* Fade in and fade out. (in progress)
* Ability to queued sounds. (in progress)

## Example:

```nim
# rotate sound in 3d
let sound = newSound("tests/drums.mono.wav")
var source = sound.play()
source.looping = true
echo "rotating sound in 3d, 1 rotation"
for i in 0..360:
  let a = float(i) / 180 * PI
  source.pos = vec3(sin(a), cos(a), 0)
  sleep(20)
source.stop()
sleep(500)
```

See [test.nim](https://github.com/treeform/Slappy/blob/master/tests/test.nim) for more details.

## Basic concepts

### Listener

**Listener** is the main ear of abstract person in the 3d world.

Listener has the following properties:
  * gain - Volume on which the listener hears.
  * pos - Position
  * vel - Velocity
  * mat - Orientation matrix

You get one global Listener.

### Sound

**Sound** the recording of the sound that can be played.

Sound can be loaded with:
`sound = newSound("path/to/wav.or.ogg")`

Sound has the following functions:
  * play() - Creates a `Source` objects that has the sound playing.

Sound has the following properties, that are read only:
  * bits - Bit rate or number of bits per sample.
  * size - Number of byte the sound takes up.
  * freq - Frequency or the samples per second rate.
  * channels - Number of channels, only 1 or 2 supported. `WARNING`: 2 channel sounds can't be positioned in 3d
  * samples - Number of samples, a sample is a single integer that sets the position of the speaker membrane.
  * duration - duration of the sound in seconds.

### Source

**Source** represents the sound playing in a 3d world, kind of like an abstract speaker.

Source has the following functions:
  * stop() - Stop the sound.
  * play() - Start playing the sound (if it was stopped before).

Source has the following properties:
  * pitch - How fast the sound plays, or how low or high it sounds.
  * gain - Volume of the sound.
  * maxDistance - Inverse Clamped Distance Model, where sound will not longer be played.
  * rolloffFactor - Set roll off rate for the source.
  * halfDistance - The distance under which the volume for the source would normally drop by half.
  * minGain - The minimum gain for this source.
  * maxGain - The minimum gain for this source.
  * playing - Is the sound playing.
  * looping - Should the sound loop.
  * pos - Position
  * vel - Velocity
  * mat - Orientation matrix
  * playback - playback position in seconds (offset)
