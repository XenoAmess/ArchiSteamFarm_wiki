ASF supports variety of commands, which can be used to control behaviour of the process and bot instances.

Below commands can be sent to the bot through three different ways:
- Through steam private chat
- Through steam group chat
- Through **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**

Keep in mind that ASF interaction requires from you to be eligible for the command according to ASF permissions. Check out `SteamUserPermissions` and `SteamOwnerID` config properties for more details.

---

### Steam private chat

Definitely the easiest method to interact with ASF - simply execute command to ASF bot that is currently running in ASF process.

![Screenshot](http://i.imgur.com/PPxx7qV.png)

---

### Steam group chat

Very similar to above, but this time on group chat of given Steam group. Keep in mind that this option requires either `SteamMasterClanID` properly set, or you inviting your bot to chat manually.

---

### WCF

Probably the most "complex" method of calling ASF, perfect for third-party tools or scripting, requires ASF to be run in server mode, and a client executing command (such as another ASF process).

![Screenshot](http://i.imgur.com/VinFVjl.png)

---

## Commands

| Command | Access | Description |
| ------- | ------ |-------------|
`!2fa` | `Master` | Generates temporary **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** token for current bot instance
`!2fa <Bots>` | `Master` | Generates temporary **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** token for given bot instances
`!2fano` | `Master` | Denies all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** confirmations for current bot instance
`!2fano <Bots>` | `Master` | Denies all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** confirmations for given bot instances
`!2faok` | `Master` | Accepts all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** confirmations for current bot instance
`!2faok <Bots>` | `Master` | Accepts all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** confirmations for given bot instances
`!addlicense <gameIDs>` | `Operator` | Activates given ```appIDs``` (Steam Network) or ```subIDs``` (Steam Store) on current bot instance (free games only) | `FamilySharing` | ```!addlicense 440,570```
`!addlicense <Bots> <gameIDs>` | `Operator` | Activates given ```appIDs``` (Steam Network) or ```subIDs``` (Steam Store) on given bot instances (free games only)
`!api` | `Master` | Returns ASF **[API](https://github.com/JustArchi/ArchiSteamFarm/wiki/API)** response in JSON, for current bot instance.
`!api <Bots>` | `Master` | Returns ASF **[API](https://github.com/JustArchi/ArchiSteamFarm/wiki/API)** response in JSON, for given bot instances.
`!bl` | `Master` | Lists blacklisted users from trading module of current bot instance.
`!bl <Bots>` | `Master` | Lists blacklisted users from trading module of given bot instances.
`!bladd <steamID64>` | `Master` | Blacklists given `steamID` from trading module of current bot instance.
`!bladd <Bots> <steamID64>` | `Master` | Blacklists given `steamID` from trading module of given bot instances.
`!blrm <steamID64>` | `Master` | Removes blacklist of given `steamID` from trading module of current bot instance.
`!blrm <Bots> <steamID64>` | `Master` | Removes blacklist of given `steamID` from trading module of given bot instances.
`!exit` | `Owner` | Stops whole ASF
`!farm` | `Master` | Restarts cards farming module for current bot instance
`!farm <Bots>` | `Master` | Restarts cards farming module for given bot instances
`!help` | `FamilySharing` | Shows help (link to this page)
`!input <Type> <Value>` | `Master` | Sets given input type to given value for current bot instance, works only in ```Headless``` mode - further explained **[below](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands#input-command)**
`!input <Bots> <Type> <Value>` | `Master` | Sets given input type to given value for given bot instances, works only in ```Headless``` mode - further explained **[below](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands#input-command)**
`!leave` | `Master` | Makes bot leave the current group chat. For obvious reasons, this command works only in group chats
`!loot` | `Master` | Sends all booster packs and Steam trading cards (including foils if ```IsBotAccount```) of current bot instance to first `Master` defined in its bot config.
`!loot <Bots>` | `Master` | Sends all booster packs and Steam trading cards (including foils if ```IsBotAccount```) of given bot instances to first `Master` defined in their bot configs.
`!nickname <nickname>` | `Master` | Changes Steam nickname of current bot instance to `<nickname>`.
`!nickname <Bots> <nickname>` | `Master` | Changes Steam nickname of given bot instances to `<nickname>`.
`!owns <appIDsOrGameNames>` | `Operator` | Checks if current bot instance already owns given ```appIDs``` and/or ```gameNames``` (can be part of the game's name). It can also be `*` to show all games available. | `FamilySharing` | ```!owns 440,570```, ```!owns 440,dota```, ```!owns roach```, ```!owns *```
`!owns <Bots> <appIDsOrGameNames>` | `Operator` | Checks if given bot instances already own given ```appIDs``` and/or ```gameNames``` (can be part of the game's name). It can also be `*` to show all games available.
`!password` | `Master` | Prints encrypted password of current bot instance (in use with ```PasswordFormat```)
`!password <Bots>` | `Master` | Prints encrypted password of given bot instances (in use with ```PasswordFormat```)
`!pause` | `Operator` | Permanently pauses automatic cards farming module of current bot instance. ASF will not attempt to farm current account in this session, unless you manually ```!resume``` it, or restart the process. Also called sticky pause.
`!pause <Bots>` | `Operator` | Permanently pauses automatic cards farming module of given bot instances. ASF will not attempt to farm current account in this session, unless you manually ```!resume``` it, or restart the process. Also called sticky pause.
`!pause~` | `FamilySharing` | Temporarily pauses automatic cards farming module of current bot instance. Farming will be automatically resumed on the next playing event, or bot disconnect. You can ```!resume``` farming to unpause it.
`!pause~ <Bots>` | `FamilySharing` | Temporarily pauses automatic cards farming module of given bot instances. Farming will be automatically resumed on the next playing event, or bot disconnect. You can ```!resume``` farming to unpause it.
`!play <appIDs>` | `Master` | Switches to manual farming - launches given ```appIDs``` on current bot instance. Use ```!resume``` for returning to automatic farming | `FamilySharing` | ```!play 440,570```, ```!play 570```
`!play <Bots> <appIDs>` | `Master` | Switches to manual farming - launches given ```appIDs``` on given bot instances. Use ```!resume``` for returning to automatic farming
`!redeem <Keys>` | `Operator` | Redeems given ```cd-keys``` on current bot instance
`!redeem <Bots> <Keys>` | `Operator` | Redeems given ```cd-keys``` on given bot instances
`!redeem^ <Keys>` | `Operator` | Redeems given ```cd-keys``` on current bot instance, never forwards keys to other bots (like ```RedeemingPreferences``` of ```None```)
`!redeem^ <Bots> <Keys>` | `Operator` | Redeems given ```cd-keys``` on given bot instances, never forwards keys to other bots (like ```RedeemingPreferences``` of ```None```)
`!redeem& <Keys>` | `Operator` | Redeems given ```cd-keys``` on **any** bot instance **apart from** current one (enforces one-time ```Forwarding``` in ```RedeemingPreferences```, even if it's not enabled)
`!redeem& <Bots> <Keys>` | `Operator` | Redeems given ```cd-keys``` on **any** bot instance **apart from** given one (enforces one-time ```Forwarding``` in ```RedeemingPreferences```, even if it's not enabled)
`!rejoinchat` | `Operator` | Forces current bot instance to rejoin its ```SteamMasterClanID``` groupchat
`!rejoinchat <Bots>` | `Operator` | Forces given bot instances to rejoin their ```SteamMasterClanID``` groupchat
`!restart` | `Owner` | Restarts ASF process
`!resume` | `FamilySharing` | Resumes automatic farming of current bot instance. Also see ```!pause```, ```!play```
`!resume <Bots>` | `FamilySharing` | Resumes automatic farming of given bot instances. Also see ```!pause```, ```!play```
`!start <Bots>` | `Master` | Starts given bot instances
`!stats` | `Owner` | Prints process statistics, such as managed memory usage
`!status` | `FamilySharing` | Prints status of current bot instance
`!status <Bots>` | `FamilySharing` | Prints status of given bot instances
`!stop` | `Master` | Stops current bot instance
`!stop <Bots>` | `Master` | Stops given bot instances
`!update` | `Owner` | Checks GitHub for ASF updates (this is done automatically every 24 hours if ```AutoUpdates```)
`!version` | `FamilySharing` | Prints version of ASF

---

### Notes

All commands are case-insensitive, but their arguments (such as bot names) are usually case-sensitive.

**Access** of the command defines **minimum** `EPermission` of `SteamUserPermissions` that is required to use the command, with an exception of `Owner` which is `SteamOwnerID` defined in global configuration file (and highest permission available).

Plural arguments, such as ```<Bots>``` or ```<appIDs>``` mean that command supports multiple arguments of given type, separated by a comma. For example, ```!status <Bots>``` can be used as ```!status MyBot,MyOtherBot,Primary```. This will cause given command to be executed on **all targeted bots** in the same way as you'd send `!status` to each bot in a separate chat window. Please notice that there is no space after `,` - it's simply a delimiter for a single string.

In addition to that, there is a special ```ASF``` keyword which acts as "all bots in the process", so ```!status ASF``` is equal to ```!status all,your,bots,listed,here```.

It's nice to note that ```<Bots>``` also supports special "range" syntax, which allows you to choose a range of bots more easily. The general syntax for ```<Bots>``` in this case is ```firstBot..lastBot```. For example, if you have bots named ```A, B, C, D, E, F```, you can execute ```!status B..E```, which is equal to ```!status B,C,D,E``` in this case. When using this syntax, ASF will use alphabetical sorting in order to determine which bots are in your specified range. Both ```firstBot``` and ```lastBot``` must be valid bot names recognized by ASF, otherwise range syntax is entirely skipped.

---

Please note that sending a command to the group chat acts like a relay - if you're saying ```!redeem X``` to 3 of your bots sitting together with you on the group chat, it'll result in the same as you'd say ```!redeem X``` to every single one of them privately. In most cases **this is not what you want**, and instead you should use ```given bot``` command that is being sent to a single bot privately. ASF supports group chat, as in many cases it can be useful source for communication with it, but you should almost never execute any command on the group chat if there are 2 or more ASF bots sitting there, unless you fully understand ASF behaviour written here and you in fact want to relay the same command to every single bot that is listening to you.

*And even in this case you should use `<Bots>` syntax instead.*

---

Some commands are also available with their aliases, to save you on typing:

| Command     | Alias |
|-------------|-------|
`!owns ASF`   | `!oa` |
`!status ASF` | `!sa` |
`!redeem`     | `!r`  |
`!redeem^`    | `!r^` |
`!redeem&`    | `!r&` |

---

It's not required to have any extra account for executing commands though Steam chat - you can create a group, set ```SteamMasterClanID``` properly to that newly created group, then give yourself access either through `SteamOwnerID` or `SteamUserPermissions` of your own bot. This way ASF bot (you) will join group and chat of your selected group, and listen to commands from your own account. You can join the same group chatroom in order to issue commands to yourself (as you'll be sending command to chatroom, and ASF instance sitting on the same chatroom will receive them, even if it shows only as your account being there). Apart from that, you can also use **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**, but chatroom way is much easier, and if you have access to some alt account, then using that instead is even easier.

---

When using **WCF**, keep in mind that:
- Commands don't have to be prefixed by ```!```, ASF prefixes them for you if needed (useful on Unix)
- When using commands that are based on ```current bot instance```, ASF will choose **any** of currently enabled bots, therefore it's highly recommended to use ```given bot instances``` commands instead.

---

## ```!input``` command

Input command can be used only in ```Headless``` mode, for inputting given data via WCF or Steam chat when ASF is running without support for user interaction.

General syntax is ```!input <Bots> <Type> <Value>```.

```<Type>``` is case-insensitive and defines input type recognized by ASF. Currently ASF recognizes following types:

| Type                    | Description                                                                |
| ----------------------- | -------------------------------------------------------------------------- |
| DeviceID                | 2FA device identificator, if missing from ```.maFile```.                   |
| Login                   | ```SteamLogin``` bot config property, if missing from config.              |
| Password                | ```SteamPassword``` bot config property, if missing from config.           |
| SteamGuard              | Auth code sent on your e-mail if you're not using 2FA.                     |
| SteamParentalPIN        | ```SteamParentalPIN``` bot config property, if missing from config.        |
| TwoFactorAuthentication | 2FA token generated from your mobile, if you're using 2FA but not ASF 2FA. |

```<Value>``` is value set for given type. Currently all values are strings.

---

### Example

Let's say that we have a bot that is protected by SteamGuard in non-2FA mode. We want to launch that bot with ```Headless``` set to true.

In order to do that, we need to execute following commands:

```!start MySteamGuardBot``` -> Bot will attempt to log in, fail due to AuthCode needed, then stop due to running in ```Headless``` mode.

```!input MySteamGuardBot SteamGuard ABCDE``` -> We set ```SteamGuard``` input of ```MySteamGuardBot``` bot to ```ABCDE```. Of course, ```ABCDE``` in this case is auth code that we got on our e-mail.

```!start MySteamGuardBot``` -> We start our (stopped) bot again, this time it automatically uses auth code that we set in previous command, properly logging in, then clearing it.

In the same way we can access 2FA-protected bots (if they're not using ASF 2FA), as well as setting other required properties during runtime.