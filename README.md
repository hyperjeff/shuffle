shuffle
=======
A pre-compiled command-line audio player for macOS.
Very light-weight music player, lots of options.
Default mode is to shuffle music randomly,
but you can also play in non-shuffle mode.

![in action](https://github.com/hyperjeff/shuffle/blob/master/screen3.png)

Tab to toggle seeing the song list:

![in action](https://github.com/hyperjeff/shuffle/blob/master/screen2.png)

## Unique features:
- Use any collections of audio/music folders at it any time you run it (no pre-ordained music locations)
- Shuffle by albums (optionally within a genre)
- Play music within multiple genres at once
- Instantly starts even for huge collections of music (in shuffle mode)
- Indefinite rewind in shuffle mode
- Jump forward/back by album (when in non-shuffle mode)
- Regex filters applied to filenames (optional)
- Uses roughly 1/10th the RAM of iTunes, and hardly any CPU resources

## Additional features:
- Supports 16 file types, including FLAC music
- Can easily create playlists by just making a folder full of aliases, symlinks or Finder SavedSearches
- Can start audio books at a specified play rate and/or time offset
- Can play music in year order regardless of album or folder

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
shuffle 1.9.0 -- Plays a shuffled list of music files
                 © 2019-2022 HyperJeff, Inc

Usage: shuffle [options] [--genre Genre] [--regex "..."]
       [--mdfind "..."] [--start hh:mm:ss] [--rate x] directories/files

 --genre  | only include items within a specific genre(s) (implies -r)
 --mdfind | add any standard mdfind filter (no need to type "kMDItem")
 --rate   | set the initial playback speed multiplier
 --regex  | filter filenames using a regular expression
 --start  | begin at specified offset time (first track only)

options for music playback:

       -a | album shuffle (implies -r)
       -n | non-shuffle mode
       -y | year order, then artist

options for info display:

       -e | print examples and exit (--ex, --examples)
       -f | show filepath
       -g | show genre
       -h | print this help and exit (--help)
       -i | force inverted colors
       -m | metadata not shown
       -q | don't clear song info on quit
       -s | short filepath display
       -t | track and year info not shown
       -v | print version number and quit (--version)
       -0 | no printed output
       -1 | monochrome output

Albums and genre only available on Spotlight-indexed volumes.
To index your music so genres work: mdimport [directory]
shuffle can't currently read year info from FLAC files.

Several genres can be specified if in quotes and comma-separated.

Playlists can be created by making a folder full of Finder aliases
or unix symlinks, or by defining and saving Finder Smart Folders.
Speed tip: don't keep playlists within main music library folder.

Handy mdfind aliases: Year, Genre, Track, Rate, Length.

    space | pause/continue song
     [ ]  | skip back/ahead 15 seconds
     { }  | skip to previous/next album
     ← →  | previous/next song
     ↑ ↓  | volume up/down
     < >  | adjust rate slower/faster
     2-9  | change the look-ahead item count
      =   | set rate back to default
      ∞   | loop current track (option-5)
      f   | toggle showing filepath
      g   | toggle showing genre
      x   | remove next song from queue
      y   | remove song after next song from queue
      Q   | quit without clearing info
      q   | quit
          |
     tab  | show current music queue
shift-tab | show current album queue (when available)
    ctl-r | reveal current tune in enclosing folder
```

## Examples
Shuffle play all music in one directory:
```
shuffle ~/Music
```

Play tracks in order:
```
shuffle -n ~/Music/AudioBooks/Other-Minds/
```

Shuffle play all music in many directories:
```
shuffle ~/Music /Volumes/Exocat/Music /Volumes/SomeNetworkVolume/Music/
```

Shuffle whole albums in a given genre:
```
shuffle -a --genre Ambient ~/Music
```

Shuffle among multiple genres:
```
shuffle ~/Music --genre "Glam Rock, Experimental Pop, Speed Bluegrass"
```

Play all songs in chronological order:
```
shuffle -y ~/Music/Compilations
```

Play all complete albums by Devo in order they came out:
```
shuffle ~/Music/Devo -ay
```

Shuffle all songs with the word Feelin', Feeling, Feelings:
```
shuffle ~/Music/ --regex "Feelin.*"
```
or use regex to pick out groups of terms into a playlist:
```
shuffle ~/Music/ --regex "Sun|Moon|Star|Planet"
```

Advanced `mdfind`-base searches can save you from some convoluted piping.
Use any clause you might want to send to mdfind to filter for songs.
And best of all, no need to type "MDItem"!
High quality music from 1970 to 1976:
```
shuffle ~/Music --mdfind 'AudioSampleRate > 44000 && Year >= 1970 && Year <= 1976'
```


## Notes
Flags can be input separately:
```
shuffle -f -i -1
```
or grouped together:
```
shuffle -1if
```

shuffle attempts to determine if it should invert colors automatically if you are using Terminal or iTerm2, so you may see, only once, an alert about it trying to make a call to AppleScript.

*Regex*es are applied (currently at least) only to filenames, and technically are formed by prepending `.*\b` and postpending `\b.*` in order to make it easier to just put in a word that one might want to make a playlist theme out of.

If you get a message that shuffle cannot find any music files when you declare a genre, but you can play music when you don't, then try the following in Terminal:
```
mdimport [directory with your music files]
```

## Tips
What are all the genres in your music library? Try this:
```
find /Your/Music/ -type f -print0 | xargs -0 mdls -name kMDItemMusicalGenre | awk '{ split($0, a, "\""); print a[2] }' | sort | uniq -i | tr '\n' ,
```
And this variation will show you the relative frequency each has in your collection:
```
find /Your/Music/ -type f -print0 | xargs -0 mdls -name kMDItemMusicalGenre | awk '{ split($0, a, "\""); print a[2] }' | sort | uniq -i -c | sort -r
```
(You may find, like me, that there is a lot of garbage genres that you may want to fix up with some metadata file editor. There are many, and you can always use iTunes/Music for this.)


## Known Issues
- Cannot read year info from FLAC files
- Remote/networked drives often don't have Spotlight indexing, causing albums shuffles and year information to fail
- `genre` and `mdfind` options don't work with aliases / playlists
- Smart Folders can't use date constraints
