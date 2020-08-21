# PUBG Web API wrapper for node.js
- [Full Documentation (Source)](https://documentation.playbattlegrounds.com)
- [Installation](#install)
- [To Do](#todo)
- [Usage](#usage)
- [Description](#description)
- [Examples](https://github.com/jylescoad-ward/node-battlegrounds-revisited/tree/master/examples)
- [Classes](#classes)
- - [API](#apiapikey-platform)
- - [Player](#player)
- - [Match](#match)
- - [Participant](#participant)
- - [Roster](#roster)
- - [Asset](#asset)
- - [Season](#season)
- - [PlayerSeason](#playerseason)

# Todo
- Telemetry
- - Object class
- - Applied Player class
- Spectators class (not yet introduced in API)
- Rounds class (not yet introduced in API)
- [Tournaments](https://documentation.pubg.com/en/tournaments-endpoint.html)

# Install
Install via npm:
```
npm install battlegrounds-revisited
```


# Usage
Requiring a module returns [API](#api) class:
```JS
const battlegrounds = require('battlegrounds-revisited')

const api = new battlegrounds(APIKEY)

// Get a player
const res = await api.getPlayers({ names: ['shroud'] })
```


# Description
The main focus of this module is it's ease of use.

For example, getting a player returns its class that contains other properties methods of which can be used:
```JS
const res = await api.getPlayers({ names: ['shroud'] })

// Is a member of Player class
const player = res[0]

// Get first match. Match class is returned
const match = player.matches[0].get()
```


# Classes
List of all classes returned by a module

## API(apikey[, platform])
Is returned by module when required

- **apikey** - your apikey (you can get it [here](https://developer.playbattlegrounds.com))
- **platform** (optional) - default platform id to be included as a parameter in every method. Full list [here](https://github.com/EpicKitten/PUBG-Resources/wiki/API-Shards))

### API.getSamples(params)
Returns a [Sample](#sample) class

- **date** (optional) - an instance of Date class. Has to be at least 24hrs in the past. Default value is 24 hours ago.

### API.getMatch(params)
Returns a [Match](#match) class

- **id** - [Match](#match) id

**Note**: currently it is not possible to get custom Matches.

### API.getPlayers(params)
Returns a list of [Player](#player) classes

- **ids** (optional) - An array of [Player](#player) ids
- **names** (optional) - An array of [Player](#player) names

**Note**: at least one of those parameters have to be provided, in other cases "MissingParameter" error will be thrown

### API.getPlayer(params)
Returns a [Player](#player) class

- **id** - [Player](#player) id

### API.getSeasons(params)
Returns a list of [Season](#season) classes

### API.getPlayerSeason(params)
Returns a [PlayerSeason](#playerseason) classes

- **id** - [Player](#player) id
- **season_id** - [Season](#season) id

## Player
Represents a Player. If only contains its **id**, [get](#playerget) method has to be called to get full info about itself.

- **id** - Player id. Has a format of ```account.x``` where x - 32 char hex
- **attributes**
- - **Name** - Player name
- - **shardId** - platform id (full list [here](https://github.com/EpicKitten/PUBG-Resources/wiki/API-Shards)))
- - **patchVersion**
- - **titleId**
- **matches** - An array of Player [Matches](#match)
- **assets** - An array of Player [Assets](#asset)

### Player.get()
Calls an API to get Player's full info and returns itself.

## Match
Represents a Match. If only contains its **id**, **get** method has to be called to get full info about itself.

- **id** - Match id. Has a format of ```a-b-b-b-c``` where a - 8 char hex, b - 4 char hex, c - 12 char hex
- **attributes**
- - **isCustomMatch**
- - **createdAt** - date when the object was created
- - **duration** - duration time in seconds
- - **gameMode** - game mode (for ex duo-fpp, squad-tpp, etc)
- - **patchVersion**
- - **shardId** - platform id (full list [here](https://github.com/EpicKitten/PUBG-Resources/wiki/API-Shards))
- - **stats**
- - **tags**
- - **titleId** - tournament title. For regular matches will be ```bluehole-pubg```
- **rosters** - An array of [Rosters](#roster)
- **assets** - An array of [Assets](#asset)
- **rounds** - An array of Rounds
- **spectators** - An array of Spectators
- **participants** - An array of [Participants](#participant)

### Match.get()
Calls an API to get Match's full info and returns itself.

## Participant
Represents a [Match](#match) Participant.

- **id** - Participant id. Has a format of ```a-b-b-b-c``` where a - 8 char hex, b - 4 char hex, c - 12 char hex
- **attributes**
- - **actor**
- - **shardId** - platform id (full list [here](https://github.com/EpicKitten/PUBG-Resources/wiki/API-Shards)))
- - **stats**
- - - **swimDistance**
- - - **DBNOs**
- - - **assists** - assists
- - - **boosts** - boosts used
- - - **damageDealt** - damage dealt to others
- - - **deathType** - death type. Can be ```byplayer``` or ```suicide```
- - - **headshotKills** - headshot kills
- - - **heals** - heals
- - - **killPlace** - place by kills
- - - **killPoints** - kill points
- - - **killPointsDelta**
- - - **killStreaks**
- - - **kills** - kills
- - - **lastKillPoints**
- - - **lastWinPoints**
- - - **longestKill**
- - - **mostDamage**
- - - **name** - [Player](#player) name
- - - **playerId** - [Player](#player) id. Has a format of ```account.x``` where x - 32 char hex
- - - **revives** - revives
- - - **rideDistance** - distance driven on a vehicle
- - - **roadKills** - kills by vehicle
- - - **teamKills** - team kills
- - - **timeSurvived** - total time survived in seconds
- - - **vehicleDestroys** - vehicles destroyed
- - - **walkDistance** - distance a [Player](#player) has walked
- - - **weaponsAcquired**
- - - **winPlace** - win place
- - - **winPoints** - total win points
- - - **winPointsDelta**

## Roster
Represents a team of [Participants](#participant) in a [Match](#match)

- **id** - Roster id. Has a format of ```a-b-b-b-c``` where a - 8 char hex, b - 4 char hex, c - 12 char hex
- **attributes**
- - **shardId** - platform id (full list [here](https://github.com/EpicKitten/PUBG-Resources/wiki/API-Shards)))
- - **won** - Boolean. Whenever Roster won or not
- - **stats**
- - - **rank** - Roster rank
- - - **teamId** - Roster team number in [Match](#match) in order of joining
- **participants** - Roster [Participants](#participant)

## Asset
Represents an Asset with an attached resource

- **id** - Asset id. Has a format of ```a-b-b-b-c``` where a - 8 char hex, b - 4 char hex, c - 12 char hex
- **attributes**
- - **URL** - resource url. You can download it using [fetch](#assetfetch) method
- - **createdAt** - date when the object was created
- - **description**
- - **name** - for [Match](#match) Assets will be ```telemetry```

## Asset.fetch()
Fetches Asset content from **URL**. Returns different objects depending on type (**name**) of an Asset:

- [Telemetry](https://documentation.playbattlegrounds.com/en/telemetry.html)

## Sample
Represents a list of [Matches](#match)

- **id** - Asset id. Has a format of ```a-b-b-b-c``` where a - 8 char hex, b - 4 char hex, c - 12 char hex
- **attributes**
- - **createdAt** - date when the object was created
- - **shardId** - platform id (full list [here](https://github.com/EpicKitten/PUBG-Resources/wiki/API-Shards)))
- - **titleId** - tournament title. For regular matches will be ```bluehole-pubg```
- **matches** - an array of [Matches](#match)

## Season
Represents a game Season

- **id** - Asset id. Has a format of ```a-b-b-b-c``` where a - 8 char hex, b - 4 char hex, c - 12 char hex
- **attributes**
- - **isCurrentSeason** - boolean that indicates if season is active or not
- - **isOffseason** - boolean, true if season is not active

## Season.get()
Queries all [Seasons](#season) and finds needed one by **id**. Returns updated [Season](#season) class

## PlayerSeason
Represents [Player](#player) [Season](#season) stats

- **attributes**
- - **assists** -	Number of enemy players this player damaged that were killed by teammates
- - **boosts** - Number of boost items used
- - **dBNOs** -	Number of enemy players knocked
- - **dailyKills**
- - **damageDealt** -	Total damage dealt. Note: Self inflicted damage is subtracted
- - **days**
- - **headshotKills** - Number of enemy players killed with headshots
- - **heals** -	Number of healing items used
- - **killPoints** - Points based on how many kills this player had
- - **kills** -	Number of enemy players killed
- - **longestKill**
- - **longestTimeSurvived** -	Longest time survived in a match
- - **losses** - Number of matches lost
- - **maxKillStreaks** -
- - **mostSurvivalTime** - Longest time survived in a match
- - **revives** - Number of times this player revived teammates
- - **rideDistance** - Total distance traveled in vehicles measured in meters
- - **roadKills** -	Number of kills while in a vehicle
- - **roundMostKills** - Highest number of kills in a single match
- - **roundsPlayed** - Number of matches played
- - **suicides** - Number of self-inflicted deaths
- - **teamKills** -	Number of times this player killed a teammate
- - **timeSurvived** - Total time survived
- - **top10s** - Number of times this player made it to the top 10 in a match
- - **vehicleDestroys** -	Number of vehicles destroyed
- - **walkDistance** - Total distance traveled on foot measured in meters
- - **weaponsAcquired** -	Number of weapons picked up
- - **weeklyKills**
- - **winPoints**
- - **wins** - Number of matches won
- **matchesSolo** - solo tpp [Match](#match) array
- **matchesSoloFPP** - solo fpp [Match](#match) array
- **matchesDuo** - duo tpp [Match](#match) array
- **matchesDuoFPP** - duo fpp [Match](#match) array
- **matchesSquad** - squad tpp [Match](#match) array
- **matchesSquadFPP** - squad fpp [Match](#match) array
- **season** - [Season](#season)
- **player** - [Player](#player)
