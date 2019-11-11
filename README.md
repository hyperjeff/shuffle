shuffle
=======
A pre-compiled command-line audio player for macOS.
Very light-weight music player, lots of options.
Default mode is to shuffle music randomly,
but you can also play in non-shuffle mode.

![in action](https://github.com/hyperjeff/shuffle/blob/master/screen1.png)

## Unique features:
- Use any collections of audio/music folders at it any time you run it (no pre-ordained music locations)
- Shuffle by albums (optionally within genre)
- Instantly starts even for huge collections of music (in shuffle mode)
- Indefinite rewind in shuffle mode
- Jump forward/back by album (when in non-shuffle mode)
- Regex filters applied to filenames (optional)
- Uses roughly 1/10th the RAM of iTunes, and hardly any CPU resources

## Install
To install:
```
brew install hyperjeff/tools/shuffle
```

Subsequent updates via:
```
brew upgrade shuffle
```

## About
`shuffle` was written purely in Swift using AVFoundation, and hence is Mac-only.

`shuffle -h` for help, which is this currently:

```
shuffle 1.5 -- Plays a shuffled list of music files
               Copyright 2019 HyperJeff, Inc

Usage: shuffle [options] [-genre Genre] [-regex "..."] [-start hh:mm:ss] [-rate x] [directories/files]

 -genre | only include items within a specific genre (implies -r)
 -rate  | set the initial playback speed multiplier
 -regex | filter filenames using a regular expression
 -start | begin at specified offset time (first track only)

options for music playback:

     -a | album shuffle (implies -r)
     -n | non-shuffle mode
     -r | recursively search directories
     -y | year order, then artist

options for info display:

     -f | filepath shown
     -h | print this help and exit
     -i | invert colors
     -m | metadata not shown
     -q | don't clear info on quit
     -s | short filepath display
     -t | track and year info not shown
     -0 | no printed output
     -1 | monochrome output

If no directory is specified, current directory will be used.
Albums and genre only available on Spotlight-indexed volumes.

  space | pause/continue song
   [ ]  | skip back/ahead 15 seconds
   { }  | skip to previous/next album
   ← →  | previous/next song
   ↑ ↓  | volume up/down
   < >  | adjust rate slower/faster
    =   | set rate back to default
    ∞   | loop current track (option-5)
    f   | toggle showing filepath
    Q   | quit without clearing info
    q   | quit
        |
   tab  | show current music lineup
  ⇧-tab | show current album lineup (when available)
  ctl-r | reveal current tune in enclosing folder
```

## Examples
Shuffle play all music in one directory:
```
shuffle -r ~/Music
```

Play tracks in order:
```
shuffle -n ~/Music/AudioBooks/Other-Minds/
```

Shuffle play all music in many directories:
```
shuffle -r ~/Music /Volumes/Exocat/Music /Volumes/SomeNetworkVolume/Music/
```

Shuffle whole albums in a given genre:
```
shuffle -a -genre Ambient ~/Music
```

Play all songs in chronological order:
```
shuffle -ry ~/Music/Compilations
```

Shuffle all songs with the word Feelin', Feeling, Feelings:
```
shuffle /Volumes/∆/Music/ -r -regex "Feelin.*"
```

## Notes
Flags can be input separately:
```
shuffle -f -i -r -1
```
or grouped together:
```
shuffle -1irf
```

*Regex*es are applied (currently at least) only to filenames, and technically are formed by prepending `.*\b` and postpending `\b.*` in order to make it easier to just put in a word that one might want to make a playlist theme out of.
