# notvox nodboxes
**(noun)** : _trolling with rich presence_

> Sitting on a cornflake
> 
> Waiting for the van to come
> 
> Corporation T-shirt
> 
> stupid bloody Tuesday
> 
> _man_ ...
> 
> You let your face grow long

Reminiscing on _Walrus_, John Lennon recounted being chuffed by some student fan mail out of Quarry Bank: a literature class at the school had taken up the folly of trying to analyze, interpret --- divine meaning --- out of Beatles lyrics.  

Lennon of course was privy to his process but these students were not:
> "I’ve had tongue in cheek all along – all of them had tongue in cheek. Just because other people see depths of whatever in it…  
> What does it really mean, ‘I am the eggman’? It could have been ‘the pudding basin,’ for all I care. It’s not that serious.”


## What?
Just a gag, really.

_Some time ago_ a person took to commenting on the music I listen to which shows up in rich presence on discord.
I have rather ...eclectic? music tastes, and a penchant for darkness in music (I was 'trained classically' on the late romantics, after all, and consider myself a Mahlerian above all else).

Whether they simply enjoyed stirring up drama or were frankly concerned, sometimes taste is just taste and listening has no subtext; trying to extrapolate a person's state of mind based on their listening habits is inane and teeters on delusional, if you ask me.

Natülich, I found myself equal parts amused and perturbed; I decided the logical thing to do was quietly escalate. 

So I started repurposing some machines I have into what I'm calling NotVox NodBoxes. NotVox Itself is a 'distributed system' deisgned for remote-controlled Spotify playback _discreetly_ and with ludicrous parameters, i.e., with the express purpose of trolling.

Each NodBox is just a fedora server 41 instance which,
* runs a daemonized `spotifyd` service via `systemd`
* My custom command executino system,  which I call `cue`

### How it Works

NotVox consists of three core components that interact via networked command execution:
1. `cue-client` (user tui)
   * primary CLI for users (me) to issue terse commands
   * Dispatches commands to a specified/fallback NodBox from bare metal, ie, not a NodBox
1. `cue-server` (command router and interpreter)
   * listens for commands from `cue-client`
   * interprets the command and translates down to an appropriate DBus or `spotifyd` command
   * dispatches execution to `cue-exec`
1. `cue-exec` (command executor)
   * responsible for directly interfacing with `spotifyd`
   * runs on the same machine as the `spotifyd` daemon for any requested box


`cue` allows me to tersely run commands from my quotidian machines to have a NodBox do things like:

* start 'playing' a {track,playlist,etc} on repeat for X {hours,days}
* stop playback
* switch tracks
* and more

using a straightforward syntax:
```bash
# on bare metal
cue notvox-nomad start quiet "Leaving the Table" end-in 2D
```
This command:
1. transmits the command over the network, here, specifically to `notvox-nomad`, which is my 'portable' NodBox.
2. `cue-server` interprets and routes accordingly
   - `start quiet` commands a NodBox to perform playback but silence it so that whoever is in the vicinity doesn't have to listen, lol.
   - "Leaving the Table" is a song title.
     - *note* human-readable titles must be mapped to hashes.
   - `end-in 2D` tells notvox-nomad to keep playing this song on repeat for 2 days straight.
4. `cue-exec` executes the action by issuing commands to spotifyd or Dbus
5. All goes well, Leonard Cohen's _Leaving the Table_ will "play quietly" for 2 days straight unless I intervene.


### Why Am I Now Publicking this?
Up to you to interpret ;)  
_Sapient sat._

### nota bene; A poem:

> ### **Once upon a Jukebox**  
> Once an acquaintance  
> A person Matt knew  
> Took up a strange habit—  
> watching sounds pass through.  
>   
> Digital specter,  
> Quiet observer,  
> Tracking not `echoes`  
> but chords, giving fervor?  
>   
> Each note a confession,  
> Each album a clue—  
> A detective of playlists  
> Interpreting mood?  
>   
> *"You must be unwell,"*  
> They whispered with dread,  
> *"I saw Nick Cave playing—  
> I know what that meant."*  
>   
> But this jukebox of life  
> Is never so plain,  
> Some days it's _just_ Cohen,  
> Some days Sugar Ray.  
>   
> So let them decode,  
> Let them divine—  
> The secrets of shuffle  
> Are no friend of mine.
>
> mfw

### Note for the (particularly) Observant
Basically no code pushed?  
What's up?  
**Auth is up.**  
Fiddling while publicking and working towards packaging for deb + homebrew broke shit lol, as it always does, and so I need to fix it but... keine zeit

