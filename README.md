# [pygame.mixer][1]

pygame module for loading and playing **sounds**.

This module contains classes for loading Sound objects and controlling playback.  
The mixer module is optional and depends on **SDL_mixer**.  
Your program should test that `pygame.mixer` is available and initialized before using it.

The mixer module has a limited number of channels for playback of sounds.  
Usually programs tell pygame to start playing audio and it selects an available channel automatically.  
The default is `8` simultaneous channels, but complex programs can get more precise control over the number of channels and their use.

All sound playback is mixed in background threads.  
When you begin to play a Sound object, it will return immediately while the sound continues to play.  
A single Sound object can also be actively played back multiple times.

The mixer also has a special streaming channel.  
This is for `music` playback and is accessed through the [pygame.mixer.music](/doc/music.md) module.  
Consider using this module for playing long running music.  
The music module streams the music from the files without loading music at once into memory.

The mixer module must be initialized like other pygame modules, but it has some extra conditions.  
The [pygame.mixer.init()](#init) function takes several optional arguments to control the playback rate and sample size.  
Pygame will default to reasonable values, but pygame cannot perform Sound resampling, so the mixer should be initialized to match the values of your audio resources.

NOTE:  
For less laggy sound use a smaller buffer size.  
The default is set to reduce the chance of scratchy sounds on some computers.  
You can change the default buffer by calling [pygame.mixer.pre_init()](#pre_init) before [pygame.mixer.init()](#init) or [pygame.init()](/doc/pygame.md/#init) is called.  
For example: `pygame.mixer.pre_init(44100,-16,2, 1024)`

### .init()

```python
init(frequency=44100, size=-16, channels=2, buffer=512, devicename=None, 
     allowedchanges=AUDIO_ALLOW_FREQUENCY_CHANGE | AUDIO_ALLOW_CHANNELS_CHANGE) -> None
```

Initialize the mixer module for Sound loading and playback.  
The default arguments can be overridden to provide specific audio mixing.  
For backwards compatibility, argument values of 0 are replaced with the startup defaults, except for allowedchanges, where -1 is used.  
(startup defaults may be changed by a `pre_init()` call).

The `size` argument represents how many **bits** are used for each audio sample.  
If the value is negative then signed sample values will be used.  
Positive values mean unsigned audio samples will be used.  
An invalid value raises an exception.

The `channels` argument is used to specify whether to use **mono (1)** or **stereo (2)**.  

The `buffer` argument controls the number of internal samples used in the sound mixer.  
It can be lowered to reduce latency, but sound dropout may occur.  
It can be raised to larger values to ensure playback never skips, but it will impose latency on sound playback.  
The buffer size must be a **power of two** (if not it is rounded up to the next nearest power of 2).
