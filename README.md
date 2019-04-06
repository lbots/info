## Common issues of bots
There are many bots on Discord and each day, new ones get added.  
While this itself isn't a issue, the fact that there are often the same problems across multiple bots can become a tedious process to filter out.
This page lists common issues and how they can/should be prevented.

## Terms
This file contains certain terms that are used and may be unknown for you or others.  
Here is a small explanation of those terms.

| Term: | Meaning:                              |
| ----- | ------------------------------------- |
| Guild | The Discord Server a bot/member is on |

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

Also there are things that people don't know is API-abuse:
- Disco-Roles: Roles that have fast-changing colours.
- Mass-DM: Sending a DM to every member of a Guild

If your bot does one or even all of those listed things, remove it since it only creates the risk of your Bot's IP being banned from Discord's API (And it will also most likely make your bot being denied from any professional Bot-list).

### Open Eval/Dev only commands
Having an Eval-command is (almost) a essential part of a dev. The eval-command allows them to test/run parts of code to see what the output could be.  
But this can also be a potential thread, when it isn't secured properly.  
The eval command could f.e. allow people to change the bot's avatar, name, game or even shut it down completely. This is annoying and just makes things worse.  
That's why you should keep the Eval-command for yourself only and not have it public. This also applies for any other command that might change the bot in a way you don't want other people be able to do, like a shutdown/reboot command.

If you really want people to use the eval command to for example, evaluate math expressions, restrict it so that potential harmfull code can't be run by normal users.

## Additional Links
- Discord's Developer-Panel: https://discordapp.com/developers
- LBots website: https://lbots.org
- Best practices for Discord Bots: https://github.com/meew0/discord-bot-best-practices
