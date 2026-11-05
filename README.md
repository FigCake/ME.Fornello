# Fornello

Runs HTML pages, with their scripts, inside [Marinara Engine](https://github.com/Pasta-Devs/Marinara-Engine) chats, and adds components, background scripts, character pictures and story values on top. 한국어: [README-ko.md](README-ko.md)

## What it does

* ```html code blocks in chat messages become running pages, in the theme's colors and font; pages a card's display regex makes run too
* **Components**: status panels under replies, or scenes where the AI puts them
* **Scripts** that run behind the chat
* **Page commands**: pages and scripts can call the AI and work with messages, variables, lorebooks, characters and the tracker
* **Character assets** (off until turned on): pictures and expressions in replies
* **Pesto** (off until turned on): a card's values (health, money, mood, items…) kept by Fornello, the AI only reporting what changed
* **SillyTavern cards**: their variable data and `<% %>` lorebook templates work here too
* English or 한국어 (🌐 in the settings)

## Setup

1. In the server's `.env`, set `ENABLE_EXTERNAL_EXTENSIONS=true` and restart Marinara
2. Settings → Addons → External Extensions: turn on **Allow third-party extension imports**, import `ME-Fornello-<version>.zip`
3. **Fornello** in the list → **Review and Run**: approve the hash (it must equal the one in `SHA256SUMS.txt`)

Or install and update it with [Fetchy](https://github.com/FigCake/ME.Fetchy): add `FigCake/ME.Fornello` as a source.

### Goes well with

* [Dispensa](https://github.com/FigCake/ME.Dispensa): quick replies in a Dock above the input. With both, pages in Dispensa's 🌟 viewer run, 🔥 Fornello in Dispensa opens these settings, a script's buttons can go in the Dock, `runSlash` runs every slash command, and the screen language is shared
* [Polyglot](https://github.com/FigCake/ME.Polyglot): language study in chats. With both, their top-bar menus become one, under the slice of cake
* [Fetchy](https://github.com/FigCake/ME.Fetchy): installs and updates extensions from their GitHub addresses

## Using it

**The Fornello window**: the slice of cake in Marinara's top bar → **Fornello**, ⚙ on a page, or 🔥 Fornello in Dispensa. Its tabs: components, scripts, character assets and **Status** (Pesto); ⚙ in its header holds how pages run and how components show.

| Setting | Means |
|---|---|
| Run scripts | **Run at once** (default: pages of recent messages start when on screen) · **Click to run** · **Off** (components and scripts too) |
| Recent messages to auto-run | How many recent messages have their pages start by themselves, every page in them (default 3) |
| Replies to keep open | How many recent replies show their components open |
| Components of older replies | **Collapse** or **Hide** |

**Buttons on a page** (top right on hover, always on touch screens): **▶ Run** · ↻ run again · 🖼 save as PNG · ⤢ fullscreen (Esc to leave) · ⚙ settings.

## Components

1. **+ New component** (or **Import**), then pick the kind:
   * **JSX**: paste a component; hooks work
   * **HTML**: a template with `{{value}}` places and its own `<style>` (no code runs). `{{a.b}}` reads inside a value, `{{#list}}…{{/list}}` repeats per item, `{{^list}}…{{/list}}` shows when empty
2. **Values from**:
   * **Tag in reply**: the AI writes the tag with values (`<Card hp={40} />`); the tag text is hidden. Put the **Rule for the AI** in the card or a lorebook, or press **Add to this chat's lorebook**. **Status** (every reply, drawn under it) or **Scene** (only when it fits, drawn where it is)
   * **Text in tag**: values as text inside the tag (`<Tag>a|b|c</Tag>`), split by `|` or `:`, or one item a line
   * **Tracker**: Marinara's tracker at each reply. **Put in** writes each value's instruction into the chat's custom tracker fields
   * **Pesto**: the [Pesto](#pesto) values after each reply
3. **Values**: name, kind, example, instruction for the AI, optional swaps (`value=shown`)
4. **Display name**, **New chats** (on or off by default), **Font**; the preview redraws as you edit; **Save**

* **In this chat**: a switch per component for the open chat
* **Theme**: `var(--fc-text)`, `--fc-muted`, `--fc-title`, `--fc-accent`, `--fc-card`, `--fc-line`, `--fc-radius`; classes `fc-card`, `fc-title`, `fc-muted`, `fc-accent`, `fc-badge`, `fc-btn`, `fc-bar` (`<div class="fc-bar" style="--value:70%"><i></i></div>`)
* **Sets** group components; **Export** / **Import** `.json` files, character cards, `.jsx` / `.html` files
* **In a character card**: one lorebook entry per component, named `fornello:Name`, the JSX code as content, turned off. They draw once you press **Allow this card's components**

## Scripts

The **Scripts** tab.

* **Your own** run in every chat; **a chat's** in that chat; **a card's** once you allow them (asked when the chat opens, and again when they change)
* The library: your list, each card's, the chat's; folders, switches, drag and drop; **Import** script files, zips, cards, `.js`; **Export** `.json`
* The editor: syntax errors as you type, buttons, variables, the **Log** and **Restart**
* They start when the chat opens and stop when another opens. Besides every page command:

| Command | What it does |
|---|---|
| `setButtons(["Hello", "Roll"])` / `onButton("Hello", fn)` | A script's buttons, shown where its ⋮ menu puts them (Dispensa's Dock, the Status tab) / what a button does |
| `getScripts(scope)` / `await setScripts([...], scope)` | Read / replace a list (`"global"`, `"char"`, `"chat"`) |
| `getScriptId()` / `getScriptName()` / `getAllEnabledScriptButtons()` | About the script / every running script's buttons |

```js
setButtons(["Roll"]);
onButton("Roll", () => showToast(`🎲 ${1 + Math.floor(Math.random() * 6)}`));
```

## Character assets

The **Character assets** tab → **How they show**: turn the ways on. Then `[img:name]` or `[img:person:name]` in a reply shows that picture after its paragraph, the expression Marinara chose shows above or below the reply, and pictures with words show when a reply uses one. The AI learns the names from a "Credenza" lorebook entry.

In the tab: upload pictures (named after their files), name Gallery pictures, give pictures a person and words, give sprites words, and **Import from a card file** (.charx, PNG or JPEG cards, or a .zip bundle: a PNG, JSON or .charx card with pictures beside it in folders or zips, whose names come from their files, a SillyTavern QR set that goes to Dispensa's Library, and a world info file that becomes a lorebook linked to the character).

Marinara's own character import does the same for a `.charx`, a JPEG card or a card `.zip` (⚙ → **Character import: pictures too**, on unless turned off): the character comes in with its pictures in its Gallery, its lorebook and its QR sets.

In an HTML page (in a reply, or in Dispensa's viewer and Gallery), `{{raw::name}}` becomes the picture's address (`<img src="{{raw::name}}">`) and `[img:name]` the picture, from the open chat's characters. With Dispensa, `/asset name` gives an address and `/assets` the chat's pictures as JSON.

## Pesto

The **Status** tab → **Settings** → **Pesto** on. A card with a `pesto:schema` lorebook entry then has its values kept by Fornello: before each reply the AI is shown the values and how to report changes, and it ends its reply with a block of what changed:

````
```pesto
hp -= 12
bag += "rope"
do rest hours=8
```
````

Fornello checks each change (kind, limits, who may change it), works out events and dice, and remembers the values per message, so a swipe, an edit or a deleted message is counted again from there. The blocks are hidden from the screen and from the AI.

**The Status tab**: the values after any message, what it changed and what was refused; set a value there (✕ takes it back); **Over the chat** graphs each number; **Try a reply** shows what a pasted reply would change, saving nothing; **The card's entries** shows what is wrong in them.

| Setting | Means |
|---|---|
| Where the values go | How many messages from the end of the chat (default 1) |
| SillyTavern variable cards too | Cards without `pesto:schema` that bring their own starting values ([below](#sillytavern-cards)) |
| Who reports the changes | **The AI** · **The AI, a helper model when it forgets** · **A helper model** (the AI only writes the story) |
| Helper model | A cheap, quick connection is enough |
| Give the values to Marinara's prompts | The values as chat variables: `{{getvar::story_hp}}`, `{{#if var:story_hp < 10}}…{{/if}}` (each value's name shows when you point at it in the window) |
| Show in Marinara's tracker | The values as the tracker's custom fields |

### In a card

Lorebook entries in the card's lorebook (or the chat's, which wins), **turned off**, written in YAML or JSON.

`pesto:schema`, the values:

```yaml
fields:
  hp: { type: int, min: 0, max: max_hp, default: 100, note: below 20 the hero can't run }
  max_hp: { type: int, default: 100, write: rules }   # only the card's rules change it
  gold: { type: int, min: 0 }
  mood: { type: enum, of: [calm, angry, sad] }
  bag: { type: list, of: text, max_items: 10 }
  trust: { type: map, of: { type: int, min: -10, max: 10 } }   # trust.mina, trust.ari…
  hero:                                  # a group
    name: { type: text, readonly: true }
    level: 1                             # a kind and a default at once
  night: { type: bool, write: rules }
  enemy_hp: { type: int, min: 0, default: 30 }
  stars: { type: int, show_when: night }  # shown to the AI only while night is true
  secret: { type: text, hidden: true }   # never shown to the AI (also any name starting with _)
derive:
  hurt: hp < 20                          # computed, shown, never written
```

Kinds: `number`, `int`, `text`, `bool`, `enum`, `list`, `map`, `any`; a name without `type` is a group. `min` / `max` keep a value inside; `write` says who may change it (`ai`, `rules`, `user`); `note` is told to the AI.

`pesto:start`, the values at the first message: `gold: 20` and so on.

`pesto:rules`, events the AI can name, steps after every reply, dice and triggers:

```yaml
events:
  rest:
    params: { hours: 8 }                 # what the AI may give, with defaults
    when: "!hurt"                        # refused otherwise (the AI is told)
    note: sleep; heals 5 an hour
    do:
      - add: { hp: hours * 5 }
  buy:
    params: [item, price]
    when: gold >= price
    do:
      - sub: { gold: price }
      - push: { bag: item }
  befriend:
    - add: { trust.{who}: 3 }            # an expression in a path
  search:
    - if: chance(30)
      then:
        - push: { bag: "'old key'" }     # a word as it is: quoted inside the quotes
  attack:
    - if: dice.check >= 12               # this reply's dice
      then:
        - sub: { enemy_hp: dice.damage }
each_reply:
  - add: { turn: 1 }
dice:                                    # rolled before each reply; the AI is told them
  check: 1d20
  damage: 2d6 + 1
triggers:                                # text for the AI while a condition holds
  wounded:
    when: hurt
    text: Ari is badly hurt and limps.
    depth: 2
  night:
    when: night
    entry: Night in the forest           # a lorebook entry (kept off), copied in with its place
```

Steps: `set`, `add`, `sub`, `push`, `pull`, `remove`, `if` / `then` / `else`, `do`. Expressions: `+ - * / %`, comparisons, `&& || !`, `a ? b : c`, paths (`hero.level`, `bag[0]`, `bag.length`), and `min max clamp round floor ceil abs sum len has keys roll("2d6+1") chance(30) pick(list)`.

`pesto:code` (optional): JavaScript run after each reply, once you allow it, cut off from the network. It gets the values and the reply's changes and answers with changes and effects (page commands run once for a new reply):

```js
function onReply({ values, derived, changes, events, dice, text }) {
  if (values.hp <= 0 && !values.dead)
    return { set: { dead: true }, effects: [{ cmd: "toast", args: ["Ari falls."] }] };
}
```

## SillyTavern cards

* **Variable cards** (setting **SillyTavern variable cards too**): a card with `[InitVar]` entries or `<VariableInsert>` tags in its first message gets Pesto from them; a schema in its scripts gives the kinds and ranges (read, not run). Its update tags (`<UpdateVariable>`, `<VariableEdit>`…) count as the AI's changes, and its MVU status pages read Pesto's values. Turn on **Scoped Regex Scripts** in Marinara's chat settings for the card's own regexes. **The card's entries** in the Pesto window copy that data as `pesto:schema` and `pesto:start`
* **Templates** (`<% %>` in lorebook entries): Settings → **SillyTavern templates** lists them (shown when the open chat's cards have some). **Convert** changes their text in place, **Put back** restores it:
  * conditions on values become Marinara's own `{{#if}}`
  * the rest (loops, functions, `setvar`, page commands…) Fornello runs before each reply, once you allow each template, cut off from the network. They can use `getvar` / `setvar` / `incvar` / `decvar`, `getwi`, `getchar`, `execute`, `_`, `user`, `char` and the page commands (`await generate(…)`)
  * **Longest wait before a reply** (5–60 s): give more to templates that call the AI

## Page commands

Message numbers count from 1; a negative one counts from the end (`-1` = the last).

| Command | What it does |
|---|---|
| `sendMsg("text")` / `sendRegen()` | Sends your message and gets the reply / rewrites the last reply |
| `await genChat("prompt", { history })` / `genRaw("prompt")` | Asks the AI with the chat as context / the prompt alone; nothing is saved |
| options | `connection`, `onToken`, `id`; `genChat`: `guide`, `preset`; `genRaw`: `images`, `temperature`, `maxTokens`… |
| `stopGen(id)` / `stopMyGens()` | Stops one / all of the page's own AI calls |
| `getVars(kind?)` / `setVars({ … }, kind?)` | Chat variables (or `"global"`, `"character"`, `"script"`) |
| `await getMsgs()` / `getLastMsg()` / `getMsgId()` | Messages / the last one / the page's own number |
| `setMsg` / `delMsg` / `addMsg` / `hideMsg` / `showMsg` / `branch` | Change, delete, add, hide, show messages; branch the chat |
| `getMsgData(num)` / `setMsgData(num, { … })` | Data kept with a message, unseen by the AI |
| `getSwipes` / `setSwipe` / `addSwipe` / `delSwipe` | Swipes |
| `getInput()` / `setInput("text")` | The chat input |
| `getUser()` / `getChar()` / `setChar` / `addChar` / `delChar` / `importChar` / `useUser` | Personas and characters |
| `getTracker()` / `setTracker({ … })` / `onTracker(fn)` / `runTrackers()` | Marinara's tracker |
| `getState(num?)` / `setState(path, value, num?)` / `onStateChange(fn)` | Pesto's values |
| `getAgents()` / `runAgents([...])` / `getAgentMemory` / `setAgentMemory` | Marinara's agents |
| `genImage("prompt")` / `getAssets(name)` / `getAsset(name)` | Pictures |
| `speak("text")` / `stopSpeaking()` | Reads aloud |
| `getSummary()` / `makeSummary()` / `setSummary([...])` | The chat's summaries |
| `getLore()` / `setLore` / `delLore` / `getBooks` / `linkBook` / `unlinkBook` / `delBook` / `importBook` | Lorebooks |
| `getRegex` / `setRegex` / `delRegex` / `applyRegex` | Marinara's regex scripts |
| `runSlash("/command")` | Slash commands: every one with Dispensa; without it `/send`, `/trigger`, `/gen`, `/genraw`, `/sendas`, `/sys`, `/regen`, `/stop`, `/echo`, `/setinput` |
| `onMsg` / `onSend` / `onSwipe` / `onEdit` / `onDelete` / `onBeforeGen` / `onGenDone` / `onGenError` | Chat events |
| `inject("text", { depth, once })` | Text in the next replies' prompts |
| `on` / `emit` / `share` / `getShared` | Between pages and scripts |
| `showToast("text")` / `reload()` / `getVersion()` | A note / start again / Fornello's version |

## Good to know

* Pages run inside Marinara and could reach its data: with cards from people you don't know, choose **Click to run**. A card's components, scripts, code and templates run only once you allow them
* Asked first: deleting messages, taking entries out of a shared lorebook, many AI calls in a short time
* Pages run in a built-in interpreter, slower than the browser: forms and quizzes are fine, heavy animation may lag. A loop busy for over 4 seconds is stopped

## License and credits

* Fornello is released under the [GNU AGPL-3.0](LICENSE). Copyright (C) 2026 FigCake
* Built against Marinara Engine's (AGPL-3.0) API and behavior
* For SillyTavern cards, the function names and usage follow [JS-Slash-Runner](https://github.com/N0VI028/JS-Slash-Runner) (PolyForm Noncommercial 1.0.0); none of its code is included
* Pesto reads cards made for [MVU (MagVarUpdate)](https://github.com/MagicalAstrogy/MagVarUpdate) (MIT, Copyright (c) 2025 MagicalAstrogy & StageDog): their `[InitVar]` values, `<UpdateVariable>` changes and `registerMvuSchema` schema, following its formats and usage; none of its code is included
* Bundles [sval](https://github.com/Siubaak/sval) (MIT, the JavaScript interpreter), [acorn](https://github.com/acornjs/acorn) (MIT), [Sucrase](https://github.com/alangpierce/sucrase) (MIT, JSX) with lines-and-columns (MIT), [html-to-image](https://github.com/bubkoo/html-to-image) (MIT, image saving) and [Lucide](https://lucide.dev) icons (ISC); their license texts are in `THIRD_PARTY_NOTICES.txt` and at the end of `extension.js`
* The complete source is `ME-Fornello-<version>-source.zip` in the public repository. If you distribute a modified version or offer it as a service, its source must be released under the AGPL-3.0 too
