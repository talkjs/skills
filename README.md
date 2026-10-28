# TalkJS plugin and skill

Agent skills that let AI coding agents add [TalkJS](https://talkjs.com) chat to your app. They work with Codex, Cursor, Claude Code, GitHub Copilot, and other agents that support [Agent Skills](https://agentskills.io).

## Install

### Agent Skills

```sh
npx skills add talkjs/skills
```

The command asks which agents you use and installs the skill for each of them.

Prefer not to install anything? Paste this prompt into your agent instead:

```
Add TalkJS chat to my app: talkjs.com/SKILL.md
```

### Cursor plugin

Once the plugin is listed, install it from its [Cursor Directory](https://cursor.directory) page. To test a local checkout before publication:

1. Copy the repository contents, including `.cursor-plugin/`, into `~/.cursor/plugins/local/talkjs/`.
2. Restart Cursor or run **Developer: Reload Window**.
3. Open **Customize** and confirm that the `talkjs` skill appears.

The plugin packages the same skill and platform guides as the Agent Skills installation. It uses HTTP requests to provision test projects; no TalkJS CLI or MCP server is required or bundled.

## What to ask your agent

- "Add a chat between buyers and sellers to my Next.js marketplace"
- "Add a messages page with an inbox to my Vue app"
- "Add a chat button to the order screen in my Expo app so customers can message their driver"
- "Secure our TalkJS chat with backend authentication"

Say who should talk to whom and where the chat goes (a button on a screen, a tab, a page). If you leave it out, your agent will suggest a spot and ask you to confirm before building.

## What the skill does

1. **Reads your project.** It detects your framework, users, and backend, and adds chat to your existing app instead of starting a new one.
2. **Creates a TalkJS account with a test project if you don't have one.** No sign-up needed: you can claim the account later with the link the agent gives you.
3. **Builds a working version first.** Your agent sets up chat with test users and conversations so you can see it running locally.

## Supported platforms

The [`talkjs` skill](skills/talkjs/SKILL.md) works with React and Next.js, Vue, Svelte, Angular, plain JavaScript, React Native and Expo, and Flutter. It also covers backend authentication and the REST API. It picks the right platform guide for your project, so point your agent at the skill, not at the individual guides.

## Update

```sh
npx skills update talkjs
```

## Feedback

This repository syncs from the TalkJS codebase, so we can't merge pull requests here. To report a problem or suggest an improvement, [open an issue](https://github.com/talkjs/skills/issues), or [chat with us](https://talkjs.com/?chat).

## Resources

- [TalkJS documentation](https://talkjs.com/docs)
- [Agent Skills specification](https://agentskills.io)

## License

[MIT](LICENSE.txt)
