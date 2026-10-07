# Flutter

> This guide continues the main [talkjs skill](../SKILL.md). If you haven't read it yet, read it first and don't start here: it covers creating the test project, the proof-of-concept scope, and verification.

Use `talkjs_flutter` and the [Flutter getting-started guide](https://talkjs.com/docs/UI_Components/Flutter.md). This SDK currently wraps the Classic web SDK in a WebView. Use its mobile widgets and session API.

Tell the user how to enable the Classic SDK settings in their [dashboard](https://talkjs.com/dashboard/): open **Project settings**, then under **Dashboard settings** select **Enable UI Theme Editor (Classic SDKs)** and **Save**. This exposes the Classic settings and theme editor used by this SDK. Include these steps in the handoff even if no theme customization was requested. See [dashboard setup](https://talkjs.com/docs/UI_Components/Flutter.md).

| Task | Documentation |
| --- | --- |
| Install and configure the app | [Installation](https://talkjs.com/docs/UI_Components/Flutter/Installation.md), [Gradle configuration](https://talkjs.com/docs/UI_Components/Flutter.md), [Firebase configuration when applicable](https://talkjs.com/docs/UI_Components/Flutter.md) |
| Create the session, users, and conversations | [Flutter data setup](https://talkjs.com/docs/UI_Components/Flutter.md), [Session reference](https://talkjs.com/docs/UI_Components/Flutter/Session.md) |
| Create or reference conversations | [getConversation: when data synchronizes and how to use backend-provisioned data](https://talkjs.com/docs/UI_Components/Flutter/Session.md) |
| Render chat and a conversation picker | [ChatBox](https://talkjs.com/docs/UI_Components/Flutter/Widgets/Chatbox.md), [ConversationList](https://talkjs.com/docs/UI_Components/Flutter/Widgets/ConversationList.md), [SelectConversationEvent](https://talkjs.com/docs/UI_Components/Flutter/Other_Interfaces.md) |
| Handle first-chat and empty Messages screens | [Classic conversation-list visibility](https://talkjs.com/docs/Concepts/Conversations.md) |
| End a session or change accounts | [Session.destroy](https://talkjs.com/docs/UI_Components/Flutter/Session.md) |
| Customize the chat UI | [Classic themes](https://talkjs.com/docs/UI_Components/JavaScript/Classic/Themes.md) |
| Add push notifications when requested | [Flutter push](https://talkjs.com/docs/Guides/Flutter/Push_Notifications.md) |
| Add attachments or voice messages when requested | [Flutter file sharing](https://talkjs.com/docs/Guides/Flutter/File_Sharing.md), [mobile voice messages](https://talkjs.com/docs/Guides/Mobile/Voice_Messages.md) |

Adapt the sample identities and conversation IDs to the app's domain. Follow the Session reference for setting `me`, authentication, and lifecycle. For authenticated chat, also read [backend.md](backend.md); the mobile Session accepts `token` or a `tokenFetcher` that returns a token.

Verify the integration in the app's device or simulator build, including navigation, keyboard/layout, account changes, and any requested notification or media behavior. Report any device checks that could not be run.
