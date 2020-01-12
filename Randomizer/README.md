## Randomizer

This script allow you to launch randomly a game.
 - from a specific system, a set of systems or from all systems
 - from a specific genre, a set of genres or all genres (using the `<genre>` tag in your `gamelist.xml`)
 - with name containing one string or one of several strings
 - from a combination of these three criterias

### How it works

The scripts uses the */usr/share/emulationstation/es_systems.cfg* to get the list of systems and their command line.

It then selects one of the non-excluded system on random.
After that it selects one non-hidden game from the system's *gamelist.xml* according to genre and name criteria, verify the file exists and launch it.

***Yes this means that only scrapped games and systems will be used, genre tag needs also to be present***

Selection is done using the content of the rdm files, based on three criteria : *systems*,*genres*,*gamestrings*.
This is customisable, see below.

If some game didn't launch you can look at the log */userdata/randomlog.csv* to see what happened

### Installation

* First copy the script in the */userdata/* folder
* Edit the */usr/share/emulationstation/es_systems.cfg* file to add the new random system (don't forget first to remount with `mount -o remount,rw /`)
Alternativaly you can modify the `/userdata/system/configs/emulationstation/es_systems.cfg` file if you have a copy of base `ex_systems.cfg` there
```xml
<system>
  <fullname>Random</fullname>
  <name>random</name>
  <path>/userdata/roms/random</path>
  <extension>.rdm</extension>
  <command>python /userdata/randomizer.py %CONTROLLERSCONFIG% -rom %ROM%</command>
  <theme>random</theme>
  <platform>random</platform>
  <emulators/>
</system>
```
* Kill EmulationStation with `/etc/init.d/S31emulationstation stop` (wait for it to die)
* Copy the `random` folder into  `/userdata/roms` directory
* Copy `theme/random` directory in `/usr/share/emulationstation/themes/es-theme-carbon` (base system theme) or in the folder of the custom theme you are using
* Save your modifications with `batocera-save-overlay`

## Initialisation

* Before using the script, you need to init the system using command `python randomizer.py` or `python randomizer.py init`
* The script will scan all your systems having a valid gamelist and construct all needed rdm files in the `random` system and scrape them
* Now just reboot and it should work flawlessly :)
* You can reinit the random script at any time in the future, you need to kill emulationstation before and reboot after

## Rdm format and customisation

Rdm files now contains three tags to allow randomization of games base on three criteria, values are sepatated by `;` :
* `systems:` list of systems (short name) which can be randomy selected. all systems if this tag is not present. case insensitive
* `genres:` list of genres which can be randomy selected. all genres if tag is not present. case insentitive, can contain spaces
* `gamestrings:` strings contained in name of selectable games. case insensitive

Here are a few examples :
* Platform games on All systems
```
genres:platform
```

* Puzzle games on all arcade systems
```
systems:mame;naomi;fba;neogeo
genres:puzzle
```

* All gameboy games
```
systems:gb
```

* All games with mario or zelda in their full name on nintendo consoles
```
systems:gb;nes;snes;n64;virtualboy;gbc;gba;gw
gamestrings:mario;zelda
```

Nonetheless, be wary of the `gamestrings` tag as it can lead to time-consuming research of a game to launch, best to restrict the search by combining it with `systems` or `genres`