# Fornello

Runs HTML pages **with their scripts** inside [Marinara Engine](https://github.com/Pasta-Devs/Marinara-Engine), in a built-in JavaScript interpreter (sval), since Marinara's CSP blocks page scripts. *Fornello* is Italian for a stove burner. 한국어: [README-ko.md](README-ko.md)

## Features

* ```html code blocks in chat messages become running pages, drawn on the message in the theme's text color and font
* Pages a character card's display regex makes run under the reply (a reply that is only pages hides its text); a card's own regex applies in its chats even with Marinara's character regex off
* React and JSX: `<script type="text/babel">`, React 18 from a CDN
* **Components**: JSX components or HTML templates, as status panels under replies or scenes where the AI puts them; cards can bring their own
* **Page commands**: call the AI; work with messages, variables, lorebooks, characters, the tracker
* **Scripts** that run behind the chat: your own, a chat's, and a card's once you allow it
* `localStorage` kept per chat in this browser; `sessionStorage` until a reload
* Pages in [Dispensa](https://github.com/FigCake/ME.Dispensa)'s 🌟 viewer run too
* English or 한국어 (🌐 in the settings; follows Marinara by default, shared with Dispensa)

## Setup

1. In the server's `.env`, set `ENABLE_EXTERNAL_EXTENSIONS=true` and restart Marinara
2. Settings → Addons → External Extensions: turn on **Allow third-party extension imports**, import `ME-Fornello-<version>.zip`
3. **Fornello** in the list → **Review and Run**: check the code, approve the hash (it must equal the one in `SHA256SUMS.txt`)

Or add `FigCake/ME.Fornello` as a source in [Fetchy](https://github.com/FigCake/ME.Fetchy).

## Using it

**Settings**: the slice of cake in Marinara's top bar (Polyglot's globe when Polyglot is installed) → **Fornello settings**, **Components** or **Script library**; also ⚙ on a page, or 🔥 Fornello in Dispensa.

| Setting | Means |
|---|---|
| Run scripts | **Run at once** (default: the recent pages start when on screen) · **Click to run** · **Off** (components and scripts too) |
| Recent pages to auto-run | How many recent pages start by themselves (default 3) |
| Components | **+ New component** · **Import** · **Export** |
| Replies to keep open | How many recent replies show their components open |
| Components of older replies | **Collapse** or **Hide** |

**Buttons on a page** (top right on hover, always on touch screens): **▶ Run** · ↻ run again · 🖼 save as PNG · ⤢ fullscreen (Esc to leave) · ⚙ settings. Text that would not show on the theme gets a plain backdrop.

## Components

1. **+ New component** (or **Import**), then pick the kind:
   * **JSX**: paste a component. `import` / `export default` lines are cleaned up; hooks work. Its name is the function's
   * **HTML**: a template with `{{value}}` places and its own `<style>`; no code runs (`<script>` and `on…` are dropped; `<details>` and labelled radio buttons work). Its **Name** is the tag (capital first). `{{stats.hp}}` reads inside a value; `{{#cast}}…{{/cast}}` repeats per list item (`{{name}}` the item's, `{{.}}` the item) or shows once for a set value; `{{^cast}}…{{/cast}}` shows when empty. Values go in as text; open `<details>` and ticked boxes survive new values
2. **Values from**:
   * **Tag in reply**: the AI writes the tag with values as attributes (`<Card hp={40} />`); the tag text is hidden. Put the **Rule for the AI** in the card or a lorebook, or press **Add to this chat's lorebook**. **How the AI uses it**: **Status** (the tag ends every reply, drawn under it) or **Scene** (only **When to use it**, in a ```fornello block, drawn there)
   * **Text in tag**: values as text inside the tag, as display-regex cards write them (`<Tag>a|b|c</Tag>`), parts in table order. **Separator** `|` or `:` (only the last part may contain it). **One item a line**: each line is an item of `rows`, with `{{1}}`, `{{2}}`… by place, `{{text}}` the line, and a mark named after its first part (`{{#E}}…{{/E}}`), so each kind of line can look its own way. **Example text in the tag** feeds the preview and the rule. Scenes of this kind are drawn where the tag is, no code block needed
   * **Tracker**: Marinara's tracker as it was at each reply, redrawn when the tracker is written. Same-named fields map by themselves, others by path (`date`, `custom.<field>`, `char.<name>.<stat>`). **Instructions in this chat's tracker → Put in** writes each value's instruction into its custom tracker field for Marinara's Custom Tracker agent (on in the chat), adding missing fields empty; Fornello puts back instructions a tracker write drops, while the component is on in the chat. **Take out** removes them
3. **Values**: one row per value: name, kind (text, number, yes/no, list, group), example, instruction for the AI. JSX: the values its code takes (`function StatusCard({ hp = 80 })`); edits here rewrite the code. HTML: what the template uses comes in by itself; a rename changes its places. Optional swaps per value (`value=shown`, one a line)
4. **Display name** (shown when folded), **New chats** (on or off where the chat chose nothing), **Font** (one of Marinara's fonts)
5. The preview redraws with the examples after each change (commands do nothing there); **Save**

* **In this chat** (settings, with a chat open): a switch per component and per set for this chat, kept in the chat. Tags of components off in a chat are still hidden
* **Theme classes and icons**: `var(--fc-text)`, `--fc-muted`, `--fc-title`, `--fc-accent`, `--fc-card`, `--fc-line`, `--fc-radius`, and classes `fc-card`, `fc-title`, `fc-muted`, `fc-accent`, `fc-badge`, `fc-btn`, `fc-bar` (`<div class="fc-bar" style="--value:70%"><i></i></div>`). Font Awesome, Material Symbols and Material Icons load when their classes are used; other stylesheets with `<link rel="stylesheet">`
* **Sets**: named groups of components (**+ Set**), each with **Export**, **Edit**, ✕
* **Export** (all, a set, or one) saves a `.json` components file. **Import** takes that file, a character card (PNG or JSON) or a bare `.jsx` / `.html` file; pick what comes in, optionally as a set. Same name replaces (each chat keeps its choice)
* Hover a component: save as image, settings, collapse. Page commands work inside; clicks and typing inside are not saved

### Components in a character card

* One lorebook entry per component, named `fornello:Name`, the JSX code as content, **turned off**; or `data.extensions.fornello.components: [{ "name": "…", "code": "…" }]` in the card's JSON
* The rule goes in another entry of the card (always on, or with keywords): copy it from the editor
* They draw only after **Allow this card's components** (asked again when they change); **Stop allowing** under **From this chat's cards** in the settings
* Your own component of the same name wins

## Scripts

Scripts that run behind the chat. Settings → **Scripts** → **Open the library**.

* **Card scripts** (carried by a character card): opening the chat asks **Run** · **Not now** · **See the code** · **Don't ask again for this card**. Asked again when they change; **Not now** lasts until a reload; **Stop allowing** in the settings
* **Your own** run in every chat; **a chat's own** (`setScripts([...], "chat")`) run in that chat and its branches, asked about like a card's
* **The library window**: three lists (yours, each character's card, the chat's), with folders, switches, **⋯** (move, copy to another list, delete) and drag and drop. **Import**: script files or zips, character cards, `.js` files. **Export**: a `.json` script file (asks before including script variables)
* **The editor**: line numbers, syntax errors as you type, buttons, note, variables (JSON), and the **Log** (`console` output and errors) with **Restart**
* They start when the chat opens and stop when another opens, taking their timers, listeners, observers and the elements they left in `<body>` / `<head>` with them. **Off** turns them off
* Scripts get every page command, plus:

| Command | What it does |
|---|---|
| `setButtons(["Hello", "Roll"])` | Buttons in a 🔥 bubble in Dispensa's Dock, or in the top-bar menu without it |
| `onButton("Hello", fn)` | Runs when that button is pressed |
| `getScripts("global" \| "char" \| "chat")` / `await setScripts([...], scope)` | Read / replace a list (scripts `{ name, code, enabled, buttons, info, data }`, folders `{ type: "folder", name, scripts }`). Pages can use these too (asked once) |

```js
setButtons(["Roll"]);
onButton("Roll", () => showToast(`🎲 ${1 + Math.floor(Math.random() * 6)}`));
onMsg((m) => setVars({ replies: (getVars("script").replies ?? 0) + 1 }, "script"));
```

* Also: `getScriptId()`, `getScriptName()`, `getAllEnabledScriptButtons()` (every running script's buttons)
* ES modules work (`import x from "https://…"`, bare names from jsDelivr, top-level `await`)
* jQuery `$`, lodash `_`, zod `z` and `YAML` load from jsDelivr when scripts or pages use them
* Not yet: MVU (MagVarUpdate)

## Page commands

Message numbers count from 1; a negative one counts from the end (`-1` = the last). A page's own function of the same name wins.

| Command | What it does |
|---|---|
| `sendMsg("text")` | Sends it as your message and gets the reply (`true` as 2nd argument: ask first) |
| `sendRegen()` | Rewrites the last reply; `sendRegen("shorter")` adds a hint for it |
| `await genChat("prompt")` | Asks the AI with the whole chat as context; nothing is saved |
| `await genChat("prompt", { history: 10 })` | Cheaper: cards, persona, always-on lorebook entries and the last 10 messages only |
| `await genRaw("prompt")` / `genRaw([{ role, content, images? }, …])` | The prompt alone, or messages in your order |
| options | `connection` (a saved connection), `onToken: (text, piece) => …`, `id`. `genChat`: `guide`, `preset`. `genRaw`: `images`, `temperature`, `maxTokens`, `topP`, `topK`, `minP`, `frequencyPenalty`, `presencePenalty`, `reasoningEffort` |
| `stopGen(id)` / `stopMyGens()` | Stops one / all of the page's own AI calls |
| `getVars()` / `setVars({ name: value })` | Chat variables, kept in the chat. `setVars` merges all the way down; `null` deletes; lists are replaced |
| `getVars(kind)` / `setVars({ … }, kind)` | `"global"`, `"character"` (starts from the card's), `"script"`: kept in Fornello's storage |
| `await getMsgs({ role, hidden })` | Messages: `num`, `role`, `name`, `text`, `hidden`, `data`, `swipe`, `swipes` |
| `await getLastMsg("user" \| "char")` / `getMsgId()` | Last message of either side / the page's own message number |
| `await setMsg(num, "text")` / `delMsg(num)` | Rewrites / deletes (asks) a message |
| `await addMsg("text", "char")` | Adds a message without generating (`"user"`, `"char"`, `"narrator"`; `name`, `hidden`, `data`, `before`); returns its number |
| `getMsgData(num)` / `await setMsgData(num, { … })` | Data kept with a message's swipe, unseen by the AI (merges; `{ replace: true }`) |
| `await getSwipes(num)` / `setSwipe(num, i)` / `addSwipe(num, "text")` / `delSwipe(num, i)` | Swipes (from 0); `addSwipe` shows it unless `{ show: false }` |
| `await hideMsg(num)` / `showMsg(num)` | Hides from the AI / shows again (one or a list) |
| `await branch(num)` | A new chat up to `num`; returns its id |
| `stopGen()` | Stops the reply being written |
| `getInput()` / `setInput("text")` | Reads / fills the chat input |
| `getUser()` / `getChar()` | Your persona / the chat's character (`name`, `description`, `personality`, `scenario`, `firstMessage`, `greetings`, `example`, `tags`, `books`, `data`) |
| `await getChar("name")` / `getUser("name")`, `getChars()` / `getUsers()` | Others in the library / their names |
| `await setChar(name, { … })` / `setUser(…)`, `addChar` / `addUser`, `delChar` / `delUser` | Change (merged, asked once), make, delete (always asked) |
| `await importChar(file)` / `useUser("name")` | Imports a card (PNG `Blob` or JSON) / switches the chat's persona |
| `await getTracker()` / `setTracker({ "custom.mood": "happy" })` | Reads / changes Marinara's tracker |
| `onTracker((tracker, changed) => …)` | Runs when the tracker changes; `changed` lists paths |
| `await runTrackers()` / `getAgents()` / `runAgents(["type"])` | Re-runs trackers / lists agents / re-runs agents (AI calls) |
| `await getAgentMemory(type)` / `setAgentMemory(type, { … })` / `clearAgentMemory(type)` | An agent's memory in this chat |
| `await genImage("prompt")` | A picture from the chat's image connection; returns its address |
| `await speak("text")` / `stopSpeaking()` | Reads aloud with Marinara's voice settings |
| `await getSummary()` / `makeSummary({ last })` / `setSummary([{ title, text }])` | The chat's summaries |
| `await getLore()` / `getLore("book")` | Book names / a book's entries; `"@chat"` is the chat's own book |
| `await setLore("book", { name, content, keys, constant, … })` | Adds or updates entries by name (`{ replace: true }`: only these) |
| `await delLore("book", "name")` | Deletes entries (from a shared book, or several at once: asks) |
| `getBooks()` / `await linkBook("book", where)` / `unlinkBook(…)` | Books in use; link to `"chat"`, `"global"`, `"char"` / `{ char }`, `"persona"` / `{ persona }` (asked outside the chat) |
| `await delBook("book")` / `importBook(json)` | Deletes a book (asks) / imports a lorebook (JSON) |
| `getRegex(scope)` / `await setRegex([...], scope)` / `delRegex("name")` | Marinara's regex scripts; scope `{ char: "current" }`, `{ preset: "in_use" }`, `{ global: true }` (asks before changing) |
| `applyRegex(text, { on, mode, depth })` | The text as this chat's regex turns it |
| `await runSlash("/command")` | Every slash command through Dispensa; without it `/send`, `/trigger`, `/gen`, `/genraw`, `/sendas`, `/sys`, `/regen`, `/stop`, `/echo`, `/setinput` |
| `onMsg` / `onSend` / `onSwipe` / `onEdit` / `onDelete` | New reply / your message / swipe shown / text changed / message deleted |
| `onBeforeGen(gen => …)` | Just before a reply (waits up to 5 s): `gen.kind`, `gen.num`, `await gen.inject(text, { depth, role })` |
| `onGenDone(fn)` / `onGenError(fn)` | A reply finished / failed or stopped |
| `await inject("text", { id, depth, role, position, once })` | Text in the next replies' prompts until `remove()`, the page goes, or one reply (`once`) |
| `on("name", fn)` / `emit("name", …values)` | Events between pages and scripts |
| `showToast("text")` | A small note |
| `share("name", value)` / `await getShared("name")` | Hands a value to other pages and scripts / waits for it |
| `catchErrors(fn)` / `reload()` / `getVersion()` | Errors as notes / start again / Fornello's version |

## Is it safe

Pages run inside Marinara: a determined script could reach Marinara's data. These are guard rails, not a security boundary.

* With cards from people you do not know, choose **Click to run**
* A card's components and scripts run only once you allow them
* `localStorage` is the chat's page storage; `fetch` to Marinara's own address is refused
* Asked first: deleting messages, taking entries out of a shared lorebook, more than 12 AI calls a minute (`runSlash` / `triggerSlash` lines count too)

## Notes

* The interpreter is slower than the browser: forms, tabs and quizzes are fine; heavy animation may lag
* Outside scripts (`<script src="https://…">`) are fetched and interpreted; big libraries may be slow or fail
* React 18 or older from a CDN is swapped for Marinara's own (full speed); `ReactDOM.render` works; `<script type="module">` works
* A loop busy for over 4 seconds is stopped with an error (only loops are watched)
* Each `<script>` runs on its own; a top-level `var` is not readable as `window.name`
* Saved images may miss outside images and fonts


## License and credits

* Fornello is released under the [GNU AGPL-3.0](LICENSE). Copyright (C) 2026 FigCake
* Built against Marinara Engine's (AGPL-3.0) API and behavior
* For SillyTavern users, the function names and usage follow [JS-Slash-Runner](https://github.com/N0VI028/JS-Slash-Runner) (PolyForm Noncommercial 1.0.0); none of its code is included
* Bundles [sval](https://github.com/Siubaak/sval) (MIT, the JavaScript interpreter), [acorn](https://github.com/acornjs/acorn) (MIT), [Sucrase](https://github.com/alangpierce/sucrase) (MIT, JSX) with lines-and-columns (MIT), [html-to-image](https://github.com/bubkoo/html-to-image) (MIT, image saving) and [Lucide](https://lucide.dev) icons (ISC); their license texts are in `THIRD_PARTY_NOTICES.txt` and at the end of `extension.js`
* The complete source is `ME-Fornello-<version>-source.zip` in the public repository. If you distribute a modified version or offer it as a service, its source must be released under the AGPL-3.0 too
