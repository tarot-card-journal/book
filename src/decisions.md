# Architecture Decisions

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

[1]: https://firebase.google.com/docs/app-check "Firebase App Check"
[2]: https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them "OpenAI Tokens"
[3]: https://ai.google.dev/gemini-api/docs/tokens?lang=python "Google Gemini Tokens"
[4]: https://ai.google.dev/gemini-api/docs/pricing "Google Gemini Pricing"
