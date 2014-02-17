BWMetaAI
========

A Brood War AI designed to follow a metagame closer to modern 1v1 than the original Brood War AI.

![Zeal Goon vs Mech](screenshots/zeal_goon_tank.png)

Purpose
--------

There are a lot of [good BW AI mods](http://broodwarai.com/wiki/index.php?title=AI_script_mods) already available . This project seeks to create a non-cheating AI that provides a unique experience with each game. It tends to avoid all-in strategies in favor of an economic focus, but it also includes a number of rushes to make scouting and early defense important.

![Muta Harass](screenshots/muta_harass.png)

This AI should provide a significantly better learning experience for new players. Because it rarely performs early all-in style attacks, it encourages scouting and fast expanding to match the AI's economy rather than 1 or 2 base turtle play.

![Large bio attack](screenshots/marines_sunkens.png)

The scripts include a number of build orders that have been relatively popular in high level play. Once the initial build is completed, one of several common midgame transitions are used to take the AI into a variety of late game attacks.

Build Orders
------------
 
### Terran

* Siege Expand
* 1 Rax FE
* 2 Rax FE
* 2 Rax Pressure
* BBS
* 14 CC
* 2 Factory Push
* +1 5 Rax
* 2 port wraith

### Zerg
 
* 5 Pool
* 9 Pool
* Overpool
* 1 Base Muta
* 1 Base Lurker
* 12 Hatch 12 Pool
* 4 Hatch Lair
* 3 Hatch muta

### Protoss

* Forge Fast Expand
* 12 Nexus
* 2 Gate Range Expand
* 10/15 Gate Dragoon Pressure
* 2 Gate Corsair
* 2 Gate Zealots
* Fast DT

Late game 
---------

### Terran

* SK
* Mech
* Battlecruisers

### Zerg

* Muta-ling
* Lurker-ling-defiler
* Ultra-ling-defiler
* Hydra

### Protoss

* Zealot Dragoon Arbiter
* Zealot Archon
* Carriers

Running from source
--------------------

Prerequisites: 

- [node.js](http://nodejs.org/download/)
- make ([MSYS](http://www.mingw.org/wiki/MSYS) on Windows)
- [Python](http://www.python.org/download/) 2.7 32 bit on Windows 

Windows is required for mpq manipulation. If you just want to generate the source of the scripts or aiscript.bin to inject manually, then any OS should be fine.

Build and patch SC
===============

1. Edit the makefile to use your SC path
2. Edit the makefile to use the absolute path to your prefered SC launcher. This can simply be starcraft.exe if no launcher is used
3. Execute run.cmd to compile the AIs, build an MPQ, and replace the default in your SC directory. Alternatively, you can call `make run` directly from the command line.

New AI commands/macros:
-----------------------

This includes an AI preprocessor which implements some new commands that you can use if you want to contribute to this project.

### If blocks

Many _jump commands can be accessed using a new Pythonic `if` structure. Here's how you would normally create a block that executes 50% of the time:

    random_jump(128, maybe)
    goto(always)
    --maybe--
    # sometimes do this
    --always--
    # always do this
    
If blocks allow you to write the same code like this:

    if random(128):
        # sometimes do this
    # always do this
    
Else is not yet supported.

### loop blocks

Blocks of code can be loop indefinitely as follows:

    loop:
        do_morph(36, Terran Marine)

### multirun blocks

Blocks of code can be run asyncronously as follows:

    multirun:
        upgrade(1, Protoss Ground Weapons)
        wait(5000)
        upgrade(2, Protoss Ground Weopons)
        
    train(5, Zealot)
    
A stop command is added at the end of the block automatically.

### multirun_loop blocks

Loops can be run asyncronously as follows:

    multirun_loop:
        if enemyowns(Dark Templar):
            build_finish(1, Forge)
            build_start(2, Photon Cannon)
            
The block will automatically repeat after a wait of 75.

### message(string)

Displays the string as a message from the AI. This does not require a block to jump to like debug() does.

### wait_resources(min, gas)

Waits until the ai has the corresponding amount of minerals and gas in the bank.

### wait_until(minutes)

Waits until {minutes} normal games minutes have elapsed.

### attack_async()

Spawns a new thread to attack and continues execution of calling thread immediately.

### attack_simple()

Prepares, attacks with, and clears the current attacking parting.

### build_start(amount, building, [priority])

Builds {amount} of {building} at {priority} and waits for construction to start. Priority defaults to 80.

### build_finish(amount, building, [priority])

Builds {amount} of {building} at {priority} and waits for construction to complete. Priority defaults to 80.

### build_separately(amount, building, [priority])

Builds {amount} of {building} at {priority} one at a time and waits for construction to complete. Priority defaults to 80.

### enemyownsairtech_jump(block)

Jumps to block if enemy has Starport, Stargate, or Spire

### enemyownscloaked_jump(block)

Jumps to block if enemy has units that can cloak

### repeat()

Jumps to the beginning of the current file

### valid_build_against(Race, Race, Race)

Skips this build if nearest race is not in the list of races

### valid_midgame_against(Race, Race, Race)

Skips this midgame if nearest race is not in the list of races

### include(file)

Includes the text of the selected file in place of this command. This is similar to the #include C preprocessor macro.
