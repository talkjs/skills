# Backend authentication and membership

> This guide continues the main [talkjs skill](../SKILL.md). If you haven't read it yet, read it first and don't start here: it covers creating the test project, the proof-of-concept scope, and verification.

Use this alongside the platform guide for requested TalkJS authentication or enforced private conversation access, necessary REST API calls, or production deployment. Follow the main skill's POC-first scope: an existing app login does not require adding TalkJS authentication during the initial POC. Read only the sections needed for the current task.

## Get backend credentials

Reuse a secret key already configured on the app's backend. Otherwise follow [the CLI secret-key workflow](cli.md#secret-keys): verify the selected app, list its keys, and retrieve an active key with `talkjs secret-keys get <id> --json`. The response's `value.secretKey` is the backend credential. This works for both an unclaimed anonymous test app and an OAuth-authorized test app; do not read access tokens from the CLI config or make claiming a prerequisite for retrieval.

Use the secret key for REST API calls and, when implementing authentication, signing user authentication tokens. Configure it yourself in the app's server-side environment or Gitignored local configuration; never expose it in frontend code, logs, or source control. The anonymous access token and OAuth credential authorize CLI settings/key management; neither is a chat REST API secret key or an SDK user authentication token.

Prefer [claiming through the CLI](cli.md#claiming-and-existing-projects) so the user takes ownership and authorizes continued CLI access together. Claiming immediately invalidates the anonymous access token, not an already retrieved secret key. Continue using the same app ID and backend key. If the user claimed through the plain creation link instead, run `talkjs login` and select that same project before further CLI configuration; do not recreate the project or rotate its backend key merely because the anonymous token stopped working. The CLI manages test-app keys only; use the owner's [TalkJS dashboard](https://talkjs.com/dashboard/) for live credentials and keep the two environments separate.

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

Generating and sending signed tokens can be implemented and tested before authentication is required, but it does not make tokens mandatory. For requested security work on the selected test app, enable enforcement with `talkjs settings set authentication required --json` once valid tokens work, and verify it with `talkjs settings get authentication --json`. This works before or after claiming. Live-app enforcement belongs in the dashboard. For server-controlled membership, also follow the browser synchronization guidance above. These settings must not block an initial POC, and existing protections must not be disabled to make a POC work.

Use the app's existing request/auth conventions and an explicit frontend/backend identity contract. When implementing security, verify token refresh, rejection of missing or invalid authentication, denial of unauthorized conversation access, logout/re-login, and failed provisioning. If dashboard enforcement is pending, report security as incomplete even if valid tokens work. Report required account configuration before describing the result as secured or ready for deployment.
