# TalkJS CLI

Use this with the main [TalkJS skill](../SKILL.md) to create and configure a TalkJS test app. The CLI handles account credentials and test app settings; the platform guides cover adding chat UI and data to the user's app. The CLI supports test apps only. Use the [TalkJS dashboard](https://talkjs.com/dashboard/) for live app configuration.

## Install

The CLI requires Node.js 22.18 or newer. If `talkjs --help` already works, use the installed CLI. Otherwise, install it from npm:

```sh
npm install --global @talkjs/cli
talkjs --help
```

Alternatively, run it without a global installation:

```sh
npx @talkjs/cli --help
```

For the examples below, substitute `npx @talkjs/cli` for `talkjs` when using this approach. If installation fails, resolve the installation problem before provisioning an account.

## Commands and options

These are all current user-facing commands. Brackets mean optional arguments; angle brackets mean required arguments. Replace placeholders rather than typing them literally.

| Command | Purpose |
| --- | --- |
| `talkjs anonymous-account create` | Create a new anonymous test account, save its credentials, and select it |
| `talkjs anonymous-account claim [appId]` | Claim the selected or explicitly named saved account and connect the CLI through the browser |
| `talkjs login` | Authorize and select one existing project's test app through the browser |
| `talkjs whoami` | Verify the active anonymous account or OAuth account/project |
| `talkjs logout` | Disconnect the active account without deleting the project |
| `talkjs settings get [field]` | Read all supported settings or one field |
| `talkjs settings set <field> <value>` | Update one setting, or merge a JSON object into `custom` |
| `talkjs settings unset custom.<key>` | Delete one custom field |
| `talkjs secret-keys list` | List key IDs and metadata, not secret values |
| `talkjs secret-keys get <id>` | Retrieve one key, including its secret |
| `talkjs secret-keys create` | Generate a new key and return its secret |
| `talkjs secret-keys set <id> <field> <value>` | Set `active` or `usedForWebhooks` to `true` or `false` |
| `talkjs secret-keys delete <id>` | Delete an inactive, non-webhook key |
| `talkjs webhooks list` | List the selected app's webhook configurations |
| `talkjs webhooks get <id>` | Read a webhook's URL and event list |
| `talkjs webhooks create <url> --events <json>` | Create a webhook with its initial subscriptions |
| `talkjs webhooks set <id> <field> <value>` | Change `url` or replace `events` |
| `talkjs webhooks delete <id>` | Delete one webhook |

| Option | Where it applies | Meaning |
| --- | --- | --- |
| `--json` | Any command | Structured stdout; prompts/warnings/errors on stderr; never launch a browser |
| `--no-open` | `login` and `anonymous-account claim` only | Print the browser URL without launching it; still wait for completion |
| `-h`, `--help` | Root or a subcommand | Show help for that command |
| `-V`, `--version` | Root | Print the CLI version |

Global options such as `--json` can precede or follow the command. The CLI has no anonymous-account `list`/`use`, project-selection override, batch `--set`, or force-delete command.

Environment variables for normal use:

- `XDG_CONFIG_HOME`: parent of the `talkjs` credential directory; keep it consistent across related commands. It is not a per-project app selector.
- `TALKJS_ACCESS_TOKEN`: explicit anonymous token for **claim only**, overriding saved anonymous credentials. It does not log in, select an account for settings, or override `whoami`/`logout`. If `claim <appId>` is also supplied, that app ID must match the token. Supply secrets through the environment securely, not literal shell history.

## Reuse and targeting

Before provisioning, inspect the app's existing TalkJS configuration and retained app ID. Reuse them. Before changing settings or retrieving keys, run `talkjs whoami --json` and compare its `value.appId` (anonymous) or `value.project.appId` (OAuth) with the app being integrated. Settings reads do not include the app ID. A failed identity or settings read does not mean a new account is needed.

The selected account is saved in the CLI's configuration and shared across working directories. Changing directories does not select a different app. The latest successful create, login, or CLI claim selects the target for settings; it does not delete other saved anonymous accounts. Creating anonymously while logged in selects the new anonymous account; logging in while anonymous selects the authorized project instead. Commands never fall back to an inactive connection when the active one fails or is logged out.

Settings commands have no `--app` override. Do not edit credential files to switch accounts. If the selected account differs from the app being worked on, leave it untouched; for a claimed project, ask the user to select it through login. There is no command to select an older unclaimed account for settings without claiming it. Do not force claiming just to switch targets: existing app integration can continue with its known app ID when no CLI settings changes are needed.

`whoami` verifies the active anonymous account or OAuth login. Anonymous creation and configuration do not require browser login.

## Create and configure

When a new test project is needed:

```sh
talkjs anonymous-account create --json
```

This creates a new anonymous account every time. Read the account from `value`: save `value.appId` in the app's configuration. Keep `value.claimUrl` private; it is an ownership-only fallback, not the normal handoff. Prefer [the CLI claim flow](#claiming-and-existing-projects). The CLI saves credentials outside the app's repository in `$XDG_CONFIG_HOME/talkjs/config.json`, or `~/.config/talkjs/config.json` by default. Files contain plaintext credentials, protected by private file permissions on Unix. Keep using the same config location for subsequent commands. Do not print or manually edit the credential file, or include the access token in the user-facing handoff.

## Settings

Configure only settings relevant to the app, then read them back:

```sh
talkjs settings set name "Marketplace chat" --json
talkjs settings set defaultLocale en --json
talkjs settings set custom.websiteUrl "https://example.com" --json
talkjs settings get --json
```

Use the app's actual name, locale, and metadata rather than copying example values. Every supported setting can be read with `settings get <field>`:

| Field | Values for `settings set` | Behavior |
| --- | --- | --- |
| `name` | Nonempty string | Test app name, not the project's dashboard label |
| `defaultLocale` | Supported TalkJS locale code | Default chat UI language; the backend validates and may normalize the code |
| `custom` | JSON object of string/null values, or `null` | Merge entries; null entries delete keys; whole-object null clears all custom fields |
| `custom.<key>` | Literal string | Set one custom key; everything after `custom.` is the key, not a nested JSON path |
| `authentication` | `required` or `optional` | Require backend proof of user identity, or allow clients without it |

For example, to change the chat language to Dutch:

```sh
talkjs settings set defaultLocale nl --json
talkjs settings get defaultLocale --json
```

Read back the canonical locale (for example, `en` becomes `en-US`). If the UI still uses another language, inspect existing SDK/session locale overrides using the platform's documentation rather than repeatedly changing app settings or creating another account. This changes TalkJS's default language, not the surrounding app's own translations.

Only change authentication for requested security work, following [the backend guide](backend.md). Prepare valid backend-generated user tokens before `talkjs settings set authentication required --json`. `optional` still validates authentication when supplied, but permits clients without it and therefore permits impersonation; do not disable an existing requirement to make a demo work. Read it with `talkjs settings get authentication --json`. Both writes print a warning to stderr. Do not invent roles, permissions-preset, upload-limit, or live-app commands.

For later reads or custom changes:

```sh
talkjs settings get name --json
talkjs settings get defaultLocale --json
talkjs settings get custom --json
talkjs settings get custom.websiteUrl --json
talkjs settings unset custom.websiteUrl --json
talkjs settings set custom '{"websiteUrl":"https://example.com","tagline:pl":"Cześć"}' --json
talkjs settings set custom '{"websiteUrl":null}' --json
talkjs settings set custom null --json
```

Custom values are strings. Bulk JSON merges keys; a null entry deletes that key. `unset custom.<key>` deletes one key without touching others; setting a single key to `null` stores the literal string `"null"`. Avoid `settings set custom null` unless the user intends to clear every custom field. Custom metadata may be readable by chat clients; never store credentials or private user data there.

Read JSON from stdout and check the exit code. Errors go to stderr as `{"status":"error","value":"..."}` and exit with status 1. Settings reads return `{"status":"ok","value":...}`: a string for an individual field, an object for `custom`, or the full settings object for bare `get`. Successful settings writes return `{"status":"ok","value":{...}}`, with `appId`, `name`, `defaultLocale`, `custom`, and `authentication` inside `value`. Verify stored values, including normalized locales such as `en` becoming `en-US`.

Creation is never automatically retried. On an ambiguous network failure, an account may already exist: stop and report the uncertainty rather than repeatedly creating accounts. If creation succeeds but saving credentials fails, the CLI prints the creation credentials in an `ok` envelope on stdout, prints an `error` envelope on stderr, and exits nonzero; preserve those credentials privately instead of creating again. Settings require saved credentials and there is no import command, so report CLI configuration as blocked until persistence is resolved; frontend integration can still use the returned app ID. For a definite rate-limit response, wait at least 30 seconds before a deliberate retry. Do not create a new account for a settings validation or permission error.

## Secret keys

Retrieve a secret only when implementing REST calls or backend authentication. Reuse a key already configured on the app's backend; otherwise verify the selected app with `whoami` as above, then list its keys:

```sh
talkjs secret-keys list --json
talkjs secret-keys get <id> --json
```

`list` returns `{"status":"ok","value":[...]}` with metadata only: `id`, `createdAt`, `active`, and `usedForWebhooks`. Choose an active key and pass its 12-character `id` to `get`, not the secret itself. `get` returns `{"status":"ok","value":{"id":"...","secretKey":"...",...}}`; use `value.secretKey` as the backend credential. It also returns inactive keys, so check `active` before using one.

These commands work for the selected unclaimed anonymous test app or the selected OAuth-authorized test app. They use existing credentials and permissions, including OAuth renewal. There is no per-command app override and no need to read tokens from the CLI config or call the anonymous-account lookup endpoint to retrieve a key.

Capture the secret privately and configure it in the app's server-side environment or Gitignored local configuration. The CLI does not save retrieved/generated secret keys. Do not put them in frontend code, custom fields, URLs, source control, user-facing output, or continuation summaries. Use the returned key yourself rather than asking the user to copy it from the dashboard. See [backend.md](backend.md) for REST usage and authentication.

When a new key is actually needed, or the user requests key management:

```sh
talkjs secret-keys create --json
talkjs secret-keys set <id> active false --json
talkjs secret-keys set <id> active true --json
talkjs secret-keys set <id> usedForWebhooks true --json
talkjs secret-keys delete <id> --json
```

`create` returns the new key's metadata and secret in `value`. `set` returns metadata only; it changes one field (`active` or `usedForWebhooks`) to `true` or `false`, leaving the other property unchanged. A successful `delete` returns `{"status":"ok","value":null}`. Only `get` and `create` reveal secrets. Do not rotate, deactivate, delete, or change webhook signing keys as routine setup or cleanup.

To replace a webhook signing key, select an active replacement with `set <replacement-id> usedForWebhooks true`; if it is inactive, first run `set <replacement-id> active true`. The current webhook key cannot be unselected, deactivated, or deleted before a replacement is selected. Deletion requires an inactive, non-webhook key. An app can have at most 10 keys, including inactive ones; do not delete existing keys just to make space without authorization.

Network failures do not automatically retry key mutations. If creation may have succeeded, inspect `list` before deliberately creating again. Permission, expired-credential, or key-limit errors are not reasons to create a replacement account. Claiming or logging out does not revoke an already retrieved chat secret key; it remains valid until deactivated, expired, or deleted.

## Webhooks

Use webhooks when the requested integration needs TalkJS events delivered to its backend. Verify the selected app with `whoami` first. Creation/editing requires a plan with webhooks; an anonymous project has delivery disabled. If that blocks requested webhook setup, explain the restriction and offer the CLI claim flow below. Do not create another project or claim on the user's behalf.

```sh
talkjs webhooks list --json
talkjs webhooks create "https://example.com/webhooks" --events '["message.sent","message.read"]' --json
talkjs webhooks get <id> --json
talkjs webhooks set <id> url "https://example.com/new-webhooks" --json
talkjs webhooks set <id> events '["message.sent","message.updated"]' --json
talkjs webhooks delete <id> --json
```

Replace example URLs with the user's actual backend endpoint and `<id>` with the UUID returned by `list` or `create`, not a URL or list index. Each webhook has `{id, url, events}`. `list` returns an array in `value`, `get`/`create`/`set` return one object, and successful deletion returns null. `set` changes only the named field; `events` is a JSON array that replaces the whole subscription list. `[]` stops subscriptions without deleting the configuration or freeing its plan slot. Duplicate events are removed.

Supported events:

- `message.sent`, `message.read`, `message.updated`, `message.deleted`
- `conversation.deleted`, `user.created`, `user.updated`
- `notification.triggered`, `notification.sent`, `notification.delivered`, `notification.bounced`, `notification.unsubscribed`, `notification.opened`, `notification.link.clicked`

Read the [webhook documentation](https://talkjs.com/docs/Webhooks/) for payloads, signature verification, and delivery behavior. Only configure an endpoint the user controls and intends to receive these events. The URL must use HTTP(S), with no embedded username/password or fragment. Do not expose webhook URLs or private query parameters in frontend code, public output, or handoff summaries. Configuring a webhook can start event delivery; do not create subscriptions as incidental demo cleanup or to test an unrelated feature.

Use the existing webhook signing key; selecting a replacement remains `secret-keys set <key-id> usedForWebhooks true` and must be intentional. Creating a webhook does not rotate keys or send a test request. Reuse an existing matching subscription when appropriate. Plan limits and existing settings permissions apply. Do not delete another webhook to make room without authorization. Mutations are not automatically retried after an ambiguous network failure; inspect `list` before a deliberate retry.

## Claiming and existing projects

Claiming is not required to get chat working. When the user is ready to take ownership, prefer the CLI claim flow rather than sending the plain claim URL from account creation. Start:

```sh
talkjs anonymous-account claim --json
```

This claims the selected anonymous account; check its app ID with `whoami` against the integration first. An optional positional app ID can target an older saved anonymous account. This selector applies only to claiming, not settings or secret-key commands.

Keep the command running while the user signs in or signs up and confirms **Claim and connect CLI**. In JSON mode it does not open a browser: forward only `value.claimUrl` from the `ok` message printed to stderr to the intended owner, not the plain link from account creation. Do not approve on their behalf. The callback must reach the same computer as the CLI within five minutes. In a remote agent environment, explain this limitation instead of claiming that a link opened on the user's laptop will complete the remote CLI login. If the callback cannot reach the CLI, the plain creation link is a fallback for transferring ownership only; explain that it will not connect that CLI.

On success the CLI saves an OAuth login for the claimed project's test app, and the anonymous token stops working immediately. Verify `value.project.appId` through `whoami --json` matches the integration's app ID, then read settings again. No separate login is needed after a successful CLI claim.

If the user independently claims through the plain creation link, ownership changes but the CLI still holds invalid anonymous credentials. This is claim-related invalidation, not early OAuth expiry; the CLI is not automatically logged in. Do not retry the claim or create a replacement project. Run `talkjs login --json` and have the user select the same project, then verify its app ID. Use the same recovery if claiming succeeds but the waiting CLI command cannot finish. Login still requires a browser callback that can reach that CLI; it does not bypass the remote-environment limitation.

For an existing account, `talkjs login --json` prints an `ok` message with `value.authorizationUrl` to stderr and waits for browser approval and project selection with the same callback restriction. Login authorizes one project's test app using the user's existing permissions. Logging in again selects a different project. OAuth access tokens last one hour and renew automatically; refresh tokens expire after 30 days without renewal. If renewal fails or access is revoked, log in again rather than creating another anonymous account.

Interactive `login` and `claim` normally open the default browser. `--no-open`, `--json`, and non-interactive terminals print the URL instead; all still wait up to five minutes. `--no-open` is not supported by create, settings, whoami, or logout. Browser-launch failure leaves the URL available for manual opening while the command waits.

`talkjs whoami --json` verifies the selected anonymous account or OAuth identity and project. `talkjs logout --json` revokes/removes the active OAuth credential, or deselects an anonymous account without deleting it. Anonymous logout keeps its saved token for a later explicit `claim <appId>`; it does not revoke a previously saved inactive OAuth session. Logout leaves nothing selected. Dashboard logout does not log the CLI out. Do not log the user out as routine demo cleanup.

## JSON result shapes

Every message with `--json` is a JSON object on its own line with the envelope `{ "status": "ok" | "warning" | "error", "value": ... }`. Successful command results use `status: "ok"` on stdout. Read command-specific fields from `value`:

| Command | stdout with `--json` |
| --- | --- |
| `anonymous-account create` | `{ "status": "ok", "value": { "appId": "...", "accessToken": "...", "claimUrl": "..." } }` |
| `login`, `anonymous-account claim` | `{ "status": "ok", "value": { "account": { "id": "...", "email": "..." }, "project": { "id": "...", "name": "...", "appId": "...", "environment": "test" }, "expiresAt": 1234567890000 } }` (expiry is Unix milliseconds) |
| `whoami` (anonymous) | `{ "status": "ok", "value": { "anonymous": true, "appId": "..." } }` |
| `whoami` (OAuth) | `{ "status": "ok", "value": { "account": { ... }, "project": { ... } } }` |
| `logout` | `{ "status": "ok", "value": { "loggedOut": true } }` |
| `settings get [field]` | `{ "status": "ok", "value": ... }` |
| `settings set`, `settings unset` | `{ "status": "ok", "value": { "appId": "...", "name": "...", "defaultLocale": "...", "custom": { ... }, "authentication": "optional" } }` |
| `secret-keys list` | `{ "status": "ok", "value": [ ...metadata ] }` |
| `secret-keys get`, `create` | `{ "status": "ok", "value": { ...metadata, "secretKey": "..." } }` |
| `secret-keys set` | `{ "status": "ok", "value": { ...metadata } }` |
| `secret-keys delete` | `{ "status": "ok", "value": null }` |
| `webhooks list` | `{ "status": "ok", "value": [ { "id": "...", "url": "...", "events": [ ... ] } ] }` |
| `webhooks get`, `create`, `set` | `{ "status": "ok", "value": { "id": "...", "url": "...", "events": [ ... ] } }` |
| `webhooks delete` | `{ "status": "ok", "value": null }` |

Help and version output also use `status: "ok"`, with text in `value`. Help may emit multiple messages.

Examples use `...` to abbreviate values, not as literal JSON to send. Login writes `{ "status": "ok", "value": { "authorizationUrl": "..." } }` to stderr while waiting; claim writes `{ "status": "ok", "value": { "appId": "...", "claimUrl": "..." } }`. Warnings are `{ "status": "warning", "value": "..." }` on stderr and do not cause a nonzero exit code. Errors are `{ "status": "error", "value": "..." }` on stderr with exit status 1; success exits 0. Keep stdout separate from stderr when parsing results, and treat creation output, claim links, and secret-key results as credentials.
