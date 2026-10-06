# TalkJS skills

Agent skills that let AI coding agents add [TalkJS](https://talkjs.com) chat to your app. They work with Codex, Cursor, Claude Code, GitHub Copilot, and other agents that support [Agent Skills](https://agentskills.io).

## Install

```sh
npx skills add talkjs/skills
```

The command asks which agents you use and installs the skill for each of them.

Prefer not to install anything? Paste this prompt into your agent instead:

```
Add TalkJS chat to my app: talkjs.com/SKILL.md
```

## What to ask your agent

- "Add a chat between buyers and sellers to my Next.js marketplace"
- "Add a messages page with an inbox to my Vue app"
- "Add TalkJS chat to my Expo app"
- "Secure our TalkJS chat with backend authentication"

## What the skill does

1. **Reads your project.** It detects your framework, users, and backend, and adds chat to your existing app instead of starting a new one.
2. **Creates a test project if you don't have one.** You can try TalkJS without signing up, and claim the project later with the link the agent gives you.
3. **Builds a working version first.** Your agent sets up chat with test users and conversations so you can see it running locally.
4. **Adds authentication when you're ready.** When you ask, it connects your backend, generates user tokens, and turns on TalkJS security settings for production.

## Skills

| Skill | Use it for |
| --- | --- |
| [`talkjs`](talkjs/SKILL.md) | Adding TalkJS chat to a new or existing app |

The `talkjs` skill includes guides for:

| Platform | Guide |
| --- | --- |
| React and Next.js | [react.md](talkjs/references/react.md) |
| Vue, Svelte, Angular, and plain JavaScript | [web-components.md](talkjs/references/web-components.md) |
| React Native and Expo | [react-native.md](talkjs/references/react-native.md) |
| Flutter | [flutter.md](talkjs/references/flutter.md) |
| Backend authentication and REST API | [backend.md](talkjs/references/backend.md) |

## Update

```sh
npx skills update
```

## Feedback

This repository syncs from the TalkJS codebase, so we can't merge pull requests here. To report a problem or suggest an improvement, [open an issue](https://github.com/talkjs/skills/issues), or [chat with us](https://talkjs.com/?chat).

## Resources

- [TalkJS documentation](https://talkjs.com/docs)
- [Agent Skills specification](https://agentskills.io)

## License

[MIT](LICENSE)
