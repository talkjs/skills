# Backend authentication and membership

> This guide continues the main [talkjs skill](../SKILL.md). If you haven't read it yet, read it first and don't start here: it covers creating the test project, the proof-of-concept scope, and verification.

Use this alongside the platform guide for requested TalkJS authentication or enforced private conversation access, necessary REST API calls, or production deployment. Follow the main skill's POC-first scope: an existing app login does not require adding TalkJS authentication during the initial POC. Read only the sections needed for the current task.

## Get backend credentials

For an anonymous project, retrieve its secret key using the `accessToken` retained in context by the main skill:

```http
GET https://api.talkjs.com/v1/anonymousAccounts/<accessToken>
```

The response contains `appId`, `secretKey`, and `claimUrl`. Use `secretKey` for REST API calls and, when implementing authentication, signing user authentication tokens. Configure the returned credentials yourself in the app's server-side environment or Gitignored local configuration; never expose the secret in frontend code or commit it to the repository. Do not wait for the user to claim the project or manually supply these credentials. The anonymous-project `accessToken` is neither a REST API secret key nor an SDK user authentication token.

After the user claims the project, its `accessToken` stops working and this GET returns 404. Continue using the same `appId`; obtain the project's secret key from its owner's [TalkJS dashboard](https://talkjs.com/dashboard/) instead of creating a replacement project. Keep test and live credentials separate.

## Integrate the backend

Read these sections for the relevant decision:

| Task | Documentation |
| --- | --- |
| Select the account environment | [Test and live environments](https://talkjs.com/docs/Features/Environments.md) |
| Generate user authentication tokens | [Generate a token](https://talkjs.com/docs/Features/Security/Authentication.md), using the backend's language example |
| Require authentication as part of securing chat | [Enable identity verification](https://talkjs.com/docs/Features/Security/Authentication.md) |
| Check exact token claims | [Token reference](https://talkjs.com/docs/Features/Security/Advanced_Authentication.md) |
| Refresh authentication | [Refreshable tokens](https://talkjs.com/docs/Features/Security/Advanced_Authentication.md), then the selected platform's session reference |
| Connect modern web sessions | [Session reuse](https://talkjs.com/docs/Data_APIs/JavaScript/TalkSession.md), [onNeedToken](https://talkjs.com/docs/Data_APIs/JavaScript/TalkSession.md), [setToken](https://talkjs.com/docs/Data_APIs/JavaScript/TalkSession.md), [onError (fatal errors)](https://talkjs.com/docs/Data_APIs/JavaScript/TalkSession.md) |
| Connect mobile sessions | [React Native Session props](https://talkjs.com/docs/UI_Components/React_Native/Components/Session.md) or [Flutter Session](https://talkjs.com/docs/UI_Components/Flutter/Session.md), using their `token` / `tokenFetcher` APIs |
| Choose how private membership is enforced | [Securing conversations](https://talkjs.com/docs/Concepts/Conversations.md), [browser synchronization](https://talkjs.com/docs/Features/Security/Browser_Synchronization.md) |
| Provision data from the server | [REST authentication](https://talkjs.com/docs/REST_API.md), [users](https://talkjs.com/docs/REST_API/Users.md), [setting conversation data](https://talkjs.com/docs/REST_API/Conversations.md), [participation](https://talkjs.com/docs/REST_API/Participation.md) |

Use the example labeled for the backend's language; in the HTML fallback, select that language's tab. The token reference supplies the language-independent contract.

When implementing requested security work, derive TalkJS identity from the app's authenticated backend session and keep secrets/admin credentials server-side. Authentication and business authorization are separate: connect membership to the app's domain rules. Explain which data is created by the client and which by the backend, according to the chosen synchronization settings and platform API.

Generating and sending signed tokens can be implemented and tested before dashboard authentication is enabled, but it does not make tokens mandatory. To enforce authentication, the user must claim an anonymous project and enable identity verification in the dashboard. For server-controlled membership, also follow the browser synchronization guidance above. These settings belong to requested security work; they must not block an initial POC, and existing protections must not be disabled to make a POC work.

Use the app's existing request/auth conventions and an explicit frontend/backend identity contract. When implementing security, verify token refresh, rejection of missing or invalid authentication, denial of unauthorized conversation access, logout/re-login, and failed provisioning. If dashboard enforcement is pending, report security as incomplete even if valid tokens work. Report required account configuration before describing the result as secured or ready for deployment.
