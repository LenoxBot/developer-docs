---
description: >-
  This ban command allows you to ban an Discord user from a Discord server with
  a specific reason
---

# Create a ban command

First of all we start with the basic setup of our new command.

```javascript
const args = message.content.split(' ').slice(1); // All arguments behind the command name with the prefix
```

With this line of code we get all the content behind the prefix with the command. 

_**Example:**_ If you enter the command in a Discord channel `?ban @Monkeyyy11#0001 Spam` , **args** will be `@Monkeyyy11#0001 Spam`.



```javascript
const args = message.content.split(' ').slice(1); // All arguments behind the command name with the prefix

const user = message.mentions.users.first(); // returns the user object if an user mention exists
const banReason = args.slice(1).join(' '); // Reason of the ban (Everything behind the mention)
```

In the third line of code we request the first mention of a Discord user in this message. If there is an user mention, you will receive the user object of this Discord user.

In the next line we slice the ban reason from our arguments behind the command.



```javascript
const args = message.content.split(' ').slice(1); // All arguments behind the command name with the prefix

const user = message.mentions.users.first(); // returns the user object if an user mention exists
const banReason = args.slice(1).join(' '); // Reason of the ban (Everything behind the mention)

// Check if an user mention exists in this message
if (!user) {
		try {
		    // Check if a valid userID has been entered instead of a Discord user mention
			if (!message.guild.members.get(args.slice(0, 1).join(' '))) throw new Error('Couldn\' get a Discord user with this userID!');
			
			// If the client (bot) can get a user with this userID, it overwrites the current user variable to the user object that the client fetched
			user = message.guild.members.get(args.slice(0, 1).join(' '));
			user = user.user;
		} catch (error) {
			return message.reply('Couldn\' get a Discord user with this userID!');
		}
	}
```

Here we have a very nice feature that allows your ban command to enter an userID of a Discord server member instead of mentioning him. First we check if the message contains a user mention, if not, then check if a valid userID has been entered. If no, the client returns an error in form of a message. If yes, the client overwrites the **`user`**variable with the new user Object.



```javascript
const args = message.content.split(' ').slice(1); // All arguments behind the command name with the prefix

const user = message.mentions.users.first(); // returns the user object if an user mention exists
const banReason = args.slice(1).join(' '); // Reason of the ban (Everything behind the mention)

// Check if an user mention exists in this message
if (!user) {
		try {
		    // Check if a valid userID has been entered instead of a Discord user mention
			if (!message.guild.members.get(args.slice(0, 1).join(' '))) throw new Error('Couldn\' get a Discord user with this userID!');
			
			// If the client (bot) can get a user with this userID, it overwrites the current user variable to the user object that the client fetched
			user = message.guild.members.get(args.slice(0, 1).join(' '));
			user = user.user;
		} catch (error) {
			return message.reply('Couldn\' get a Discord user with this userID!');
		}
	}
	
if (user === message.author) return message.channel.send('You can\'t ban yourself'); // Check if the user mention or the entered userID is the message author himsmelf
if (!reason) return message.reply('You forgot to enter a reason for this ban!'); // Check if a reason has been given by the message author
if (!message.guild.member(user).bannable) return message.reply('You can\'t ban this user because you the bot has not sufficient permissions!'); // Check if the user is bannable with the bot's permissions
```

3 different checks have been added here. 

The first if checks if the user variable is the same user object as the message author object.

The next code line checks if the message author hasn't forgotten to enter a reason for the ban of the Discord user.

The last line that we've added checks if the bot has even enough permissions to ban this Discord user because otherwise the following code that we will cover as next will not work.



```javascript
const args = message.content.split(' ').slice(1); // All arguments behind the command name with the prefix

const user = message.mentions.users.first(); // returns the user object if an user mention exists
const banReason = args.slice(1).join(' '); // Reason of the ban (Everything behind the mention)

// Check if an user mention exists in this message
if (!user) {
		try {
		    // Check if a valid userID has been entered instead of a Discord user mention
			if (!message.guild.members.get(args.slice(0, 1).join(' '))) throw new Error('Couldn\' get a Discord user with this userID!');
			
			// If the client (bot) can get a user with this userID, it overwrites the current user variable to the user object that the client fetched
			user = message.guild.members.get(args.slice(0, 1).join(' '));
			user = user.user;
		} catch (error) {
			return message.reply('Couldn\' get a Discord user with this userID!');
		}
	}
	
if (user === message.author) return message.channel.send('You can\'t ban yourself'); // Check if the user mention or the entered userID is the message author himsmelf
if (!reason) return message.reply('You forgot to enter a reason for this ban!'); // Check if a reason has been given by the message author
if (!message.guild.member(user).bannable) return message.reply('You can\'t ban this user because you the bot has not sufficient permissions!'); // Check if the user is bannable with the bot's permissions

await message.guild.ban(user) // Bans the user

const Discord = require('discord.js'); // We need Discord for our next RichEmbeds
const banConfirmationEmbed = new Discord.RichEmbed()
		.setColor('RED')
		.setDescription(`✅ ${user.tag} has been successfully banned!`);
message.channel.send({
	embed: banConfirmationEmbed
}); // Sends a confirmation embed that the user has been successfully banned
```

With the new code in line 24, we ban the Discord user from the current Discord server where we enter the bot command. 

After this we send a confirmation RichEmbed in the current channel where we entered our command to confirm that the user has been successfully banned.







_\*\*\*\*_



