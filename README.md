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

You may also need to install the [Swift 5 Runtime Support for Command Line Tools](https://support.apple.com/kb/DL1998?locale=en_US) from Apple.

Subsequent updates via:
```
brew upgrade shuffle
```

## About
`shuffle` was written purely in Swift using AVFoundation, and hence is Mac-only.

`shuffle -h` for help, which is this currently:

```
shuffle 1.4 -- Plays a shuffled list of music files
               Copyright 2019 HyperJeff, Inc

Usage: shuffle [options] [-genre Genre] [-regex "..."] [directories &/on files]

   -h | print this help and exit

options for music playback:

   -a | album shuffle
   -n | non-shuffle mode
   -r | recursively search directories
   -y | year order, then artist

options for info display:

   -f | filepath shown
   -i | invert colors
   -m | metadata not shown
   -s | short filepath display
   -t | track and year info not shown
   -0 | no printed output
   -1 | monochrome output

If no directory is specified, current directory will be used.
Albums and genre only available on Spotlight-indexed volumes.
Albums and genre always use recursive folder descent.
The Regex filter is performed against filenames only.

  space | pause/continue song
   [ ]  | skip back/ahead 15 seconds
   { }  | skip to previous/next album
   ← →  | previous/next song
   ↑ ↓  | volume up/down
   < >  | adjust rate slower/faster
    =   | set rate back to default
    ∞   | loop current track (option-5)
    f   | toggle showing filepath
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
