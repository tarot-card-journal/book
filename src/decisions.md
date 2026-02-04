# Architecture Decisions

## Tech Stack

- Flutter iPhone and Android app.
- Rust backend proxy service for holding API secrets. (maybe?)

## User Data

For security and simplicity we do not want to store user data or have user
accounts.
However, we must allow users to easily migrate from one device to another.

Google and Apple provide features for doing this and there is built in Flutter
tooling for using those services.
For apple it is called CloudKit and for android it is the Google Drive AppData.
In Flutter you can use [`icloud_storage`][5] or [`icloud_storage_sync`][6] for 
iPhone and [`google_sign_in`][7] and [`googleapis`][8] to store app data.

This will not allow users to migrate between an iPhone and Google device as
their data will be tied to their Apple or Google account.
This is fine because we are also using the app stores of each ecosystem and
there is no good way to transfer a paid app from one to the other.

## AI Costs

AI costs are measured in tokens.
For OpenAI, [1500 words is ~2048 tokens][2].
For Gemini, [60-80 words is 100 tokens][3].
The number of tokens per word is around the same for each.
To calculate cost you need to calculate number of input and output tokens as
they have different pricing models.

To get started we should use Gemini 3 Flash Preview as it is [Free][4].

## Backend AI Request Proxy

While we are using the free tier of Gemini we do not need to build a backend.
When we transition to a paid tier we will need to build the backend.

For the initial design the backend will only be a proxy that stamps requests
with our API key and forwards them to the AI provider. This will keep our hosting
costs low and user data private as we will not store any user data.

One complication is we need to authenticate the app so that only valid apps can
send requests otherwise we might be charged for unauthorized usage.
[Firebase App Check][1] is a solution that abstracts Apple's DeviceCheck and 
Google's Play Integrity.

## Firebase AI

An alternative to the backend proxy is to use Firebase AI and the flutter
plugin [`flutter_ai_toolkit`][9]. This handles client authentication for us.

[1]: https://firebase.google.com/docs/app-check "Firebase App Check"
[2]: https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them "OpenAI Tokens"
[3]: https://ai.google.dev/gemini-api/docs/tokens?lang=python "Google Gemini Tokens"
[4]: https://ai.google.dev/gemini-api/docs/pricing "Google Gemini Pricing"
[5]: https://pub.dev/packages/icloud_storage "Flutter iCloud Storage"
[6]: https://pub.dev/packages/icloud_storage_sync "Flutter iCloud Storage Sync"
[7]: https://pub.dev/packages/google_sign_in "Flutter Google Sign In"
[8]: https://pub.dev/packages/googleapis "Flutter Google APIs"
[9]: https://pub.dev/packages/flutter_ai_toolkit "Flutter AI Toolkit"
