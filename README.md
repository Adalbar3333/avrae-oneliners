Avrae One-Liners
================

A list of commonly used one-liners/code snippets for use in the Draconic Scripting language.

Table of Contents
=================
* [Get Combatants from Targets](#get-combatants-from-targets)
* [Basic Roll String Construction](#basic-roll-string-construction)
* [Load SVAR and overwrite from CVAR/UVAR](#load-svar-and-overwrite-from-cvaruvar)
* [Search Through a List of Dictionaries](#search-through-a-list-of-dictionaries)
* [Get a Subclass for a certain Class](#get-a-subclass-for-a-certain-class)
* [Change Die Size based on Level](#change-die-size-based-on-level)

Contributing
===========

If you'd like to contribute, open an issue with the one-liner you'd like to add, what it does, and some example use cases for it!

One Liners
==========

Get Combatants from Targets
---------------------------

This takes any targets made with `-t` and tries to convert them into combatants, and then stores them in a list called `combatants`.

```py
{{c = combat() if combat() else None}}
{{targets = args.get('-t') if args.get('t') else None}}
{{combatants = [c.get_combatant(t) for t in targets] if c else []}}
```

Basic Roll String Construction
------------------------------

This creates a simple roll string based off of a skill bonus (in this case Perception) and provided arguments (`-b`, `adv`/`dis`)

```py
<drac2>
args = argparse(&ARGS&) # Parse our Arguments
skill_mod = character().skills.perception.value # Store our perception mod
roll_str = f"{['1d20', '2d20kh1', '2d20kl1'][args.adv()]}{skill_mod:+}{f'+{b}' if (b := args.join('b', '+')) else ''}" # Construct our roll with adv parsing and bonuses.
</drac2>
```

Load SVAR and overwrite from CVAR/UVAR
---------------------

This loads an SVAR as a dictionary from JSON and then tries to update the JSON using a CVAR/UVAR. Used for server settings with local overrides.

Replaceables: 
* `varname` - The variable you wish to store the C/U/SVAR in.
* `CVAR_NAME` - The name of the CVAR/UVAR.
* `SVAR_NAME` - The name of the SVAR.
```py
{{varname = load_json(get_svar('SVAR_NAME', '{}')).update(load_json(get('CVAR_NAME', '{}')))}}
```

Search Through a List of Dictionaries
-------------------------------------

This will search through a list of dictionaries to compare a value in the dictionary to a certain string.
To make it an exact search, replace `in` with `==`. To make it case-sensitive, remove the two `.lower()`

Replaceables:
* `search_var` - The variable that we are searching for
* `search_key` - The key in each dictionary we are looking at.
* `search` - The list we are looking through.
* `out` - The resulting intersection (List of dictionaries that match the search.)

```py
{{out = [x for x in search if search_var.lower() in x["search_key"].lower()]}}
```

Get a Subclass for a certain Class
----------------------------------

This will get the player's subclass for a certain class. Will only work if said player has setup their character with the `!level` alias. If the player does not have a subclass, it will be `""`, an empty string.

Replaceables:
* `sClass` - The variable to store the subclass in.
* `XLevel` - The Class you want to use. `ClericLevel`, `BardLevel`, etc.
```py
{{sClass=load_json(get("subclass", "{}")).get("XLevel", "")}}
```

Change Die Size based on Level
------------------------------

This will create a dice string where the die string changes based on the character's level. The example shown below is for Monk's Martial Arts die.

Replaceables:
* `dice` - The variable to store the dice string in.

```py
{{L=level}}
{{dice = f"1d{4+2*((L>4)+(L>9)+(L>15))}"}}
```