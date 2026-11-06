# Veo 3 API: Retired Google IDs, Current Routes, and Provider Differences

![Conceptual video frame at a fork: one path ends at a closed doorway, one leads to an open workspace, and a third ends at a contract icon.](veo-3-api-cover.png)

*A Veo label alone does not identify the current API route or its contract.*


You find a Veo 3 API example, copy its model ID, and start planning a video feature. The example looks plausible. A provider still advertises “Veo 3,” and a Google model page still shows a quickstart. Yet the model ID in that sample may already be retired.

Before reading every parameter, ask three questions: **Which model ID? Whose API? Whose key?** “Veo 3 API” is a useful search phrase, but it does not identify a single contract. Google’s own model lifecycle and a third party’s Veo-branded endpoint can be true at the same time. This guide maps the current public documentation as checked on November 6, 2026, then shows what to confirm before you build or estimate cost. It does not assume that a documented model is enabled in your project.

## The status of Google Veo 3 IDs

Google’s [Agent Platform model lifecycle](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/model-versions) lists both `veo-3.0-generate-001` and `veo-3.0-fast-generate-001` as retired on **June 30, 2026**. It recommends the corresponding Veo 3.1 IDs. Separately, Google’s [Gemini API video guide](https://ai.google.dev/gemini-api/docs/video) says its Veo 3.1 Preview IDs shut down on **October 22, 2026**. That notice is about those preview IDs, not all video generation in the Gemini API; the same guide now points to Gemini Omni Flash for that API.

| ID or family | Documented surface | Status on November 6, 2026 | What to do with an old sample |
| --- | --- | --- | --- |
| `veo-3.0-generate-001`, `veo-3.0-fast-generate-001` | Google Agent Platform | Retired June 30, 2026 | Replace the ID and review request compatibility before reusing code. |
| `veo-3.1-generate-preview`, `veo-3.1-fast-generate-preview`, `veo-3.1-lite-generate-preview` | Gemini API | Preview access shut down October 22, 2026 | Do not treat an old Gemini API sample as a current Veo endpoint. |
| `veo-3.1-generate-001`, `veo-3.1-fast-generate-001` | Google Agent Platform | Listed as GA Veo 3.1 models | Check project access, supported region, quota, request format and current terms. |

The last row is a documented route, not a promise that every account can call it. Google’s [Veo 3.1 model card](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/veo/3-1-generate) lists `us-central1`, fixed quota support and no pay-as-you-go option for these specific model cards. It also lists 4, 6 and 8 second outputs, with resolution support varying by variant. Check your intended variant before assuming that a feature or length transfers from a different sample.

There is a documentation conflict worth keeping visible: the model card’s version table shows an earlier retirement date for the 3.1 GA models than the [central lifecycle table](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/model-versions). For forward planning, verify the current lifecycle page and your project’s terms before relying on a future retirement date. An older page or example may still be accessible after its ID has ceased to be usable. The [AI Studio Veo landing page](https://aistudio.google.com/models/veo) observed for this article still contained a `veo-3.1-fast-generate-preview` snippet, so a visible quickstart alone is not proof of a live route.

## A Google route has a project and an operation

If you want Google’s direct Agent Platform route, start from the [current text-to-video guide](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/video/generate-videos-from-text), not from a provider’s cURL command. Its REST example uses your Google Cloud project and `us-central1`, then calls the selected model through `:predictLongRunning`. That response identifies an operation. A submitted request and a completed video are different states. If your app displays the result immediately after submission, it can present an unfinished job as a finished asset. Track the operation, handle failure and retrieve the output using the route’s documented method.

Before writing the integration, confirm the exact 3.1 model ID, project permissions, regional availability, quota and output configuration. The public model card identifies `us-central1` and fixed-quota support; it does not show whether your particular project has available quota or permission to run the request. This is why “I have a Google API key” is not a complete answer to “Can this Veo request run?” The authentication and resource context must match the Google surface you actually selected.

## A provider Veo 3 API has its own contract

A vendor can expose a Veo-labeled API without exposing Google’s direct Agent Platform endpoint. [Pollo’s Veo 3 API documentation](https://docs.pollo.ai/m/google/veo3), for example, shows a POST to `https://pollo.ai/api/platform/generation/google/veo3`, an `x-api-key` header, its own JSON `input` fields and a response containing a `taskId` and status values such as `waiting`, `processing`, `succeed` and `failed`. Those fields describe **Pollo’s** contract. They do not make a Pollo key a Google Cloud credential, or make the Pollo path interchangeable with `:predictLongRunning`.

This distinction helps with more than authentication. If your app stores an operation ID, retries jobs, receives a webhook or fetches a finished file, each behavior needs to follow the chosen provider’s documentation. A successful HTTP response may only mean that a job was accepted. It does not establish that a video has been generated, that a particular Google model ID ran behind the provider, or that the output configuration matches another provider’s offer.

Provider pages from [Kie](https://kie.ai/features/v3-api), [AIMLAPI](https://aimlapi.com/veo-3) and [Crun](https://crun.ai/models/veo-3-1) show how broad the label can be: model variants, playgrounds, credits and API promises appear together. Their published pages can establish their advertised offers, but their own “cheapest,” quality or savings comparisons need matching configurations and independent support before becoming a cross-provider conclusion. Read the label, then inspect the endpoint, version, mode, terms and result lifecycle you would actually use.

## Compare access and prices at the same setting

Google publishes [Agent Platform Veo prices](https://cloud.google.com/model-garden/pricing) in USD per **count**, with separate rows for model variant, audio and resolution. On November 6, 2026, its Veo 3.1 video-with-audio row showed **$0.40 per count at 720p or 1080p**; that is a Google published quote for that row, not a measured charge or a price for another provider. A provider may publish a price per generated clip or use credits. Both are genuine published quotes for their respective offers. They are not automatically the same unit, product configuration or access path, and neither is a measured bill from this article.

To make a budget estimate you can defend, write down the model and version, clip duration, resolution, audio setting, number of outputs, processing mode, billing unit and quote date. Then check quota or capacity and any account-specific terms. For example, comparing a Google 3.1 video-with-audio 1080p row with a vendor’s unspecified “Veo 3 Fast” clip price would blend a different variant and possibly a different duration. A lower displayed number would not prove a cheaper equivalent generation. The same caution applies if a pricing page still lists Veo 3.0 after Google’s lifecycle table marks its Agent Platform IDs retired: a price row is not an availability check.

## Choose the route before you implement

If you need Google’s direct route, use the current 3.1 Agent Platform model card and generation guide, then check your project’s access and quota. If a provider route better fits your integration, read that provider’s endpoint, credential, payload, job-status, output and billing documentation as its own contract. Keep the exact model or variant in the comparison rather than treating every Veo-branded page as the same service.

The public [Moduck model catalog](https://moduck.ai/models) checked for this article did not establish a current Veo endpoint or matching price. If Moduck is one of the routes you are evaluating, [Read the documentation](https://docs.moduck.ai/). Look for the exact model and method before requesting a key. Verify a current Veo listing before planning a migration. The decision only becomes implementable when the selected route’s current docs and your own account agree.

