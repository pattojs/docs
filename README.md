<div align="center">
  <img
    src="https://avatars.githubusercontent.com/u/314748062?v=4"
    title="Pattojs"
    alt="Pattojs logo"
    width="96"
  />
  <p>
    <em>
      The local sandbox for JavaScript <br />developers
    </em>
  </p>
</div>

# Pattojs

A JavaScript playground that lives on your desktop. Write code, press **Play**, and watch the output appear in the built-in console. No terminal, no installation, no configuration.

> [!IMPORTANT]
> Pattojs is currently in Beta. This means it's still being actively tested and refined. You can use it normally, but you might occasionally run into bugs or performance changes.

## Quick start

1. Open Pattojs and select **New File...**
2. Type some code:

   ```js
   console.log("Hello, world!");
   console.log(2 + 2);
   ```

3. Press **Play**. Output appears in the console below.

## Tutorials

Follow these step by step to learn the basics.

### Hello, world

1. Open Pattojs and select **New File...**
2. Type the following:

   ```js
   console.log("Hello, world!");
   console.log(2 + 2);
   console.log("Apples:", 3, "Bananas:", 7);
   ```

3. Press **Play**. Three lines appear in the console.
4. Change something and press **Play** again. Each run starts clean.
5. Try variables and template literals:

   ```js
   const fruit = "mango";
   const count = 12;
   console.log(`I bought ${count} ${fruit}s today.`);
   ```

### Explore data with tables

```js
const inventory = [
  { item: "apple", qty: 12, price: 0.4 },
  { item: "banana", qty: 0, price: 0.25 },
  { item: "cherry", qty: 300, price: 3.1 },
];

console.log(inventory); // Expandable tree
console.table(inventory); // Table view
console.table(inventory, ["item", "qty"]); // Specific columns
```

### Fetch live data

```js
const res = await fetch("https://jsonplaceholder.typicode.com/todos");
const todos = await res.json();
console.log("Received", todos.length, "todos");

const unfinished = todos.filter((t) => !t.completed && t.userId === 1);
console.table(unfinished.slice(0, 10), ["id", "title"]);
```

### Keep secrets with Props

1. Open **Settings** > **Properties**
2. Add key `USERNAME` with your name
3. Run:

   ```js
   const user = Props.get("USERNAME");
   if (user === null) {
     console.error("USERNAME not set. Add it in Settings > Properties.");
   } else {
     console.log(`Welcome back, ${user}!`);
   }
   ```

### Build a currency converter

1. Add `BASE_CURRENCY` = `USD` in Properties
2. Run:

   ```js
   const base = Props.get("BASE_CURRENCY") ?? "EUR";

   async function getRate(from, to) {
     const url = `https://api.frankfurter.app/latest?from=${from}&to=${to}`;
     const res = await fetch(url);
     if (!res.ok) throw new Error(`API error ${res.status}`);
     const data = await res.json();
     return data.rates[to];
   }

   const rate = await getRate(base, "JPY");
   console.info(`1 ${base} = ${rate} JPY`);

   console.table(
     [10, 50, 100, 500].map((amount) => ({
       amount,
       converted: Number((amount * rate).toFixed(2)),
     })),
   );
   ```

### Read error messages

- `ReferenceError: X is not defined` — typo in a variable name
- `TypeError: Cannot read properties of null` — use optional chaining: `user?.name`
- `RangeError: Infinite loop prevented` — loops are capped at 2000 iterations

## How-to guides

### Save and reopen scripts

- **Save:** `Ctrl+S`
- **Open:** `Ctrl+O`
- **Save As:** `Ctrl+Shift+S`

### Change the theme

1. Select **Manage** (gear icon)
2. Under **Common**, switch **Color scheme** between Light and Dark

### Import a library

You can import from URLs or npm packages directly:

```js
import { z } from "https://esm.sh/zod";
```

Or using the npm prefix:

```js
import { z } from "npm:zod";
```

Example usage:

```js
const User = z.object({
  name: z.string(),
  age: z.number().int().positive(),
});

console.log(User.safeParse({ name: "Ada", age: 36 }).success);
```

### Recover from a stuck script

1. Press **Stop** — the runtime is killed immediately
2. Clear console if needed
3. Fix code and press **Play** again

## Reference

### Keyboard shortcuts

| Shortcut         | Action           |
| ---------------- | ---------------- |
| `Ctrl+O`         | Open file        |
| `Ctrl+S`         | Save             |
| `Ctrl+Shift+S`   | Save as          |
| `Ctrl+Q`         | Quit             |
| `Ctrl+Z`         | Undo             |
| `Ctrl+Y`         | Redo             |
| `Ctrl+X / C / V` | Cut, copy, paste |

### Settings

Open with the **Manage** button (gear icon).

| Setting             | Values      | Default |
| ------------------- | ----------- | ------- |
| Color scheme        | Light, Dark | Dark    |
| Font size (editor)  | 10–24 px    | 14      |
| Font size (console) | 9–20 px     | 13      |
| Tab size            | 1–8 spaces  | 2       |
| Line wrapping       | on, off     | on      |

### Props

Store tokens in **Settings > Properties**. Access them in code:

```js
const token = Props.get("MY_TOKEN");
```

- Values are always strings
- Missing keys return `null`
- Keys are case-sensitive

## Next steps

1. Work through the [tutorials](#tutorials) in order
2. Move tokens into [Props](props/README.md) before saving scripts
3. Check the [how-to guides](#how-to-guides) when you need help with a task

Want to understand how Pattojs works under the hood? Read the [architecture notes](architecture/README.md).
