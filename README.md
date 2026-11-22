<div align="center">

# 🎨 GitCord

### The copy-paste guide to **colored code blocks in Discord**

![Discord](https://img.shields.io/badge/Discord-Formatting-5865F2?logo=discord&logoColor=white)
![Markdown](https://img.shields.io/badge/Made%20with-Markdown-000000?logo=markdown&logoColor=white)
![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen)
![License: MIT](https://img.shields.io/badge/license-MIT-blue)

[🚀 Quick start](#quick-start) · [🎨 Colors](#colors) · [📚 Styles](#styles) · [🧪 Templates](#templates) · [🧭 Roadmap](#roadmap)

</div>

---

<a id="what-is-gitcord"></a>
## 📖 What is GitCord?

Discord has no "text color" button. But if you put your text inside a **code block** and add a **language name**, Discord colors it for you. That is how servers get green and red changelogs, blue announcement headers, and yellow warnings.

GitCord collects those tricks in one place, with **ready-to-copy examples**, **tips**, **templates**, and **fixes** for when something does not work.

---

<a id="contents"></a>
## 📑 Contents

| | Section | What's inside |
| --- | --- | --- |
| 🚀 | [Quick start](#quick-start) | Use GitCord in 3 steps |
| 🎨 | [Color cheat sheet](#colors) | Which block gives which color |
| 📚 | [Styles](#styles) | `diff`, `fix`, `ini`, `css`, `json`, `js`, plain, and more |
| 🧪 | [Templates](#templates) | Announcements, changelogs, rules, status boards |
| ✨ | [Other formatting](#other-formatting) | Bold, italic, spoilers, quotes, headings |
| 📏 | [Rules and limits](#rules) | What to remember |
| 🛟 | [Troubleshooting](#troubleshooting) | Fix common problems |
| 🧭 | [Roadmap](#roadmap) | What is coming next |
| ➕ | [Add your own style](#add-your-own) | Template for new sections |
| 🤝 | [Contributing](#contributing) | Help improve GitCord |

---

<a id="quick-start"></a>
## 🚀 Quick start

1. 📋 **Copy** a block from this guide. Hover over it and click the copy icon in the top-right corner.
2. 💬 **Paste** it into the Discord message box.
3. ✏️ **Edit** the text, then press Enter to send.

> 💡 **Tip:** To make a new line while typing in Discord, press **Shift + Enter**. Plain Enter sends the message.

### ⌨️ Typing the backtick yourself

| Device | Where to find the backtick `` ` `` |
| --- | --- |
| 🖥 Windows / Linux | The key to the left of `1` |
| 🍎 Mac | The key to the left of `1` (below Esc) |
| 📱 Phone | On the symbols keyboard. On some keyboards you can also long-press the apostrophe |

### 🧠 The one formula to remember

````
```LANGUAGE
your text here
```
````

Replace `LANGUAGE` with `diff`, `ini`, `css`, `fix`, and so on. The language name decides the color.

> 📌 The examples in this guide are wrapped in **four** backticks so GitHub can show you the exact three-backtick block to copy. In Discord you only type **three**.

---

<a id="colors"></a>
## 🎨 Color cheat sheet

| Color | Use this style | Type this |
| --- | --- | --- |
| 🟥 Red | [`diff`](#style-diff) | `- your text` |
| 🟩 Green | [`diff`](#style-diff) | `+ your text` |
| 🟨 Yellow | [`fix`](#style-fix) | `your text` |
| 🟦 Blue | [`ini`](#style-ini) | `[your text]` |
| 🟧 Orange | [`css`](#style-css) | `[your text]` |
| 🟦 Cyan / teal | [`json`](#style-json) | `"your text"` |
| ⬜ No color | [plain](#style-plain) | `your text` |

> ⚠️ Colors depend on your theme (dark, light, AMOLED) and device, so they can look a little different on mobile. Test a block in your own Discord before using it in an announcement.

---

<a id="styles"></a>
## 📚 Styles

<a id="style-diff"></a>
### ➕➖ diff: green and red

| | |
| --- | --- |
| **Colors** | `+` lines are 🟩 green, `-` lines are 🟥 red |
| **Best for** | Changelogs, status boards, pros and cons, yes/no lists |

**Copy this:**

````
```diff
+ Online
- Offline
+ New feature added
- Old feature removed
```
````

**Tips**
- The `+` or `-` must be the **first character** on the line, with no spaces before it.
- A line with no sign stays in the default color.
- Add a space after the sign so it is easier to read.

<details>
<summary>👀 Preview (GitHub's colors)</summary>

```diff
+ Online
- Offline
+ New feature added
- Old feature removed
```

</details>

---

<a id="style-fix"></a>
### 🟨 fix: yellow text

| | |
| --- | --- |
| **Colors** | The **whole block** turns 🟨 yellow |
| **Best for** | Warnings, notices, "read this" messages |

**Copy this:**

````
```fix
WARNING: The server restarts in 5 minutes.
```
````

**Tips**
- You cannot mix colors inside a `fix` block. Use several blocks if you need more than one color.
- GitHub does not color `fix`, so there is no preview here. It works in Discord.

---

<a id="style-ini"></a>
### 🟦 ini: blue headers

| | |
| --- | --- |
| **Colors** | `[Section]` text turns 🟦 blue. Keys, values and comments get their own shades |
| **Best for** | Announcements, rules, profile cards, settings lists |

**Copy this:**

````
```ini
[ Server Rules ]
Be kind = true
No spam = true
Max members = 100
; this is a comment
```
````

**Tips**
- Put the text you want in blue inside `[square brackets]`.
- Use `key = value` lines for neat label and value pairs.
- Comments start with `;` or `#`.

<details>
<summary>👀 Preview (GitHub's colors)</summary>

```ini
[ Server Rules ]
Be kind = true
No spam = true
Max members = 100
; this is a comment
```

</details>

---

<a id="style-css"></a>
### 🟧 css: orange text

| | |
| --- | --- |
| **Colors** | `[brackets]` usually show 🟧 orange. Real CSS colors selectors, properties and values |
| **Best for** | Eye-catching titles, highlighted words, styled snippets |

**Copy this (orange title):**

````
```css
[ Orange title ]
```
````

**Copy this (real CSS):**

````
```css
.title {
  color: orange;
  font-size: 20px;
}
```
````

**Tips**
- Comments use `/* like this */`.
- Short one-line blocks make good banners.

<details>
<summary>👀 Preview (GitHub's colors)</summary>

```css
.title {
  color: orange;
  font-size: 20px;
}
```

</details>

---

<a id="style-json"></a>
### 🟦 json: keys, strings and numbers

| | |
| --- | --- |
| **Colors** | Keys, text in `"quotes"`, numbers and `true` / `false` each get a different color |
| **Best for** | Data, settings, API examples, cyan-ish highlighted text |

**Copy this:**

````
```json
{
  "name": "GitCord",
  "members": 100,
  "active": true,
  "tags": ["discord", "colors"]
}
```
````

**Tips**
- Your text does not have to be valid JSON to get colors.
- Text in `"double quotes"` is the easiest way to get a cyan or teal look.

<details>
<summary>👀 Preview (GitHub's colors)</summary>

```json
{
  "name": "GitCord",
  "members": 100,
  "active": true,
  "tags": ["discord", "colors"]
}
```

</details>

---

<a id="style-js"></a>
### 🟨 js: JavaScript

| | |
| --- | --- |
| **Colors** | Keywords, strings, numbers and comments each get their own color |
| **Best for** | Sharing real code in chat |

**Copy this:**

````
```js
const name = "GitCord";
// this is a comment
console.log(`Hello, ${name}!`);
```
````

**Tips**
- Discord only colors the code. It never runs it.
- Use `js` for JavaScript, `ts` for TypeScript, `py` for Python.

<details>
<summary>👀 Preview (GitHub's colors)</summary>

```js
const name = "GitCord";
// this is a comment
console.log(`Hello, ${name}!`);
```

</details>

---

<a id="style-plain"></a>
### ⬜ Plain: no color

Leave out the language name. Good for file trees, command lists and anything that is not code.

**Copy this:**

````
```
/github username
/users
/roles
/channels
```
````

---

<a id="style-more"></a>
### 🧰 More languages

Discord can color many other languages too. Use the name right after the three backticks:

| Name | Language | Example start |
| --- | --- | --- |
| `yaml` | YAML | ` ```yaml ` |
| `bash` | Terminal commands | ` ```bash ` |
| `md` | Markdown | ` ```md ` |
| `py` | Python | ` ```py ` |
| `html` | HTML | ` ```html ` |
| `sql` | SQL | ` ```sql ` |
| `cpp` | C++ | ` ```cpp ` |
| `java` | Java | ` ```java ` |

Exact colors differ by language, so try one and see how it looks. More styles will be added here over time.

---

<a id="templates"></a>
## 🧪 Templates

Ready-made messages. Copy, edit the words, send.

### 📢 Announcement

````
```ini
[ Server Announcement ]
Event = Game night
Date = Friday 8 PM
Where = Voice channel 1
```
````

### 📝 Changelog

````
```diff
+ Added: new /help command
+ Added: dark mode theme
- Removed: old welcome message
```
````

### 🟢 Status board

````
```diff
+ Website: online
+ Bot: online
- Database: offline
```
````

### ⚠️ Warning

````
```fix
WARNING: Server restart in 5 minutes. Please save your progress.
```
````

### 📜 Rules (two colors in one message)

You can send several code blocks in one message.

````
```ini
[ Server Rules ]
```
```diff
+ Be respectful
+ Use the right channel
- No spam
- No self-promotion
```
````

---

<a id="other-formatting"></a>
## ✨ Other Discord formatting

Not code blocks, but handy to know:

| Type this | You get |
| --- | --- |
| `**bold**` | **bold** |
| `*italic*` | *italic* |
| `__underline__` | underlined text |
| `~~strikethrough~~` | ~~strikethrough~~ |
| `\|\|spoiler\|\|` | hidden text, click to reveal |
| `` `inline code` `` | `inline code` in a small gray box |
| `> quote` | a quoted line (put it at the start of a line) |
| `# Big heading` | a large heading (start of a line) |
| `-# small text` | small gray text (start of a line) |

---

<a id="rules"></a>
## 📏 Rules and limits

1. **No space** between the backticks and the language name. ` ```diff ` works, ` ``` diff ` does not.
2. Language names are **lowercase**.
3. The language name goes on the **first line**. Your text starts on the **next line**.
4. Close the block with three backticks on **their own line**.
5. Messages are limited to **2000 characters** (4000 with Nitro).
6. You cannot put three backticks **inside** a Discord code block.
7. Colors vary by theme and device.

---

<a id="troubleshooting"></a>
## 🛟 Troubleshooting

| Problem | Fix |
| --- | --- |
| No color at all | Check the spelling. It must be lowercase with no space after the backticks |
| The word `diff` shows inside the block | The language name and your text are on the same line. Press **Shift + Enter** after the name |
| Enter sends the message too early | Use **Shift + Enter** for a new line |
| `+` and `-` are not green and red | They must be the very first character on the line |
| Block looks broken | Put the closing three backticks on their own line |
| Message will not send | It is over 2000 characters. Shorten it or split it |
| Looks different on my phone | Mobile themes use slightly different colors. Test on both |

---

<a id="roadmap"></a>
## 🧭 Roadmap

- [x] `diff`, `fix`, `ini`, `css`, `json`, `js` and plain blocks
- [x] Color cheat sheet
- [x] Ready-made templates
- [ ] 🌈 `ansi` blocks for more colors (desktop only)
- [ ] 👤 Mentions for users, roles, channels and slash commands
- [ ] 🕒 Timestamps that show in each person's time zone
- [ ] 🖼 Screenshots and GIFs of every style
- [ ] 📱 Mobile notes
- [ ] 🧪 More templates (tickets, giveaways, schedules)

---

<a id="add-your-own"></a>
## ➕ Add your own style

Copy this template to add a new section to the guide. Keep the same layout so the guide stays easy to scan.

`````
<a id="style-NAME"></a>
### EMOJI NAME: short title

| | |
| --- | --- |
| **Colors** | What turns which color |
| **Best for** | When to use it |

**Copy this:**

````
```NAME
your example here
```
````

**Tips**
- One useful tip
- Another useful tip
`````

Then add a row to the [color cheat sheet](#colors) and a checkbox to the [roadmap](#roadmap).

---

<a id="contributing"></a>
## 🤝 Contributing

Found a new trick, a better example, or a mistake? Contributions are welcome.

1. 🍴 Fork this repository
2. 🌿 Create a branch, for example `add-ansi-colors`
3. ✏️ Add your section using the template above
4. 📬 Open a pull request and say what you added

Please test every example in a real Discord server before submitting.

---

<a id="credits"></a>
## 🙏 Credits

Made for the Discord community. If you add icons from Flaticon or another site, credit the authors here.

<!-- Example: Icons by AUTHOR NAME from Flaticon (https://www.flaticon.com) -->

## 📄 License

MIT. Free to use, share and improve.

<div align="center">

⭐ If GitCord helped you, give it a star so others can find it.

</div>
