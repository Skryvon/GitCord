# GitCord

A beginner-friendly Discord bot (discord.js v14) that shows GitHub profiles, lists your server's **users, roles and channels**, and teaches you how Discord's mention shortcuts and clickable slash commands work.

[![Node](https://img.shields.io/badge/node-%3E%3D18-brightgreen)](https://nodejs.org)
[![discord.js](https://img.shields.io/badge/discord.js-v14-5865F2)](https://discord.js.org)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

---

## Contents

1. [What you get](#what-you-get)
2. [Discord elements cheat sheet](#discord-elements-cheat-sheet)
3. [Quick start](#quick-start)
4. [Project files](#project-files)
5. [Commands](#commands)
6. [How to use the `</commands:ID>` integration command](#how-to-use-the-commandsid-integration-command)
7. [Troubleshooting](#troubleshooting)
8. [Contributing](#contributing)

---

## What you get

| Command | What it does |
| --- | --- |
| `/github username` | Shows a GitHub profile (repos, followers, bio) |
| `/users` | Lists server members as mentions |
| `/roles` | Lists every role in the server |
| `/channels` | Lists channels grouped by type |
| `/commands` | Lists every bot command as a **clickable** command mention |

---

## Discord elements cheat sheet

Everything in Discord has a numeric **ID**. Mentions are just the ID wrapped in special characters.

| Element | Mention format | Example | discord.js property |
| --- | --- | --- | --- |
| User | `<@USER_ID>` | `<@123456789012345678>` | `user.id`, `` `${user}` `` |
| Role | `<@&ROLE_ID>` | `<@&123456789012345678>` | `role.id`, `` `${role}` `` |
| Channel | `<#CHANNEL_ID>` | `<#123456789012345678>` | `channel.id`, `` `${channel}` `` |
| Slash command | `</name:COMMAND_ID>` | `</github:123456789012345678>` | `command.id` |
| Subcommand | `</name subcommand:COMMAND_ID>` | `</github profile:123456789012345678>` | parent `command.id` |
| Custom emoji | `<:name:EMOJI_ID>` | `<:wave:123456789012345678>` | `emoji.id` |
| Timestamp | `<t:UNIX_SECONDS:R>` | `<t:1700000000:R>` | `Math.floor(Date.now() / 1000)` |

**How to get an ID:** Discord Settings > Advanced > turn on **Developer Mode**, then right-click a user, role, channel or server and choose **Copy ID**.

### Quick snippets

```js
// USERS
const user = interaction.user;                    // who ran the command
const mention = `<@${user.id}>`;                  // same as `${user}`
const avatar = user.displayAvatarURL({ size: 256 });

// MEMBERS (a user inside a server)
const member = await interaction.guild.members.fetch(user.id);
const joined = `<t:${Math.floor(member.joinedTimestamp / 1000)}:R>`;
const roleMentions = member.roles.cache.map(r => `${r}`).join(' ');

// ROLES
const role = interaction.guild.roles.cache.find(r => r.name === 'Moderator');
const everyoneWithRole = role?.members.map(m => `<@${m.id}>`);

// CHANNELS
const { ChannelType } = require('discord.js');
const channels = await interaction.guild.channels.fetch();
const textChannels = channels.filter(c => c.type === ChannelType.GuildText);
const voiceChannels = channels.filter(c => c.type === ChannelType.GuildVoice);

// SERVER (guild)
const guild = interaction.guild;
console.log(guild.name, guild.memberCount, guild.id);
```

> **Tip:** Mentions inside **embeds** never ping anyone. In normal message text, add `allowedMentions: { parse: [] }` to stop pings.

---

## Quick start

### 1. Requirements

- [Node.js](https://nodejs.org) 18 or newer
- A Discord account and a server where you can add bots

### 2. Create the bot

1. Go to the [Discord Developer Portal](https://discord.com/developers/applications) and click **New Application**.
2. Open **Bot**, then click **Reset Token** and copy it. Never share or commit it.
3. Under **Bot > Privileged Gateway Intents**, enable **Server Members Intent** (needed for `/users`).
4. Open **OAuth2 > URL Generator**, tick `bot` and `applications.commands`, and give it **Send Messages** and **Embed Links**.
5. Open the generated URL and add the bot to your server.
6. Copy the **Application ID** from **General Information**.

### 3. Install

```bash
git clone https://github.com/YOURNAME/gitcord.git
cd gitcord
npm install
```

Create a file named `.env` (it is already ignored by git):

```env
DISCORD_TOKEN=your-bot-token
CLIENT_ID=your-application-id
GUILD_ID=your-server-id
```

`GUILD_ID` is optional. Set it while developing so commands update instantly. Remove it to register commands globally (global commands can take up to an hour to appear).

### 4. Register commands, then run

```bash
npm run deploy
npm start
```

You should see `Logged in as ...`. Type `/` in your server and the commands appear.

---

## Project files

```
gitcord/
├── commands/
│   ├── github.js
│   ├── users.js
│   ├── roles.js
│   ├── channels.js
│   └── commands.js
├── utils.js
├── index.js
├── deploy-commands.js
├── package.json
├── .env              (never commit this)
└── .gitignore
```

### `package.json`

```json
{
  "name": "gitcord",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "deploy": "node deploy-commands.js"
  },
  "dependencies": {
    "discord.js": "^14.16.0",
    "dotenv": "^16.4.5"
  }
}
```

### `.gitignore`

```
node_modules
.env
```

### `utils.js`

```js
const { Collection } = require('discord.js');

/** Cut text so it fits inside an embed description (max 4096 characters). */
function truncate(text, max = 4000) {
  return text.length > max ? text.slice(0, max - 1) + '…' : text;
}

/**
 * Returns a Map of command name -> command object, including the real command ID.
 * Fetches global commands, then guild commands (which win if the names clash).
 */
async function getCommandMap(client, guildId) {
  const map = new Map();

  const globalCommands = await client.application.commands.fetch();
  globalCommands.forEach(cmd => map.set(cmd.name, cmd));

  if (guildId) {
    const guildCommands = await client.application.commands.fetch({ guildId });
    guildCommands.forEach(cmd => map.set(cmd.name, cmd));
  }

  return map;
}

/** Builds a clickable command mention such as </github:123456789012345678>. */
function commandMention(map, name, subcommand) {
  const cmd = map.get(name);
  if (!cmd) return `/${name}${subcommand ? ' ' + subcommand : ''}`; // plain-text fallback
  return `</${cmd.name}${subcommand ? ' ' + subcommand : ''}:${cmd.id}>`;
}

module.exports = { truncate, getCommandMap, commandMention, Collection };
```

### `index.js`

```js
require('dotenv').config();
const fs = require('node:fs');
const path = require('node:path');
const { Client, Collection, Events, GatewayIntentBits, MessageFlags } = require('discord.js');

const client = new Client({
  intents: [GatewayIntentBits.Guilds, GatewayIntentBits.GuildMembers],
});

// Load every file in /commands
client.commands = new Collection();
const commandsDir = path.join(__dirname, 'commands');
for (const file of fs.readdirSync(commandsDir).filter(f => f.endsWith('.js'))) {
  const command = require(path.join(commandsDir, file));
  client.commands.set(command.data.name, command);
}

client.once(Events.ClientReady, c => {
  console.log(`Logged in as ${c.user.tag}`);
});

client.on(Events.InteractionCreate, async interaction => {
  if (!interaction.isChatInputCommand()) return;

  const command = client.commands.get(interaction.commandName);
  if (!command) return;

  try {
    await command.execute(interaction);
  } catch (error) {
    console.error(error);
    const message = { content: 'Something went wrong running that command.', flags: MessageFlags.Ephemeral };
    if (interaction.deferred || interaction.replied) await interaction.followUp(message);
    else await interaction.reply(message);
  }
});

client.login(process.env.DISCORD_TOKEN);
```

### `deploy-commands.js`

```js
require('dotenv').config();
const fs = require('node:fs');
const path = require('node:path');
const { REST, Routes } = require('discord.js');

const commandsDir = path.join(__dirname, 'commands');
const body = fs
  .readdirSync(commandsDir)
  .filter(f => f.endsWith('.js'))
  .map(f => require(path.join(commandsDir, f)).data.toJSON());

const rest = new REST().setToken(process.env.DISCORD_TOKEN);
const { CLIENT_ID, GUILD_ID } = process.env;

(async () => {
  const route = GUILD_ID
    ? Routes.applicationGuildCommands(CLIENT_ID, GUILD_ID)
    : Routes.applicationCommands(CLIENT_ID);

  const result = await rest.put(route, { body });
  console.log(`Registered ${result.length} command(s) ${GUILD_ID ? 'to your server' : 'globally'}.`);
})();
```

---

## Commands

Put each file below in the `commands/` folder.

### `/github`: show a GitHub profile

`commands/github.js`

```js
const { SlashCommandBuilder, EmbedBuilder } = require('discord.js');

module.exports = {
  data: new SlashCommandBuilder()
    .setName('github')
    .setDescription('Show a GitHub profile')
    .addStringOption(o =>
      o.setName('username').setDescription('GitHub username, e.g. octocat').setRequired(true)),

  async execute(interaction) {
    await interaction.deferReply();
    const username = interaction.options.getString('username', true).trim();

    const res = await fetch(`https://api.github.com/users/${encodeURIComponent(username)}`, {
      headers: { 'User-Agent': 'gitcord-bot', Accept: 'application/vnd.github+json' },
    });

    if (res.status === 404) return interaction.editReply(`No GitHub user called **${username}**.`);
    if (!res.ok) return interaction.editReply(`GitHub returned an error (${res.status}). Try again later.`);

    const u = await res.json();
    const embed = new EmbedBuilder()
      .setTitle(u.name ? `${u.name} (@${u.login})` : `@${u.login}`)
      .setURL(u.html_url)
      .setThumbnail(u.avatar_url)
      .setDescription(u.bio || 'No bio.')
      .addFields(
        { name: 'Repos', value: String(u.public_repos), inline: true },
        { name: 'Followers', value: String(u.followers), inline: true },
        { name: 'Following', value: String(u.following), inline: true },
      )
      .setColor(0x24292f);

    await interaction.editReply({ embeds: [embed] });
  },
};
```

> GitHub allows 60 unauthenticated API requests per hour per IP. For more, send an `Authorization: Bearer YOUR_GITHUB_TOKEN` header.

### `/users`: list server members

`commands/users.js`

```js
const { SlashCommandBuilder, EmbedBuilder } = require('discord.js');
const { truncate } = require('../utils');

module.exports = {
  data: new SlashCommandBuilder()
    .setName('users')
    .setDescription('List the members of this server')
    .setDMPermission(false),

  async execute(interaction) {
    await interaction.deferReply();
    const members = await interaction.guild.members.fetch(); // needs the Server Members Intent

    const humans = members.filter(m => !m.user.bot);
    const bots = members.filter(m => m.user.bot);

    const embed = new EmbedBuilder()
      .setTitle(`Members of ${interaction.guild.name}`)
      .setDescription(truncate(
        `**Humans (${humans.size})**\n${humans.map(m => `<@${m.id}>`).join(' ') || 'None'}\n\n` +
        `**Bots (${bots.size})**\n${bots.map(m => `<@${m.id}>`).join(' ') || 'None'}`
      ))
      .setColor(0x5865f2);

    await interaction.editReply({ embeds: [embed] });
  },
};
```

### `/roles`: list roles

`commands/roles.js`

```js
const { SlashCommandBuilder, EmbedBuilder } = require('discord.js');
const { truncate } = require('../utils');

module.exports = {
  data: new SlashCommandBuilder()
    .setName('roles')
    .setDescription('List all roles in this server')
    .setDMPermission(false),

  async execute(interaction) {
    const guild = interaction.guild;
    const roles = (await guild.roles.fetch())
      .filter(r => r.id !== guild.id)            // hide @everyone
      .sort((a, b) => b.position - a.position);  // highest first

    const lines = roles.map(r => `<@&${r.id}> (${r.members.size} members)`);

    const embed = new EmbedBuilder()
      .setTitle(`Roles in ${guild.name} (${roles.size})`)
      .setDescription(truncate(lines.join('\n') || 'No roles.'))
      .setColor(0xfaa61a);

    await interaction.reply({ embeds: [embed] });
  },
};
```

> `r.members` only counts members that are cached. `/users` fetches all members first, and the bot also caches members as they are seen, so counts may be slightly low right after a restart.

### `/channels`: list channels

`commands/channels.js`

```js
const { SlashCommandBuilder, EmbedBuilder, ChannelType } = require('discord.js');
const { truncate } = require('../utils');

module.exports = {
  data: new SlashCommandBuilder()
    .setName('channels')
    .setDescription('List all channels in this server')
    .setDMPermission(false),

  async execute(interaction) {
    const channels = await interaction.guild.channels.fetch();

    const groups = [
      ['Text', ChannelType.GuildText],
      ['Voice', ChannelType.GuildVoice],
      ['Announcements', ChannelType.GuildAnnouncement],
      ['Forums', ChannelType.GuildForum],
      ['Stages', ChannelType.GuildStageVoice],
    ];

    const sections = groups
      .map(([label, type]) => {
        const list = channels.filter(c => c && c.type === type).sort((a, b) => a.rawPosition - b.rawPosition);
        return list.size ? `**${label} (${list.size})**\n${list.map(c => `<#${c.id}>`).join(' ')}` : null;
      })
      .filter(Boolean);

    const embed = new EmbedBuilder()
      .setTitle(`Channels in ${interaction.guild.name}`)
      .setDescription(truncate(sections.join('\n\n') || 'No channels.'))
      .setColor(0x57f287);

    await interaction.reply({ embeds: [embed] });
  },
};
```

### `/commands`: the integration command

`commands/commands.js`

```js
const { SlashCommandBuilder, EmbedBuilder } = require('discord.js');
const { truncate, getCommandMap, commandMention } = require('../utils');

module.exports = {
  data: new SlashCommandBuilder()
    .setName('commands')
    .setDescription('List every bot command as a clickable shortcut'),

  async execute(interaction) {
    await interaction.deferReply();
    const map = await getCommandMap(interaction.client, interaction.guildId);

    const lines = [...map.values()].map(cmd => `${commandMention(map, cmd.name)} - ${cmd.description}`);

    const embed = new EmbedBuilder()
      .setTitle('Available commands')
      .setDescription(truncate(lines.join('\n') || 'No commands found.'))
      .setFooter({ text: 'Click a command to run it.' })
      .setColor(0x5865f2);

    await interaction.editReply({ embeds: [embed] });
  },
};
```

---

## How to use the `</commands:ID>` integration command

### What the syntax means

Discord lets a bot post a **clickable slash command**. The format is:

```
</commandName:COMMAND_ID>
```

When someone clicks it, Discord puts that command into their message box, ready to send.

Two things to know:

- `</commands:00000>` is a **placeholder**. `00000` is not a real ID. If you post it as-is, Discord shows it as plain text instead of a clickable link.
- The real ID is assigned by Discord when you register the command, so every bot and every server (for guild commands) has a different one.

### Option A: just run `/commands`

After `npm run deploy` and `npm start`, type `/commands` in your server. The bot looks up the real IDs and replies with a clickable list of every command. Nothing to configure.

### Option B: get the ID yourself

1. Register your commands (`npm run deploy`).
2. Run this one-off script (or add it to any command):

   ```js
   // get-ids.js
   require('dotenv').config();
   const { Client, GatewayIntentBits } = require('discord.js');
   const client = new Client({ intents: [GatewayIntentBits.Guilds] });

   client.once('ready', async () => {
     const cmds = process.env.GUILD_ID
       ? await client.application.commands.fetch({ guildId: process.env.GUILD_ID })
       : await client.application.commands.fetch();
     cmds.forEach(c => console.log(`</${c.name}:${c.id}>`));
     client.destroy();
   });

   client.login(process.env.DISCORD_TOKEN);
   ```

3. Run `node get-ids.js` and copy the printed mentions.

### Option C: use it anywhere in your own code

```js
const { getCommandMap, commandMention } = require('./utils');

const map = await getCommandMap(interaction.client, interaction.guildId);

await interaction.reply(`Try ${commandMention(map, 'github')} to look up a profile!`);
// For a subcommand: commandMention(map, 'github', 'profile')
```

If the command is not found, the helper falls back to plain text like `/github`, so your bot never breaks.

---

## Troubleshooting

| Problem | Fix |
| --- | --- |
| Commands don't show up | Run `npm run deploy` again. Global commands can take up to an hour; set `GUILD_ID` for instant updates. |
| `Used disallowed intents` | Enable **Server Members Intent** in the Developer Portal under Bot. |
| `/users` only shows a few people | The Server Members Intent is off, or the server is very large. |
| `Missing Access` / `Missing Permissions` | Re-invite the bot using a URL with the `applications.commands` scope and the permissions above. |
| `</commands:00000>` isn't clickable | `00000` is a placeholder. Use the real ID from `/commands` or Option B. |
| Token leaked | Reset it right away in the Developer Portal and update `.env`. |

---

## Contributing

Pull requests are welcome. Fork the repo, create a branch, make your change, and open a PR. Please keep new commands in their own file under `commands/`.

## License

MIT
