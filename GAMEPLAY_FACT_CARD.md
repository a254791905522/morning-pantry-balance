# Morning Pantry Balance - Gameplay Fact Card

## App Identity
- **App Name**: Morning Pantry Balance
- **Bundle ID**: com.rosewood.morningpantrybalance.game
- **Version**: 1.0
- **Platform**: iOS (iPhone only)
- **Minimum iOS**: 15.0
- **Framework**: SpriteKit + Objective-C
- **Orientation**: Portrait only
- **Age Rating**: 4+
- **Network**: Fully offline, no ads, no IAP, no analytics, no tracking

## Core Gameplay Loop
1. Open a level: a week of seven breakfast days is initialized with a fixed pantry of 14 food items.
2. For each day (Day 1 through Day 7), the game calculates a target calorie range and macro (carbs/protein/fat) targets from a deterministic plan.
3. Tap food cards from the pantry to add items to the plate. Each tap consumes one stock of that food and updates the current calories and macros.
4. Use Undo to remove the last item, or Clear to reset the plate, before confirming.
5. Tap Confirm to submit the day. The game checks calories within tolerance and macros within macro tolerance.
6. If the day passes, the game also checks that the remaining pantry can still complete the remaining days. If not, the week fails.
7. Complete all seven days to clear the level and unlock the next one.

## Food Items (14 total)
| Food | Calories | Carbs (g) | Protein (g) | Fat (g) |
|------|----------|-----------|-------------|---------|
| Egg | 78 | 1 | 6 | 5 |
| Toast | 80 | 15 | 3 | 1 |
| Milk | 150 | 12 | 8 | 8 |
| Oats | 150 | 27 | 5 | 3 |
| Yogurt | 100 | 12 | 9 | 3 |
| Bacon | 120 | 0 | 9 | 10 |
| Banana | 105 | 27 | 1 | 0 |
| Apple | 95 | 25 | 0 | 0 |
| Strawberry | 50 | 12 | 1 | 0 |
| Blueberry | 85 | 21 | 1 | 0 |
| Orange | 62 | 15 | 1 | 0 |
| Kiwi | 61 | 15 | 1 | 1 |
| Grapes | 69 | 18 | 1 | 0 |
| Pear | 101 | 27 | 1 | 0 |

The first six items (Egg through Bacon) are pantry staples available every day. The remaining eight are fruits; only a rotating subset of two or three fruits is available each day.

## Level Progression
- **Total levels**: 50 (each level is one 7-day week)
- **Tutorial levels**: Levels 1-3. The tutorial highlights the next food card to tap with a pulsing green ring and shows a prompt banner with the next food name.
- **Event levels**: Levels 4+. Daily events (Gym Workout, Soccer Practice, Weekend Hike, Long Bike Ride, Moving Day, Stage Performance, Doctor Checkup, Holiday Dinner) may raise or lower the day's calorie target by 30-50%. Event chance scales with level: 15% at levels 4-10, 25% at 11-20, 40% at 21-35, 55% at 36+.
- **Tolerance**: Calorie tolerance = max(18, 60 - level). Macro tolerance = max(3, 9 - level/8). Higher levels require tighter matching.
- **Extra stock**: Levels get less bonus stock as they progress (extra = max(0, 8 - (level-1)/6)).

## Scoring System
- A day is complete when total calories fall within [target - tolerance, target + tolerance] AND carbs, protein, and fat each fall within their macro tolerance.
- If calories are outside the range, the game shows "too light" or "too heavy" feedback.
- If calories pass but macros are off, the game indicates which macro is short or over.
- After a day passes, the game runs a feasibility check: a recursive search verifies that the remaining pantry can satisfy the remaining days' targets. If not, the week fails with "The remaining stock cannot cover the rest of the week."
- Completing all 7 days unlocks the next level (stored in NSUserDefaults key "MorningPantryBalanceProgress").

## Mechanics
- **Pantry sharing**: The pantry is shared across all 7 days. Once an item is used, it is gone for the week.
- **Fruit rotation**: Fruits available each day rotate based on level and day index, so the player cannot rely on the same fruit every day.
- **Daily events**: From level 4, events modify the planned counts (adding or removing an item) and show a prompt describing the event and its effect.
- **Undo/Clear**: Players can undo the last item or clear the entire plate before confirming. Stock is restored on undo/clear.
- **Tutorial prompts**: Levels 1-3 show a pulsing green ring around the next food card to tap, plus a banner with the food name. Tapping the wrong card shows a correction prompt.

## Settings
- Music toggle (NSUserDefaults key "MorningPantryBalanceMusic")
- Sound Effects toggle (NSUserDefaults key "MorningPantryBalanceSound")
- Haptics toggle (NSUserDefaults key "MorningPantryBalanceHaptics")
- All settings default to ON and persist locally.

## Monetization
- None. No ads, no in-app purchases, no subscriptions.

## Data Collection
- None. All progress and settings saved locally via NSUserDefaults. No network requests, no analytics, no tracking.

## Template Residue Check
- No tw_*, game_01_*, Template, or other template residue found. The variable names `baseTemplates` and `dailyEventTemplates` in BPGameScene.m are gameplay logic, not template assets.

## Debug UI
- showsFPS = NO
- showsNodeCount = NO
