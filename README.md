Showdown umbreon
A Pokémon battle-bot that can play battles on Pokemon Showdown.

The bot can play single battles in generations 3 through 8.

badge

Python version
Developed and tested using Python 3.8.

Getting Started
Configuration
Environment variables are used for configuration. You may either set these in your environment before running, or populate them in the env file.

The configurations available are:

![image](https://github.com/ididoopsy123/Pokemon-showdown-ai/assets/87253312/52940924-860c-4230-b99b-8500840f64ee)


Running without Docker
1. Clone

Clone the repository with git clone https://github.com/pmariglia/showdown.git

2. Install Requirements

Install the requirements with pip install -r requirements.txt.

3. Configure your env file

Here is a sample:

BATTLE_BOT=safest
WEBSOCKET_URI=sim.smogon.com:8000
PS_USERNAME=MyUsername
PS_PASSWORD=MyPassword
BOT_MODE=SEARCH_LADDER
POKEMON_MODE=gen7randombattle
RUN_COUNT=1
4. Run

Run with python run.py

Running with Docker
This requires Docker 17.06 or higher.

1. Clone the repository

git clone https://github.com/pmariglia/showdown.git

2. Build the Docker image

docker build . -t showdown

3. Run with an environment variable file

docker run --env-file env showdown

Battle Bots
This project has a few different battle bot implementations. Each of these battle bots use a different method to determine which move to use.

Safest
use BATTLE_BOT=safest

The bot searches through the game-tree for two turns and selects the move that minimizes the possible loss for a turn.

For decisions with random outcomes a weighted average is taken for all possible end states. For example: If using draco meteor versus some arbitrary other move results in a score of 1000 if it hits (90%) and a score of 900 if it misses (10%), the overall score for using draco meteor is (0.9 * 1000) + (0.1 * 900) = 990.

This is equivalent to the Expectiminimax strategy.

This decision type is deterministic - the bot will always make the same move given the same situation again.

Nash-Equilibrium (experimental)
use BATTLE_BOT=nash_equilibrium

Using the information it has, plus some assumptions about the opponent, the bot will attempt to calculate the Nash-Equilibrium with the highest payoff and select a move from that distribution.

The Nash Equilibrium is calculated using command-line tools provided by the Gambit project. This decision method should only be used when running with Docker and will fail otherwise.

This decision method is not deterministic. The bot may make a different move if presented with the same situation again.

Team Datasets (experimental)
use BATTLE_BOT=team_datasets

Using a file of sets & teams, this battle-bot is meant to have a better understanding of Pokeon sets that may appear. Populate this dataset by editing data/team_datasets.json.

Still uses the safest decision making method for picking a move, but in theory the knowledge of sets should result in better decision making.

Most Damage
use BATTLE_BOT=most_damage

Selects the move that will do the most damage to the opponent

Does not switch

Write your own bot
Create a package in showdown/battle_bots with a module named main.py. In this module, create a class named BattleBot, override the Battle class, and implement your own find_best_move function.

Set the BATTLE_BOT environment variable to the name of your package and your function will be called each time PokemonShowdown prompts the bot for a move

The Battle Engine
The bots in the project all use a Pokemon battle engine to determine all possible transpositions that may occur from a pair of moves.

For more information, see ENGINE.md

Specifying Teams
You can specify teams by setting the TEAM_NAME environment variable. Examples can be found in teams/teams/.

Passing in a directory will cause a random team to be selected from that directory.

The path specified should be relative to teams/teams/.

Examples
Specify a file:

TEAM_NAME=gen8/ou/clef_sand
Specify a directory:

TEAM_NAME=gen8/ou
