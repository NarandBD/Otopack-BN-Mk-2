### What doesn't work vs What does work.
### Last updated: 6/20/2025

### Preloading SFX

Everything here works fine, Sound effects can be included for preloading with a format like this:

```json
[
  {
    "type": "sound_effect_preload",
    "preload": [
      { "id": "fire_gun", "variant": "all" },
      { "id": "environment", "variant": "daytime" },
      { "id": "environment" }
    ]
  }
]
```

`"variant": "all"` will be treated specially and load all variants of the given id.

:::warning

`"variant": "all"` uses unoptimal algorithm (because the dev was dumb and lazy and used hacks) and
will slow down game loading time.

:::

If `"variant"` is omitted, it defaults to `"default"`.

## NOTE:
I'm not fully familiar with how preloading works for a game like CBN. I'm assuming its like how preloading is 
done in other games where its any sounds put under `preload.json` get preloaded so as to not use up resources getting the sound ready.

I don't how bad performance could be if I shove everything into that .json file ( assuming without utilizing `"variant": "all"` ),
so I'm going to utilize it sparingly for stuff I believe is alot to load on the spot. 

### Playlist

Haven't fiddled with this much, although I do want to add more music at _some_ _point_. A playlist can be included with 
a format like this:

```json
[
  {
    "type": "playlist",
    "playlists": [
      {
        "id": "title",
        "shuffle": false,
        "files": [
          {
            "file": "Dark_Days_Ahead_demo_2.wav",
            "volume": 100
          },
          {
            "file": "cataclysmthemeREV6.wav",
            "volume": 90
          }
        ]
      }
    ]
  }
]
```

Each sound effect is identified by an id and a variant. If a sound effect is played with a variant
that does not exist in the json files, but a variant "default" exists, then the "default" variant is
played instead. The file name of the sound effect is relative to the soundpack directory, so if the
file name is set to "sfx.wav" and your soundpack is in `data/sound/mypack`, the file must be placed
at `data/sound/mypack/sfx.wav`.

## Sound effects list, aka the meat of this repository lol.

How stuff is formatted here is going to be slightly different from CBN's doc of this but not by much.
I'm going to simply label all the sections with ids and their respective variants into three categories,
- What I think does work/does indeed work. :white_check_mark for what works, and :grey_question for what I think works.
and
- What doesn't work. This will have a :x

### My understanding for stuff with :white_check_mark / :grey_question :
Works as expected or works for the most part, with certain exceptions mentioned with parenthesis for specific sections.

### My understanding for stuff with :x :
Might've worked on older releases of CBN, but I have yet to test this possibility.
All I know is a mix of what i've tested so far and the fact that the docs for Soundpacks is two years outdated xd
Subject to change or at least have a workaround mentioned, but unlikely as that'd be outside my current knowledge of assigning sounds.
~~and i'm horrified of learning new things~~

A full list of sound effect id's and variants is given in the following. Each line in the list has
the following format:

`id variant1|variant2`

Where id describes the id of the sound effect, and a list of variants separated by | follows. When
the variants are omitted, the variant "default" is assumed. Where the variants do not represent
literal strings, but variables, they will be enclosed in `<` `>`. For instance, `<furniture>` is a
placeholder for any valid furniture ID (as in the furniture definition JSON).


    # opening/closing doors 

- `open_door default|<furniture>|<terrain>` :white_check_mark
- `close_door default|<furniture>|<terrain>` :white_check_mark

  # smashing attempts and results, few special ones and furniture/terrain specific
- `bash default` :grey_question
- `smash wall|door|door_boarded|glass|swing|web|paper_torn|metal` :grey_question
- `smash_success hit_vehicle|smash_glass_contents|smash_cloth|<furniture>|<terrain>` :white_check_mark
- `smash_fail default|<furniture>|<terrain>` :white_check_mark

  # melee sounds
- `melee_swing default|small_bash|small_cutting|small_stabbing|big_bash|big_cutting|big_stabbing` :white_check_mark
- `melee_hit_flesh default|small_bash|small_cutting|small_stabbing|big_bash|big_cutting|big_stabbing|<weapon>` :white_check_mark
- `melee_hit_metal default|small_bash|small_cutting|small_stabbing|big_bash|big_cutting|big_stabbing!<weapon>` :white_check_mark
- `melee_hit <weapon>` # note: use weapon id "null" for unarmed attacks (haven't used/tested `null` yet but maybe.) :grey_question

  # firearm/ranged weapon sounds
- `fire_gun <weapon>|brass_eject|empty` :white_check_mark
(`fire_gun empty` doesn't do anything as it's not supported sadly, the rest work. Might make an issue to suggest adding empty firing.)

- `fire_gun_distant <weapon>` :white_check_mark
- `reload <weapon>` :white_check_mark
(Works for a majority of guns, except specific ones/types i've documented on related issues or pull requests. Most of the time works.)

- `bullet_hit hit_flesh|hit_wall|hit_metal|hit_glass|hit_water` :grey_question

  # environmental sfx, here divided by sections for clarity
  # :white_check_mark , except for deafness.
- `environment thunder_near|thunder_far`
- `environment daytime|nighttime`
- `environment indoors|indoors_rain|underground`
- `environment <weather_type>` # examples:
  `WEATHER_DRIZZLE|WEATHER_RAINY|WEATHER_THUNDER|WEATHER_FLURRIES|WEATHER_SNOW|WEATHER_SNOWSTORM`
(Side note: Needs something like WEATHER_DRIZZLE_LIGHT for light drizzle, and ones for when the Japanese weathers pr gets merged.)

- `environment alarm|church_bells|police_siren`
- `environment deafness_shock|deafness_tone_start|deafness_tone_light|deafness_tone_medium|deafness_tone_heavy` :x

  # misc environmental sounds
- `footstep default|light|clumsy|bionic` :grey_question
(I think this is for bionics or less likely mutations. Assuming its for bionics, which i haven't tested yet cuz i dont use bionics much.)

- `explosion default|small|huge` :x

  # ambient danger theme for seeing large numbers of zombies :x
- `danger_low`
- `danger_medium`
- `danger_high`
- `danger_extreme`

  # chainsaw pack :x , except for weapon_theme which idk if it works. Probably not.
- `chainsaw_cord     chainsaw_on`
- `chainsaw_start    chainsaw_on`
- `chainsaw_start    chainsaw_on`
- `chainsaw_stop     chainsaw_on`
- `chainsaw_idle     chainsaw_on`
- `melee_swing_start chainsaw_on`
- `melee_swing_end   chainsaw_on`
- `melee_swing       chainsaw_on`
- `melee_hit_flesh   chainsaw_on`
- `melee_hit_metal   chainsaw_on`
- `weapon_theme      chainsaw`

  # monster death and bite attacks :white_check_mark
- `mon_death zombie_death|zombie_gibbed`
- `mon_bite bite_miss|bite_hit`

- `melee_attack monster_melee_hit`

- `player_laugh laugh_f|laugh_m` :grey_question

  # player movement sfx
  **important:** `plmove <terrain>` has priority over default `plmove|walk_<what>` (excluding
  `|barefoot`) example: if `plmove|t_grass_long` is defined it will be played before default
  `plmove|walk_grass` default for all grassy terrains

- `plmove <terrain>|<vehicle_part>` :white_check_mark
- `plmove walk_grass|walk_dirt|walk_metal|walk_water|walk_tarmac|walk_barefoot|clear_obstacle` :grey_question
(`clear_obstacle` does not work despite it being used in this repository. I think the rest here gets priority over/overrides `clear_obstacle`. Would make sense if so.)

  # fatigue
- `plmove fatigue_m_low|fatigue_m_med|fatigue_m_high|fatigue_f_low|fatigue_f_med|fatigue_f_high` :white_check_mark

  # player hurt sounds
- `deal_damage hurt_f|hurt_m` :white_check_mark

  # player death and end-game sounds
- `clean_up_at_end game_over|death_m|death_f` :white_check_mark
(might use `game_over` at some point? probably not but sounds interesting to mess with.)

  # various bionic sounds
- `bionic elec_discharge|elec_crackle_low|elec_crackle_med|elec_crackle_high|elec_blast|elec_blast_muffled|acid_discharge|pixelated` :grey_question
- `bionic bio_resonator|bio_hydraulics|` :grey_question
(I usually get bored and restart far before I reach getting to bionics so this is going into "idk probably works" for now.)

  # various tools/traps being used (including some associated terrain/furniture) :grey_question
- `tool alarm_clock|jackhammer|pickaxe|oxytorch|hacksaw|axe|shovel|crowbar|boltcutters|compactor|gaspump|noise_emitter|repair_kit|camera_shutter|handcuffs`
- `tool geiger_low|geiger_medium|geiger_high`
- `trap bubble_wrap|bear_trap|snare|teleport|dissector|glass_caltrop|glass`
(I don't think `trap bubble_wrap` works. Vaugely remember using Kenan's version of Otopack and it not working on CBN but I think working on CDDA?)

  # various activities :grey_question
- `activity burrow`
(utilized this in a recent update, idk if it works)

  # musical instruments, `_bad` is used when you fail to play it well :white_check_mark
- `musical_instrument <instrument>`
- `musical_instrument_bad <instrument>`
(never used instruments yet but I assume it still works)

  # various shouts and screams :white_check_mark
- `shout default|scream|scream_tortured|roar|squeak|shriek|wail|howl`
(used `shout default` and it works, so i assume the rest is fine even though I think they're not utilized)

  # speech, it is currently linked with either item or monster id, or is special `NPC` or `NPC_loud` :white_check_mark
  # TODO: full vocalization of speech.json
- `speech <item_id>` # examples: talking_doll, creepy_doll, Granade,
- `speech <monster_id>` # examples: eyebot, minitank, mi-go, many robots
- `speech NPC_m|NPC_f|NPC_m_loud|NPC_f_loud` # special for NPCs
- `speech robot` # special for robotic voice from a machine etc.
(haven't dipped my toes into using all of these minus `speech <monster_id>` for ferals and such like them. could try to use this section more at some point but I assume it all works.)

  # radio chatter :grey_question
- `radio static|inaudible_chatter`

  # humming sounds of various origin :grey_question
- `humming electric|machinery`

  # sounds related to (burning) fire :x
- `fire ignition`

  # vehicle sounds - engine and other parts in action :white_check_mark
  
(**note:** defaults are executed when specific option is not defined)
- `engine_start <vehicle_part>` # note: specific engine start (id of any
  engine/motor/steam_engine/paddle/oar/sail/etc. )
- `engine_start combustion|electric|muscle|wind` # default engine starts groups
- `engine_stop <vehicle_part>` # note: specific engine stop (id of any
  engine/motor/steam_engine/paddle/oar/sail/etc. )
- `engine_stop combustion|electric|muscle|wind` # default engine stop groups

  # note: internal engine sound is dynamically pitch shifted depending on vehicle speed
  
  # (it is an ambient looped sound with dedicated channel)
- `engine_working_internal <vehicle_part>` # note: sound of engine working heard inside vehicle
- `engine_working_internal combustion|electric|muscle|wind` # default engine working (inside) groups

  # note: external engine sound volume and pan is dynamically shifted depending on distance and angle to vehicle
  
  # volume heard at given distance is linked to engine's `noise_factor` and stress to the engine (see `vehicle::noise_and_smoke()` )
  
  # it is an ambient looped sound with dedicated channel
  
  # this is a single-channel solution (TODO: multi-channel for every heard vehicle); it picks loudest heard vehicle
  
  # there is no pitch shift here (may be introduced when need for it emerges)
- `engine_working_external <vehicle_part>` # note: sound of engine working heard outside vehicle
- `engine_working_external combustion|electric|muscle|wind` # default engine working (outside)
  groups

  # note: gear_up/gear_down is done automatically by pitch manipulation
  
  # gear shift is dependant on max safe speed, and works in assumption, that there are
  
  # 6 forward gears, gear 0 = neutral, and gear -1 = reverse
- `vehicle gear_shift`

- `vehicle engine_backfire|engine_bangs_start|fault_immobiliser_beep|engine_single_click_fail|engine_multi_click_fail|engine_stutter_fail|engine_clanking_fail`
- `vehicle horn_loud|horn_medium|horn_low|rear_beeper|chimes|car_alarm`
- `vehicle reaper|scoop|scoop_thump`

- `vehicle_open <vehicle_part>` # note: id of: doors, trunks, hatches, etc.
- `vehicle_close <vehicle part>`

  # miscellaneous sounds
- `misc flashbang|flash|shockwave|earthquake|stairs_movement|stones_grinding|bomb_ticking|lit_fuse|cow_bell|bell|timber` :grey_question
(`misc bomb_ticking` does work but others don't such as `stairs_movement` and I think `lit_fuse`. Why? But why tho?
Haven't tested the rest yet.)

- `misc rc_car_hits_obstacle|rc_car_drives` :white_check_mark
(`misc rc_car_drives` doesn't work, needs to be fixed. Mentioned why while working on a pr but essentially
assigning a sound using this resulted in it being played whenever the player moves while an RC car is turned on.
Very weird coding magic happening here.)

- `misc default|whistle|airhorn|horn_bicycle|servomotor` :grey_question
(Probs works but haven't tested anything else except `airhorn` with a clown start on CDDA.)

- `misc beep|ding|` :grey_question
- `misc rattling|spitting|coughing|heartbeat|puff|inhale|exhale|insect_wings|snake_hiss` :grey_question
(mostly organic noises)

(Again haven't tested everything here but `coughing` works, ableit it occurs slightly too often but hey it works. `inhale` and `exhale` might be for going underwater and resurfacing?)
