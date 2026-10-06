# Other web frameworks

Read the guide for the project's actual framework; don't translate React examples by changing tag names:

| Project | Starting guide |
| --- | --- |
| Vue | [Vue guide](https://talkjs.com/docs/UI_Components/Vue.md), [custom-element configuration](https://talkjs.com/docs/UI_Components/Vue.md) |
| Svelte / SvelteKit | [Svelte guide](https://talkjs.com/docs/UI_Components/Svelte.md), [SvelteKit setup](https://talkjs.com/docs/UI_Components/Svelte.md) |
| Angular | [Angular setup and schema configuration](https://talkjs.com/docs/UI_Components/Angular.md) |
| Plain HTML / JavaScript | [JavaScript setup, including CDN/import maps](https://talkjs.com/docs/UI_Components/JavaScript.md) |

Follow the selected guide's package or CDN setup, element registration, stylesheet, and framework configuration. Preserve existing compiler configuration. For SSR frameworks, verify that browser-dependent imports as well as rendering stay in the browser, including in a production build.

Follow that guide's links to its Chatbox, Inbox, ConversationList, and customization references. Use the JavaScript Data API links from the main skill for data setup and [TalkSession](https://talkjs.com/docs/Data_APIs/JavaScript/TalkSession.md) for session lifecycle, including [automatic cleanup](https://talkjs.com/docs/Data_APIs/JavaScript/TalkSession.md). Adapt these to the framework's existing loading and cleanup patterns.

For Messages, read the modern `t-inbox` [userId requirements](https://talkjs.com/docs/UI_Components/Web_Components/t-inbox.md) and [conversationFilter](https://talkjs.com/docs/UI_Components/Web_Components/t-inbox.md). Use this reference for empty-conversation behavior instead of a Classic Inbox guide.

For authenticated chat, also read [backend.md](backend.md). Verify first-time Messages access, changing users/conversations, and unmounting chat views in the chosen framework.
