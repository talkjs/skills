# React and Next.js

Read the [React getting-started guide](https://talkjs.com/docs/UI_Components/React.md) before implementing. For Next.js, also read its [browser/client setup](https://talkjs.com/docs/UI_Components/Nextjs.md). New integrations use the modern React Components and JavaScript Data API; don't mix them with Classic React examples.

Use the docs according to the part being implemented:

| Task | Documentation |
| --- | --- |
| Install packages and styles | [React installation](https://talkjs.com/docs/UI_Components/React.md) |
| Initialize the app's users and conversations | [React data setup](https://talkjs.com/docs/UI_Components/React.md), plus the Data API references linked from the main skill |
| Show one conversation | [Chatbox](https://talkjs.com/docs/UI_Components/React/Chatbox.md) |
| Build a Messages page | [Inbox userId requirements](https://talkjs.com/docs/UI_Components/React/Inbox.md), [examples](https://talkjs.com/docs/UI_Components/React/Inbox.md), [conversationFilter (empty-conversation filtering)](https://talkjs.com/docs/UI_Components/React/Inbox.md) |
| Compose a custom conversation picker | [ConversationList](https://talkjs.com/docs/UI_Components/React/ConversationList.md) |
| Integrate session ownership, errors, and cleanup | [Session reuse](https://talkjs.com/docs/Data_APIs/JavaScript/TalkSession.md), [onError (fatal errors)](https://talkjs.com/docs/Data_APIs/JavaScript/TalkSession.md), [automatic cleanup](https://talkjs.com/docs/Data_APIs/JavaScript/TalkSession.md) |
| Match the app's appearance | [Customization](https://talkjs.com/docs/UI_Components/React/Customization.md) |

Adapt the guide's sample identities and setup to the existing app. Before implementing each entry point, read its component requirements; the quickstart's existing sample users may hide first-visit setup needs. For Messages, read both the Inbox `userId` and `conversationFilter` sections linked above, including when the user has no conversations yet.

Fit initialization into the project's React data-loading and auth patterns. Verify pending navigation/account changes, errors and retry, and development Strict Mode. Use the documented session lifecycle; don't introduce an alternative application-wide state system just for chat.

For authenticated chat, read [backend.md](backend.md) and the TalkSession reference together before connecting the auth layer to UI components. Test logout and re-login as well as opening more than one chat view.
