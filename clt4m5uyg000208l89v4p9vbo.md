---
title: "Hacking FF7: Data Maps"
datePublished: Tue Feb 27 2024 17:00:23 GMT+0000 (Coordinated Universal Time)
cuid: clt4m5uyg000208l89v4p9vbo
slug: hacking-ff7-data-maps
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708965599353/cbf1585d-6217-4406-81b3-aa2f8f1d3796.png
tags: memory, ff7

---

This series of posts is going to look at hacking into the memory of a much-beloved game from the 90's: Final Fantasy VII. Specifically, we're looking at the PC release (whether it be the original, or the Steam re-release, it doesn't make much difference).

To begin with, we're going to start with 2 prime locations for data we can use. Data save map, and the battle map. These will hold the majority of the useful data for us.

## Save Map

The save map is a series of binary data that gets saved to the memory card or save file when the game is saved. It is also updated whenever changes occur within the Field (i.e. not in battle) as we play the game. Having a good idea of at least some of how this is formatted will help us utilise this information.

In the English translations of the game, the save map begins at offset `0xDBFD38`, and has a length of `0x10F4` bytes. This is the `ff7.exe` in the original release, and `ff7_en.exe` in the steam release.

All offsets are slightly different in other official languages for the game. However, mod-translations based on the English version retain the same offsets as the English version.

| Offset | Length | Description |
| --- | --- | --- |
| 0x4 | 0x26 | Save Preview (information shown on the Load Game screen) |
| 0x48 | 0xB | Window Colour |
| 0x54 | 0x80 \[Character\] | Cloud |
| 0xD8 | 0x80 \[Character\] | Barret |
| 0x15C | 0x80 \[Character\] | Tifa |
| 0x1E0 | 0x80 \[Character\] | Aeris |
| 0x264 | 0x80 \[Character\] | Red XIII |
| 0x2E8 | 0x80 \[Character\] | Yuffie |
| 0x36C | 0x80 \[Character\] | Cait Sith / Young Cloud |
| 0x3F0 | 0x80 \[Character\] | Vincent / Sephiroth |
| 0x474 | 0x80 \[Character\] | Cid |
| 0x4F8 | 0x1 | Party Member 1 |
| 0x4F9 | 0x1 | Party Member 2 |
| 0x4FA | 0x1 | Party Member 3 |
| 0x4FC | 0x280 | Items Inventory |
| 0x77C | 0x320 | Materia Inventory |
| 0xA9C | 0xC0 | Stolen Materia |
| 0xB7C | 0x3 | Gil |
| 0xB80 | 0x4 | Number of Seconds Played |

Obviously, there's more to the save map than just this, but this gives us a good start without getting too long. For a full list of what I've found so far, please see my [SaveMapOffsets file on GitHub](https://github.com/Shojy/Reno/blob/main/Shojy.FF7.Reno/MemoryAddresses/Offsets/SaveMapOffsets.cs)

You may have also noticed that within some of the records, I've denoted a `[Character]` type for the data. This is a common structure used for each character, and resembles the following:

| Offset | Description |
| --- | --- |
| 0x0 | Character Id |
| 0x1 | Level |
| 0x2 | Strength |
| 0x3 | Vitality |
| 0x4 | Magic |
| 0x5 | Spirit |
| 0x6 | Dexterity |
| 0x7 | Luck |
| 0x8 | Strength Bonus |
| 0x9 | Vitality Bonus |
| 0xA | Magic Bonus |
| 0xB | Spirit Bonus |
| 0xC | Dexterity Bonus |
| 0xD | Luck Bonus |
| 0xE | Limit Level |
| 0xF | Limit Gauge |
| 0x10 | Name |
| 0x1C | Equipped Weapon |
| 0x1D | Equipped Armor |
| 0x1E | Equipped Accessory |
| 0x1F | Status Flags |
| 0x20 | Row |
| 0x21 | Level Progress |
| 0x22 | Limit Breaks |
| 0x24 | Number of kills |
| 0x26 | Limit Level 1 Uses |
| 0x28 | Limit Level 2 Uses |
| 0x2A | Limit Level 3 Uses |
| 0x2C | Current HP |
| 0x2E | Base HP |
| 0x30 | Current MP |
| 0x32 | Base MP |
| 0x38 | Max HP |
| 0x3A | Max MP |
| 0x3C | Current EXP |
| 0x40 | Weapon Materia |
| 0x60 | Armor Materia |
| 0x80 | EXP to next level |

As you can see, we've got some quite rich information about our characters right now. Unfortunately, this is only half of the story when it comes to our party. You might remember that I mentioned the save map is updated on the field. What this means is that during battle, this data isn't updated until the battle ends. To compliment this data, lets look at the Battle Map next.

## Battle Map

The battle map is a shorter set of memory used when a battle is active. This can be found at offset `0x9AB0DC`, and has a length of `0x750` bytes. It's comprised of a series of actors, each holding some basic information about the combatants:

| Offset | Length | Description |
| --- | --- | --- |
| 0x0 | 0x68 \[BattleActor\] | Party Member 1 |
| 0x68 | 0x68 \[BattleActor\] | Party Member 2 |
| 0xD0 | 0x68 \[BattleActor\] | Party Member 3 |
| 0x1A0 | 0x68 \[BattleActor\] | Enemy 1 |
| 0x208 | 0x68 \[BattleActor\] | Enemy 2 |
| 0x270 | 0x68 \[BattleActor\] | Enemy 3 |
| 0x2D8 | 0x68 \[BattleActor\] | Enemy 4 |
| 0x340 | 0x68 \[BattleActor\] | Enemy 5 |
| 0x3A8 | 0x68 \[BattleActor\] | Enemy 6 |

Like with `[Character]` above, we also have `[BattleActor]` here. These represent both allied and enemy characters, and hold basic information required for battle. The basics of which look like this:

| Offset | Description |
| --- | --- |
| 0x0 | Status |
| 0x4 | Row |
| 0x9 | Level |
| 0x28 | Current MP |
| 0x2A | Max MP |
| 0x2C | Current HP |
| 0x30 | Max HP |

Obviously, you'll notice this isn't complete, but again, it's enough to get us started. Using both the save and battle maps above lets us access the core stats of our party and enemies whatever state the game is in.

However, to ensure that we know exactly which one to use at any given time, we have one more location to look at: Offset `0x9A8AF8` holds a flag indicating whether or not we're currently in an active battle.

We'll look more in-depth in a number of these in a future post, but for now, we have enough to watch our party both in and out of battle.