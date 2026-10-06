---
name: talkjs
description: Add TalkJS chat to a new or existing web, React Native, or Flutter app. Choose the appropriate platform guide, connect the app's users and conversations, and integrate backend authentication when needed.
license: MIT
metadata:
  author: talkjs
  version: "1.0.0"
---

# Add TalkJS to an app

## Start with the project

If chat placement is unclear, infer the best fit from the app. When multiple placements or conversation scopes are plausible (e.g. per project or per card), recommend one and ask the user to choose before implementing. Otherwise, proceed.

Inspect the existing app's framework, package manager, navigation, users, and backend authentication. Extend that app. If the user wants a new project, scaffold their chosen framework first, then add chat to it. Preserve the requested scope: implement when asked to implement; give advice when asked for advice.

Unless the user requests secure chat or production deployment, first deliver a working local proof of concept (POC) with test users and data. Use client-side user and conversation setup where possible. Defer adding TalkJS backend authentication and configuring dashboard security settings until the POC works and the user chooses that follow-up. An app already having a backend or login does not by itself put TalkJS authentication in scope. Preserve the app's existing access controls and any existing TalkJS security settings.

Read only the relevant companion files:

| Project | Read next |
| --- | --- |
| React web, including Next.js | [references/react.md](references/react.md) |
| Vue, Svelte, Angular, or plain browser JavaScript | [references/web-components.md](references/web-components.md) |
| React Native, including Expo | [references/react-native.md](references/react-native.md) |
| Flutter | [references/flutter.md](references/flutter.md) |
| Requested TalkJS authentication or enforced private membership, necessary REST API calls, or production deployment | Also read [references/backend.md](references/backend.md) |

These are companion instructions within this skill.

For new web integrations, use the modern Components SDKs. For mobile, use the React Native or Flutter SDK; both currently wrap the Classic web SDK in a WebView. Follow the selected platform guide. If a web project already uses a Classic TalkJS SDK, retain it unless migration is requested and follow its own reference: [Classic JavaScript](https://talkjs.com/docs/UI_Components/JavaScript/Classic.md) or [Classic React](https://talkjs.com/docs/UI_Components/React/Classic.md).

## Get a TalkJS project

Reuse the TalkJS project already configured in the app or retained in context. If there is no project yet, create an anonymous test project without requiring the user to sign up. Set the request's `Content-Type` header to `application/json`:

```http
POST https://api.talkjs.com/v1/anonymousAccounts
```

The response contains `appId`, `accessToken`, and `claimUrl`. Use the returned `appId` in the SDK integration. Retain `appId` and `accessToken` in your working context and carry them into continuation summaries so future work can reuse the same project. Retain `claimUrl` for the user as well. Keep `accessToken` private; do not put it in frontend code or commit it to the repository. For REST API calls or backend authentication, read [references/backend.md](references/backend.md).

Configure the returned `appId` in the app yourself and enable the chat entry point. If the implementation needs backend credentials, retrieve and configure them yourself using the backend guide. A working anonymous-project POC does not require claiming the project or changing dashboard settings; do not leave placeholder credentials or ask the user to finish local configuration after provisioning succeeds.

Anonymous projects are for testing: REST is available, uploads are limited to 1 MB, and features with external side effects are disabled. Creation is limited to one project per IP every 30 seconds; reuse the project rather than creating one for each step or retry.

If the user wants to change dashboard settings, invite colleagues, or push the chat live to production, they must first **claim the project** by visiting the returned `claimUrl`, creating a proper TalkJS account (or logging into an existing one), and confirming the claim. Give the user this URL and explain when claiming is required. Claiming keeps the project in test mode; production setup is still a separate step.

## Connect chat to the app

Choose identities and conversation granularity from the app's actual users and domain objects. Read [user IDs](https://talkjs.com/docs/Concepts/Users.md) and [conversation IDs](https://talkjs.com/docs/Concepts/Conversations.md). Keep identities stable, and use the same mapping throughout the frontend and backend.

For data setup with the modern web SDKs, read the [JavaScript Data API](https://talkjs.com/docs/Data_APIs/JavaScript.md), then the exact [UserRef.createIfNotExists](https://talkjs.com/docs/Data_APIs/JavaScript/Users.md), [ConversationRef.createIfNotExists](https://talkjs.com/docs/Data_APIs/JavaScript/Conversations.md), and [ParticipantRef.createIfNotExists](https://talkjs.com/docs/Data_APIs/JavaScript/Participants.md) methods. For profile or metadata updates, consult their `set` methods. For SDK-specific behavior, use the selected SDK's component/API reference: shared concept pages sometimes describe Classic behavior, including which conversations appear in an Inbox.

Open the linked docs before implementing that part. Markdown guides expose all language examples. If a reader rejects `text/markdown`, fetch the URL as plain text or replace `.md` with `/` for HTML. Search the returned documentation for the named section or API member, and read only the sections needed for the task.

## Verify the integration

Run the app and the project's relevant checks and build. Before calling the POC working, send a real TalkJS message from one participant, receive it and reply as another participant, and reload the chat to confirm the messages persist. Check that the chat is reachable through the intended app entry point without the user supplying credentials or claiming the project.

Exercise first-time Messages access, switching users/topics, and recovery from a failed setup. Verify that unrelated users/topics have separate histories; this alone does not prove access restrictions are enforced. For requested security work or a deployment, also verify authentication and membership restrictions.

Use the app's existing loading, error, and authentication patterns. Report what was actually tested. If provisioning, configuration, or live messaging fails, report the POC as blocked with the concrete reason. If browser or device verification is unavailable, say the POC is unverified. A successful build, mocked tests, or a functioning setup screen does not count as a working POC.

## Offer the security follow-up

After verifying a working POC that does not yet enforce TalkJS authentication, say: "The chat works, but it is not secured for real users yet. Shall I add TalkJS authentication to your backend next?" Adapt the question if the app has no backend. Keep the POC usable while awaiting the answer; do not begin this follow-up unless the user requests it. When they do, follow [references/backend.md](references/backend.md), including dashboard enforcement and membership restrictions as appropriate.
