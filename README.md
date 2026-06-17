# Advanced Python Module Project: Defeat the Evil Wizard 🧙‍♂️
## RPG Battle Game

A turn-based, text-driven battle game pitting a player-controlled hero against an Evil Wizard. The player picks a character class and name, then fights using basic attacks, healing, and class-specific special abilities until either the hero or the wizard runs out of health.

## Features

- Four playable character classes (`Warrior`, `Mage`, `Archer`, `Paladin`), each inheriting from a shared `Character` base class
- Two unique special abilities per class: a **special attack** and a **special defense**
- A shared `heal()` method that restores a fixed amount of health without exceeding the character's maximum
- Randomized basic attack damage on every hit
- A turn-based battle loop with a player action menu (attack, use special ability, heal, view stats)
- An `EvilWizard` opponent that regenerates health and attacks the player on its turn
- Victory and defeat messages printed as soon as either combatant's health reaches 0

## Character Classes

All classes share the same base mechanics from `Character` (basic attack, heal, stat display) and add two special abilities each.

| Class | Health | Attack Power |
|---|---|---|
| Warrior | 140 | 25 |
| Mage | 100 | 35 |
| Archer | 120 | 20 |
| Paladin | 160 | 10 |
| Evil Wizard | 150 | 40 |

### Warrior
- **Axe Strike** (special attack) — Deals `attack_power × 3` damage.
- **Shield Block** (special defense) — Sets a dodge flag so the Warrior takes no damage from the opponent's next attack.

### Mage
- **Lightning Bolt** (special attack) — Deals `attack_power × 3` damage.
- **Magic Shield** (special defense) — Sets a dodge flag that absorbs the next incoming attack entirely.

### Archer
- **Quick Shot** (special attack) — A double arrow attack dealing `attack_power × 2` damage.
- **Evasive Roll** (special defense) — Sets a dodge flag so the next attack against the Archer is dodged completely.

### Paladin
- **Smite** (special attack) — A holy strike dealing `attack_power × 4` damage.
- **Divine Shield** (special defense) — Sets a dodge flag that blocks the next attack against the Paladin.

> Note: in the current implementation, all four "special defense" abilities work the same way under the hood — they set a `dodging` flag that causes the character to completely avoid (rather than reduce) the next incoming attack. Only the name and flavor text differ between classes.

## Healing Mechanic

`heal()` is defined once on the base `Character` class and inherited by every class, including the Evil Wizard's regeneration logic. It restores a fixed **20 health** per use, and `min(self.max_health, self.health + heal_amount)` ensures health never exceeds the character's maximum.

## Randomized Attack Damage

The base `attack()` method does not deal fixed damage. Each basic attack rolls a random value **between 80% and 120% of the attacker's `attack_power`** using `random.randint`, so identical characters won't always deal the same damage twice in a row. If the target is currently "dodging," the attack is negated entirely and no damage is dealt.

## Turn-Based Battle System

The `battle()` function runs the main loop while both the player and the wizard have health remaining. Each round:

1. **Player Turn** — The player chooses from a menu:
   - `1` — Attack the wizard with a basic attack
   - `2` — Use a special ability (a follow-up prompt asks whether to use the special **attack** or special **defense**)
   - `3` — Heal (restore 20 health, capped at max)
   - `4` — View stats (does not use up the wizard's turn)
2. **Evil Wizard Turn** — If the wizard is still alive and the player's choice wasn't "View Stats," the wizard:
   - Regenerates a fixed **5 health** (capped at its maximum)
   - Attacks the player with randomized damage (unless the player is currently dodging)
3. After each exchange, the game checks both characters' health and prints a defeat message the moment either one drops to 0 or below.

## Evil Wizard Logic

The `EvilWizard` class extends `Character` and adds a `regenerate()` method that heals it for a flat 5 health each turn (never exceeding its max health of 150). It does not have special abilities of its own — it relies on the base `attack()` method, so its attacks are also randomized between 80% and 120% of its 40 attack power.

## End of Game

- If the wizard's health reaches 0, the script prints a message declaring it has been **defeated by the player**.
- If the player's health reaches 0, the script prints a message declaring the **player has been defeated**, ending the battle loop.

## How to Run

```bash
python rpg_battle_game.py
```

You'll first be prompted to choose a class (1–4) and enter a character name, then the turn-based battle against "The Dark Wizard" begins automatically.


## Tech Stack
![Python](https://img.shields.io/badge/python-3670A0?style=flat&logo=python&logoColor=ffdd54)
![Git](https://img.shields.io/badge/git-%23F05033.svg?style=flat&logo=git&logoColor=white)
