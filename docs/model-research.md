# GPT Image 2.5 research and implementation decisions

Checked 2026-09-09 against official OpenAI documentation and the installed Codex CLI.

| Model | Official positioning | Implication for our workflow |
|---|---|---|
| [GPT Image 2.5 Flare](https://developers.openai.com/api/docs/models/gpt-image-2.5-flare) | Fast, high-quality everyday generation | Resolve creative direction efficiently and avoid unnecessary revisions. |
| [GPT Image 2.5 Sunburst](https://developers.openai.com/api/docs/models/gpt-image-2.5-sunburst) | Most capable generation/editing, with an emphasis on editing precision | Treat identity, geometry, lettering, and unchanged regions as explicit acceptance criteria. |

The [official prompting guide](https://developers.openai.com/api/docs/guides/image-prompting) describes both models as improving precise edits and subject preservation, and supports transparent backgrounds. It recommends concrete visual requirements, reference roles, and checking retained details across edits. These inform the skill's craft and review instructions. We do not copy the guide into every Claude request.

## What the subscription path actually exposes

On codex-cli 0.153.4, `codex exec --help` identifies `--model` as the agent model. The built-in image tool has no verified selector for these two image models in this launcher. The public [image generation guide](https://developers.openai.com/api/docs/guides/image-generation) documents explicit model selection for API integrations; that does not establish equivalent controls for Codex subscriptions.

Therefore this release has no image API path and no fake `--image-model` flag. Backend identity remains Codex-managed and unreported. Documentation knowledge is distinguished from verified runtime availability. We cannot promise a Flare-to-Sunburst switch during refinement through this transport.

### Direct Codex response — 2026-09-10

We also asked the actual subscription CLI, using the same lightweight relay and
tool isolation as the launcher, to report its currently visible image-tool
parameters without generating an image. This checks the agent-facing interface,
rather than assuming the CLI help lists every tool argument.

Codex reported `image_gen__imagegen` with these three parameters:

| Parameter | Reported type |
|---|---|
| `prompt` | Required string |
| `referenced_image_paths` | Optional nullable array of strings |
| `num_last_images_to_include` | Optional nullable integer |

It reported `supported_by_parameter: false` for both `gpt-image-2.5-flare` and
`gpt-image-2.5-sunburst`. For natural-language selection, its exact response was:

> Unknown; the visible tool description does not say that a natural-language model-name instruction selects the backend.

The [full request and response](model-capability-probe.json) records CLI 0.153.4,
Luna with reasoning `none`, and 2,810 reported tokens. This is Codex's account-local
report of its exposed interface; it does not establish undocumented server routing
behavior or future versions. We therefore keep the existing subscription-only
implementation and do not add an inert model-selection flag. If a future probe
exposes a real selector, use its exact parameter and accepted values. Do not run a
capability interview before every image; check only when explicitly needed.

### Re-check on codex-cli 0.164.0 — 2026-10-25

The same probe on 0.164.0 (`gpt-6-luna`, reasoning `none`) reported a fourth tool
parameter, `transparent_background: boolean|null`. There is still no image-model
parameter. The relay instructions now ask for `transparent_background=true` when the
brief requests a transparent background.

### Re-check on codex-cli 0.169.0 — 2026-11-15

The same probe on 0.169.0 (`gpt-6-luna`, reasoning `none`, 8,873 tokens) reported
the same four parameters as 0.164.0 (`prompt`, `referenced_image_paths`,
`num_last_images_to_include`, `transparent_background`). There is still no
image-model parameter, and whether a model name in the prompt selects the backend
remains unknown.
A second no-image probe the same day returned `{"tools":[]}`, because the image tool
is reached through the code-mode `exec` wrapper. The relay's self-report varies between
runs, so a live generation is the reliable check.

### Re-check on codex-cli 0.170.0 — 2026-11-18

The same probe on 0.170.0 (`gpt-6-luna`, reasoning `none`, 12,160 tokens) reported
`image_gen.imagegen` with the same four parameters as 0.164.0 and 0.169.0. There is
still no image-model parameter, and the tool description still does not say whether a
model name in the prompt selects the backend.

## Decisions for people using Claude Code

- Clear brief: execute immediately, with no mandatory questionnaire.
- An explicit interview: ask a small set of useful questions and wait for answers.
- Multiple directions: generate separate full-size files, then compare locally.
- Editing: attach the selected source and list change/preserve constraints.
- Long generation: detach local workers and continue independent work.
- Token budget: finish all creative reasoning in Claude; forward the short final brief to a light relay, omit skill discovery and generic coding instructions.
- Quota: bound concurrency and repairs; persist completed results instead of regenerating.

These are product/design judgments for this skill, not comparative model benchmarks. Faster does not automatically mean cheaper; subscription limits and image-generation charges to the allowance are controlled by Codex.
