[LBots.org]: https://lbots.org
[Discord]: https://discord.gg/EKv9k6p

[Discord Developer Panel]: https://discordapp.com/developers

[Best Practices for Discord Bots]: https://github.com/meew0/discord-bot-best-practices

# Common issues
This are common issues most users come accross when they either make a bot, or make a own Discord Server.  
The goal of this file is to provide ideas and solution on how to fix and/or prevent those issues.

## Terms
This file will use certain terms that aren't known to everyone.

| Term: | Meaning                                   |
| ----- | ----------------------------------------- |
| Guild | The Discord Server a Bot or Member is on. |

## Bots
There are many bots on Discord and each day, new ones get added.  
While this itself isn't a issue, the fact that there are often the same problems across multiple bots can become a tedious process to filter out.
This page lists common issues and how they can/should be prevented.

### Responding to other bots/itself
Your bot shouldn't respond to other bots or (even worse) to itself.  
Your bot and someone else's bot could get caught in an infinite loop and cause spam to a server, resulting in them being muted, kicked or even banned.
This will gain you and your bot a bad reputation, so make sure it only replies to users for commands.

### Unknown command message
Don't add a "Unknown command, type `xxx` for help" message to your bot.  
If someone types `!text` and your bot doesn't have `text` as a command, let it stay silent and not respond to it.
The exception is, if the bot is clearly mentioned (`@SomeBot text`) or you are providing a "not found" error as a part of your help command (`!help text` is not found since `text` is not one of the bot's commands, so an error is okay.)

### Not "conversation safe"
There are bots that aren't "Conversation safe", meaning that they will respond when their command is mentioned somewhere in a normal message/conversation. For example, if a bot has the prefix `help` and someone says "I need help please", the bot might be triggered and respond with a "Unknown command" error. This will frustrate the users in the conversation.  
Make your bot only reply if the message clearly is a command (Starts with the set prefix/mention of the bot).

### Staying silent when a command fails
When your bot can't perform a command, because it misses permissions or because it had an error occur, just let it say in the chat and don't let it fail silently.  

Imagine the following scenario: A staff member tries to run `!ban @SomeUser#1234`, but this requires the member to have `ban member` permissions, which they lack.  
When the bot now doesn't respond and the member isn't banned, the staff member might think one of the following things:
1. The bot is down/offline.
2. The bot is broken.
3. The command actually isn't available, even tho it is mentioned.

Don't let your bot stay silent when the member (or the bot itself) lacks certain permissions.  
Just let it respond with something like "You/I miss the `<permission>` permission, to execute this!" so that the members know that there is something missing.  
The same applies for when the bot actually has an error happening while executing a command. Let the member know that there was an error and that they perhaps should let the dev know about it.

### Asking for Admin-permissions in the OAuth invite
You shouldn't let your bots invite link ask for Administrator (Permission ID 8), since this is not only just a lazy move to do, but also opens a door for a potential threat that could damage servers, users, your bot, and even worse, your very own reputation.  
If someone manages to get access to your bot (through a token leak or perhaps by gaining access to your bot's VPS or your account) they can do all sorts of stuff such as mass-banning people.  
So, please take your time and make a proper invite with the right permissions. Only bad devs use the admin-perm.

P.S. Discord even offers a OAuth link generator under https://discordapp.com/developers/applications/:id/oauth (Replace `:id` with your bot's ID)  
Just select `Bot` under Scopes and the required permissions and use the generated URL.

### Having the bot token public
This seems to be an obvious thing, but it's still surprising how many people still make this mistake.  
You should **never** share your bot's token with other people, nor have it publicly visible somewhere (like on a GitHub repository).  
The token allows others to gain access to your bot and abuse it in many ways.

When you have the feeling that someone might got access to the token, go to your bot's application-page on Discord and imidiately regenerate the token!

### Not limiting API requests (API-abuse, getting rate-limited)
Most if not all APIs have a ratelimit (how often you can access it per time-interval) to prevent spam and Discord's API is no exception.  
Most Libraries/Wrappers already have an inbuild system, to account for the ratelimit and do certain actions if the ratelimit is reached. In most cases is that just delaying the requests for a certain amount of time. (Examples are JDA (Java) and Discord.js (Javascript)).  
But not every library has that, or the developer isn't aware of it.

Please make sure your bot respects the ratelimit of Discord, so that they don't have to block or even ban your bot's IP-adress from their API.

#### What can be seen as API-abuse?
There are a lot of things that can be seen as API-abuse. The general rule is that if it exceeds the ratelimit it is counted as API-abuse.  
Here are some of the most commonly known cases of API-abuse (which not always cause rate-limits):

##### Disco-Roles
"Disco-Roles" (Also known as Rainbow-Roles) are roles the bot updates the colour of all the time in a fast manner.  
This is API-abuse in that it causes a lot of Role-Updates and therefor a lot of Audit-Log entries.

This is in most if not all botlists a reason for an instant-deny.

##### Sending DMs to all members of a Guild (Mass-DM)
Sending every member of a Guild or even all Members the bot knows a DM is considered Mass-DM and is not allowed.  
Only send a DM to a user if they clearly asked for it (e.g. executed a Command).

##### Fast Status-Refresh
Discord allows your bot to update their Playing-Status quite often. However this doesn't mean you can update the status of your bot every 2 seconds.  
You're allowed to update the Status of your bot **5 times per minute** (Every 12 seconds). Everything above that is considered abuse.

#### What could happen to you and your bot?
If your bot does one or even all of those listed things, remove those since it only creates the risk of your Bot's IP being banned from Discord's API (And it will also most likely make your bot being denied from any professional Bot-list).

### Open Eval/Dev only commands
Having an Eval-command is (almost) a essential part of a dev. The eval-command allows them to test/run parts of code to see what the output could be, without the need of adding that code to the bot, compile it, restart the bot and test it.  
But this can also be a potential threat, when it isn't secured properly.  
The eval command could f.e. allow people to change the bot's avatar, name, game or even shut it down completely. This is annoying and just makes things worse.  
That's why you should keep the Eval-command for yourself only and not have it public. This also applies for any other command that might change the bot in a way you don't want other people be able to do, like a shutdown/reboot command.

If you really want people to use the eval command to for example, evaluate math expressions, restrict it so that potential harmfull code can't be run by normal users.

----
## Discord
Discord allows everyone to create a own Server for free without any ads or restrictions.  
The issue there is, that many users make the same mistakes over and over again without realising it.  
This part lists some if not all known issues and mistakes people make with their Discord.

### Bot Guilds
This is something most Bot-developers know all to well: Guilds that have more bots than users.  
There are so many Guilds that have 1 user and well over 50 bots on it. Even Guilds with way over 1,000 Bots and less than 10 users isn't that uncommon.  
The solution to this is really simple: Don't constantly invite bots. Get friends and like-minded people instead of bots.

### "n invites = 1 nitro" and other Reward Guilds
A other common issue existing are Guilds where the owner rewards people for inviting n people to their Guild.  
This is annoying and can even be seen against Discord's ToS and Guidelines, since it animates people to DM-Advertise or advertise in general which is just wrong.  

### DM/Send an invite without permission (DM-Advertising/Advertising)
Many people just go and straight up post a Link in a Text Channel or straight up DM a person with an Invite to their new "Amazing Server".  
But many people don't realize that at least sending an invite in DMs without even asking first isn't just rude but also against Discord's Guidelines and can be a valid reason for your account to be suspended by Discord.  
So for your own safety, ask first if you can share an invite to your Discord before risking a ban and perhaps even a suspension from Discord.

----
## Additional/Useful links
* [LBots.org] - A new NSFW-loving Botlist! ([Discord])
* [Discord Developer Panel] - The place where your Bot's account is created.
* [Best Practices for Discord Bots] - A Readme from PikaDude listing (other) common issues of bots.
