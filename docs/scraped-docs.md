# PromptLayer — Scraped Documentation

**Source:** https://promptlayer.com
**Pages scraped:** 15
**Total characters:** 91412

---

## 

**URL:** https://docs.promptlayer.com/features/evaluations/overview.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# Evals Overview
**We believe that evaluation engineering is half the challenge of building a good prompt.** The Evaluations page is designed to help you iterate, build, and run batch evaluations on top of your prompts. Every prompt and every use case is different.
Inspired by the flexibility of tools like Excel, we offer a visual pipeline builder that allows users to construct complex evaluation batches tailored to their specific requirements. Whether you're scoring prompts, running bulk jobs, or conducting regression testing, the Evaluations page provides the tools needed to assess prompt quality effectively. Made for both engineers and subject-matter experts.
## Common Tasks
* **Scoring Prompts**: Utilize golden datasets for comparing prompt outputs with ground truths and incorporate human or AI evaluators for quality assessment.
* **One-off Bulk Jobs**: Ideal for prompt experimentation and iteration.
* **Backtesting**: Use historical data to build datasets and compare how a new prompt version performs against real production examples.
* **Regression Testing**: Build evaluation pipelines and datasets to prevent edge-case regression on prompt template updates.
* **Continuous Integration**: Connect evaluation pipelines to prompt templates to automatically run an eval with each new version (and catologue the results). Think of it like a Github action.
## Examples Use-Cases
* **Chatbot Enhancements**: Improve chatbot interactions by evaluating responses to user requests against semantic criteria.
* **RAG System Testing**: Build a RAG pipeline and validate responses against a golden dataset.
* **SQL Bot Optimization**: Test Natural Language to SQL generation prompts by *actually* running generated queries against a database (using the API Endpoint step), followed by an evaluation of the results' accuracy.
* **Improving Summaries**: Combine AI evaluating prompts and human graders to help improve prompts without a ground truth.
## Additional Resources
For a deeper understanding of evaluation approaches, especially for complex LLM applications beyond simple classification or programming tasks, check out our blog post: [How to Evaluate LLM Prompts Beyond Simple Use Cases](https://blog.promptlayer.com/how-to-evaluate-llm-prompts-beyond-simple-use-cases/). This guide explores strategies like Decomposition Testing, working with Negative Examples, and implementing LLM as a Judge Rubric frameworks.
[Click here to see in-depth examples.](/features/evaluations/examples)

---

## 

**URL:** https://docs.promptlayer.com/features/prompt-registry/overview.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# Prompt Registry Overview
The Prompt Registry allows you to easily manage your prompt templates, which are customizable prompt strings with placeholders for variables.
Specifically, a prompt template is your prompt string with variables indicated in curly brackets (`This is a prompt by {author_name}`). Learn more about using [template variables](/features/prompt-registry/template-variables). Prompt templates can have tags and are uniquely named.
You can use this tool to programmatically retrieve and publish prompts (even at runtime!). That is, this registry makes it easy to start [A/B testing](/why-promptlayer/ab-releases) your prompts. Viewed as a "**Prompt Management System**”, this registry allows your org to pull out and organize the prompts that are currently dispersed throughout your codebase.
## Collaboration
The Prompt Registry is perfect for engineering teams looking to organize & track their many prompt templates.
... but the **real** power of the registry is collaboration. Engineering teams waste cycles deploying prompts and content teams are often blocked waiting on these deploys.
By programmatically pulling down prompt templates at runtime, product and content teams can visually update & deploy prompt templates without waiting on eng deploys.
We know quick feedback loops are important, but the Prompt Registry also makes those annoying last-minute prompt updates easy.
### Comments
The Prompt Registry supports threaded comments on prompt versions, enabling collaborative discussions about edits and ideas. This feature facilitates communication between team members, allowing them to share feedback, suggest improvements, and document the reasoning behind prompt changes. By fostering collaboration through comments, teams can make more informed decisions and maintain a clear history of prompt evolution.
## Getting a Template
The Prompt Registry is designed to be used at runtime to pull the latest prompt template for your request.
It’s simple.
```python Python SDK theme={null}
template_dict = promptlayer_client.templates.get("my_template")
```
```js JavaScript theme={null}
const template_dict = await promptLayerClient.templates.get("my_template");
```
Alternatively, use the REST API:
* `POST /prompt-templates/{prompt_name}` to get a template with input variables and provider-formatted output ([read more](/reference/templates-get))
* `GET /prompt-templates/{prompt_name}` to get raw template data without applying input variables, useful for syncing, caching, or inspection ([read more](/reference/templates-get-raw))
### By Release Label
Release labels like `prod` and `staging` can be optionally applied to template versions and used to retrieve the template.
```python Python SDK theme={null}
promptlayer_client.templates.get("my_template", { "label": "prod" })
```
```js JavaScript theme={null}
const template_dict = await promptLayerClient.templates.get("my_template", { label: "prod" });
```
### By Version
You can also optionally pass `version` to get an older version of a prompt. By default the newest version of a prompt is returned
```python Python SDK theme={null}
template_dict = promptlayer_client.templates.get("my_template", { "version": 3 })
```
```js JavaScript theme={null}
const template_dict = await promptLayerClient.templates.get("my_template", { version: 3 });
```
### Metadata
When fetching a prompt template, you can view your `metadata` using the following code snippet:
```python Python SDK theme={null}
template = promptlayer_client.templates.get("my_template")
print(template["metadata"])
```
```js JavaScript theme={null}
const template = await promptLayerClient.templates.get("my_template");
console.log(template.metadata);
```
### Formatting
PromptLayer can format and convert your prompt to the correct LLM format. You can do this by passing the arguments `provider` and `input_variables`.
Currently we support `provider` type of either `openai` or `anthropic`.
#### Setting Execution Parameters
When using PromptLayer, ensure you set any necessary parameters for execution, such as `provider`, `input_variables`, and other specific parameters required by the LLM provider (e.g., `temperature`, `max_tokens` for OpenAI). Use the `llm_kwargs` as provided. If you need to override certain arguments, it is recommended to create a new version on PromptLayer. Alternatively, you can override them on your end if necessary.
**Provider-Specific Schema Notice**
The `llm_kwargs` object structure is provider-specific and may change without notice as LLM providers update their APIs. For example, Provider's system message format may change from a string to an array of objects.
PromptLayer passes through the native format required by each provider. If you need stable, provider-agnostic access to prompt data, use `prompt_template` instead, which provides a consistent structure regardless of the target provider.
We recommend:
* Do not hard-code assumptions about `llm_kwargs` structure in your application
* Use `prompt_template` for storing or caching prompt data
* Test your integration when switching providers or after provider API updates
```python Python SDK theme={null}
from promptlayer import PromptLayer
promptlayer_client = PromptLayer()
response = promptlayer_client.run(
prompt_name="city_choice",
input_variables={
"city": "Washington, D.C.",
"interests": "resorts, museums, beaches"
},
tags=["getting_started_example"]
)
pl_request_id = response["request_id"]
```
```js JavaScript theme={null}
import { PromptLayer } from "promptlayer";
const promptLayerClient = new PromptLayer();
const response = await promptLayerClient.run({
promptName: "city_choice",
inputVariables: {
city: "Washington, D.C.",
interests: "resorts, museums, beaches",
},
tags: ["getting_started_example"],
});
const plRequestId = response.request_id;
```
## Publishing a Template
You can use the UI or API to create a template.
Templates are unique by name, which means that publishing a template with the same name will overwrite old templates.
### String Formats
You may choose to select from one of the two supported template formats (`f-string, jinja2`) to declare variables. (`f-string`) allows you to declare variables using curly brackets (`{variable_name}`) while (`jinja2`) allows you to declare variables using double curly brackets (`{{variable_name}}`). For a more detailed explanation of both formats, see our [Template Variables](/features/prompt-registry/template-variables) documentation.
### Visually
To create a template using the UI, simply navigate to the Registry and click "Create Template". This will allow you to create the template visually. You can also edit old templates from the UI.
Rename a template by triple-clicking on its name. It’s best practice to keep template names unique and lowercase.
### Programmatically
While it's easiest to publish prompt templates visually through the dashboard, some users prefer the programatic interface detailed below.
```python Python SDK theme={null}
promptlayer_client.templates.publish({
"prompt_name": "my_template",
"prompt_template": my_template,
"tags": ["my_tag"]
})
```
```js JavaScript theme={null}
promptLayerClient.templates.publish({
prompt_name: "my_template",
prompt_template: my_template,
tags: ["my_tag"],
});
```
### Release Labels
Prompt labels are a way to put a label on your prompt template to help you organize and search for them. This enables you to get a specific version of prompt using the label. You can add as many labels as you want to a prompt template with one restriction - the label must be unique across all versions. This means that you cannot have a label called `prod` on both version 1 and version 2 of a prompt template. This restriction is in place to prevent confusion when searching for prompt templates.
You can also set release labels via the SDK
```python Python SDK theme={null}
pt = promptlayer_client.templates.get("demo-template")
promptlayer_client.templates.publish({
"prompt_name": "demo-template",
"commit_message": "This is a test",
"prompt_template": pt["prompt_template"],
"release_labels": ["prod"],
})
```
```js JavaScript theme={null}
const pt = await promptLayerClient.templates.get({
prompt_name: "demo-template"
});
promptLayerClient.templates.publish({
prompt_name: "demo-template",
commit_message: "This is a test",
prompt_template: pt.prompt_template,
"release_labels": ["prod"],
});
```
### Commit Messages
Prompt commit messages allow you to set a brief 72 character-long description on each of your prompt template version to help you keep track of changes.
You can also retrieve the commit messages through code. You'll see them when you list all templates.
```python Python SDK theme={null}
promptlayer_client.templates.all()
```
```js JavaScript theme={null}
promptLayerClient.templates.all()
```
Or set them, by specifing a commit\_message arg to templates.publish, like this:
```python Python SDK theme={null}
pt = promptlayer_client.templates.get("demo-template")
promptlayer_client.templates.publish({
"prompt_name": "demo-template",
"commit_message": "This is a test",
"prompt_template": pt["prompt_template"],
})
```
```js JavaScript theme={null}
const pt = await promptLayerClient.templates.get({
prompt_name: "demo-template"
});
promptLayerClient.templates.publish({
prompt_name: "demo-template",
commit_message: "This is a test",
prompt_template: pt.prompt_template,
});
```
They are also available through the following REST API endpoints
`/prompt-templates/{identifier}` ([read more](/reference/templates-get))
`/rest/prompt-templates` ([read more](/reference/templates-publish))
### Metadata
Custom metadata can be associated with individual prompt template versions. This allows you to set values such as provider, model, temperature, or any other key-value pair for each prompt template version. Please note that the `model` attribute is reserved for model parameters, avoid putting custom metadata here.
Custom non-model related metadata can be seen and edited through the Prompt Template Version edit page.
You can use our Python SDK to publish a prompt template with metadata. For example, here's how you can set the `model` metadata along with a custom `category` metadata:
```python Python SDK theme={null}
promptlayer_client.templates.publish({
"prompt_name": "my_template",
"prompt_template": my_template,
"metadata": {
"model": {
"provider": "openai",
"name": "gpt-3.5-turbo",
"parameters": {"temperature": 0.5, "max_tokens": 256},
},
"category": "weather",
}
})
```
```js JavaScript theme={null}
promptLayerClient.templates.publish({
prompt_name: "my_template",
prompt_template: my_template,
metadata: {
model: {
provider: "openai",
name: "gpt-3.5-turbo",
parameters: { temperature: 0.5, max_tokens: 256 },
},
category: "weather",
},
});
```
Alternatively, use the REST API endpoint `/rest/prompt-templates` ([read more](/reference/templates-publish)).
## Tracking templates
The power of the Prompt Registry comes from associating requests with the template they used. This allows you to track average score, latency, and cost of each prompt template. You can also see the individual requests using the template.
To associate a request with a prompt template and some input variables, use the code below with the [pl\_request\_id](/features/prompt-history/request-id) for the request.
```python Python SDK theme={null}
promptlayer_client.track.prompt(request_id=pl_request_id,
prompt_name='
', prompt_input_variables={...})
```
```js JavaScript theme={null}
promptLayerClient.track.prompt({
request_id: pl_request_id,
prompt_name: '
',
prompt_input_variables: {...}
})
```
Alternatively, use the REST API endpoint `/rest/track-prompt` ([read more](/reference/track-prompt)).
Learn more about tracking templates [here](/features/prompt-history/tracking-templates)
## A/B Testing
The Prompt Registry, combined with our powerful [A/B Releases](/why-promptlayer/ab-releases) feature, allows you to easily perform A/B tests on your prompts. This feature enables you to test different versions of your prompts in production, safely roll out updates, and segment users.
With A/B Releases, you can:
* Test new prompt versions with a subset of users before a full rollout
* Gradually release updates to minimize risk
* Segment users to receive specific versions (e.g., beta users, internal employees)
Here's how it works:
1. Create multiple versions of a prompt template in the Prompt Registry.
2. Use [Dynamic Release Labels](/features/prompt-registry/dynamic-release-labels) to split traffic between different prompt versions based on percentages or user segments.
3. Retrieve the appropriate prompt version at runtime using the `get` method with a release label.
CMS systems like the Prompt Registry are useful for A/B testing because they allow you to programmatically manage and publish different versions of a prompt template. By creating multiple versions of a prompt template, you can serve different prompts to different users and measure which version performs better.
## Getting all Prompts Programmatically
To get all prompts from prompt registry you can use the following code snippet:
```python Python SDK theme={null}
all_prompts = promptlayer_client.templates.all()
```
```js JavaScript theme={null}
const all_prompts = promptLayerClient.templates.all()
```
By default this returns 30 prompts. You can change this by passing in the `per_page` argument. For example, to get 100 prompts you can do the following:
```python Python SDK theme={null}
all_prompts = templates.all(per_page=100)
```
```js JavaScript theme={null}
const all_prompts = promptLayerClient.templates.all({ per_page: 100 })
```
You can also define the page you want to get by passing in the `page` argument. For example, to get the second page of prompts you can do the following:
```python Python SDK theme={null}
all_prompts = templates.all(page=2)
```
```js JavaScript theme={null}
const all_prompts = promptLayerClient.templates.all({ page: 2 })
```
You can filter prompts by release label using the `label` argument. This will return all prompts that have that release label:
```python Python SDK theme={null}
all_prompts = templates.all(label="prod")
```
```js JavaScript theme={null}
const all_prompts = promptLayerClient.templates.all({ label: "prod" })
```
It is important to note that `prompt_template` represents the latest version of the prompt template.
Alternatively, use the REST API endpoint `/prompt-templates` ([read more](/reference/list-prompt-templates)).
## Using Images as Input Variables
You can dynamically set images into your user prompt by setting an image input variable in your prompt template. This allows you to provide a list of input images to be used in your prompt.
To define an image input variable, simply click the attach icon in the prompt registry and enter a name for the variable in the resulting modal dialog.
You can then use this input variable in your prompt template as you would any other input variable. Keep in mind we expect the image input variable to be a list of image urls or base64 encoded images.
```python Python SDK theme={null}
input_variables = {
"user_photos": [
"https://example.com/image1.jpg",
"https://example.com/image2.jpg"
]
}
promptlayer_client.run("my_template", input_variables=input_variables)
```
```js JavaScript theme={null}
const inputVariables = {
user_photos: [
"https://example.com/image1.jpg",
"https://example.com/image2.jpg"
]
}
promptLayerClient.run("my_template", inputVariables=inputVariables)
```

---

## 

**URL:** https://docs.promptlayer.com/features/skill-collections/overview.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# Skill Collections
> Versioned folders of agent instructions (SKILL.md files) you can share, remix, and pull into Claude Code, OpenAI/Codex, or OpenClaw.
A **Skill Collection** is a versioned folder of instructions that teach your coding agent how to behave. Each skill lives in its own subfolder with a `SKILL.md`, alongside provider-specific config (like `CLAUDE.md` or `AGENTS.md`). Edit in the PromptLayer dashboard, version with commit messages and release labels, share a public link, let others **Remix** a copy, and **pull** the collection into your local agent with the PromptLayer SDK.
## Supported layouts
Skill Collections support the following agent environments:
| Layout             | Core config     | Typical skill paths                                             |
| ------------------ | --------------- | --------------------------------------------------------------- |
| **Claude Code**    | `CLAUDE.md`     | `my-skill/SKILL.md`                                             |
| **OpenAI / Codex** | `AGENTS.md`     | `my-skill/SKILL.md`, `my-skill/agents/openai.yaml`              |
| **OpenClaw**       | `openclaw.json` | `workspace/skills/my-skill/SKILL.md` plus workspace files       |
| **Universal**      | Flexible        | Portable structure; you can switch to a specific provider later |
**Exports and zip pulls** include the on-disk prefix your tool expects (`.claude/`, `.agents/`, `.openclaw/`). **Public API calls** use paths relative to the collection root (e.g. `triage/SKILL.md`) — not the exported form like `.claude/skills/triage/SKILL.md`.
## What you can do
* **Version** every change with a commit message; optional **release labels** for staging vs production.
* **Share** a read-only link and optionally allow **Remix** so others copy the collection into their workspace.
* **Pull** via the Python or JavaScript SDK into `.claude`, `.agents`, `.openclaw`, or a generic `skills` folder.
* **Compare** versions and roll back from version history in the dashboard.
## Plan limits
These are the default capacity guidelines.
| Plan | Collections | Files per collection |
| ---- | ----------- | -------------------- |
| Free | 1           | 30                   |
| Pro  | 5           | 50                   |
| Team | Unlimited   | 100                  |
**Hard limit:** any single file must be **5 MiB** or smaller.
## Next steps
Use the in-dashboard assistant to create, edit, rename, or migrate Skill
Collections with natural language.
Publish a read-only link, let others remix into their own workspace, and
show them how to pull the collection.
Sync a collection to disk with the Python or JavaScript SDK, or download via
the public REST API.
Rename folders, edit frontmatter, and pin SDK pulls to release labels.

---

## 

**URL:** https://docs.promptlayer.com/overview.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# PromptLayer Documentation
> Version, test, and monitor every prompt and agent with evals, tracing, and datasets.
PromptLayer
Version, test, and monitor every prompt and agent with evals, tracing, and datasets.
Go to Quickstart
→
Prompt Registry
Version prompts outside your codebase with reusable templates and inputs.
Observability
Inspect traces, latency, token usage, and production behavior across every request.
Agents
Compose multi-step workflows with reusable components, branching, and orchestration.
Evaluations
Score prompt and agent changes before shipping them.
Dataset Management
Build, organize, and reuse datasets for evaluation, regression testing, and iteration.
Skills Registry
Store and reuse structured skills so agents can consistently apply the right capabilities.
Features
Release Labels
Promote tested prompt versions across environments without changing application code.
AB Testing
Compare prompt variants in production and route traffic based on measured performance.
Analytics
Track usage, quality, and outcome trends as prompts and agents evolve over time.
Role-Based Access Control
Manage workspace permissions with granular roles for engineers, operators, and reviewers.
Single Sign-On
Enable secure team access with enterprise identity providers and centralized login flows.
Self-Hosting
Deploy PromptLayer in your own environment when security or compliance requires it.
Resources
API Reference
Explore the REST API surface for prompt execution, datasets, and workflows.
MCP
Documentation for model context protocol integrations, tools, and connected workflows.
Migration Guides
Move hard-coded prompts and ad hoc eval scripts into a managed PromptLayer workflow.
SDKs
Official client libraries, examples, and package references for supported runtimes.
OpenTelemetry Integrations
Connect PromptLayer with model providers, tracing pipelines, and internal tooling.
Changelog
Product updates covering PromptLayer platform improvements and launches.
Blog
Engineering and product insights from the PromptLayer team.

---

## 

**URL:** https://docs.promptlayer.com/quickstart.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# Quickstart
PromptLayer is your workbench for AI engineering. Version, test, and monitor every prompt and agent with robust evals and tracing. Empower domain experts to iterate alongside engineers in the visual editor.
This tutorial walks you through building an AI application that generates cake recipes. By the end, you'll know how to:
* Create and run prompts in the visual editor
* Track versions with diffs and commit messages
* Switch between LLMs (OpenAI, Anthropic, etc.)
* Deploy to production with release labels
* View logs and debug issues
[Create an account](https://dashboard.promptlayer.com/create-account) to
follow along.
## Creating Your First Prompt
Prompts are the core IP of any AI application. By managing them in PromptLayer instead of hardcoding them, you can edit prompts without deploying code, track every change, and test new versions safely.
From the PromptLayer dashboard, click **New** → **Prompt**.
A prompt starts with two messages. The **System** message sets the AI's behavior. The **User** message is what gets sent each time you run it.
**System message**: Defines the AI's persona, tone, and rules. Think of it as the "instruction manual" that stays constant across all runs. Use it for things like:
* Setting a persona ("You are a helpful assistant...")
* Defining output format ("Always respond in JSON...")
* Establishing guardrails ("Never discuss competitors...")
**User message**: The input that changes each time the prompt runs. This is where you put the actual request and any input variables. In production, your code will dynamically fill in this message.
Some prompts also use **Assistant** messages to show example responses, which helps the AI understand the expected format.
Learn more about [LLM idioms](https://blog.promptlayer.com/llm-idioms/) and why chat formats matter.
Replace the default content with:
```markdown System theme={null}
You are a Michelin-star pastry chef. Generate cake recipes with:
**Overview**: One paragraph about the cake
**Ingredients**: Bullet points with metric and US measurements
**Instructions**: Numbered steps with temperatures and timing
**Variations**: Optional frostings or substitutions
```
```markdown User theme={null}
Create a recipe for {{cake_type}} that serves {{serving_size}} people.
```
Notice `{{cake_type}}` and `{{serving_size}}`. These are [input variables](/features/prompt-registry/template-variables)—think of them like mad-libs. When you run the prompt, you'll fill these in with real values.
### Running Your Prompt
To test your prompt:
1. Click **Define input variables** in the right panel
2. Set `cake_type` to "Chocolate Cake" and `serving_size` to "8"
3. Click **Run**
Save your prompt by clicking **Save Template**.
Your application fetches prompts from PromptLayer at runtime using the SDK or REST API. This keeps prompts out of your codebase and lets you update them without redeploying. It also means PMs and domain experts can edit prompts directly in the dashboard without waiting for engineering.
```python theme={null}
from promptlayer import PromptLayer
client = PromptLayer()
# Fetch and run a prompt by name
response = client.run(
prompt_name="cake-recipe",
input_variables={"cake_type": "Chocolate", "serving_size": "8"}
)
```
See [Deployment Strategies](/onboarding-guides/deployment-strategies) for caching strategies.
## Versioning Your Prompt
PromptLayer tracks every change you make to a prompt. Each save creates a new version with a record of what changed, when, and by whom.
* Use headers to structure your prompt (`**Section**:`) - Be specific about
output format - Include examples when possible PromptLayer supports [Jinja2
templates](/features/prompt-registry/template-variables#jinja2-templates) for
more advanced variable logic. For structured outputs, see our guide on [tool
calling with
LLMs](https://blog.promptlayer.com/tool-calling-with-llms-how-and-when-to-use-it/).
### Editing Prompts
To edit a prompt, open it in the editor and make your changes. You can edit any message, add new messages, or change model settings.
Let's try it. Add this line to the end of your System message:
```
Always include a "Baker's Tip" at the end with advice for beginners.
```
Click **Save Template**. Before saving, PromptLayer shows you a diff of exactly what changed: deletions in red, additions in green. Add a commit message like "Added baker's tip requirement" and save.
Your version history appears in the left panel. You can click any previous version to view it.
Hover over a version and click **View Diff** to compare any two versions side by side.
### Writing Prompts with AI
Click the magic wand icon to open the AI prompt writer. It can help rewrite or improve your prompts based on your instructions. Try asking it to "add allergy warnings" to the recipe generator.
### Switching LLMs
PromptLayer is model-agnostic. You can switch between OpenAI, Anthropic, Google, and [other providers](/features/supported-providers) without changing your prompt. Click the model name at the bottom of the editor to switch.
All prompts work across models, including [function calling and tool use](/features/prompt-registry/tool-calling). You can also connect [private models or custom hosts](/features/custom-providers), and build [fine-tuned models](/why-promptlayer/fine-tuning).
Any model with a `base_url` can be added as a [custom
provider](/features/custom-providers) - including self-hosted models, Azure
OpenAI, or any OpenAI-compatible API.
## Deploying to Prod
Once your prompt is ready, PromptLayer can manage which version goes live. Release labels let you control which version is in production, so you can update prompts without touching code.
For engineers: PromptLayer offers several [deployment strategies](/onboarding-guides/deployment-strategies) including our Python or TypeScript SDKs, webhook-driven caching, fully managed agents, or [self-hosted](/self-hosted) deployments.
### Release Labels
Release labels control which prompt version is live. You can mark versions as "production", "staging", or "testing". Your code fetches the prompt by label, so you can update the live version without changing any code.
Learn more about [release labels](/features/prompt-registry/release-labels).
### A/B Testing
PromptLayer supports A/B testing prompts in production. Common use cases include:
* Testing a new prompt version on 10% of traffic before full rollout
* Segmenting beta users to receive an experimental prompt based on user metadata
To start an A/B test, assign release labels to different prompt versions and configure traffic splits in the dashboard.
Learn more about [A/B testing](/why-promptlayer/ab-releases).
## Building Agents
Agents are multi-step AI workflows. Unlike a single prompt, an agent can chain multiple prompts together, use tools, and make decisions based on intermediate results.
For example, you could extend the cake recipe generator into an agent that:
1. Generates the recipe (using our prompt from earlier)
2. Scales the ingredients for 100 people
3. Calculates the total cost based on current grocery prices
Create an agent by clicking **New** → **Agent**. You can build workflows visually using a drag-and-drop editor. Connect prompts, add conditionals, and loop over data. No code required.
Like prompts, agents are versioned with commit messages and can be retrieved via the SDK.
Learn more about [Agents](/why-promptlayer/agents).
## Evaluations
Evals test how well your prompts perform. Common use cases include:
* **LLM-as-judge**: Use AI to score outputs against criteria like tone, accuracy, or formatting
* **Historical backtests**: Compare a new prompt version against real production data
* **Model comparisons**: Test the same prompt across GPT-4, Claude, Gemini, etc.
* **Regression testing**: Automatically run evals when a prompt is updated to catch edge cases
* **Human grading**: Collect feedback from domain experts on prompt quality
You can build evaluation pipelines visually and connect them to prompts for continuous testing. Learn more about [Evaluations](/features/evaluations/overview).
## Logs and Analytics
Every prompt run is logged with full request and response details. You can attach [metadata](/features/prompt-history/metadata) like user IDs, session IDs, or feature flags to each request, making it easy to debug issues for specific users. [Analytics](/why-promptlayer/analytics) show cost, latency, and usage patterns across your prompts.
### Viewing Logs
Click **Logs** in the sidebar to see all requests. You can filter by prompt, [search by content](/why-promptlayer/advanced-search), and debug errors.
You can also view logs for a specific prompt by clicking **Analytics & Logs** in the prompt editor.
From the logs table, you can select historical requests and click **Backtest** to run a new prompt version against those inputs and compare results.
### Traces and Spans
For agents, [traces](/running-requests/traces) show each step of the workflow as spans. You can see timing, inputs, and outputs for every step. Traces are OpenTelemetry (OTEL) compatible, so you can integrate with your existing observability stack.
Continue to [Quickstart Part 2](/quickstart-part-two) to learn about
evaluations, backtests, and connecting PromptLayer to your code.
You can also watch our [Tutorial Videos](/tutorial-videos) for guided walkthroughs.
### Prompt Management
* [Scalable Prompt Management and Collaboration](https://blog.promptlayer.com/scalable-prompt-management-and-collaboration/) — Best practices for organizing and collaborating on prompts
* [Prompt Management](/why-promptlayer/prompt-management) — Why prompt management matters
### Evaluations
* [Eval Examples](/features/evaluations/examples) — Building RAG chatbots and migrating prompts
* [How to Evaluate LLM Prompts Beyond Simple Use Cases](https://blog.promptlayer.com/how-to-evaluate-llm-prompts-beyond-simple-use-cases/) — Advanced evaluation techniques
* [Production Traffic is the Key to Prompt Engineering](https://blog.promptlayer.com/production-traffic-is-the-key-to-prompt-engineering/) — Using real data to improve prompts
### Migration
* [Migrating Prompts to Open-Source Models](https://blog.promptlayer.com/migrating-prompts-to-open-source-models/) — Switching to Mistral and other open-source LLMs
* [Migration Guide](/migration) — Moving to PromptLayer from other solutions

---

## 

**URL:** https://docs.promptlayer.com/quickstart-part-two.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# Quickstart - Pt 2
This continues from [Quickstart Part 1](/quickstart), where we built a cake recipe generator prompt.
In Part 1, you created a prompt, tested it in the playground, and learned about versioning. Now let's evaluate prompt quality, test different models, and connect everything to your code.
## Evaluating a Prompt
Before deploying a prompt, you want to know if it's actually good. PromptLayer lets you build evaluation pipelines that score your prompt's outputs automatically.
### Creating a Dataset
Evaluations run against a dataset - a collection of test cases with inputs and expected outputs. Let's create one for our cake recipe prompt.
Click **New** → **Dataset** and name it "cake-recipes-test".
Add a few test cases. Each row needs the input variables your prompt expects (`cake_type`, `serving_size`) and optionally an expected output to compare against:
```csv theme={null}
cake_type,serving_size,expected_output
Chocolate Cake,8,"Should include cocoa or chocolate, have clear measurements"
Vanilla Birthday Cake,12,"Should be festive, mention frosting options"
Gluten-Free Lemon Cake,6,"Must not include wheat flour, should use alternatives"
Vegan Carrot Cake,10,"No eggs or dairy, should suggest substitutes"
```
Download this CSV
or add rows manually in the UI.
Learn more about [Datasets](/features/evaluations/datasets).
### Creating an Eval Pipeline
Now let's build a pipeline that runs your prompt against each test case and scores the results.
Click **New** → **Evaluation** and select your dataset.
First, add a **Prompt Template** column. This runs your prompt against each row in the dataset, using the column values as input variables. The output appears in a new column.
Next, add an **LLM-as-judge** scoring column. This uses AI to score each output against criteria you define. For our recipe prompt, we might check:
* Does the recipe include all required sections (Overview, Ingredients, Instructions)?
* Are measurements provided in both metric and US units?
* Is the serving size correct?
You can also add an **Equality Comparison** column to compare the prompt output against the `expected_output` column in your dataset.
Run the evaluation to see scores across all test cases. Learn more about [Evaluations](/features/evaluations/overview).
Beyond LLM-as-judge, PromptLayer supports:
* **Human grading**: Collect scores from domain experts
* **Equality Comparison**: Compare outputs to expected results
* **Cosine similarity**: Measure semantic similarity between outputs
* **Code evaluators**: Write custom Python scoring functions
Agent nodes work the same way in eval pipelines.
### Testing Different Models
Want to compare how your prompt performs across GPT, Claude, and Gemini? Create a new evaluation for model comparison.
Add multiple **Prompt Template** columns, each configured with a different model override. The pipeline runs your prompt on each model and shows results side by side.
This helps you find the best price/performance balance for your use case. PromptLayer supports all major providers, plus any OpenAI-compatible API via [custom base URLs](/features/custom-providers).
## Historical Backtests
Once your prompt is in production, you'll have real request logs. Use these to test new prompt versions against actual user inputs.
### Creating a Historical Dataset
Go to **Datasets** and click **Add from Request History**. This opens a request log browser where you can filter and select requests.
Filter by prompt name, date range, metadata, or search content. Select the requests you want and click **Add Requests**. This captures the real inputs your users sent, along with the outputs your current prompt produced.
### Running a Backtest
Create an evaluation that runs your new prompt version against this historical dataset. Add columns for:
* **New prompt output**: The response from your updated prompt version
* **Equality Comparison**: Side-by-side comparison highlighting changes from the original output
Backtests are powerful because they show impact on real user data without needing to define exact pass/fail criteria upfront. You can quickly spot if a change produces dramatically different outputs.
Learn more about [backtesting](/features/evaluations/continuous-integration#backtesting).
## CI/CD Evaluation
Attach an evaluation pipeline to run automatically every time you save a new prompt version - similar to GitHub Actions running tests on each commit.
When saving a prompt, the commit dialog lets you select an evaluation pipeline. Choose one and click **Next**.
From then on, each new version you create will run through the eval and show its score in the version history. This makes it easy to spot regressions before they reach production.
Learn more about [continuous integration](/features/evaluations/continuous-integration).
## Ad-Hoc Batch Runs
Evaluations aren't just for testing prompts - they're one of the best tools for running prompts in batch. Think of it like a spreadsheet where each column can be an AI-powered computation.
Upload a CSV or create a dataset, add prompt columns, run the batch, and export the results. Common use cases:
* **Data labeling**: Run GPT over production data to create labeled training datasets
* **Research**: Use web search prompts to find information about a list of companies or people
* **Content generation**: Process hundreds of items (commits, support tickets, reviews) to generate summaries or emails
* **Data enrichment**: Take a list of names and enrich with company, location, or other attributes
You don't need to build permanent evaluation infrastructure for this. One-off, ad-hoc batch runs are a perfectly valid use case. Create a dataset, add your prompt columns, run it, export, and move on.
## Connecting in Code
PromptLayer serves as the source of truth for your prompts. Your application fetches prompts by name, keeping prompt logic out of your codebase and enabling non-engineers to make changes.
### Installation
```bash Python theme={null}
pip install promptlayer
```
```bash JavaScript theme={null}
npm install promptlayer
```
### Running Prompts
Initialize the client and run a prompt. The SDK fetches the prompt template from PromptLayer, runs it against your configured LLM provider locally, then logs the result back.
```python Python theme={null}
from promptlayer import PromptLayer
client = PromptLayer()  # Uses PROMPTLAYER_API_KEY env var
response = client.run(
prompt_name="cake-recipe",
input_variables={
"cake_type": "Chocolate Cake",
"serving_size": "8"
}
)
```
```javascript JavaScript theme={null}
import { PromptLayer } from "promptlayer";
const client = new PromptLayer();
const response = await client.run({
promptName: "cake-recipe",
inputVariables: {
cake_type: "Chocolate Cake",
serving_size: "8"
}
});
```
The response includes a [prompt blueprint](/running-requests/prompt-blueprints) - a model-agnostic format that works the same whether you're using OpenAI, Anthropic, or any other provider. Access the generated content with:
```python theme={null}
print(response["prompt_blueprint"]["prompt_template"]["messages"][-1]["content"])
```
This lets you switch models without changing how you read responses.
After running, head to **Logs** in PromptLayer to see your request with the full input, output, and latency.
Use `prompt_release_label="production"` to fetch the version labeled for production. Use `prompt_version=3` to pin to a specific version number. Agents work the same way - just pass the agent name. Store API keys as environment variables (`PROMPTLAYER_API_KEY`, `OPENAI_API_KEY`) - the client reads these automatically.
### Metadata and Logging
Add metadata to track requests by user, session, or feature flag:
```python theme={null}
response = client.run(
prompt_name="cake-recipe",
input_variables={"cake_type": "Chocolate", "serving_size": "8"},
tags=["production", "recipe-feature"],
metadata={
"user_id": "user_123",
"session_id": "sess_abc"
}
)
# Add a score after reviewing the output
client.track.score(request_id=response["request_id"], score=95)
```
Your metadata and tags appear in the log details, letting you filter and search by user or feature.
Learn more about [metadata](/features/prompt-history/metadata) and [tagging](/features/prompt-history/tagging-requests).
For agents or complex workflows, enable tracing with `client = PromptLayer(enable_tracing=True)` and use the `@client.traceable` decorator on your functions to see each step as spans. Learn more about [traces](/running-requests/traces).
## Organizations
Use [organizations and workspaces](/why-promptlayer/shared-workspaces) to manage teams and environments. Common setups include separate workspaces for Production, Staging, and Development.
Key features:
* **Role-based access control**: Owner, Admin, Editor, and Viewer roles at organization and workspace levels
* **Audit logs**: Track who changed what and when
* **Author attribution**: See who created and modified each prompt version
* **Centralized billing**: Manage usage across all workspaces
Each workspace has its own API key. Switch between workspaces using the dropdown in the top navigation.
## Next Steps
* [Deployment Strategies](/onboarding-guides/deployment-strategies) - Caching, webhooks, and production patterns
* [Tutorial Videos](/tutorial-videos) - Watch walkthroughs of common workflows
* [API Reference](/reference) - Full SDK documentation

---

## 

**URL:** https://docs.promptlayer.com/reference/introduction.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# Introduction
> Use the PromptLayer REST API to manage prompts, agents, evaluations, datasets, request logs, traces, and other workspace resources programmatically.
The PromptLayer REST API lets you interact with your workspace directly over HTTP so you can automate updates, run workflows, log activity, and export or organize data from your own systems.
## Authentication
Authenticate API requests with a PromptLayer API key using the `X-API-Key` header.
Generate API keys from the API keys page in your PromptLayer dashboard. Each workspace has its own API keys, and requests use the workspace associated with the key by default.
## Endpoints
### Prompt Templates
* [Get Prompt Template](/reference/templates-get)
* [Get Prompt Template (Raw)](/reference/templates-get-raw)
* [List Prompt Templates](/reference/list-prompt-templates)
* [Publish Prompt Template](/reference/templates-publish)
* [List Prompt Template Labels](/reference/templates-labels-get)
* [Create Prompt Template Label](/reference/prompt-labels-create)
* [Move Prompt Template Label](/reference/prompt-labels-patch)
* [Delete Prompt Template Label](/reference/prompt-labels-delete)
* [Get Snippet Usage](/reference/get-snippet-usage)
### Tracking
* [Get Request](/reference/get-request)
* [Search Request Logs](/reference/search-request-logs)
* [Log Request](/reference/log-request)
* [Track Prompt](/reference/track-prompt)
* [Track Score](/reference/track-score)
* [Track Metadata](/reference/track-metadata)
* [Create Spans Bulk](/reference/spans-bulk)
* [Ingest Traces (OTLP)](/reference/otlp-ingest-traces)
### Datasets
* [List Datasets](/reference/list-datasets)
* [Get Dataset Rows](/reference/get-dataset-rows)
* [Create Dataset Group](/reference/create-dataset-group)
* [Create Dataset Version from File](/reference/create-dataset-version-from-file)
* [Create Dataset Version from Filter Params](/reference/create-dataset-version-from-filter-params)
### Evaluations
* [List Evaluations](/reference/list-evaluations)
* [Get Evaluation Rows](/reference/get-evaluation-rows)
* [Create Evaluation Pipeline](/reference/create-reports)
* [Run Report](/reference/run-report)
* [Get Report](/reference/get-report)
* [Get Report Score](/reference/get-report-score)
* [Add Report Columns](/reference/add-report-columns)
* [Edit Evaluation Pipeline Column](/reference/edit-report-column)
* [Delete Evaluation Pipeline Column](/reference/delete-report-column)
* [Update Report Score Card](/reference/update-report-score-card)
* [Rename Evaluation Pipeline](/reference/rename-report)
* [Delete Evaluation Pipeline](/reference/delete-report)
* [Delete Reports by Name](/reference/delete-reports-by-name)
Pass `folder_id` on `Create Evaluation Pipeline` (and on the create endpoints under [Prompt Templates](#prompt-templates), [Datasets](#datasets), and [Agents](#agents)) to organize new resources directly into a folder. Use [Resolve Folder ID by Path](/reference/resolve-folder-id) to look up an ID, or [Create Folder](/reference/create-folder) to make one. See [Unified Registry](#unified-registry).
### Agents
* [List Agents](/reference/list-workflows)
* [Create Agent](/reference/create-workflow)
* [Update Agent](/reference/patch-workflow)
* [Run Agent](/reference/run-workflow)
* [Get Agent Version Execution Results](/reference/workflow-version-execution-results)
### Skill Collections
* [List Skill Collections](/reference/list-skill-collections)
* [Create Skill Collection](/reference/create-skill-collection)
* [Get Skill Collection](/reference/get-skill-collection)
* [Update Skill Collection](/reference/update-skill-collection)
* [Save Skill Collection Version](/reference/save-skill-collection-version)
### Unified Registry
* [Create Folder](/reference/create-folder)
* [Update Folder](/reference/update-folder)
* [List Folder Entities](/reference/list-folder-entities)
* [Move Folder Entities](/reference/move-folder-entities)
* [Delete Folder Entities](/reference/delete-folder-entities)
* [Resolve Folder ID by Path](/reference/resolve-folder-id)

---

## 

**URL:** https://docs.promptlayer.com/CLAUDE.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# CLAUDE
# CLAUDE.md - Documentation Update Policy
## Core Principle
Documentation serves three specific audiences:
1. **Brand new users** learning what PromptLayer does
2. **Prospective users** deciding whether to use PromptLayer
3. **Current users** seeking help with specific features
Focus on features that expand WHAT users can do, not HOW WELL they can do it.
## Decision Framework
When reviewing a commit, ask:
1. **Does this introduce a NEW capability?**
* Yes → Document it
* No, just improves existing → Skip
2. **Would this influence a purchase decision?**
* Yes → Document it
* No → Skip
3. **Do users need instructions to use this?**
* Yes → Document it
* No, it's self-evident → Skip
4. **Is this part of our public API/SDK?**
* Yes → Must document
* No → Skip
## What counts as "user-facing"?
### ✅ Include these changes:
* **New features that expand capabilities** - Things users couldn't do before
* **Features that differentiate from competitors** - Unique selling points
* **Major workflow changes** - New ways of working
* **New integrations or model support** - Expands what can be connected
* **Public API/SDK changes** - All endpoints, methods, breaking changes
* **Deployment & setup options** - Self-hosting, new installation methods
* **Authentication & rate limit changes** - Security and access updates
* **Deprecations or removals** - Features going away
### ❌ Ignore these changes:
* **UI/UX improvements** - Visual enhancements, reorganizations, loading states
* **Quality of life updates** - Button positions, tooltips, helper features
* **Performance improvements** - Speed, reliability, optimization
* **Backend refactoring** - Database changes, internal APIs, queue improvements
* **Bug fixes** - Unless they prevented core functionality
* **Error message improvements** - Better wording or formatting
* **Internal API routes** - `/api/dashboard/v2/*` endpoints (internal use only)
* **Test updates** - Testing infrastructure
* **Documentation-only changes** - Doc fixes themselves
* **CI/CD changes** - Build and deployment processes
## Documentation Style
### File to Update
* Primary file: `docs/changelog/whats-new.md` (or as specified)
* Only modify this designated file - no other files
### Format Requirements
* Create or update a section titled with today's date in **YYYY-MM-DD** format
* Use bullet points only, no paragraphs
* Format each bullet as: **Component: Short title** — one-sentence summary
* Include commit hash or PR link when available
* Group changes by component if multiple changes on same day:
* **Frontend**: changes
* **API**: changes
* **Python SDK**: changes
* **JavaScript SDK**: changes
### Example Entry
```markdown theme={null}
## 2024-01-15
**Frontend: Added dark mode toggle** — Users can now switch between light and dark themes from settings menu. [#123](link)
**API: New /analytics endpoint** — Provides detailed usage analytics with customizable date ranges. [abc123](commit)
**Python SDK: Added batch processing** — New `batch_process()` method for handling multiple requests efficiently. [#456](link)
**JavaScript SDK: Breaking change to client init** — Constructor now requires explicit API version parameter. [def456](commit)
```
## Safety Guidelines
### Do's:
* Only modify the designated documentation file
* Verify changes are actually user-facing before documenting
* Keep descriptions concise and clear
* Focus on the "what" and "why" for users
* Preserve existing documentation sections
### Don'ts:
* Don't modify source code files
* Don't create new files unless explicitly instructed
* Don't duplicate entries across different dates
* Don't include internal implementation details
* Don't document draft PRs or unmerged changes
## Real Examples
### ✅ Examples to Document:
* "Add self-hosted deployment option" → New deployment capability
* "Add support for Claude 3.5 Sonnet" → New model support
* "New public API endpoint for prompt templates" → Public API addition
* "Add workflow branching and conditionals" → New feature capability
* "Introduce evaluation framework" → Major new capability
### ❌ Examples to Skip:
* "Add Jinja template snippets to slash command menu" → UX helper for existing feature
* "Add workflow execution counts display" → UI enhancement
* "Improved report score loading states" → UX improvement
* "Enhanced input variable parsing" → Backend improvement
* "Fix pagination end\_time parameter" → Internal logic fix
* "Update prompt editor modal" → UI reorganization
## Commit Review Process
When reviewing commits:
1. Apply the Decision Framework questions first
2. Look for keywords: "new", "introduce", "support for", "integration"
3. Be skeptical of: "improve", "enhance", "fix", "update", "refactor"
4. Check if it's a PUBLIC API/SDK change (not internal `/api/dashboard/` routes)
5. Skip commits with only test files or internal refactoring
## Important API Distinctions
### Public APIs (DOCUMENT):
* REST API endpoints at `/api/v1/*` or `/api/public/*`
* SDK methods in promptlayer-python or promptlayer-js
* Webhook endpoints
* Authentication endpoints
### Internal APIs (SKIP):
* `/api/dashboard/v2/*` - Internal dashboard endpoints
* `/api/internal/*` - Internal service communication
* GraphQL mutations/queries for UI only
* Admin-only endpoints
## Priority Order
When multiple changes exist, prioritize documentation by impact:
1. Breaking changes (highest priority)
2. New features or capabilities
3. Deprecations
4. UI/UX improvements
5. API/SDK enhancements
6. Bug fixes (only if user-impacting)

---

## 

**URL:** https://docs.promptlayer.com/DOCS_UPDATER_SETUP_TODO.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# DOCS UPDATER SETUP TODO
# Documentation Updater Setup - TODO
## ✅ Completed Steps
* [x] Workflow file deployed to `.github/workflows/update-docs.yml`
* [x] CLAUDE.md policy file copied to docs repository root
## 📋 Remaining Setup Steps
### 1. Install Claude Code GitHub App
Run this command in Claude Code (in a new session):
```bash theme={null}
/install-github-app
```
This will open a browser to authorize the Claude Code GitHub integration.
### 2. Create Personal Access Token (PAT)
1. **Go to:** [https://github.com/settings/tokens/new](https://github.com/settings/tokens/new)
2. **Configure the token:**
* **Token name:** `Docs Updater Read Token`
* **Expiration:** 90 days (or custom)
* **Repository access:** Select "Selected repositories" and add:
* `MagnivOrg/prompt-layer-front-end`
* `MagnivOrg/prompt-layer-api`
* `MagnivOrg/prompt-layer-library`
* `MagnivOrg/prompt-layer-js`
* **Permissions:**
* Repository permissions → **Contents:** Read
* Repository permissions → **Metadata:** Read (automatically selected)
3. **Click:** "Generate token"
4. **IMPORTANT:** Copy the token immediately (starts with `ghp_`)
### 3. Add Secrets to GitHub Repository
1. **Go to:** [https://github.com/MagnivOrg/prompt-layer-docs/settings/secrets/actions](https://github.com/MagnivOrg/prompt-layer-docs/settings/secrets/actions)
2. **Add Secret #1:**
* Click "New repository secret"
* **Name:** `GH_READ_TOKEN`
* **Value:** \[Paste your Personal Access Token from step 2]
* Click "Add secret"
3. **Add Secret #2:**
* Click "New repository secret"
* **Name:** `ANTHROPIC_API_KEY`
* **Value:** \[Your Anthropic API key from console.anthropic.com]
* Click "Add secret"
### 4. Commit and Push the Changes
```bash theme={null}
cd prompt-layer-docs
git add .github/workflows/update-docs.yml CLAUDE.md
git commit -m "feat: add automated documentation updater workflow
- Daily GitHub Action to scan repos for user-facing changes
- Uses Claude Code to identify documentable changes
- Creates one PR per day with updates
- Excludes internal APIs and UI improvements"
git push origin main
```
### 5. Test the Workflow
**Option A: Wait for the daily run**
* The workflow runs daily at 09:00 UTC
**Option B: Trigger manually (recommended for first test)**
```bash theme={null}
# From the prompt-layer-docs directory
gh workflow run update-docs.yml
gh run watch  # Watch the execution
```
**Option C: Via GitHub UI**
1. Go to: [https://github.com/MagnivOrg/prompt-layer-docs/actions](https://github.com/MagnivOrg/prompt-layer-docs/actions)
2. Click "Update Documentation" workflow
3. Click "Run workflow" → "Run workflow"
### 6. Verify the Setup
After the first run, check:
* [ ] Workflow completes successfully (green checkmark)
* [ ] If changes were found, a PR was created
* [ ] The PR contains only legitimate user-facing changes
* [ ] No internal API endpoints were documented
## 🔒 Security Checklist
* [ ] PAT has minimal permissions (read-only)
* [ ] PAT is set to expire (90 days recommended)
* [ ] Secrets are stored in GitHub, not in code
* [ ] Workflow file doesn't expose any secrets
## 🚨 Troubleshooting
### If the workflow fails:
1. **Check Actions tab:** [https://github.com/MagnivOrg/prompt-layer-docs/actions](https://github.com/MagnivOrg/prompt-layer-docs/actions)
2. **Common issues:**
* Missing secrets (GH\_READ\_TOKEN or ANTHROPIC\_API\_KEY)
* PAT doesn't have access to all 4 repos
* Claude Code GitHub app not installed
### If no PR is created:
* This is normal if no user-facing changes were found
* Check the workflow logs to see what commits were analyzed
### If wrong things are documented:
* Review the CLAUDE.md policy file
* Internal `/api/dashboard/v2/*` endpoints should be skipped
* UI improvements should be skipped
## 📞 Support
* **Claude Code issues:** [https://github.com/anthropics/claude-code/issues](https://github.com/anthropics/claude-code/issues)
* **Workflow logs:** [https://github.com/MagnivOrg/prompt-layer-docs/actions](https://github.com/MagnivOrg/prompt-layer-docs/actions)

---

## 

**URL:** https://docs.promptlayer.com/TESTING_INSTRUCTIONS.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# TESTING INSTRUCTIONS
# Testing Instructions for Documentation Examples
This document provides instructions for testing code examples in the PromptLayer documentation.
## Test Workspace
We maintain a dedicated test workspace for documentation examples with pre-configured prompts and settings.
### Workspace Details
* **Purpose**: Testing documentation code examples
* **API Key**: Available in secure storage (not committed to repo)
* **Workspace ID**: Contact team for access
## Testing Multi-Turn Chat Examples
### Required Prompts
To test the multi-turn chat examples from the documentation, you only need TWO prompts set up in your workspace:
1. **multi-turn-assistant**
* Type: Chat prompt template
* Required placeholders:
* `{{chat_history}}` - Message placeholder for conversation history
* `{{user_question}}` - Text variable for current user input
* `{{ai_in_progress}}` - Array variable for tool call tracking (can be empty list if not using tools)
* Example system message: "You are a helpful assistant. Use the conversation history to maintain context."
* Used for: Basic multi-turn conversations, context retention tests, and examples without tools
2. **multi-turn-assistant-with-tools** (Optional - only if testing tool functionality)
* Type: Chat prompt template
* Required placeholders (in this order):
* `{{chat_history}}` - Message placeholder for conversation history
* `{{user_question}}` - Text variable for current user input
* `{{ai_in_progress}}` - Message placeholder for tool interaction messages (MUST come AFTER user\_question)
* `{{user_context}}` - Object variable for additional context (optional)
* Important: The order matters! `ai_in_progress` must come after `user_question` because it contains the AI's tool interactions in response to the user's question
* Tool definitions (if testing tools):
* `search_kb` - Search knowledge base
* `create_ticket` - Create support ticket
* `escalate` - Escalate to human agent
* `end_conversation` - End the conversation
* Used for: Examples demonstrating tool usage in multi-turn conversations
### Running Tests
1. Install dependencies:
```bash theme={null}
pip install promptlayer python-dotenv
```
2. Set your API key:
* Copy `.env.example` to `.env`
* Add your PromptLayer API key to the `.env` file:
```
PROMPTLAYER_API_KEY=your_api_key_here
```
3. Run the test scripts:
```bash theme={null}
# Run all basic tests
python testing/test_multi_turn_chat.py
# Run simplified sanity checks
python testing/test_multi_turn_chat_simple.py
# Run multi-turn conversation loop test (3+ turns)
python testing/test_multi_turn_loop.py
# Run tool conversation test
python testing/test_multi_turn_with_tools.py
# Debug tool interactions
python testing/test_tool_debug.py
```
### Test Script Locations
All test scripts are located in the `testing/` folder:
* **`test_multi_turn_chat.py`** - Main test suite with all examples from documentation
* **`test_multi_turn_chat_simple.py`** - Quick sanity checks without loops
* **`test_multi_turn_loop.py`** - Tests multiple conversation turns (3+) with context retention
* **`test_multi_turn_with_tools.py`** - Demonstrates proper tool handling with ai\_in\_progress
* **`test_tool_debug.py`** - Debug script to understand tool conversation flow
## Adding New Test Prompts
When adding new documentation examples that require testing:
1. Create the prompt template in the test workspace
2. Document the required placeholders and configuration here
3. Add corresponding test cases to the test script
4. Verify all examples work before publishing documentation
## Troubleshooting
### Common Issues
1. **Missing placeholders**: Ensure all required placeholders are defined in your prompt template
2. **API key errors**: Verify your API key has access to the test workspace
3. **Tool call errors**: Check that tool definitions match the expected format in the documentation
### Debug Mode
Enable debug output by setting:
```python theme={null}
import logging
logging.basicConfig(level=logging.DEBUG)
```
## Contact
For access to the test workspace or assistance with testing, contact the PromptLayer team.

---

## 

**URL:** https://docs.promptlayer.com/agents/mcp.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# MCP
The PromptLayer MCP server lets any [Model Context Protocol](https://modelcontextprotocol.io/) compatible client interact with PromptLayer — managing prompts, tracking requests, running evaluations, and more.
## Remote Server (Streamable HTTP)
The hosted MCP server is available at:
```
https://mcp.promptlayer.com/mcp
```
Pass your PromptLayer API key via the `Authorization` header. No installation required.
### Claude Desktop / Cursor / Windsurf
Add to your MCP config:
```json theme={null}
{
"mcpServers": {
"promptlayer": {
"type": "streamable-http",
"url": "https://mcp.promptlayer.com/mcp",
"headers": {
"Authorization": "Bearer pl_your_key_here"
}
}
}
}
```
### OpenAI Responses API
The remote server works with [OpenAI's MCP tool support](https://platform.openai.com/docs/guides/tools-mcp):
```json theme={null}
{
"tools": [{
"type": "mcp",
"server_label": "promptlayer",
"server_url": "https://mcp.promptlayer.com/mcp",
"require_approval": "never",
"headers": {
"Authorization": "Bearer pl_your_key_here"
}
}]
}
```
## Local Server (stdio)
Install the server from npm: [@promptlayer/mcp-server](https://www.npmjs.com/package/@promptlayer/mcp-server)
For clients that support stdio transport (e.g. Claude Desktop, Cursor), you can run the server locally via npx:
```json theme={null}
{
"mcpServers": {
"promptlayer": {
"command": "npx",
"args": ["-y", "@promptlayer/mcp-server"],
"env": {
"PROMPTLAYER_API_KEY": "pl_your_key_here"
}
}
}
}
```
## Available Tools
The MCP server exposes 37 tools covering all major PromptLayer features:
| Category             | Tools                                                                                                                                                                                                                                    |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Prompt Templates** | `get-prompt-template`, `get-prompt-template-raw`, `list-prompt-templates`, `publish-prompt-template`, `list-prompt-template-labels`, `create-prompt-label`, `move-prompt-label`, `delete-prompt-label`, `get-snippet-usage`              |
| **Request Logs**     | `get-request`, `search-request-logs`, `get-trace`                                                                                                                                                                                        |
| **Tracking**         | `log-request`, `create-spans-bulk`                                                                                                                                                                                                       |
| **Datasets**         | `list-datasets`, `get-dataset-rows`, `create-dataset-group`, `create-dataset-version-from-file`, `create-dataset-version-from-filter-params`, `create-draft-dataset-version`, `add-request-log-to-dataset`, `save-draft-dataset-version` |
| **Evaluations**      | `list-evaluations`, `get-evaluation-rows`, `create-report`, `run-report`, `get-report`, `get-report-score`, `update-report-score-card`, `delete-reports-by-name`                                                                         |
| **Agents**           | `list-workflows`, `create-workflow`, `patch-workflow`, `run-workflow`, `get-workflow-version-execution-results`, `get-workflow`                                                                                                          |
| **Folders**          | `create-folder`, `edit-folder`, `get-folder-entities`, `move-folder-entities`, `delete-folder-entities`, `resolve-folder-id`                                                                                                             |
## Source Code
The MCP server is open source: [github.com/MagnivOrg/promptlayer-mcp](https://github.com/MagnivOrg/promptlayer-mcp)

---

## 

**URL:** https://docs.promptlayer.com/features/custom-providers.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# Custom Providers
Custom providers let you connect to additional LLM providers beyond the built-in options, including DeepSeek, Grok, and more!
## Setting Up a Custom Provider
To add a custom provider to your workspace:
1. Navigate to **Settings → Custom Providers and Models**
2. Click the **Add Custom Provider** button
3. Configure the provider with the following details:
* **Name**: A descriptive name for your provider (e.g., "DeepSeek")
* **Client**: Select the appropriate client type for your provider's base URL
* **Base URL**: The endpoint URL for your custom provider
* **API Key**
## Creating Custom Models
Once your provider is configured, you can define models for it:
1. In **Settings → Custom Providers and Models**, click on your custom provider row to expand it
2. Click **Create Custom Model**
3. Fill in the model configuration:
* **Provider**: Select the custom provider you created earlier
* **Model Name**: Choose from known models or enter a custom identifier
* **Display Name**: A friendly name that appears in the prompt playground
* **Model Type**: Specify whether this is a Chat or Completion model
## Using Custom Models
After setup, your custom models seamlessly integrate with PromptLayer's features. You can:
* Select them in the Playground alongside standard models
* Use them in the Prompt Editor for template creation
* Track requests and analyze performance just like any other model
Custom providers give you complete control over your model infrastructure while maintaining all the benefits of PromptLayer's prompt management and observability features.
## Example Integrations
Looking for specific integration guides? See our detailed setup instructions for [OpenRouter](/features/openrouter-integration), [Exa](/features/exa-integration), and [xAI (Grok)](/features/xai-integration).
Follow the steps above to configure any OpenAI-compatible provider as a custom provider in PromptLayer.

---

## 

**URL:** https://docs.promptlayer.com/features/evaluations/building-pipelines.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# Getting Started
The overall process of building an evaluation pipeline looks like this:
1. **Select Your Dataset**: Choose or upload datasets to serve as the basis for your evaluations, whether for scoring, regression testing, or bulk job processing.
2. **Build Your Pipeline**: Start by visually constructing your evaluation pipeline, defining each step from input data processing to final evaluation.
3. **Run Evaluations**: Execute your pipeline, observe the results in a spreadsheet-like interface, and make informed decisions based on comprehensive metrics and scores.
## Creating a Pipeline
1. **Initiate a Batch Run**: Start by creating a new batch run, which requires specifying a name and selecting a dataset.
2. **Dataset Selection**: Upload a CSV/JSON dataset, or create a dataset from historical data using filters like time range, prompt template logs, scores, and metadata. [Learn more here.](/features/evaluations/datasets)
You now have a pipeline. Preview mode allows you to iterate with live feedback, allowing for adjustments in real-time.
## Setting up the Pipeline
### Adding Steps
Click 'Add Step' to start building your pipeline, with each column representing a step in the evaluation process.
Steps execute in order left to right. That means that if a column depends on a previous column, make sure it appears to the right of the dependency.
#### Common Step Types
* **Prompt Template**: Select a prompt template from the registry, set model parameters, LLM, arguments, and template version.
* **Custom API Endpoint**: Define a URL to send and receive data, suitable for custom evaluators or external systems.
* **Human Input**: Engage human graders by adding a step that allows for textual input.
* **String Comparison**: Use this step to compare the outputs of two previous step, showing a visual diff when relevant.
#### Scoring
If the last step of your evaluation pipeline contains all booleans or numeric values, that will be consider the score for the row. Your full evaluation report will have a scorecard of the average of this last step.
*NOTE: All cells in the last column must be boolean or all must be numeric. If any cell deviates, the score will not be calculated*
## Executing Full Batch Runs
Transition from pipeline to full batch run to apply your pipeline across the entire dataset for comprehensive evaluation.

---

## 

**URL:** https://docs.promptlayer.com/features/evaluations/column-types.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# Node & Column Types
> Complete reference for all node types used in Agents and evaluation pipelines
This page documents all available node types for Agents (workflows) and column types for evaluation pipelines. Agents and evaluations share the same node types—each has specific configuration options that determine its behavior.
In Agents, these are called **nodes**. In evaluation pipelines, they're called
**columns**. The configuration is identical.
## How Column Sources Work
Columns can reference data from two places:
1. **Dataset columns** - Reference data directly from your dataset by using the dataset column name
2. **Other evaluation columns** - Reference the output of a previous column by using that column's `name`
When you specify a `source` or include a column name in `sources`, the system first looks for an evaluation column with that name, then falls back to looking for a dataset column.
Columns are executed in order based on their `position`. A column can only
reference other columns that come before it in the pipeline.
### Example: Chaining Columns Together
A common pattern is to chain columns: run a prompt, extract a field from the JSON output, then compare it to a ground truth value from the dataset.
```python theme={null}
columns = [
# Step 1: Run the prompt template (position 1)
{
"column_type": "PROMPT_TEMPLATE",
"name": "LLM Output",  # Other columns reference this name
"configuration": {
"template": {"name": "my-prompt"},
"prompt_template_variable_mappings": {
"question": "user_question"  # Maps to dataset column
}
}
},
# Step 2: Extract a field from the LLM output (position 2)
{
"column_type": "JSON_PATH",
"name": "Extracted Status",
"configuration": {
"source": "LLM Output",  # References the column above
"json_path": "$.status"
}
},
# Step 3: Compare extracted value to dataset ground truth (position 3)
{
"column_type": "COMPARE",
"name": "Status Match",
"configuration": {
"sources": [
"Extracted Status",   # References the column above
"expected_status"     # References a dataset column
],
"comparison_type": {"type": "STRING"}
},
"is_part_of_score": True
}
]
```
## Execution Types
These columns execute prompts, code, or external services.
Runs a prompt template against each row. You can reference a template from the Prompt Registry or define one inline.
**Registry Reference (using `template`)**
| Field                               | Type    | Required | Description                                           |
| ----------------------------------- | ------- | -------- | ----------------------------------------------------- |
| `template.name`                     | string  | Yes      | Name of the prompt template                           |
| `template.version_number`           | integer | No       | Specific version number. Uses latest if omitted       |
| `template.label`                    | string  | No       | Release label to use, e.g. "production"               |
| `prompt_template_variable_mappings` | object  | Yes      | Maps template input variables to dataset/column names |
| `engine`                            | object  | No       | Override the template's default model settings        |
| `engine.provider`                   | string  | No       | Provider name, e.g. "openai", "anthropic"             |
| `engine.model`                      | string  | No       | Model name, e.g. "gpt-4", "claude-3-opus"             |
| `engine.parameters`                 | object  | No       | Model parameters like temperature, max\_tokens        |
The `prompt_template_variable_mappings` object maps **prompt input variables** (keys) to **dataset or column names** (values). The key is the variable name in your prompt template (e.g., `{{question}}`), and the value is where to get the data from.
```json theme={null}
{
"column_type": "PROMPT_TEMPLATE",
"name": "Generate Response",
"configuration": {
"template": {
"name": "my-prompt",
"label": "production"
},
"prompt_template_variable_mappings": {
"question": "user_question",
"context": "retrieved_context"
}
}
}
```
**Complete example with all input variables:**
If your prompt template has variables `{{company}}`, `{{product}}`, and `{{query}}`, map each one:
```json theme={null}
{
"column_type": "PROMPT_TEMPLATE",
"name": "Product Analysis",
"configuration": {
"template": {
"name": "product-analyzer"
},
"prompt_template_variable_mappings": {
"company": "company_name",
"product": "product_name",
"query": "user_query"
}
}
}
```
**Inline Template (using `inline_template`)**
Define a prompt template directly in the configuration without saving it to the registry. This is useful for quick experimentation or one-off evaluations.
| Field                                   | Type    | Required | Description                                           |
| --------------------------------------- | ------- | -------- | ----------------------------------------------------- |
| `inline_template.inline`                | boolean | Yes      | Must be `true`                                        |
| `inline_template.prompt_template`       | object  | Yes      | The template content (chat or completion format)      |
| `inline_template.metadata`              | object  | No       | Model configuration (provider, name, parameters)      |
| `inline_template.source_prompt_name`    | string  | No       | Name of the registry prompt this was derived from     |
| `inline_template.source_prompt_version` | integer | No       | Version number of the source prompt                   |
| `prompt_template_variable_mappings`     | object  | Yes      | Maps template input variables to dataset/column names |
```json theme={null}
{
"column_type": "PROMPT_TEMPLATE",
"name": "Generate Response",
"configuration": {
"inline_template": {
"inline": true,
"prompt_template": {
"type": "chat",
"messages": [
{
"role": "system",
"content": [{"type": "text", "text": "You are a helpful assistant."}]
},
{
"role": "user",
"content": [{"type": "text", "text": "Answer: {question}"}]
}
]
},
"metadata": {
"model": {
"provider": "openai",
"name": "gpt-4",
"parameters": {"temperature": 0.7}
}
}
},
"prompt_template_variable_mappings": {
"question": "user_question"
}
}
}
```
You must provide exactly one of `template` or `inline_template`. They are mutually exclusive.
Executes custom Python or JavaScript code. The code receives a `data` dictionary containing all column values for the current row.
| Field      | Type   | Required | Description              |
| ---------- | ------ | -------- | ------------------------ |
| `code`     | string | Yes      | The code to execute      |
| `language` | string | Yes      | "PYTHON" or "JAVASCRIPT" |
```json theme={null}
{
"column_type": "CODE_EXECUTION",
"name": "Custom Logic",
"configuration": {
"code": "result = len(data['response'].split())\nreturn result",
"language": "PYTHON"
}
}
```
Calls an external HTTP endpoint. The request body contains all column values for the current row.
| Field     | Type   | Required | Description             |
| --------- | ------ | -------- | ----------------------- |
| `url`     | string | Yes      | The HTTP endpoint URL   |
| `headers` | object | No       | HTTP headers to include |
```json theme={null}
{
"column_type": "ENDPOINT",
"name": "External Validator",
"configuration": {
"url": "https://api.example.com/validate",
"headers": {
"Authorization": "Bearer token123"
}
}
}
```
Runs a PromptLayer workflow.
| Field                     | Type    | Required | Description                              |
| ------------------------- | ------- | -------- | ---------------------------------------- |
| `workflow_id`             | integer | Yes      | ID of the workflow to run                |
| `workflow_version_number` | integer | No       | Specific version. Uses latest if omitted |
| `workflow_label`          | string  | No       | Release label to use                     |
| `input_mappings`          | object  | Yes      | Maps workflow inputs to column names     |
```json theme={null}
{
"column_type": "WORKFLOW",
"name": "Run Analysis Workflow",
"configuration": {
"workflow_id": 123,
"input_mappings": {
"input_text": "response"
}
}
}
```
Executes an MCP (Model Context Protocol) action.
| Field            | Type    | Required | Description                      |
| ---------------- | ------- | -------- | -------------------------------- |
| `mcp_server_id`  | integer | Yes      | ID of the MCP server             |
| `tool_name`      | string  | Yes      | Name of the tool to call         |
| `input_mappings` | object  | Yes      | Maps tool inputs to column names |
```json theme={null}
{
"column_type": "MCP",
"name": "MCP Tool Call",
"configuration": {
"mcp_server_id": 456,
"tool_name": "search",
"input_mappings": {
"query": "search_query"
}
}
}
```
Adds a column for manual human evaluation.
| Field        | Type   | Required | Description                    |
| ------------ | ------ | -------- | ------------------------------ |
| `data_type`  | string | Yes      | "number" or "string"           |
| `ui_element` | object | Yes      | UI configuration for the input |
```json theme={null}
{
"column_type": "HUMAN",
"name": "Human Rating",
"configuration": {
"data_type": "number",
"ui_element": {
"type": "slider",
"min": 1,
"max": 5
}
}
}
```
Simulates multi-turn conversations to test chatbots and conversational agents. An AI-powered user persona engages in realistic dialogue with your prompt template, allowing you to evaluate how well your agent handles extended interactions.
| Field                                  | Type    | Required    | Description                                                                                                                                                            |
| -------------------------------------- | ------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `template.name`                        | string  | Yes         | Name of the prompt template to test                                                                                                                                    |
| `template.version_number`              | integer | No          | Specific version number                                                                                                                                                |
| `template.label`                       | string  | No          | Release label to use                                                                                                                                                   |
| `prompt_template_variable_mappings`    | object  | Yes         | Maps template input variables to dataset columns                                                                                                                       |
| `user_persona`                         | string  | Conditional | Static persona description. Required if `user_persona_source` not set                                                                                                  |
| `user_persona_source`                  | string  | Conditional | Column name containing the persona. Required if `user_persona` not set                                                                                                 |
| `conversation_completed_prompt`        | string  | No          | Guidance for when to consider the conversation complete (e.g., "End when the user confirms their order" or "Complete when the assistant calls the submit\_order tool") |
| `conversation_completed_prompt_source` | string  | No          | Column name containing the completion guidance. Use instead of `conversation_completed_prompt` for dynamic guidance                                                    |
| `is_user_first`                        | boolean | No          | If true, simulated user sends the first message (default: false)                                                                                                       |
| `max_turns`                            | integer | No          | Maximum conversation turns (default: system setting, max: 150)                                                                                                         |
| `conversation_samples`                 | array   | No          | Example conversations to guide the simulation style                                                                                                                    |
The `user_persona` defines how the simulated user behaves - their goals, communication style, and what questions they ask. Use `user_persona_source` to pull different personas from your dataset for varied test scenarios.
The `conversation_completed_prompt` provides explicit guidance for determining when a conversation should end. This is useful for defining specific end conditions like tool calls, confirmation messages, or goal achievement. The guidance can be holistic (general rules) or specific (look for a certain phrase or tool call).
**Basic example with static persona:**
```json theme={null}
{
"column_type": "CONVERSATION_SIMULATOR",
"name": "Support Chat Test",
"configuration": {
"template": {
"name": "customer-support-bot"
},
"prompt_template_variable_mappings": {
"customer_name": "customer_name",
"product": "product_name"
},
"user_persona": "You are a frustrated customer who purchased a defective product. You want a refund but will accept a replacement if the agent is helpful. Ask follow-up questions and push back on unhelpful responses.",
"is_user_first": true,
"max_turns": 5
}
}
```
**Dynamic personas from dataset:**
For comprehensive testing, store different user personas in your dataset to test various scenarios:
```json theme={null}
{
"column_type": "CONVERSATION_SIMULATOR",
"name": "Multi-Scenario Test",
"configuration": {
"template": {
"name": "sales-assistant"
},
"prompt_template_variable_mappings": {
"rep_name": "rep_name",
"product": "product_name",
"customer_context": "customer_context"
},
"user_persona_source": "test_persona",
"is_user_first": true,
"max_turns": 6
}
}
```
Where your dataset has a `test_persona` column with different personas:
* Row 1: "You are a busy executive who needs quick answers. Be impatient if responses are too long."
* Row 2: "You are a technical user who asks detailed follow-up questions about implementation."
* Row 3: "You are price-sensitive and keep asking about discounts and alternatives."
**Custom completion conditions:**
Use `conversation_completed_prompt` to define specific end conditions for your conversations:
```json theme={null}
{
"column_type": "CONVERSATION_SIMULATOR",
"name": "Order Flow Test",
"configuration": {
"template": {
"name": "order-assistant"
},
"prompt_template_variable_mappings": {
"customer_name": "customer_name",
"order_items": "items"
},
"user_persona": "You are a customer placing an order. Provide your shipping address and payment method when asked.",
"conversation_completed_prompt": "The conversation is complete when the assistant calls the submit_order tool or confirms that the order has been placed successfully.",
"max_turns": 10
}
}
```
You can also use `conversation_completed_prompt_source` to pull completion guidance from your dataset:
```json theme={null}
{
"column_type": "CONVERSATION_SIMULATOR",
"name": "Goal-Based Test",
"configuration": {
"template": {
"name": "support-agent"
},
"prompt_template_variable_mappings": {
"context": "support_context"
},
"user_persona_source": "test_persona",
"conversation_completed_prompt_source": "completion_condition",
"max_turns": 8
}
}
```
Where your dataset has a `completion_condition` column with different end conditions:
* Row 1: "End when the

[SCRAPER NOTE: This page was truncated by the scraper due to length. Content continues on the live page.]

---

## 

**URL:** https://docs.promptlayer.com/features/evaluations/continuous-integration.md

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.promptlayer.com/llms.txt
> Use this file to discover all available pages before exploring further.
# Continuous Integration
Continuous Integration (CI) of prompt evaluations is the holy grail of prompt engineering. 🏆
CI in the context of prompt engineering involves the automated testing and validation of prompts every time a new version is created or updated. LLMs are a probabilistic technology. It is hard (read: virtually impossible) to ensure a new prompt version doesn't break old user behavior just by eyeballing the prompt. Rigorous testing is the best tool we have.
We believe that it's important to both allow subject-matter experts to write new prompts and provide them with tools to easily test if the prompts broke anything. That's where PromptLayer evaluations comes in.
## Test-driven Prompt Engineering
Similar to test-driven development (TDD) in software engineering, test-driven prompt engineering involves writing and running evaluations against new prompt versions before they are used in production. This proactive testing ensures that new prompts meet predefined criteria and behave as expected, minimizing the risk of unintended consequences.
Setting up automatic evaluations on a specific prompt template is easy. When creating a new version, after adding a commit message, you will be prompted to select an evaluation pipeline to run. After doing this once, every new prompt template you create will run this pipeline by default.
**NOTE**: Make sure your evaluation pipeline uses the "latest" version of the prompt template in its column step. The template is fetched at runtime. If you specify a frozen version, the evaluation report won't reflect your newest prompt template.
## Testing Strategies
### Backtesting
Backtesting involves running new prompt versions against a dataset compiled from historical production data. This strategy provides a real-world context for evaluating prompts, allowing you to assess how new versions would have performed under past conditions. It's an effective way to detect potential regressions and validate improvements, ensuring that updates enhance rather than detract from the user experience.
To set up backtests, follow the steps below:
**1. Create a historical dataset**
[Create a dataset](/features/evaluations/datasets) using a search query. For example, I might want to create a dataset using all logged requests:
* That use `my_prompt_template` version 6 or version 5
* That were made in the last 2 months
* That were using the tag `prod`
* That users gave a 👍 response to
This dataset will help you understand if your new prompt version broke any previous versions!
**2. Build an evaluation pipeline**
The next step is to create an evaluation pipeline using our new historical dataset.
In plain English, this evaluation will feed in historical request context into your new prompt version then compare the new results to the old results. You can do a simple string comparison or get fancy with cosine similarities. PromptLayer will even show you a diff view for responses that are different.
**3. Run it when you make a new version**
This is the fun part. Next time you make a new prompt version, just select our new backtesting pipeline to see how the new prompt version fairs.
### Regression Testing
Regression testing is the continuous refinement of evaluation datasets to include new edge cases and scenarios as they are discovered. This iterative process ensures that prompts remain robust against a growing set of challenges, preventing regressions in areas previously identified as potential failure points. By continually updating evaluations with new edge cases, you maintain a high standard of prompt quality and reliability.
The process of setting up regression tests looks similar to backtesting.
[Create a dataset](/features/evaluations/datasets) containing test cases for every edge case you can think of. The dataset should include context variables that you can input to your prompt template.
### Scoring
The evaluation can result in a single quantitative final score. To configure the score card, all you need to do is make sure that the last step consists entirely of numbers or Booleans. A final objective score makes comparing prompt performance easy, and it will be displayed alongside prompts in the Prompt Registry.
