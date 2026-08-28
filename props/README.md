# Props

Props is a key/value store for the configuration your scripts need: API
tokens, webhook URLs, usernames, and similar values. You add them in
Settings, and your scripts read them through the global `Props` object.

## Managing props

Open **Settings > Properties**:

| Goal           | Action                                                                     |
| -------------- | -------------------------------------------------------------------------- |
| Add or update  | Type a key and value, then select **Add**. An existing key is overwritten. |
| Reveal or hide | Select the eye icon. Values stay masked until revealed.                    |
| Copy           | Select the copy icon.                                                      |
| Delete         | Select the trash icon.                                                     |

Changes apply immediately; nothing needs a restart.

## Using Props in scripts

- `Props.get(key)` returns the value as a string, or `null` if missing.
- `Props.getAll()` returns every property as a plain object.

Behavior rules:

- Values are always strings; convert explicitly with `Number(...)`,
  `=== "true"`, or `JSON.parse(...)`.
- Missing keys return `null`; calls never throw.
- Keys are case-sensitive: `my_token` and `MY_TOKEN` differ.
- Every read sees the latest edits, even mid-run.
- Scripts can't write props; only the app UI does.

## Security notes

- Props are plain text on disk and aren't encrypted.
- Any script you run can read all props, and the sandbox allows network
  access, so only run code you trust and delete props you no longer use.
- For high-value credentials, we recommend your operating system keychain;
  paste only short-lived tokens into Props.

## See also

- [Main documentation](../README.md) — tutorials, how-to guides, and reference.
- [Architecture notes](../architecture/README.md) — why the sandbox allows
  network access while locking down everything else.
