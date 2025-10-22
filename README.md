# island
A somewhat customisable optimiser for the Azur Lane island game mode. (Windows only)

## Usage
All the configuration is done via the `config.json` file.
After that, all you need to do is click the executable.

## Configuration
Examples for all possible configs can be found in `config.json`.
- `research` The number of the respective research done. Default: No research done. 6 possible keys:
  - `More Chickens` The number of "More Chickens!" researches you have done, corresponding to a speed increase of 2/5/8/11/15% respectively. Range: [0,5]
  - `More Pigs` The number of "More Pigs!" researches you have done, corresponding to a speed increase of 2/5/10% respectively. Range: [0,3]
  - `More Cows` The number of "More Cows!" researches you have done, corresponding to a speed increase of 2/5/10% respectively. Range: [0,3]
  - `More Sheep` The number of "More Sheep!" researches you have done, corresponding to a speed increase of 2/5/10% respectively. Range: [0,3]
  - `Advanced Mining` 1 if you have researched "Mining Slot Efficiency+", 0 else.
  - `Advanced Logging` 1 if you have researched "Logging Slot Efficiency+", 0 else.
- `collection` The number of collection points you acquired. Default: 0
- `slots` The number of slots you have unlocked. Required for all places:
  - `farm` `[manual slots, auto slots]` The number of manual and auto slots you have unlocked for the farm.
  - `orchard` `[manual slots, auto slots]` The number of manual and auto slots you have unlocked for the orchard.
  - `nursery` `[manual slots, auto slots]` The amount of manual and auto slots you have unlocked for the nursery.
  - `Café Manjuu` The amount of cooking slots you have unlocked for Café Manjuu
  - `Golden Koi Restaurant` The amount of cooking slots you have unlocked for Golden Koi Restaurant
  - `Polar Bear Teahouse` The amount of cooking slots you have unlocked for Polar Bear Teahouse
  - `Manjuu Eatery` The amount of cooking slots you have unlocked for Manjuu Eatery
  - `Fin-'n'-Feather Grill` The amount of cooking slots you have unlocked for Fin-'n'-Feather Grill
  - `mine` The amount of automatic mining slots you have unlocked
  - `forest` The amount of automatic logging slots you have unlocked
  - `Lumber Processing` The amount of manufacturing slots you have unlocked for Lumber Processing
  - `Machinery Production` The amount of manufacturing slots you have unlocked for Industrial Production
  - `Arts & Crafts` The amount of manufacturing slots you have unlocked for Arts & Crafts
  - `Electronic Production` The amount of manufacturing slots you have unlocked for Electronic Production
- `extra daily` `{item name: amount}` Number of daily extra items from found items, transport missions etc. Must be spelt exactly like ingame
  Default: {"Fresh Honey": 4, "Peanuts": 24, "Matsutake": 6, "Autumn Chrysanthemum": 8, "Reed Flowers": 16}
- `daily drinks` Number of energy drinks to be used per day
- `factories` List of the items you have unlocked for production by factory. Required for all factories (Lumber Processing, Machinery Production, Arts & Crafts, Electronic Production)
- `shops` List of shops, their ranks and items you can produce and sell there. Required for all shops
- `not sellable` List of items that can't be sold in the shops. Default: seasonal items
- `production` List of items you can produce by mining and logging. Default: all
- `season pts weight` The priority for the optimiser to maximise season points`. Default 1
- `profit weight` The priority for the optimiser to maximise coin gain. Default 1
  Only the ratio between these 2 matters, so setting both to 10 is the same as keeping them at 1. For numerical stability use numbers in the range [1e-6, 1e6] or 0.
- `ignore cost` Ignore costs when optimising. If `season pts weight` is not 0, this *will* make your profits go negative. Default false
- `characters` List of all the characters you have. Required. Definition is in the form:
  - `character name`: [`level`, `skill level`] Characters at level 10 etc. are automatically assumed to be limit broken
- `books` Number of stat raising books you used by character and stat. Default no books used
- `use books as override` If true, interprets `books` as total stat rather than number of books used. (So you can just put the stat number from ingame instead of calculating the number of books. However this means you need to update it every time the character levels up.) Default false
- `hours per day` How many hours per day you want to allow projects to be run (This limits not just starting times, but also end times.) Default 24
- `number of days` Number of days you wish to simulate. Default 1
- `max project runs per day` Limit the number of times a project is run per day and character. Default 10000
- `time limit` Number of seconds after which the optimiser aborts and prints the current best result. Default 300
- `gap` Relative distance of the best found solution to the best possible solution at which the solver stops optimising. Eg best possible solution is score 100, solver found a solution with score 99, then if gap is 0.01 = 1% or greater the solver quits. Default 0.05
- `print log` Print the optimiser log while it progresses. Default false

Expected runtime with a `gap` of 5% is just a few seconds, the smaller you make this gap the larger the runtime becomes, 1% already often takes about a minute.

## Results
This executable will open a terminal window. If it instantly closes again, an error occured and you should double check whether your config is correct.\
If it works it will print the results in the terminal window in a more or less pretty format.\
It starts by listing the obtained objective value. This is generally profit * `profit weight` + season pts * `season pts weight`, however do not be alarmed if the numbers don't match as it does not count the base points from `extra daily` towards the objective.\
Then comes the amount of energy drinks consumed and by which character.\
Next is a long list of which character did which project how often. A thing to watch out for is that if you see Workerjuu working on the farm, orchard or nursery that actually means manual planting and harvesting rather than actually assigning Workerjuu to do it automatically.\
After that it prints an unsorted summary of time used per place.\
Then it prints a table of item balances with associated season pts you'd make by converting them.\
Penultimately it prints the items sold on average, by which clerk and how many were put on display. If you run multiple days this becomes a bit long and disorganised as it prints one time for every entry put on display and it isn't sorted by days.\
Finally there is the items you need to buy to finance it and the profit calculated from the shop income and the expenses.\

## Remarks
This solver isn't perfect, it cannot properly handle downtime, it assumes you are always awake, ready to instantly start the next project once the first has ended. Configuring a regular check in or sleep time is very difficult to keep performant at the same time so I probably won't do it.\
There is no way I will ever implement special energy drinks even if they make them work at some point.\
Sometimes it will produce some odd things in very low quantity, this can be simply due to it still having some time left that isn't enough to start a properly good project so it fills it with something short instead.\
The more days, items you can produce, characters and shops at gold+ rank you have the slower the solver gets. So far I haven't found a setting that is perticularly slow with the default gap of 0.05, but if you go with the underlying software's default of 0.0001 everything maxed can take an hour for just a single day.\
I think that's it from me, have fun! Oh and if you're feeling adventurous, you can also edit some values in the accompanying json files to see for example what the world would look like if Tofu took only a minute to make. But explaining the structure of those files here would take too long, so if you have questions regarding that, just contact me.
