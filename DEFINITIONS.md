# Arc Raiders - Game Data Definitions

This document provides detailed explanations of all specifications, attributes, and game mechanics found in the Arc Raiders data files.

---

## Table of Contents

1. [Core Item Attributes](#core-item-attributes)
2. [Rarity Tiers](#rarity-tiers)
3. [Item Types](#item-types)
4. [Weapon Specifications](#weapon-specifications)
5. [Shield Specifications](#shield-specifications)
6. [Augment Specifications](#augment-specifications)
7. [Consumable & Grenade Specifications](#consumable--grenade-specifications)
8. [Crafting & Economy](#crafting--economy)
9. [Bot (ARC Enemy) Specifications](#bot-arc-enemy-specifications)
10. [Skill Node Specifications](#skill-node-specifications)

---

## Core Item Attributes

### id
- **Type**: `string`
- **Definition**: Unique identifier for the item, matching the JSON filename without extension
- **Example**: `"arc_alloy"`, `"venator_ii"`

### name
- **Type**: `string`
- **Definition**: Display name of the item shown in-game
- **Example**: `"ARC Alloy"`, `"Venator II"`

### description
- **Type**: `string`
- **Definition**: Flavor text and functional information about the item

### type
- **Type**: `string`
- **Definition**: Category classification determining item behavior and UI placement
- **See**: [Item Types](#item-types)

### value
- **Type**: `number`
- **Definition**: Base coin value for selling to traders. Higher values indicate rarer or more useful items
- **Unit**: Coins (in-game currency)
- **Range**: 6 (ammunition) to 15,000+ (high-tier weapons)

### weightKg
- **Type**: `number`
- **Definition**: Physical weight contributing to the player's loadout limit. Heavier items restrict mobility and total carry capacity
- **Unit**: Kilograms
- **Range**: 0.025 (ammo) to 12+ (LMGs, heavy weapons)
- **Impact**: Affects movement speed and determines how much a player can carry before hitting their Max Loadout Weight

### stackSize
- **Type**: `number`
- **Definition**: Maximum quantity of identical items that can occupy a single inventory slot
- **Range**: 1 (weapons, unique items) to 80+ (ammunition)
- **Note**: Items without stackSize are typically non-stackable equipment

### imageFilename
- **Type**: `string`
- **Definition**: Relative path to the item's icon image
- **Format**: `images/items/{filename}.png`

### updatedAt
- **Type**: `string`
- **Definition**: Last modification date for the data entry
- **Format**: `MM/DD/YYYY`

---

## Rarity Tiers

Rarity indicates item power level, availability, and typical value. Listed from lowest to highest:

| Tier | Color | Description |
|------|-------|-------------|
| **Common** | White/Gray | Basic items found frequently. Low value, easily replaceable |
| **Uncommon** | Green | Slightly improved stats or utility. Moderately available |
| **Rare** | Blue | Significant stat improvements. Found in locked areas or crafted |
| **Epic** | Purple | High-tier equipment with notable bonuses. Requires advanced crafting |
| **Legendary** | Gold/Orange | Top-tier items with unique traits. Rare drops or complex crafting |

---

## Item Types

### Weapons
| Type | Description |
|------|-------------|
| **Pistol** | Secondary sidearms. Low weight, moderate damage |
| **Hand Cannon** | High-damage pistols with slower fire rate |
| **SMG** | Submachine guns. High fire rate, lower damage per shot |
| **Assault Rifle** | Balanced automatic weapons |
| **LMG** | Light Machine Guns. Large magazines, high weight, accuracy penalty when standing |
| **Shotgun** | Close-range weapons dealing spread damage |
| **Launcher** | Heavy weapons firing explosive projectiles |

### Equipment
| Type | Description |
|------|-------------|
| **Shield** | Defensive equipment absorbing incoming damage |
| **Augment** | Wearable gear defining loadout capacity and passive bonuses |
| **Modification** | Weapon attachments improving specific stats |

### Consumables
| Type | Description |
|------|-------------|
| **Quick Use** | Items used during raids (medkits, grenades, stims) |
| **Ammunition** | Bullets/rounds for weapons |

### Resources
| Type | Description |
|------|-------------|
| **Material** | Basic crafting components |
| **Topside Material** | Resources gathered during raids on the surface |
| **ARC Component** | Parts salvaged from destroyed ARC robots |
| **Recyclable** | Items primarily used for breaking down into materials |

### Special
| Type | Description |
|------|-------------|
| **Key** | Single-use items unlocking specific locked doors/containers |
| **Blueprint** | Unlocks crafting recipes at hideout workbenches |
| **Trinket** | Collectible/valuable items for selling |

---

## Weapon Specifications

### Durability
- **Format**: `"current/maximum"` (e.g., `"110/110"`)
- **Definition**: Weapon condition. Degrades with use. At 0, weapon becomes unusable until repaired
- **Impact**: Must be maintained through repairs at hideout

### Ammo Type
- **Values**: `Light Ammo`, `Medium Ammo`, `Heavy Ammo`, `Shotgun Ammo`, `Energy Ammo`, `Launcher Ammo`
- **Definition**: Determines which ammunition the weapon consumes
- **Note**: Players must carry matching ammo type

### Magazine Size
- **Type**: `number`
- **Definition**: Rounds fired before requiring reload
- **Range**: 6 (revolvers) to 80+ (LMGs)

### Firing Mode
| Mode | Description |
|------|-------------|
| **Semi-Automatic** | One shot per trigger pull |
| **Fully-Automatic** | Continuous fire while trigger held |
| **Burst** | Multiple shots per trigger pull (typically 2-3) |

### ARC Armor Penetration
- **Values**: `Very Weak`, `Weak`, `Moderate`, `Strong`, `Very Strong`
- **Definition**: Effectiveness against armored ARC robot components
- **Impact**: Higher penetration deals more damage to armored bot parts
- **Ranking** (low to high):
  1. Very Weak - Minimal effect on armor
  2. Weak - Reduced damage to armored parts
  3. Moderate - Standard effectiveness
  4. Strong - Good against armored targets
  5. Very Strong - Pierces heavy armor effectively

### Special Trait
- **Definition**: Unique weapon behavior or bonus
- **Examples**:
  - `"Twin Shot"` - Fires two projectiles per trigger pull
  - `"Burst Fire"` - Fires in controlled bursts

### Weapon Stat Modifiers

| Modifier | Description |
|----------|-------------|
| **Increased Fire Rate** | Percentage faster rate of fire vs base model |
| **Reduced Reload Time** | Percentage faster reloads vs base model |
| **Reduced Vertical Recoil** | Less upward kick when firing |
| **Reduced Horizontal Recoil** | Less side-to-side deviation when firing |
| **Increased Accuracy** | Tighter bullet spread |
| **Increased Damage** | Higher damage per shot |

### isWeapon
- **Type**: `boolean`
- **Definition**: Flag indicating item is a weapon (affects UI, inventory handling)

### upgradeCost
- **Type**: `Record<itemId, quantity>`
- **Definition**: Materials required to upgrade weapon to next tier (e.g., Venator I -> Venator II)

---

## Shield Specifications

### Charge
- **Type**: `number`
- **Definition**: Energy pool absorbing incoming damage. Regenerates over time when not taking hits
- **Range**: 40 (Light) to 120+ (Heavy)

### Damage Reduction
- **Format**: Percentage (e.g., `"52.5%"`)
- **Definition**: Portion of incoming damage absorbed by shield while charge remains
- **Impact**: Higher values mean less health damage while shield is active

### Movement Speed Modifier
- **Format**: Negative percentage (e.g., `-15%`)
- **Definition**: Reduction to player movement speed while shield equipped
- **Trade-off**: Heavy shields offer more protection but slow the player

### Shield Types
| Type | Charge | Damage Reduction | Speed Penalty |
|------|--------|------------------|---------------|
| **Light Shield** | Low | ~30-40% | Minimal |
| **Medium Shield** | Medium | ~45-50% | Moderate |
| **Heavy Shield** | High | ~50-65% | Significant |

---

## Augment Specifications

Augments are the core equipment piece defining a Raider's loadout capacity and passive abilities.

### Max Loadout Weight / Weight Limit
- **Type**: `number`
- **Definition**: Maximum total weight (kg) of all carried items
- **Range**: 50-80 kg depending on augment tier/type
- **Impact**: Exceeding limit prevents picking up more items

### Backpack Slots
- **Type**: `number`
- **Definition**: Total inventory slot count for storing items
- **Range**: 14-24 slots

### Safe Pocket Slots
- **Type**: `number`
- **Definition**: Protected inventory slots. Items here are NOT lost on death
- **Range**: 0-3 slots
- **Critical**: Only items in Safe Pocket survive extraction failure

### Quick Use Slots
- **Type**: `number`
- **Definition**: Hotbar slots for consumables (medkits, stims)
- **Range**: 2-4 slots

### Weapon Slots
- **Type**: `number`
- **Definition**: Number of weapons that can be equipped
- **Standard**: 2 slots

### Grenade Use Slots
- **Type**: `number`
- **Definition**: Hotbar slots specifically for throwables
- **Range**: 1-2 slots

### Shield Compatibility
- **Format**: Comma-separated list (e.g., `"Light, Medium, Heavy"`)
- **Definition**: Which shield types can be equipped with this augment
- **Note**: Some augments restrict heavy shields for balance

### Health Regeneration
- **Type**: `string` (descriptive)
- **Definition**: Passive health recovery over time
- **Example**: `"Restores 2 health every 5 seconds. When damage is taken the effect is paused for 30 seconds."`

### Augment Categories

| Category | Focus |
|----------|-------|
| **Combat** | Balanced combat stats, health regen |
| **Tactical** | Defensive bonuses, shield support |
| **Looting** | Maximum carry capacity, safe pocket slots |

---

## Consumable & Grenade Specifications

### Use Time
- **Format**: Seconds (e.g., `"1s"`, `"3s"`)
- **Definition**: Animation duration to consume/deploy item
- **Impact**: Longer use times leave player vulnerable

### Duration
- **Format**: Seconds (e.g., `"10s"`, `"30s"`)
- **Definition**: How long the effect persists after activation

### Radius
- **Format**: Meters (e.g., `"6m"`, `"10m"`)
- **Definition**: Area of effect for grenades/explosives

### Damage
- **Format**: Value or `"X/s"` for damage-over-time
- **Definition**: Health damage dealt to targets
- **Example**: `"5/s"` = 5 damage per second

### Healing Effects

| Effect | Description |
|--------|-------------|
| **Health Restored** | Immediate HP recovery |
| **Health Regeneration** | HP/second over duration |
| **Stamina Regeneration** | Stamina/second recovery |

### Grenade Effects

| Effect | Description |
|--------|-------------|
| **ARC Stun Duration** | How long ARC robots are disabled |
| **Raider Stun Duration** | How long human players are stunned (shorter for balance) |
| **Fire Duration** | How long incendiary area persists |

---

## Crafting & Economy

### recipe
- **Type**: `Record<itemId, quantity>`
- **Definition**: Materials required to craft this item
- **Example**: `{"chemicals": 3, "plastic_parts": 3}`

### craftBench
- **Type**: `string` or `string[]`
- **Definition**: Which hideout workbench(es) can craft this item
- **Values**:
  - `workbench` - Basic crafting
  - `weapon_bench` - Weapons and gun parts
  - `equipment_bench` - Shields, augments, gear
  - `med_station` - Medical items
  - `explosives_bench` - Grenades, explosives
  - `utility_bench` - Tools, utility items
  - `refiner` - Material processing

### stationLevelRequired
- **Type**: `number`
- **Definition**: Minimum workbench upgrade level needed to craft
- **Range**: 1-3

### blueprintLocked
- **Type**: `boolean`
- **Definition**: If true, requires finding/unlocking blueprint before crafting

### recyclesInto
- **Type**: `Record<itemId, quantity>`
- **Definition**: Materials returned when recycling item at hideout
- **Note**: Typically returns ~50% of crafting cost

### salvagesInto
- **Type**: `Record<itemId, quantity>`
- **Definition**: Materials returned when salvaging in the field (less efficient than recycling)

### repairCost
- **Type**: `Record<itemId, quantity>`
- **Definition**: Materials needed to restore durability

### repairDurability
- **Type**: `number`
- **Definition**: How much durability is restored per repair

---

## Bot (ARC Enemy) Specifications

### Bot Types
| Type | Description |
|------|-------------|
| **Reconnaissance** | Non-aggressive scouts, flee when spotted |
| **Scout Drone** | Flying recon units, call reinforcements |
| **Defense System** | Stationary turrets |
| **Ambush Predator** | Fast, close-range attackers |
| **Area Denial** | Control zones with fire/hazards |
| **Medium Drone** | Flying combat units |
| **Flying Drone** | Aggressive aerial attackers |
| **Flying Artillery** | Airborne explosive support |
| **Heavy Assault** | Armored ground combatants |
| **Heavy Artillery** | Long-range bombardment units |
| **Siege Engine** | Massive armored units |
| **Sniper Turret** | Long-range precision units |
| **Boss** | Raid event targets (The Queen) |

### Threat Levels
| Level | Description |
|-------|-------------|
| **Low** | Minimal danger, easily defeated |
| **Moderate** | Requires attention, can hurt careless players |
| **High** | Dangerous, requires tactical approach |
| **Critical** | Severe threat, team coordination recommended |
| **Extreme** | Raid bosses, maximum danger |

### destroyXp
- **Type**: `number`
- **Definition**: Experience points awarded for destroying the ARC unit

### lootXp
- **Type**: `number`
- **Definition**: Experience points awarded for looting the destroyed unit's drops

### drops
- **Type**: `string[]`
- **Definition**: Item IDs that can drop when unit is destroyed and looted

### weakness
- **Type**: `string`
- **Definition**: Tactical information about vulnerable points
- **Example**: `"Exposes his energy core when shooting his blue beam into the sky."`

---

## Skill Node Specifications

### category
- **Values**: `CONDITIONING`, `WEAPONS`, `SURVIVAL`, etc.
- **Definition**: Skill tree branch this node belongs to

### maxPoints
- **Type**: `number`
- **Definition**: Maximum skill points that can be invested
- **Range**: 1-5

### isMajor
- **Type**: `boolean`
- **Definition**: Major nodes provide significant bonuses and often unlock new tree branches

### impactedSkill
- **Type**: `string`
- **Definition**: The specific stat or ability affected
- **Examples**: `Movement Speed`, `Stamina Regeneration`, `Noise Reduction`

### prerequisiteNodeIds
- **Type**: `string[]`
- **Definition**: Skill nodes that must be unlocked before this one becomes available

### position
- **Type**: `{x: number, y: number}`
- **Definition**: Visual position in skill tree UI

---

## Compatibility System

### compatibleWith
- **Type**: `string[]`
- **Definition**: List of weapon names this item works with
- **Applies to**: Ammunition, Modifications (grips, stocks, sights)
- **Example**: `["Arpeggio", "Rattler", "Tempest"]`

---

## Unit Reference

| Abbreviation | Meaning |
|--------------|---------|
| `/s` | Per second (rate) |
| `s` | Seconds (duration) |
| `m` | Meters (distance/radius) |
| `kg` | Kilograms (weight) |
| `%` | Percentage modifier |

---

---

## Weapons Reference

### Weapon Families

Each weapon has tiers I through IV with progressive stat improvements:

| Weapon | Type | Ammo Type | ARC Penetration | Signature Trait |
|--------|------|-----------|-----------------|-----------------|
| **Arpeggio** | Assault Rifle | Medium | Moderate | 3-Round Burst |
| **Tempest** | Assault Rifle | Medium | Moderate | Fully-Automatic |
| **Rattler** | SMG | Medium | Moderate | High fire rate, 2-round reload |
| **Stitcher** | SMG | Light | Very Weak | Fully-Automatic |
| **Ferro** | Battle Rifle | Heavy | Strong | Break-Action (1 shot) |
| **Bettina** | Battle Rifle | Heavy | Strong | Semi-Automatic |
| **Renegade** | Assault Rifle | Medium | Moderate | Fully-Automatic |
| **Torrente** | LMG | Medium | Moderate | 80-round mag, crouch accuracy |
| **Venator** | Pistol | Medium | Moderate | Twin Shot |
| **Anvil** | Hand Cannon | Heavy | Strong | Single-Action, high headshot |
| **Osprey** | Pistol | Light | Weak | Semi-Automatic |
| **Kettle** | SMG | Light | Weak | Compact |
| **Il Toro** | Shotgun | Shotgun | Weak | Pump-Action |
| **Vulcano** | Shotgun | Shotgun | Weak | Semi-Automatic |
| **Hullcracker** | Launcher | Launcher | Very Strong | Explosive |
| **Hairpin** | Pistol | Light | Weak | Rapid fire |
| **Burletta** | Assault Rifle | Medium | Moderate | - |
| **Jupiter** | Launcher | Launcher | Very Strong | Heavy explosive |
| **Equalizer** | Special | Energy | Strong | Unique |

### Detailed Weapon Stats (Base Tier I)

*Updated December 2025 - Post-Patch 1.7*

| Weapon | Damage | Fire Rate | Range | Mag Size | Weight | Sell Price |
|--------|--------|-----------|-------|----------|--------|------------|
| **Anvil** | 40 | 16.3 | 50.2 | 6 | 5 kg | $2,900 |
| **Arpeggio** | 9.5 | 18.3 | 55.9 | 24 | 7 kg | $5,500 |
| **Rattler** | 9 | 33.3 | 56.2 | 12 | 6 kg | $1,750 |
| **Stitcher** | 7 | 45.3 | 42.1 | 20 | 5 kg | $800 |
| **Bettina** | ~12 | ~15 | ~55 | 22 | 8 kg | - |

> **Note**: Fire Rate is shots per second multiplier; higher = faster firing.

### Recent Balance Changes (Patch 1.3+)

| Weapon | Change | Details |
|--------|--------|---------|
| **Venator** | **NERFED** | Weight 2kg → 5kg; fire rate upgrade bonuses reduced |
| **Bettina** | **BUFFED** | Magazine 20 → 22; reload 5s → 4.5s; durability burn 0.43% → 0.17% per shot |
| **Rattler** | **BUFFED** | Magazine 10 → 12 rounds |

> **Developer Note**: Venator was "capable of outgunning full squads" before the nerf. Bettina and Rattler buffs improve their viability as primary weapons.

### Weapon Tier Progression

| Tier | Rarity | Typical Improvements |
|------|--------|---------------------|
| **I** | Common-Uncommon | Base stats |
| **II** | Uncommon-Rare | +10% durability, minor stat boosts |
| **III** | Rare | +20% durability, significant stat boosts |
| **IV** | Rare-Epic | +20-30% durability, major stat boosts, reduced reload |

---

## Weapon Attachments Reference

### Attachment Categories

| Category | Slot | Primary Effect |
|----------|------|----------------|
| **Grips** | Underbarrel | Recoil reduction |
| **Muzzle Devices** | Barrel | Dispersion, recoil, noise |
| **Stocks** | Stock | Stability, handling speed |
| **Magazines** | Magazine | Ammo capacity |
| **Barrels** | Barrel | Bullet velocity |
| **Chokes** | Barrel (Shotgun) | Spread reduction |

---

### Grips

*All Tier I attachments: Trader Price $1,920 / Sell Price $640*

#### Vertical Grip (Reduces Vertical Recoil)

| Tier | Rarity | Effect | Trade-off | Weight | Value | Recipe |
|------|--------|--------|-----------|--------|-------|--------|
| **I** | Common | -20% Vertical Recoil | None | 0.25 kg | $640 | 6x Plastic Parts, 1x Duct Tape |
| **II** | Uncommon | -30% Vertical Recoil | None | 0.5 kg | $2,000 | - |
| **III** | Rare | -40% Vertical Recoil | -30% ADS Speed | 0.75 kg | $5,000 | 2x Mod Components, 5x Duct Tape |

**Recycles Into**: 6x Plastic Parts  
**Compatible Weapons**: Arpeggio, Rattler, Kettle, Il Toro, Stitcher, Tempest, Ferro, Hullcracker, Venator

---

#### Angled Grip (Reduces Horizontal Recoil)

| Tier | Rarity | Effect | Trade-off | Weight | Value | Recipe |
|------|--------|--------|-----------|--------|-------|--------|
| **I** | Common | -20% Horizontal Recoil | None | 0.25 kg | $640 | 6x Plastic Parts, 1x Duct Tape |
| **II** | Uncommon | -30% Horizontal Recoil | None | 0.5 kg | $2,000 | - |
| **III** | Rare | -40% Horizontal Recoil | -30% ADS Speed | 0.75 kg | $5,000 | - |

**Recycles Into**: 6x Plastic Parts  
**Compatible Weapons**: Arpeggio, Ferro, Venator, Il Toro, Stitcher, Tempest, Kettle, Hullcracker

---

#### Horizontal Grip (Epic - Dual Recoil Reduction)

| Tier | Rarity | Effects | Trade-off | Weight | Value | Recipe |
|------|--------|---------|-----------|--------|-------|--------|
| **-** | Epic | -30% Horizontal, -30% Vertical | -30% ADS Speed | 0.5 kg | 7,000 | 2x Mod Components, 5x Duct Tape |

**Requires**: Weapon Bench Level 3, Blueprint  
**Compatible Weapons**: Ferro, Kettle, Rattler, Stitcher, Arpeggio, Il Toro, Venator, Bettina, Bobcat, Tempest, Vulcano

---

### Muzzle Devices

*All Tier I muzzle devices: Trader Price $1,920 / Sell Price $640*

#### Compensator (Reduces Dispersion)

| Tier | Rarity | Per-Shot Dispersion | Max Dispersion | Trade-off | Recipe |
|------|--------|---------------------|----------------|-----------|--------|
| **I** | Common | -20% | -10% | None | 6x Metal Parts, 1x Wires |
| **II** | Uncommon | -40% | -20% | None | 2x Mechanical Components, 4x Wires |
| **III** | Uncommon | -60% | -30% | +20% Durability Burn | 2x Mechanical Components, 8x Wires |

**Recycles Into**: 5x Metal Parts  
**Compatible Weapons**: Ferro, Kettle, Rattler, Stitcher, Anvil, Arpeggio, Burletta, Osprey, Renegade, Torrente, Venator, Bobcat, Tempest, Bettina

---

#### Muzzle Brake (Reduces Both Recoils)

| Tier | Rarity | Vertical Recoil | Horizontal Recoil | Trade-off | Recipe |
|------|--------|-----------------|-------------------|-----------|--------|
| **I** | Common | -15% | -15% | None | 6x Metal Parts, 1x Wires |
| **II** | Uncommon | -20% | -20% | None | 2x Mechanical Components, 4x Wires |
| **III** | Rare | -25% | -25% | +20% Durability Burn | - |

**Recycles Into**: 5x Metal Parts  
**Compatible Weapons**: Arpeggio, Rattler, Kettle, Burletta, Stitcher, Ferro, Tempest

---

#### Silencer (Reduces Noise)

| Tier | Rarity | Noise Reduction | Trade-off | Value | Recipe |
|------|--------|-----------------|-----------|-------|--------|
| **I** | Uncommon | -20% | None | 2,000 | - |
| **II** | Rare | -40% | None | 5,000 | - |
| **III** | Epic | -60% | +20% Durability Burn | 7,000 | Requires Weapon Bench L3 |

**Compatible Weapons**: Arpeggio, Rattler, Ferro, Anvil, Torrente

---

### Stocks

#### Stable Stock (Faster Recovery)

| Tier | Rarity | Recoil Recovery | Dispersion Recovery | Trade-off | Recipe | Prices |
|------|--------|-----------------|---------------------|-----------|--------|--------|
| **I** | Common | -20% | -20% | None | 7x Rubber Parts, 1x Duct Tape | $1,920 / $640 |
| **II** | Uncommon | -35% | -35% | None | - | - |
| **III** | Rare | -50% | -50% | +20% Equip/Unequip Time | - | - |

**Compatible Weapons**: Rattler, Ferro, Stitcher, Arpeggio, Bettina, Kettle, Il Toro

---

#### Lightweight Stock (Epic - Speed Focus)

| Tier | Rarity | Effects | Trade-offs | Recipe |
|------|--------|---------|------------|--------|
| **-** | Epic | +200% ADS Speed, -30% Equip/Unequip Time | +50% Vertical Recoil, +50% Recoil Recovery Time | 2x Mod Components, 5x Duct Tape |

**Compatible Weapons**: Arpeggio, Kettle, Ferro, Renegade, Hullcracker

---

#### Padded Stock (Epic - Maximum Stability)

| Tier | Rarity | Effects | Trade-offs | Value |
|------|--------|---------|------------|-------|
| **-** | Epic | -15% Vertical Recoil, -15% Horizontal Recoil, -20% Per-Shot Dispersion | +20% Equip/Unequip Time, -30% ADS Speed | 5,000 |

**Compatible Weapons**: Arpeggio, Rattler, Ferro, Renegade, Torrente

---

### Extended Magazines

*Updated December 2025 - stats confirmed via Arc Raiders Wiki*

#### Light Ammo Magazines

| Tier | Recipe | Magazine Bonus | Trader Price | Sell Price |
|------|--------|----------------|--------------|------------|
| **I** | 6x Plastic Parts, 1x Steel Spring | +5 rounds | $1,920 | $640 |
| **II** | 2x Mechanical Components, 3x Steel Spring | +10 rounds | - | - |
| **III** | 1x Mod Component, 6x Steel Spring | +15 rounds | - | - |

**Compatible Weapons**: Stitcher, Kettle, Osprey, Hairpin, Bobcat, Burletta

> **Note**: Extended Light Mag III provides +15 rounds with no drawbacks - top tier DPS attachment.

---

#### Medium Ammo Magazines

| Tier | Recipe | Magazine Bonus | Trader Price | Sell Price |
|------|--------|----------------|--------------|------------|
| **I** | 6x Plastic Parts, 1x Steel Spring | +4 rounds | $1,920 | $640 |
| **II** | 2x Mechanical Components, 3x Steel Spring | +8 rounds | - | - |
| **III** | 1x Mod Component, 6x Steel Spring | +12 rounds | - | - |

**Compatible Weapons**: Arpeggio, Renegade, Venator, Tempest, Torrente, Osprey

> **Note**: Extended Medium Mag III is statistically the best attachment for most medium-ammo weapons.

---

#### Shotgun Magazines

| Tier | Recipe | Magazine Bonus |
|------|--------|----------------|
| **I** | 6x Plastic Parts, 1x Steel Spring | +2 rounds |
| **II** | 2x Mechanical Components, 3x Steel Spring | +4 rounds |
| **III** | 2x Mod Components, 5x Steel Spring | +6 rounds |

**Compatible Weapons**: Il Toro, Vulcano

---

### Shotgun-Specific

#### Shotgun Choke (Reduces Spread)

| Tier | Rarity | Base Dispersion | Trade-off | Recipe |
|------|--------|-----------------|-----------|--------|
| **I** | Common | -10% | None | 6x Metal Parts, 1x Wires |
| **II** | Uncommon | -20% | None | - |
| **III** | Rare | -30% | +20% Durability Burn | - |

**Compatible Weapons**: Il Toro, Vulcano

---

### Special Attachments

#### Extended Barrel (Epic)

| Effect | Trade-off | Value |
|--------|-----------|-------|
| +25% Bullet Velocity | +15% Vertical Recoil | 5,000 |

**Compatible Weapons**: Tempest, Arpeggio, Ferro, Renegade, Anvil

---

## Attachment Trade-off Analysis

### Tier III Trade-offs

Higher tier attachments often come with drawbacks:

| Attachment | Benefit | Cost |
|------------|---------|------|
| Vertical Grip III | -40% Vertical Recoil | -30% ADS Speed |
| Angled Grip III | -40% Horizontal Recoil | -30% ADS Speed |
| Compensator III | -60% Dispersion | +20% Durability Burn |
| Muzzle Brake III | -25% Both Recoils | +20% Durability Burn |
| Silencer III | -60% Noise | +20% Durability Burn |
| Stable Stock III | -50% Recovery Times | +20% Equip/Unequip Time |
| Padded Stock | Maximum Stability | Slower handling |
| Lightweight Stock | Maximum Speed | Much worse recoil |

---

## Recommended Loadout Combinations

### Stealth Build
- **Weapon**: Arpeggio or Rattler
- **Attachments**: Silencer III + Stable Stock II + Extended Mag
- **Trade-off**: +20% Durability Burn, excellent for avoiding ARC aggro

### Maximum Control Build
- **Weapon**: Arpeggio or Tempest
- **Attachments**: Horizontal Grip + Compensator II + Padded Stock
- **Trade-off**: Slower ADS, but nearly no recoil

### Speed/Aggression Build
- **Weapon**: Any SMG/Assault Rifle
- **Attachments**: Lightweight Stock + Muzzle Brake I
- **Trade-off**: More recoil, but very fast weapon swaps

### Shotgun CQC Build
- **Weapon**: Il Toro or Vulcano
- **Attachments**: Shotgun Choke III + Extended Shotgun Mag III
- **Result**: Tighter spread, more shots before reload

### LMG Suppression Build
- **Weapon**: Torrente
- **Attachments**: Vertical Grip II + Compensator II
- **Note**: Crouch for accuracy bonus, high sustained fire

---

## Crafting Material Tiers

| Material | Used In | Where to Find |
|----------|---------|---------------|
| **Plastic Parts** | Tier I attachments | World loot, recycling |
| **Rubber Parts** | Stocks Tier I | World loot, recycling |
| **Metal Parts** | Muzzle devices Tier I | World loot, recycling |
| **Duct Tape** | Grips, Stocks | World loot |
| **Wires** | Compensators, Muzzle Brakes | World loot, recycling |
| **Steel Spring** | Extended Magazines | World loot, recycling |
| **Mechanical Components** | Tier II attachments | Crafting, recycling gear |
| **Mod Components** | Tier III & Epic attachments | Rare loot, high-tier recycling |

---

## Version Information

| Field | Value |
|-------|-------|
| Game | Arc Raiders |
| Version | Tech Test 2 |
| Last Updated | December 2025 |

