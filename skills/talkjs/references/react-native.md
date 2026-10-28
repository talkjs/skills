# React Native and Expo

> This guide continues the main [talkjs skill](../SKILL.md). If you haven't read it yet, read it first and don't start here: it covers creating the test project, the proof-of-concept scope, and verification.

Use the [React Native getting-started guide](https://talkjs.com/docs/UI_Components/React_Native.md). This SDK currently wraps the Classic web SDK in a WebView. Use its mobile components and session API.

Tell the user how to enable the Classic SDK settings in their [dashboard](https://talkjs.com/dashboard/): open **Project settings**, then under **Dashboard settings** select **Enable UI Theme Editor (Classic SDKs)** and **Save**. This exposes the Classic settings and theme editor used by this SDK. Include these steps in the handoff even if no theme customization was requested. See [dashboard setup](https://talkjs.com/docs/UI_Components/React_Native.md).

| Task | Documentation |
| --- | --- |
| Select and install the package | [Installation](https://talkjs.com/docs/UI_Components/React_Native/Installation.md): `@talkjs/react-native` without Expo, `@talkjs/expo` with Expo |
| Configure an Expo build | [Expo setup](https://talkjs.com/docs/UI_Components/React_Native.md), including required Notifee configuration; Expo Go is unsupported |
| Reference conversations and synchronize participants | [getConversationBuilder](https://talkjs.com/docs/UI_Components/React_Native/API/getConversationBuilder.md), [User](https://talkjs.com/docs/UI_Components/React_Native/Object_Types/User.md), [ConversationBuilder](https://talkjs.com/docs/UI_Components/React_Native/Object_Types/ConversationBuilder.md) |
| Render chat and authenticate | [Session props](https://talkjs.com/docs/UI_Components/React_Native/Components/Session.md), [Chatbox props](https://talkjs.com/docs/UI_Components/React_Native/Components/Chatbox.md) |
| Customize the chat UI | [Classic themes](https://talkjs.com/docs/UI_Components/JavaScript/Classic/Themes.md) |
| Add push notifications when requested | [React Native push](https://talkjs.com/docs/Guides/React_Native/Push_Notifications.md) or [Expo push](https://talkjs.com/docs/Guides/React_Native/Push_Notifications_Expo.md) |
| Manage push registration across accounts | [Notification handlers](https://talkjs.com/docs/UI_Components/React_Native/API/registerPushNotificationHandlers.md), [enablePushNotifications (automatic registration)](https://talkjs.com/docs/UI_Components/React_Native/Components/Session.md), [unsetPushRegistration (remove a device)](https://talkjs.com/docs/UI_Components/React_Native/Components/Session.md) |
| Add voice messages when requested | [Mobile voice messages](https://talkjs.com/docs/Guides/Mobile/Voice_Messages.md) |

Follow the selected installation guide's peer dependencies and build configuration even when push notifications are not requested. Adapt the sample identities and conversation IDs to the app's domain. For authenticated chat, also read [backend.md](backend.md); the mobile Session accepts `token` or a `tokenFetcher` that returns a token.

Verify the integration in the app's device or simulator build, including navigation, keyboard/layout, account changes, and any requested notification behavior. Report any device checks that could not be run.
