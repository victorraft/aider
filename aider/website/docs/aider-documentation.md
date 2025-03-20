This file is a merged representation of a subset of the codebase, containing specifically included files and files not matching ignore patterns, combined into a single document by Repomix.
The content has been processed where comments have been removed, empty lines have been removed, content has been formatted for parsing in markdown style, content has been compressed (code blocks are separated by ⋮---- delimiter).

# File Summary

## Purpose
This file contains a packed representation of the entire repository's contents.
It is designed to be easily consumable by AI systems for analysis, code review,
or other automated processes.

## File Format
The content is organized as follows:
1. This summary section
2. Repository information
3. Directory structure
4. Multiple file entries, each consisting of:
  a. A header with the file path (## File: path/to/file)
  b. The full contents of the file in a code block

## Usage Guidelines
- This file should be treated as read-only. Any changes should be made to the
  original repository files, not this packed version.
- When processing this file, use the file path to distinguish
  between different files in the repository.
- Be aware that this file may contain sensitive information. Handle it with
  the same level of security as you would the original repository.

## Notes
- Some files may have been excluded based on .gitignore rules and Repomix's configuration
- Binary files are not included in this packed representation. Please refer to the Repository Structure section for a complete list of file paths, including binary files
- Only files matching these patterns are included: usage.md, install.md, config.md, faq.md, git.md, llms.md, languages.md, usage/**/*.md, troubleshooting/**/*.md, config/**/*.md, install/**/*.md, llms/**/*.md, ../../README.md, ../../CHANGELOG.md, ../../CONTRIBUTING.md
- Files matching these patterns are excluded: **/*.svg, **/*.jpg, **/*.jpeg, **/*.png, **/*.gif, **/LICENSE*, **/node_modules/**, **/venv/**, **/.git/**, **/tests/**, **/test/**, **/*_test.py, **/*_tests.py, **/test_*.py, ../**/*.html, ../**/*.yml, ../**/*.json, ../**/*.css, ../**/*.js, ../**/*.scss, ../**/*.ps1, ../**/*.sh, ../_*, ../assets/**, ../blog/**, ../_posts/**, ../_sass/**, ../_includes/**, ../_layouts/**, ../_data/**, ../examples/**, ../share/**, recordings/**, leaderboards/**, legal/**, more/**, repomap.md, scripting.md, unified-diffs.md, benchmarks*.md, ctags.md, more-info.md, **/HISTORY.md
- Files matching patterns in .gitignore are excluded
- Files matching default ignore patterns are excluded
- Code comments have been removed from supported file types
- Empty lines have been removed from all files
- Content has been formatted for parsing in markdown style
- Content has been compressed - code blocks are separated by ⋮---- delimiter
- Files are sorted by Git change count (files with more changes are at the bottom)

## Additional Info

# Directory Structure
```
config/
  adv-model-settings.md
  aider_conf.md
  api-keys.md
  dotenv.md
  editor.md
  model-aliases.md
  options.md
  reasoning.md
install/
  codespaces.md
  docker.md
  optional.md
  replit.md
llms/
  anthropic.md
  azure.md
  bedrock.md
  cohere.md
  deepseek.md
  gemini.md
  groq.md
  lm-studio.md
  ollama.md
  openai-compat.md
  openai.md
  openrouter.md
  other.md
  vertex.md
  warnings.md
  xai.md
troubleshooting/
  aider-not-found.md
  edit-errors.md
  imports.md
  models-and-keys.md
  support.md
  token-limits.md
  warnings.md
usage/
  browser.md
  caching.md
  commands.md
  conventions.md
  copypaste.md
  images-urls.md
  lint-test.md
  modes.md
  not-code.md
  notifications.md
  tips.md
  tutorials.md
  voice.md
  watch.md
config.md
faq.md
git.md
install.md
languages.md
llms.md
usage.md
```

# Files

## File: config/adv-model-settings.md
`````markdown
---
parent: Configuration
nav_order: 950
description: Configuring advanced settings for LLMs.
---

# Advanced model settings

## Context window size and token costs

In most cases, you can safely ignore aider's warning about unknown context
window size and model costs.

{: .note }
Aider never *enforces* token limits, it only *reports* token limit errors
from the API provider.
You probably don't need to
configure aider with the proper token limits
for unusual models.

But, you can register context window limits and costs for models that aren't known
to aider. Create a `.aider.model.metadata.json` file in one of these locations:

- Your home directory.
- The root if your git repo.
- The current directory where you launch aider.
- Or specify a specific file with the `--model-metadata-file <filename>` switch.


If the files above exist, they will be loaded in that order. 
Files loaded last will take priority.

The json file should be a dictionary with an entry for each model, as follows:

```
{
    "deepseek/deepseek-chat": {
        "max_tokens": 4096,
        "max_input_tokens": 32000,
        "max_output_tokens": 4096,
        "input_cost_per_token": 0.00000014,
        "output_cost_per_token": 0.00000028,
        "litellm_provider": "deepseek",
        "mode": "chat"
    }
}
```

{: .tip }
Use a fully qualified model name with a `provider/` at the front
in the `.aider.model.metadata.json` file.
For example, use `deepseek/deepseek-chat`, not just `deepseek-chat`.
That prefix should match the `litellm_provider` field.

### Contribute model metadata

Aider relies on
[litellm's model_prices_and_context_window.json file](https://github.com/BerriAI/litellm/blob/main/model_prices_and_context_window.json) 
for model metadata.

Consider submitting a PR to that file to add missing models.

## Model settings

Aider has a number of settings that control how it works with
different models.
These model settings are pre-configured for most popular models.
But it can sometimes be helpful to override them or add settings for
a model that aider doesn't know about.


### Configuration file locations

You can override or add settings for any model by creating a `.aider.model.settings.yml` file in one of these locations:

- Your home directory.
- The root of your git repo.
- The current directory where you launch aider.
- Or specify a specific file with the `--model-settings-file <filename>` switch.

If the files above exist, they will be loaded in that order. 
Files loaded last will take priority.

The yaml file should be a list of dictionary objects for each model.


### Global extra params

You can use the special model name `aider/extra_params` to define 
`extra_params` that will be passed to `litellm.completion()` for all models.
Only the `extra_params` dict is used from this special model name.

For example:

```yaml
- name: aider/extra_params
  extra_params:
    extra_headers:
      Custom-Header: value
    max_tokens: 8192
```

These settings will be merged with any model-specific settings, with the 
`aider/extra_params` settings taking precedence for any direct conflicts.

### Controlling o1 reasoning effort

You need this chunk of yaml:

```
  extra_params:
    extra_body:
      reasoning_effort: high
```

This is a full entry for o1 with that setting, obtained by finding the default
entry in the list below and adding the above `extra_params` entry:

```
- name: o1
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  send_undo_reply: false
  lazy: false
  reminder: user
  examples_as_sys_msg: false
  cache_control: false
  caches_by_default: false
  use_system_prompt: true
  use_temperature: false
  streaming: false
  editor_model_name: gpt-4o
  editor_edit_format: editor-diff
  extra_params:
    extra_body:
      reasoning_effort: high
```

### Default model settings

Below are all the pre-configured model settings to give a sense for the settings which are supported.

You can also look at the `ModelSettings` class in
[models.py](https://github.com/Aider-AI/aider/blob/main/aider/models.py)
file for more details about all of the model setting that aider supports.

The first entry shows all the settings, with their default values.
For a real model,
you just need to include whichever fields that you want to override the defaults.

<!--[[[cog
from aider.models import get_model_settings_as_yaml
cog.out("```yaml\n")
cog.out(get_model_settings_as_yaml())
cog.out("```\n")
]]]-->
```yaml
- name: (default values)
  edit_format: whole
  weak_model_name: null
  use_repo_map: false
  send_undo_reply: false
  lazy: false
  reminder: user
  examples_as_sys_msg: false
  extra_params: null
  cache_control: false
  caches_by_default: false
  use_system_prompt: true
  use_temperature: true
  streaming: true
  editor_model_name: null
  editor_edit_format: null
  reasoning_tag: null
  remove_reasoning: null
  system_prompt_prefix: null
  accepts_settings: null

- name: anthropic/claude-3-5-haiku-20241022
  edit_format: diff
  weak_model_name: anthropic/claude-3-5-haiku-20241022
  use_repo_map: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25
  cache_control: true

- name: anthropic/claude-3-5-sonnet-20240620
  edit_format: diff
  weak_model_name: anthropic/claude-3-5-haiku-20241022
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25
    max_tokens: 8192
  cache_control: true
  editor_model_name: anthropic/claude-3-5-sonnet-20240620
  editor_edit_format: editor-diff

- name: anthropic/claude-3-5-sonnet-20241022
  edit_format: diff
  weak_model_name: anthropic/claude-3-5-haiku-20241022
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25
    max_tokens: 8192
  cache_control: true
  editor_model_name: anthropic/claude-3-5-sonnet-20241022
  editor_edit_format: editor-diff

- name: anthropic/claude-3-5-sonnet-latest
  edit_format: diff
  weak_model_name: anthropic/claude-3-5-haiku-20241022
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25
    max_tokens: 8192
  cache_control: true
  editor_model_name: anthropic/claude-3-5-sonnet-20241022
  editor_edit_format: editor-diff

- name: anthropic/claude-3-7-sonnet-20250219
  edit_format: diff
  weak_model_name: anthropic/claude-3-5-haiku-20241022
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25,output-128k-2025-02-19
    max_tokens: 64000
  cache_control: true
  editor_model_name: anthropic/claude-3-7-sonnet-20250219
  editor_edit_format: editor-diff
  accepts_settings:
  - thinking_tokens

- name: anthropic/claude-3-7-sonnet-latest
  edit_format: diff
  weak_model_name: anthropic/claude-3-5-haiku-20241022
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25,output-128k-2025-02-19
    max_tokens: 64000
  cache_control: true
  editor_model_name: anthropic/claude-3-7-sonnet-latest
  editor_edit_format: editor-diff
  accepts_settings:
  - thinking_tokens

- name: anthropic/claude-3-haiku-20240307
  weak_model_name: anthropic/claude-3-haiku-20240307
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25
  cache_control: true

- name: azure/o1
  edit_format: diff
  weak_model_name: azure/gpt-4o-mini
  use_repo_map: true
  use_temperature: false
  streaming: false
  editor_model_name: azure/gpt-4o
  editor_edit_format: editor-diff
  accepts_settings:
  - reasoning_effort

- name: azure/o1-mini
  weak_model_name: azure/gpt-4o-mini
  use_repo_map: true
  use_system_prompt: false
  use_temperature: false
  editor_model_name: azure/gpt-4o
  editor_edit_format: editor-diff

- name: azure/o1-preview
  edit_format: diff
  weak_model_name: azure/gpt-4o-mini
  use_repo_map: true
  use_system_prompt: false
  use_temperature: false
  editor_model_name: azure/gpt-4o
  editor_edit_format: editor-diff

- name: azure/o3-mini
  edit_format: diff
  weak_model_name: azure/gpt-4o-mini
  use_repo_map: true
  use_temperature: false
  editor_model_name: azure/gpt-4o
  editor_edit_format: editor-diff
  system_prompt_prefix: 'Formatting re-enabled. '
  accepts_settings:
  - reasoning_effort

- name: bedrock/anthropic.claude-3-5-haiku-20241022-v1:0
  edit_format: diff
  weak_model_name: bedrock/anthropic.claude-3-5-haiku-20241022-v1:0
  use_repo_map: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25
  cache_control: true

- name: bedrock/anthropic.claude-3-5-sonnet-20241022-v2:0
  edit_format: diff
  weak_model_name: bedrock/anthropic.claude-3-5-haiku-20241022-v1:0
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25
    max_tokens: 8192
  cache_control: true
  editor_model_name: bedrock/anthropic.claude-3-5-sonnet-20241022-v2:0
  editor_edit_format: editor-diff

- name: bedrock/anthropic.claude-3-7-sonnet-20250219-v1:0
  edit_format: diff
  weak_model_name: bedrock/anthropic.claude-3-5-haiku-20241022-v1:0
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25,output-128k-2025-02-19
    max_tokens: 64000
  cache_control: true
  editor_model_name: bedrock/anthropic.claude-3-7-sonnet-20250219-v1:0
  editor_edit_format: editor-diff
  accepts_settings:
  - thinking_tokens

- name: bedrock/us.anthropic.claude-3-7-sonnet-20250219-v1:0
  edit_format: diff
  weak_model_name: bedrock/us.anthropic.claude-3-5-haiku-20241022-v1:0
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25,output-128k-2025-02-19
    max_tokens: 64000
  cache_control: true
  editor_model_name: bedrock/us.anthropic.claude-3-7-sonnet-20250219-v1:0
  editor_edit_format: editor-diff
  accepts_settings:
  - thinking_tokens

- name: bedrock_converse/anthropic.claude-3-7-sonnet-20250219-v1:0
  edit_format: diff
  weak_model_name: bedrock_converse/anthropic.claude-3-5-haiku-20241022-v1:0
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25,output-128k-2025-02-19
    max_tokens: 64000
  cache_control: true
  editor_model_name: bedrock_converse/anthropic.claude-3-7-sonnet-20250219-v1:0
  editor_edit_format: editor-diff
  accepts_settings:
  - thinking_tokens

- name: bedrock_converse/us.anthropic.claude-3-7-sonnet-20250219-v1:0
  edit_format: diff
  weak_model_name: bedrock_converse/us.anthropic.claude-3-5-haiku-20241022-v1:0
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25,output-128k-2025-02-19
    max_tokens: 64000
  cache_control: true
  editor_model_name: bedrock_converse/us.anthropic.claude-3-7-sonnet-20250219-v1:0
  editor_edit_format: editor-diff
  accepts_settings:
  - thinking_tokens

- name: claude-3-5-haiku-20241022
  edit_format: diff
  weak_model_name: claude-3-5-haiku-20241022
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25
  cache_control: true

- name: claude-3-5-sonnet-20240620
  edit_format: diff
  weak_model_name: claude-3-5-haiku-20241022
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25
    max_tokens: 8192
  cache_control: true
  editor_model_name: claude-3-5-sonnet-20240620
  editor_edit_format: editor-diff

- name: claude-3-5-sonnet-20241022
  edit_format: diff
  weak_model_name: claude-3-5-haiku-20241022
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25
    max_tokens: 8192
  cache_control: true
  editor_model_name: claude-3-5-sonnet-20241022
  editor_edit_format: editor-diff

- name: claude-3-7-sonnet-20250219
  edit_format: diff
  weak_model_name: claude-3-5-haiku-20241022
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25,output-128k-2025-02-19
    max_tokens: 64000
  cache_control: true
  editor_model_name: claude-3-7-sonnet-20250219
  editor_edit_format: editor-diff
  accepts_settings:
  - thinking_tokens

- name: claude-3-7-sonnet-latest
  edit_format: diff
  weak_model_name: claude-3-5-haiku-20241022
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25,output-128k-2025-02-19
    max_tokens: 64000
  cache_control: true
  editor_model_name: claude-3-7-sonnet-latest
  editor_edit_format: editor-diff
  accepts_settings:
  - thinking_tokens

- name: claude-3-haiku-20240307
  weak_model_name: claude-3-haiku-20240307
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25
  cache_control: true

- name: claude-3-opus-20240229
  edit_format: diff
  weak_model_name: claude-3-5-haiku-20241022
  use_repo_map: true

- name: claude-3-sonnet-20240229
  weak_model_name: claude-3-5-haiku-20241022

- name: cohere_chat/command-a-03-2025
  examples_as_sys_msg: true

- name: command-r-08-2024
  weak_model_name: command-r-08-2024
  use_repo_map: true

- name: command-r-plus
  weak_model_name: command-r-plus
  use_repo_map: true

- name: command-r-plus-08-2024
  weak_model_name: command-r-plus-08-2024
  use_repo_map: true

- name: deepseek-chat
  edit_format: diff
  use_repo_map: true
  reminder: sys
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 8192

- name: deepseek-coder
  edit_format: diff
  use_repo_map: true
  reminder: sys
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 8192
  caches_by_default: true

- name: deepseek/deepseek-chat
  edit_format: diff
  use_repo_map: true
  reminder: sys
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 8192
  caches_by_default: true

- name: deepseek/deepseek-coder
  edit_format: diff
  use_repo_map: true
  reminder: sys
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 8192
  caches_by_default: true

- name: deepseek/deepseek-reasoner
  edit_format: diff
  weak_model_name: deepseek/deepseek-chat
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 8192
  caches_by_default: true
  use_temperature: false
  editor_model_name: deepseek/deepseek-chat
  editor_edit_format: editor-diff

- name: fireworks_ai/accounts/fireworks/models/deepseek-r1
  edit_format: diff
  weak_model_name: fireworks_ai/accounts/fireworks/models/deepseek-v3
  use_repo_map: true
  extra_params:
    max_tokens: 160000
  use_temperature: false
  editor_model_name: fireworks_ai/accounts/fireworks/models/deepseek-v3
  editor_edit_format: editor-diff
  reasoning_tag: think

- name: fireworks_ai/accounts/fireworks/models/deepseek-v3
  edit_format: diff
  use_repo_map: true
  reminder: sys
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 128000

- name: fireworks_ai/accounts/fireworks/models/qwq-32b
  edit_format: diff
  weak_model_name: fireworks_ai/accounts/fireworks/models/qwen2p5-coder-32b-instruct
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 32000
    top_p: 0.95
  use_temperature: 0.6
  editor_model_name: fireworks_ai/accounts/fireworks/models/qwen2p5-coder-32b-instruct
  editor_edit_format: editor-diff
  reasoning_tag: think

- name: gemini/gemini-1.5-flash-002

- name: gemini/gemini-1.5-flash-exp-0827

- name: gemini/gemini-1.5-pro
  edit_format: diff-fenced
  use_repo_map: true

- name: gemini/gemini-1.5-pro-002
  edit_format: diff
  use_repo_map: true

- name: gemini/gemini-1.5-pro-exp-0827
  edit_format: diff-fenced
  use_repo_map: true

- name: gemini/gemini-1.5-pro-latest
  edit_format: diff-fenced
  use_repo_map: true

- name: gemini/gemini-2.0-flash
  edit_format: diff
  use_repo_map: true

- name: gemini/gemini-2.0-flash-exp
  edit_format: diff
  use_repo_map: true

- name: gemini/gemini-exp-1114
  edit_format: diff
  use_repo_map: true

- name: gemini/gemini-exp-1121
  edit_format: diff
  use_repo_map: true

- name: gemini/gemini-exp-1206
  edit_format: diff
  use_repo_map: true

- name: gemini/gemma-3-27b-it
  use_system_prompt: false

- name: gpt-3.5-turbo
  weak_model_name: gpt-4o-mini
  reminder: sys

- name: gpt-3.5-turbo-0125
  weak_model_name: gpt-4o-mini
  reminder: sys

- name: gpt-3.5-turbo-0613
  weak_model_name: gpt-4o-mini
  reminder: sys

- name: gpt-3.5-turbo-1106
  weak_model_name: gpt-4o-mini
  reminder: sys

- name: gpt-3.5-turbo-16k-0613
  weak_model_name: gpt-4o-mini
  reminder: sys

- name: gpt-4-0125-preview
  edit_format: udiff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  lazy: true
  reminder: sys
  examples_as_sys_msg: true

- name: gpt-4-0314
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  reminder: sys
  examples_as_sys_msg: true

- name: gpt-4-0613
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  reminder: sys

- name: gpt-4-1106-preview
  edit_format: udiff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  lazy: true
  reminder: sys

- name: gpt-4-32k-0613
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  reminder: sys

- name: gpt-4-turbo
  edit_format: udiff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  lazy: true
  reminder: sys

- name: gpt-4-turbo-2024-04-09
  edit_format: udiff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  lazy: true
  reminder: sys

- name: gpt-4-vision-preview
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  reminder: sys

- name: gpt-4.5-preview
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  lazy: true
  reminder: sys
  examples_as_sys_msg: true
  editor_model_name: gpt-4o
  editor_edit_format: editor-diff

- name: gpt-4o
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  lazy: true
  reminder: sys
  examples_as_sys_msg: true
  editor_edit_format: editor-diff

- name: gpt-4o-2024-08-06
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  lazy: true
  reminder: sys
  examples_as_sys_msg: true

- name: gpt-4o-2024-11-20
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  lazy: true
  reminder: sys
  examples_as_sys_msg: true

- name: gpt-4o-mini
  weak_model_name: gpt-4o-mini
  lazy: true
  reminder: sys

- name: groq/llama3-70b-8192
  edit_format: diff
  weak_model_name: groq/llama3-8b-8192
  examples_as_sys_msg: true

- name: groq/qwen-qwq-32b
  edit_format: diff
  weak_model_name: groq/qwen-2.5-coder-32b
  use_repo_map: true
  extra_params:
    max_tokens: 128000
    top_p: 0.95
  use_temperature: 0.6
  editor_model_name: groq/qwen-2.5-coder-32b
  editor_edit_format: editor-diff
  reasoning_tag: think

- name: o1
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  use_temperature: false
  streaming: false
  editor_model_name: gpt-4o
  editor_edit_format: editor-diff
  system_prompt_prefix: 'Formatting re-enabled. '
  accepts_settings:
  - reasoning_effort

- name: o1-mini
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  use_system_prompt: false
  use_temperature: false
  editor_model_name: gpt-4o
  editor_edit_format: editor-diff

- name: o1-preview
  edit_format: architect
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  use_system_prompt: false
  use_temperature: false
  editor_model_name: gpt-4o
  editor_edit_format: editor-diff

- name: o3-mini
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  use_temperature: false
  editor_model_name: gpt-4o
  editor_edit_format: editor-diff
  system_prompt_prefix: 'Formatting re-enabled. '
  accepts_settings:
  - reasoning_effort

- name: openai/gpt-4.5-preview
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  lazy: true
  reminder: sys
  examples_as_sys_msg: true
  editor_model_name: openai/gpt-4o
  editor_edit_format: editor-diff

- name: openai/gpt-4o
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  lazy: true
  reminder: sys
  examples_as_sys_msg: true
  editor_edit_format: editor-diff

- name: openai/gpt-4o-2024-08-06
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  lazy: true
  reminder: sys
  examples_as_sys_msg: true

- name: openai/gpt-4o-2024-11-20
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  lazy: true
  reminder: sys
  examples_as_sys_msg: true

- name: openai/gpt-4o-mini
  weak_model_name: openai/gpt-4o-mini
  lazy: true
  reminder: sys

- name: openai/o1
  edit_format: diff
  weak_model_name: openai/gpt-4o-mini
  use_repo_map: true
  use_temperature: false
  streaming: false
  editor_model_name: openai/gpt-4o
  editor_edit_format: editor-diff
  system_prompt_prefix: 'Formatting re-enabled. '
  accepts_settings:
  - reasoning_effort

- name: openai/o1-mini
  weak_model_name: openai/gpt-4o-mini
  use_repo_map: true
  use_system_prompt: false
  use_temperature: false
  editor_model_name: openai/gpt-4o
  editor_edit_format: editor-diff

- name: openai/o1-preview
  edit_format: diff
  weak_model_name: openai/gpt-4o-mini
  use_repo_map: true
  use_system_prompt: false
  use_temperature: false
  editor_model_name: openai/gpt-4o
  editor_edit_format: editor-diff

- name: openai/o3-mini
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  use_temperature: false
  editor_model_name: gpt-4o
  editor_edit_format: editor-diff
  system_prompt_prefix: 'Formatting re-enabled. '
  accepts_settings:
  - reasoning_effort

- name: openrouter/anthropic/claude-3-opus
  edit_format: diff
  weak_model_name: openrouter/anthropic/claude-3-5-haiku
  use_repo_map: true

- name: openrouter/anthropic/claude-3.5-sonnet
  edit_format: diff
  weak_model_name: openrouter/anthropic/claude-3-5-haiku
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 8192
  cache_control: true
  editor_model_name: openrouter/anthropic/claude-3.5-sonnet
  editor_edit_format: editor-diff

- name: openrouter/anthropic/claude-3.5-sonnet:beta
  edit_format: diff
  weak_model_name: openrouter/anthropic/claude-3-5-haiku:beta
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 8192
  cache_control: true
  editor_model_name: openrouter/anthropic/claude-3.5-sonnet:beta
  editor_edit_format: editor-diff

- name: openrouter/anthropic/claude-3.7-sonnet
  edit_format: diff
  weak_model_name: openrouter/anthropic/claude-3-5-haiku
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25,output-128k-2025-02-19
    max_tokens: 64000
  cache_control: true
  editor_model_name: openrouter/anthropic/claude-3.7-sonnet
  editor_edit_format: editor-diff
  accepts_settings:
  - thinking_tokens

- name: openrouter/anthropic/claude-3.7-sonnet:beta
  edit_format: diff
  weak_model_name: openrouter/anthropic/claude-3-5-haiku
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25,output-128k-2025-02-19
    max_tokens: 64000
  cache_control: true
  editor_model_name: openrouter/anthropic/claude-3.7-sonnet
  editor_edit_format: editor-diff
  accepts_settings:
  - thinking_tokens

- name: openrouter/cohere/command-a-03-2025
  examples_as_sys_msg: true

- name: openrouter/deepseek/deepseek-chat
  edit_format: diff
  use_repo_map: true
  reminder: sys
  examples_as_sys_msg: true

- name: openrouter/deepseek/deepseek-chat:free
  edit_format: diff
  weak_model_name: openrouter/deepseek/deepseek-chat:free
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 8192
  caches_by_default: true
  use_temperature: false
  editor_model_name: openrouter/deepseek/deepseek-chat:free
  editor_edit_format: editor-diff

- name: openrouter/deepseek/deepseek-coder
  edit_format: diff
  use_repo_map: true
  reminder: sys
  examples_as_sys_msg: true

- name: openrouter/deepseek/deepseek-r1
  edit_format: diff
  weak_model_name: openrouter/deepseek/deepseek-chat
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 8192
    include_reasoning: true
  caches_by_default: true
  editor_model_name: openrouter/deepseek/deepseek-chat
  editor_edit_format: editor-diff

- name: openrouter/deepseek/deepseek-r1-distill-llama-70b
  edit_format: diff
  weak_model_name: openrouter/deepseek/deepseek-chat
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 8192
  caches_by_default: true
  use_temperature: false
  editor_model_name: openrouter/deepseek/deepseek-chat
  editor_edit_format: editor-diff

- name: openrouter/deepseek/deepseek-r1:free
  edit_format: diff
  weak_model_name: openrouter/deepseek/deepseek-r1:free
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 8192
  caches_by_default: true
  use_temperature: false
  editor_model_name: openrouter/deepseek/deepseek-r1:free
  editor_edit_format: editor-diff

- name: openrouter/google/gemma-3-27b-it
  use_system_prompt: false

- name: openrouter/google/gemma-3-27b-it:free
  use_system_prompt: false

- name: openrouter/meta-llama/llama-3-70b-instruct
  edit_format: diff
  weak_model_name: openrouter/meta-llama/llama-3-70b-instruct
  examples_as_sys_msg: true

- name: openrouter/openai/gpt-4o
  edit_format: diff
  weak_model_name: openrouter/openai/gpt-4o-mini
  use_repo_map: true
  lazy: true
  reminder: sys
  examples_as_sys_msg: true
  editor_edit_format: editor-diff

- name: openrouter/openai/o1
  edit_format: diff
  weak_model_name: openrouter/openai/gpt-4o-mini
  use_repo_map: true
  use_temperature: false
  streaming: false
  editor_model_name: openrouter/openai/gpt-4o
  editor_edit_format: editor-diff
  system_prompt_prefix: 'Formatting re-enabled. '
  accepts_settings:
  - reasoning_effort

- name: openrouter/openai/o1-mini
  weak_model_name: openrouter/openai/gpt-4o-mini
  use_repo_map: true
  use_system_prompt: false
  use_temperature: false
  streaming: false
  editor_model_name: openrouter/openai/gpt-4o
  editor_edit_format: editor-diff

- name: openrouter/openai/o1-preview
  edit_format: diff
  weak_model_name: openrouter/openai/gpt-4o-mini
  use_repo_map: true
  use_system_prompt: false
  use_temperature: false
  streaming: false
  editor_model_name: openrouter/openai/gpt-4o
  editor_edit_format: editor-diff

- name: openrouter/openai/o3-mini
  edit_format: diff
  weak_model_name: openrouter/openai/gpt-4o-mini
  use_repo_map: true
  use_temperature: false
  editor_model_name: openrouter/openai/gpt-4o
  editor_edit_format: editor-diff
  system_prompt_prefix: 'Formatting re-enabled. '
  accepts_settings:
  - reasoning_effort

- name: openrouter/openai/o3-mini-high
  edit_format: diff
  weak_model_name: openrouter/openai/gpt-4o-mini
  use_repo_map: true
  use_temperature: false
  editor_model_name: openrouter/openai/gpt-4o
  editor_edit_format: editor-diff
  system_prompt_prefix: 'Formatting re-enabled. '
  accepts_settings:
  - reasoning_effort

- name: openrouter/qwen/qwen-2.5-coder-32b-instruct
  edit_format: diff
  weak_model_name: openrouter/qwen/qwen-2.5-coder-32b-instruct
  use_repo_map: true
  editor_model_name: openrouter/qwen/qwen-2.5-coder-32b-instruct
  editor_edit_format: editor-diff

- name: vertex_ai-anthropic_models/vertex_ai/claude-3-7-sonnet@20250219
  edit_format: diff
  weak_model_name: vertex_ai/claude-3-5-haiku@20241022
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 64000
  editor_model_name: vertex_ai-anthropic_models/vertex_ai/claude-3-7-sonnet@20250219
  editor_edit_format: editor-diff
  accepts_settings:
  - thinking_tokens

- name: vertex_ai/claude-3-5-haiku@20241022
  edit_format: diff
  weak_model_name: vertex_ai/claude-3-5-haiku@20241022
  use_repo_map: true
  extra_params:
    max_tokens: 4096

- name: vertex_ai/claude-3-5-sonnet-v2@20241022
  edit_format: diff
  weak_model_name: vertex_ai/claude-3-5-haiku@20241022
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 8192
  editor_model_name: vertex_ai/claude-3-5-sonnet-v2@20241022
  editor_edit_format: editor-diff

- name: vertex_ai/claude-3-5-sonnet@20240620
  edit_format: diff
  weak_model_name: vertex_ai/claude-3-5-haiku@20241022
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 8192
  editor_model_name: vertex_ai/claude-3-5-sonnet@20240620
  editor_edit_format: editor-diff

- name: vertex_ai/claude-3-7-sonnet@20250219
  edit_format: diff
  weak_model_name: vertex_ai/claude-3-5-haiku@20241022
  use_repo_map: true
  examples_as_sys_msg: true
  extra_params:
    max_tokens: 64000
  editor_model_name: vertex_ai/claude-3-7-sonnet@20250219
  editor_edit_format: editor-diff
  accepts_settings:
  - thinking_tokens

- name: vertex_ai/claude-3-opus@20240229
  edit_format: diff
  weak_model_name: vertex_ai/claude-3-5-haiku@20241022
  use_repo_map: true

- name: vertex_ai/claude-3-sonnet@20240229
  weak_model_name: vertex_ai/claude-3-5-haiku@20241022

- name: vertex_ai/gemini-pro-experimental
  edit_format: diff-fenced
  use_repo_map: true
```
<!--[[[end]]]-->
`````

## File: config/aider_conf.md
`````markdown
---
parent: Configuration
nav_order: 15
description: How to configure aider with a yaml config file.
---

# YAML config file

Most of aider's options can be set in an `.aider.conf.yml` file.
Aider will look for a this file in these locations:

- Your home directory.
- The root of your git repo.
- The current directory.

If the files above exist, they will be loaded in that order. Files loaded last will take priority.

You can also specify the `--config <filename>` parameter, which will only load the one config file.

{% include keys.md %}

## A note on lists

Lists of values can be specified either as a bulleted list:

```
read:
  - CONVENTIONS.md
  - anotherfile.txt
  - thirdfile.py
```

Or lists can be specified using commas and square brackets:

```
read: [CONVENTIONS.md, anotherfile.txt, thirdfile.py]
```

## Sample YAML config file

Below is a sample of the YAML config file, which you
can also
[download from GitHub](https://github.com/Aider-AI/aider/blob/main/aider/website/assets/sample.aider.conf.yml).

<!--[[[cog
from aider.args import get_sample_yaml
from pathlib import Path
text=get_sample_yaml()
Path("aider/website/assets/sample.aider.conf.yml").write_text(text)
cog.outl("```")
cog.out(text)
cog.outl("```")
]]]-->
```
##########################################################
# Sample .aider.conf.yml
# This file lists *all* the valid configuration entries.
# Place in your home dir, or at the root of your git repo.
##########################################################

# Note: You can only put OpenAI and Anthropic API keys in the yaml
# config file. Keys for all APIs can be stored in a .env file
# https://aider.chat/docs/config/dotenv.html

##########
# options:

## show this help message and exit
#help: xxx

#############
# Main model:

## Specify the model to use for the main chat
#model: xxx

########################
# API Keys and settings:

## Specify the OpenAI API key
#openai-api-key: xxx

## Specify the Anthropic API key
#anthropic-api-key: xxx

## Specify the api base url
#openai-api-base: xxx

## (deprecated, use --set-env OPENAI_API_TYPE=<value>)
#openai-api-type: xxx

## (deprecated, use --set-env OPENAI_API_VERSION=<value>)
#openai-api-version: xxx

## (deprecated, use --set-env OPENAI_API_DEPLOYMENT_ID=<value>)
#openai-api-deployment-id: xxx

## (deprecated, use --set-env OPENAI_ORGANIZATION=<value>)
#openai-organization-id: xxx

## Set an environment variable (to control API settings, can be used multiple times)
#set-env: xxx
## Specify multiple values like this:
#set-env:
#  - xxx
#  - yyy
#  - zzz

## Set an API key for a provider (eg: --api-key provider=<key> sets PROVIDER_API_KEY=<key>)
#api-key: xxx
## Specify multiple values like this:
#api-key:
#  - xxx
#  - yyy
#  - zzz

#################
# Model settings:

## List known models which match the (partial) MODEL name
#list-models: xxx

## Specify a file with aider model settings for unknown models
#model-settings-file: .aider.model.settings.yml

## Specify a file with context window and costs for unknown models
#model-metadata-file: .aider.model.metadata.json

## Add a model alias (can be used multiple times)
#alias: xxx
## Specify multiple values like this:
#alias:
#  - xxx
#  - yyy
#  - zzz

## Set the reasoning_effort API parameter (default: not set)
#reasoning-effort: xxx

## Set the thinking token budget for models that support it (default: not set)
#thinking-tokens: xxx

## Verify the SSL cert when connecting to models (default: True)
#verify-ssl: true

## Timeout in seconds for API calls (default: None)
#timeout: xxx

## Specify what edit format the LLM should use (default depends on model)
#edit-format: xxx

## Use architect edit format for the main chat
#architect: false

## Enable/disable automatic acceptance of architect changes (default: True)
#auto-accept-architect: true

## Specify the model to use for commit messages and chat history summarization (default depends on --model)
#weak-model: xxx

## Specify the model to use for editor tasks (default depends on --model)
#editor-model: xxx

## Specify the edit format for the editor model (default: depends on editor model)
#editor-edit-format: xxx

## Only work with models that have meta-data available (default: True)
#show-model-warnings: true

## Check if model accepts settings like reasoning_effort/thinking_tokens (default: True)
#check-model-accepts-settings: true

## Soft limit on tokens for chat history, after which summarization begins. If unspecified, defaults to the model's max_chat_history_tokens.
#max-chat-history-tokens: xxx

#################
# Cache settings:

## Enable caching of prompts (default: False)
#cache-prompts: false

## Number of times to ping at 5min intervals to keep prompt cache warm (default: 0)
#cache-keepalive-pings: false

###################
# Repomap settings:

## Suggested number of tokens to use for repo map, use 0 to disable
#map-tokens: xxx

## Control how often the repo map is refreshed. Options: auto, always, files, manual (default: auto)
#map-refresh: auto

## Multiplier for map tokens when no files are specified (default: 2)
#map-multiplier-no-files: true

################
# History Files:

## Specify the chat input history file (default: .aider.input.history)
#input-history-file: .aider.input.history

## Specify the chat history file (default: .aider.chat.history.md)
#chat-history-file: .aider.chat.history.md

## Restore the previous chat history messages (default: False)
#restore-chat-history: false

## Log the conversation with the LLM to this file (for example, .aider.llm.history)
#llm-history-file: xxx

##################
# Output settings:

## Use colors suitable for a dark terminal background (default: False)
#dark-mode: false

## Use colors suitable for a light terminal background (default: False)
#light-mode: false

## Enable/disable pretty, colorized output (default: True)
#pretty: true

## Enable/disable streaming responses (default: True)
#stream: true

## Set the color for user input (default: #00cc00)
#user-input-color: #00cc00

## Set the color for tool output (default: None)
#tool-output-color: "xxx"

## Set the color for tool error messages (default: #FF2222)
#tool-error-color: #FF2222

## Set the color for tool warning messages (default: #FFA500)
#tool-warning-color: #FFA500

## Set the color for assistant output (default: #0088ff)
#assistant-output-color: #0088ff

## Set the color for the completion menu (default: terminal's default text color)
#completion-menu-color: "xxx"

## Set the background color for the completion menu (default: terminal's default background color)
#completion-menu-bg-color: "xxx"

## Set the color for the current item in the completion menu (default: terminal's default background color)
#completion-menu-current-color: "xxx"

## Set the background color for the current item in the completion menu (default: terminal's default text color)
#completion-menu-current-bg-color: "xxx"

## Set the markdown code theme (default: default, other options include monokai, solarized-dark, solarized-light, or a Pygments builtin style, see https://pygments.org/styles for available themes)
#code-theme: default

## Show diffs when committing changes (default: False)
#show-diffs: false

###############
# Git settings:

## Enable/disable looking for a git repo (default: True)
#git: true

## Enable/disable adding .aider* to .gitignore (default: True)
#gitignore: true

## Specify the aider ignore file (default: .aiderignore in git root)
#aiderignore: .aiderignore

## Only consider files in the current subtree of the git repository
#subtree-only: false

## Enable/disable auto commit of LLM changes (default: True)
#auto-commits: true

## Enable/disable commits when repo is found dirty (default: True)
#dirty-commits: true

## Attribute aider code changes in the git author name (default: True)
#attribute-author: true

## Attribute aider commits in the git committer name (default: True)
#attribute-committer: true

## Prefix commit messages with 'aider: ' if aider authored the changes (default: False)
#attribute-commit-message-author: false

## Prefix all commit messages with 'aider: ' (default: False)
#attribute-commit-message-committer: false

## Commit all pending changes with a suitable commit message, then exit
#commit: false

## Specify a custom prompt for generating commit messages
#commit-prompt: xxx

## Perform a dry run without modifying files (default: False)
#dry-run: false

## Skip the sanity check for the git repository (default: False)
#skip-sanity-check-repo: false

## Enable/disable watching files for ai coding comments (default: False)
#watch-files: false

########################
# Fixing and committing:

## Lint and fix provided files, or dirty files if none provided
#lint: false

## Specify lint commands to run for different languages, eg: "python: flake8 --select=..." (can be used multiple times)
#lint-cmd: xxx
## Specify multiple values like this:
#lint-cmd:
#  - xxx
#  - yyy
#  - zzz

## Enable/disable automatic linting after changes (default: True)
#auto-lint: true

## Specify command to run tests
#test-cmd: xxx

## Enable/disable automatic testing after changes (default: False)
#auto-test: false

## Run tests, fix problems found and then exit
#test: false

############
# Analytics:

## Enable/disable analytics for current session (default: random)
#analytics: xxx

## Specify a file to log analytics events
#analytics-log: xxx

## Permanently disable analytics
#analytics-disable: false

############
# Upgrading:

## Check for updates and return status in the exit code
#just-check-update: false

## Check for new aider versions on launch
#check-update: true

## Show release notes on first run of new version (default: None, ask user)
#show-release-notes: xxx

## Install the latest version from the main branch
#install-main-branch: false

## Upgrade aider to the latest version from PyPI
#upgrade: false

## Show the version number and exit
#version: xxx

########
# Modes:

## Specify a single message to send the LLM, process reply then exit (disables chat mode)
#message: xxx

## Specify a file containing the message to send the LLM, process reply, then exit (disables chat mode)
#message-file: xxx

## Run aider in your browser (default: False)
#gui: false

## Enable automatic copy/paste of chat between aider and web UI (default: False)
#copy-paste: false

## Apply the changes from the given file instead of running the chat (debug)
#apply: xxx

## Apply clipboard contents as edits using the main model's editor format
#apply-clipboard-edits: false

## Do all startup activities then exit before accepting user input (debug)
#exit: false

## Print the repo map and exit (debug)
#show-repo-map: false

## Print the system prompts and exit (debug)
#show-prompts: false

#################
# Voice settings:

## Audio format for voice recording (default: wav). webm and mp3 require ffmpeg
#voice-format: wav

## Specify the language for voice using ISO 639-1 code (default: auto)
#voice-language: en

## Specify the input device name for voice recording
#voice-input-device: xxx

#################
# Other settings:

## specify a file to edit (can be used multiple times)
#file: xxx
## Specify multiple values like this:
#file:
#  - xxx
#  - yyy
#  - zzz

## specify a read-only file (can be used multiple times)
#read: xxx
## Specify multiple values like this:
#read:
#  - xxx
#  - yyy
#  - zzz

## Use VI editing mode in the terminal (default: False)
#vim: false

## Specify the language to use in the chat (default: None, uses system settings)
#chat-language: xxx

## Always say yes to every confirmation
#yes-always: false

## Enable verbose output
#verbose: false

## Load and execute /commands from a file on launch
#load: xxx

## Specify the encoding for input and output (default: utf-8)
#encoding: utf-8

## Line endings to use when writing files (default: platform)
#line-endings: platform

## Specify the config file (default: search for .aider.conf.yml in git root, cwd or home directory)
#config: xxx

## Specify the .env file to load (default: .env in git root)
#env-file: .env

## Enable/disable suggesting shell commands (default: True)
#suggest-shell-commands: true

## Enable/disable fancy input with history and completion (default: True)
#fancy-input: true

## Enable/disable multi-line input mode with Meta-Enter to submit (default: False)
#multiline: false

## Enable/disable terminal bell notifications when LLM responses are ready (default: False)
#notifications: false

## Specify a command to run for notifications instead of the terminal bell. If not specified, a default command for your OS may be used.
#notifications-command: xxx

## Enable/disable detection and offering to add URLs to chat (default: True)
#detect-urls: true

## Specify which editor to use for the /editor command
#editor: xxx

############################
# Deprecated model settings:

## Use claude-3-opus-20240229 model for the main chat (deprecated, use --model)
#opus: false

## Use anthropic/claude-3-7-sonnet-20250219 model for the main chat (deprecated, use --model)
#sonnet: false

## Use claude-3-5-haiku-20241022 model for the main chat (deprecated, use --model)
#haiku: false

## Use gpt-4-0613 model for the main chat (deprecated, use --model)
#4: false

## Use gpt-4o model for the main chat (deprecated, use --model)
#4o: false

## Use gpt-4o-mini model for the main chat (deprecated, use --model)
#mini: false

## Use gpt-4-1106-preview model for the main chat (deprecated, use --model)
#4-turbo: false

## Use gpt-3.5-turbo model for the main chat (deprecated, use --model)
#35turbo: false

## Use deepseek/deepseek-chat model for the main chat (deprecated, use --model)
#deepseek: false

## Use o1-mini model for the main chat (deprecated, use --model)
#o1-mini: false

## Use o1-preview model for the main chat (deprecated, use --model)
#o1-preview: false
```
<!--[[[end]]]-->
`````

## File: config/api-keys.md
`````markdown
---
parent: Configuration
nav_order: 5
description: Setting API keys for API providers.
---

# API Keys

Aider lets you specify API keys in a few ways:

- On the command line
- As environment variables
- In a `.env` file
- In your `.aider.conf.yml` config file

---

## OpenAI and Anthropic

Aider has special support for providing
OpenAI and Anthropic API keys
via dedicated switches and configuration options.
Settings keys for other providers works a bit differently, see below.

#### Command line

You can set OpenAI and Anthropic API keys via
[command line switches](/docs/config/options.html#api-keys-and-settings)
`--openai-api-key` and `--anthropic-api-key`.


#### Environment variables or .env file

You can also store them in environment variables or a 
[.env file](/docs/config/dotenv.html), which also works
for every API provider:

```
OPENAI_API_KEY=<key>
ANTHROPIC_API_KEY=<key>
```

#### Yaml config file
You can also set those API keys via special entries in the
[yaml config file](/docs/config/aider_conf.html), like this:

```yaml
openai-api-key: <key>
anthropic-api-key: <key>
```


---

## Other API providers

All other LLM providers can use one of these other methods to set their API keys.

#### Command line
{: .no_toc }

Use `--api-key provider=<key>` which has the effect of setting the environment variable `PROVIDER_API_KEY=<key>`. So `--api-key gemini=xxx` would set `GEMINI_API_KEY=xxx`.

#### Environment variables or .env file
{: .no_toc }

You can set API keys in environment variables.
The [.env file](/docs/config/dotenv.html)
is a great place to store your API keys and other provider API environment variables:

```bash
GEMINI_API_KEY=foo
OPENROUTER_API_KEY=bar
DEEPSEEK_API_KEY=baz
```

#### Yaml config file


You can also set API keys in the 
[`.aider.conf.yml` file](/docs/config/aider_conf.html)
via the `api-key` entry:

```
api-key:
- gemini=foo      # Sets env var GEMINI_API_KEY=foo
- openrouter=bar  # Sets env var OPENROUTER_API_KEY=bar
- deepseek=baz    # Sets env var DEEPSEEK_API_KEY=baz
```
`````

## File: config/dotenv.md
`````markdown
---
parent: Configuration
nav_order: 20
description: Using a .env file to store LLM API keys for aider.
---

# Config with .env

You can use a `.env` file to store API keys and other settings for the
models you use with aider.
You can also set many general aider options
in the `.env` file.

Aider will look for a `.env` file in these locations:

- Your home directory.
- The root of your git repo.
- The current directory.
- As specified with the `--env-file <filename>` parameter.

If the files above exist, they will be loaded in that order. Files loaded last will take priority.

{% include keys.md %}

## Sample .env file

Below is a sample `.env` file, which you
can also
[download from GitHub](https://github.com/Aider-AI/aider/blob/main/aider/website/assets/sample.env).

<!--[[[cog
from aider.args import get_sample_dotenv
from pathlib import Path
text=get_sample_dotenv()
Path("aider/website/assets/sample.env").write_text(text)
cog.outl("```")
cog.out(text)
cog.outl("```")
]]]-->
```
##########################################################
# Sample aider .env file.
# Place at the root of your git repo.
# Or use `aider --env <fname>` to specify.
##########################################################

#################
# LLM parameters:
#
# Include xxx_API_KEY parameters and other params needed for your LLMs.
# See https://aider.chat/docs/llms.html for details.

## OpenAI
#OPENAI_API_KEY=

## Anthropic
#ANTHROPIC_API_KEY=

##...

#############
# Main model:

## Specify the model to use for the main chat
#AIDER_MODEL=

########################
# API Keys and settings:

## Specify the OpenAI API key
#AIDER_OPENAI_API_KEY=

## Specify the Anthropic API key
#AIDER_ANTHROPIC_API_KEY=

## Specify the api base url
#AIDER_OPENAI_API_BASE=

## (deprecated, use --set-env OPENAI_API_TYPE=<value>)
#AIDER_OPENAI_API_TYPE=

## (deprecated, use --set-env OPENAI_API_VERSION=<value>)
#AIDER_OPENAI_API_VERSION=

## (deprecated, use --set-env OPENAI_API_DEPLOYMENT_ID=<value>)
#AIDER_OPENAI_API_DEPLOYMENT_ID=

## (deprecated, use --set-env OPENAI_ORGANIZATION=<value>)
#AIDER_OPENAI_ORGANIZATION_ID=

## Set an environment variable (to control API settings, can be used multiple times)
#AIDER_SET_ENV=

## Set an API key for a provider (eg: --api-key provider=<key> sets PROVIDER_API_KEY=<key>)
#AIDER_API_KEY=

#################
# Model settings:

## List known models which match the (partial) MODEL name
#AIDER_LIST_MODELS=

## Specify a file with aider model settings for unknown models
#AIDER_MODEL_SETTINGS_FILE=.aider.model.settings.yml

## Specify a file with context window and costs for unknown models
#AIDER_MODEL_METADATA_FILE=.aider.model.metadata.json

## Add a model alias (can be used multiple times)
#AIDER_ALIAS=

## Set the reasoning_effort API parameter (default: not set)
#AIDER_REASONING_EFFORT=

## Set the thinking token budget for models that support it (default: not set)
#AIDER_THINKING_TOKENS=

## Verify the SSL cert when connecting to models (default: True)
#AIDER_VERIFY_SSL=true

## Timeout in seconds for API calls (default: None)
#AIDER_TIMEOUT=

## Specify what edit format the LLM should use (default depends on model)
#AIDER_EDIT_FORMAT=

## Use architect edit format for the main chat
#AIDER_ARCHITECT=

## Enable/disable automatic acceptance of architect changes (default: True)
#AIDER_AUTO_ACCEPT_ARCHITECT=true

## Specify the model to use for commit messages and chat history summarization (default depends on --model)
#AIDER_WEAK_MODEL=

## Specify the model to use for editor tasks (default depends on --model)
#AIDER_EDITOR_MODEL=

## Specify the edit format for the editor model (default: depends on editor model)
#AIDER_EDITOR_EDIT_FORMAT=

## Only work with models that have meta-data available (default: True)
#AIDER_SHOW_MODEL_WARNINGS=true

## Check if model accepts settings like reasoning_effort/thinking_tokens (default: True)
#AIDER_CHECK_MODEL_ACCEPTS_SETTINGS=true

## Soft limit on tokens for chat history, after which summarization begins. If unspecified, defaults to the model's max_chat_history_tokens.
#AIDER_MAX_CHAT_HISTORY_TOKENS=

#################
# Cache settings:

## Enable caching of prompts (default: False)
#AIDER_CACHE_PROMPTS=false

## Number of times to ping at 5min intervals to keep prompt cache warm (default: 0)
#AIDER_CACHE_KEEPALIVE_PINGS=false

###################
# Repomap settings:

## Suggested number of tokens to use for repo map, use 0 to disable
#AIDER_MAP_TOKENS=

## Control how often the repo map is refreshed. Options: auto, always, files, manual (default: auto)
#AIDER_MAP_REFRESH=auto

## Multiplier for map tokens when no files are specified (default: 2)
#AIDER_MAP_MULTIPLIER_NO_FILES=true

################
# History Files:

## Specify the chat input history file (default: .aider.input.history)
#AIDER_INPUT_HISTORY_FILE=.aider.input.history

## Specify the chat history file (default: .aider.chat.history.md)
#AIDER_CHAT_HISTORY_FILE=.aider.chat.history.md

## Restore the previous chat history messages (default: False)
#AIDER_RESTORE_CHAT_HISTORY=false

## Log the conversation with the LLM to this file (for example, .aider.llm.history)
#AIDER_LLM_HISTORY_FILE=

##################
# Output settings:

## Use colors suitable for a dark terminal background (default: False)
#AIDER_DARK_MODE=false

## Use colors suitable for a light terminal background (default: False)
#AIDER_LIGHT_MODE=false

## Enable/disable pretty, colorized output (default: True)
#AIDER_PRETTY=true

## Enable/disable streaming responses (default: True)
#AIDER_STREAM=true

## Set the color for user input (default: #00cc00)
#AIDER_USER_INPUT_COLOR=#00cc00

## Set the color for tool output (default: None)
#AIDER_TOOL_OUTPUT_COLOR=

## Set the color for tool error messages (default: #FF2222)
#AIDER_TOOL_ERROR_COLOR=#FF2222

## Set the color for tool warning messages (default: #FFA500)
#AIDER_TOOL_WARNING_COLOR=#FFA500

## Set the color for assistant output (default: #0088ff)
#AIDER_ASSISTANT_OUTPUT_COLOR=#0088ff

## Set the color for the completion menu (default: terminal's default text color)
#AIDER_COMPLETION_MENU_COLOR=

## Set the background color for the completion menu (default: terminal's default background color)
#AIDER_COMPLETION_MENU_BG_COLOR=

## Set the color for the current item in the completion menu (default: terminal's default background color)
#AIDER_COMPLETION_MENU_CURRENT_COLOR=

## Set the background color for the current item in the completion menu (default: terminal's default text color)
#AIDER_COMPLETION_MENU_CURRENT_BG_COLOR=

## Set the markdown code theme (default: default, other options include monokai, solarized-dark, solarized-light, or a Pygments builtin style, see https://pygments.org/styles for available themes)
#AIDER_CODE_THEME=default

## Show diffs when committing changes (default: False)
#AIDER_SHOW_DIFFS=false

###############
# Git settings:

## Enable/disable looking for a git repo (default: True)
#AIDER_GIT=true

## Enable/disable adding .aider* to .gitignore (default: True)
#AIDER_GITIGNORE=true

## Specify the aider ignore file (default: .aiderignore in git root)
#AIDER_AIDERIGNORE=.aiderignore

## Only consider files in the current subtree of the git repository
#AIDER_SUBTREE_ONLY=false

## Enable/disable auto commit of LLM changes (default: True)
#AIDER_AUTO_COMMITS=true

## Enable/disable commits when repo is found dirty (default: True)
#AIDER_DIRTY_COMMITS=true

## Attribute aider code changes in the git author name (default: True)
#AIDER_ATTRIBUTE_AUTHOR=true

## Attribute aider commits in the git committer name (default: True)
#AIDER_ATTRIBUTE_COMMITTER=true

## Prefix commit messages with 'aider: ' if aider authored the changes (default: False)
#AIDER_ATTRIBUTE_COMMIT_MESSAGE_AUTHOR=false

## Prefix all commit messages with 'aider: ' (default: False)
#AIDER_ATTRIBUTE_COMMIT_MESSAGE_COMMITTER=false

## Commit all pending changes with a suitable commit message, then exit
#AIDER_COMMIT=false

## Specify a custom prompt for generating commit messages
#AIDER_COMMIT_PROMPT=

## Perform a dry run without modifying files (default: False)
#AIDER_DRY_RUN=false

## Skip the sanity check for the git repository (default: False)
#AIDER_SKIP_SANITY_CHECK_REPO=false

## Enable/disable watching files for ai coding comments (default: False)
#AIDER_WATCH_FILES=false

########################
# Fixing and committing:

## Lint and fix provided files, or dirty files if none provided
#AIDER_LINT=false

## Specify lint commands to run for different languages, eg: "python: flake8 --select=..." (can be used multiple times)
#AIDER_LINT_CMD=

## Enable/disable automatic linting after changes (default: True)
#AIDER_AUTO_LINT=true

## Specify command to run tests
#AIDER_TEST_CMD=

## Enable/disable automatic testing after changes (default: False)
#AIDER_AUTO_TEST=false

## Run tests, fix problems found and then exit
#AIDER_TEST=false

############
# Analytics:

## Enable/disable analytics for current session (default: random)
#AIDER_ANALYTICS=

## Specify a file to log analytics events
#AIDER_ANALYTICS_LOG=

## Permanently disable analytics
#AIDER_ANALYTICS_DISABLE=false

############
# Upgrading:

## Check for updates and return status in the exit code
#AIDER_JUST_CHECK_UPDATE=false

## Check for new aider versions on launch
#AIDER_CHECK_UPDATE=true

## Show release notes on first run of new version (default: None, ask user)
#AIDER_SHOW_RELEASE_NOTES=

## Install the latest version from the main branch
#AIDER_INSTALL_MAIN_BRANCH=false

## Upgrade aider to the latest version from PyPI
#AIDER_UPGRADE=false

########
# Modes:

## Specify a single message to send the LLM, process reply then exit (disables chat mode)
#AIDER_MESSAGE=

## Specify a file containing the message to send the LLM, process reply, then exit (disables chat mode)
#AIDER_MESSAGE_FILE=

## Run aider in your browser (default: False)
#AIDER_GUI=false

## Enable automatic copy/paste of chat between aider and web UI (default: False)
#AIDER_COPY_PASTE=false

## Apply the changes from the given file instead of running the chat (debug)
#AIDER_APPLY=

## Apply clipboard contents as edits using the main model's editor format
#AIDER_APPLY_CLIPBOARD_EDITS=false

## Do all startup activities then exit before accepting user input (debug)
#AIDER_EXIT=false

## Print the repo map and exit (debug)
#AIDER_SHOW_REPO_MAP=false

## Print the system prompts and exit (debug)
#AIDER_SHOW_PROMPTS=false

#################
# Voice settings:

## Audio format for voice recording (default: wav). webm and mp3 require ffmpeg
#AIDER_VOICE_FORMAT=wav

## Specify the language for voice using ISO 639-1 code (default: auto)
#AIDER_VOICE_LANGUAGE=en

## Specify the input device name for voice recording
#AIDER_VOICE_INPUT_DEVICE=

#################
# Other settings:

## specify a file to edit (can be used multiple times)
#AIDER_FILE=

## specify a read-only file (can be used multiple times)
#AIDER_READ=

## Use VI editing mode in the terminal (default: False)
#AIDER_VIM=false

## Specify the language to use in the chat (default: None, uses system settings)
#AIDER_CHAT_LANGUAGE=

## Always say yes to every confirmation
#AIDER_YES_ALWAYS=

## Enable verbose output
#AIDER_VERBOSE=false

## Load and execute /commands from a file on launch
#AIDER_LOAD=

## Specify the encoding for input and output (default: utf-8)
#AIDER_ENCODING=utf-8

## Line endings to use when writing files (default: platform)
#AIDER_LINE_ENDINGS=platform

## Specify the .env file to load (default: .env in git root)
#AIDER_ENV_FILE=.env

## Enable/disable suggesting shell commands (default: True)
#AIDER_SUGGEST_SHELL_COMMANDS=true

## Enable/disable fancy input with history and completion (default: True)
#AIDER_FANCY_INPUT=true

## Enable/disable multi-line input mode with Meta-Enter to submit (default: False)
#AIDER_MULTILINE=false

## Enable/disable terminal bell notifications when LLM responses are ready (default: False)
#AIDER_NOTIFICATIONS=false

## Specify a command to run for notifications instead of the terminal bell. If not specified, a default command for your OS may be used.
#AIDER_NOTIFICATIONS_COMMAND=

## Enable/disable detection and offering to add URLs to chat (default: True)
#AIDER_DETECT_URLS=true

## Specify which editor to use for the /editor command
#AIDER_EDITOR=

############################
# Deprecated model settings:

## Use claude-3-opus-20240229 model for the main chat (deprecated, use --model)
#AIDER_OPUS=false

## Use anthropic/claude-3-7-sonnet-20250219 model for the main chat (deprecated, use --model)
#AIDER_SONNET=false

## Use claude-3-5-haiku-20241022 model for the main chat (deprecated, use --model)
#AIDER_HAIKU=false

## Use gpt-4-0613 model for the main chat (deprecated, use --model)
#AIDER_4=false

## Use gpt-4o model for the main chat (deprecated, use --model)
#AIDER_4O=false

## Use gpt-4o-mini model for the main chat (deprecated, use --model)
#AIDER_MINI=false

## Use gpt-4-1106-preview model for the main chat (deprecated, use --model)
#AIDER_4_TURBO=false

## Use gpt-3.5-turbo model for the main chat (deprecated, use --model)
#AIDER_35TURBO=false

## Use deepseek/deepseek-chat model for the main chat (deprecated, use --model)
#AIDER_DEEPSEEK=false

## Use o1-mini model for the main chat (deprecated, use --model)
#AIDER_O1_MINI=false

## Use o1-preview model for the main chat (deprecated, use --model)
#AIDER_O1_PREVIEW=false
```
<!--[[[end]]]-->
`````

## File: config/editor.md
`````markdown
---
parent: Configuration
nav_order: 100
description: How to configure a custom editor for aider's /editor command
---

# Editor configuration

Aider allows you to configure your preferred text editor for use with the `/editor` command. The editor must be capable of running in "blocking mode", meaning the command line will wait until you close the editor before proceeding.

## Using `--editor`

You can specify the text editor with the `--editor` switch or using
`editor:` in aider's
[yaml config file](https://aider.chat/docs/config/aider_conf.html).

## Environment variables

Aider checks the following environment variables in order to determine which editor to use:

1. `AIDER_EDITOR`
2. `VISUAL`
3. `EDITOR`

## Default behavior

If no editor is configured, aider will use these platform-specific defaults:

- Windows: `notepad`
- macOS: `vim`
- Linux/Unix: `vi`

## Using a custom editor

You can set your preferred editor in your shell's configuration file (e.g., `.bashrc`, `.zshrc`):

```bash
export AIDER_EDITOR=vim
```

## Popular Editors by Platform

### macOS

1. **vim**
   ```bash
   export AIDER_EDITOR=vim
   ```

2. **Emacs**
   ```bash
   export AIDER_EDITOR=emacs
   ```

3. **VSCode**
   ```bash
   export AIDER_EDITOR="code --wait"
   ```

4. **Sublime Text**
   ```bash
   export AIDER_EDITOR="subl --wait"
   ```

5. **BBEdit**
   ```bash
   export AIDER_EDITOR="bbedit --wait"
   ```

### Linux

1. **vim**
   ```bash
   export AIDER_EDITOR=vim
   ```

2. **Emacs**
   ```bash
   export AIDER_EDITOR=emacs
   ```

3. **nano**
   ```bash
   export AIDER_EDITOR=nano
   ```

4. **VSCode**
   ```bash
   export AIDER_EDITOR="code --wait"
   ```

5. **Sublime Text**
   ```bash
   export AIDER_EDITOR="subl --wait"
   ```

### Windows

1. **Notepad**
   ```bat
   set AIDER_EDITOR=notepad
   ```

2. **VSCode**
   ```bat
   set AIDER_EDITOR="code --wait"
   ```

3. **Notepad++**
   ```bat
   set AIDER_EDITOR="notepad++ -multiInst -notabbar -nosession -noPlugin -waitForClose"
   ```

## Editor command arguments

Some editors require specific command-line arguments to operate in blocking mode. The `--wait` flag (or equivalent) is commonly used to make the editor block until the file is closed.

## Troubleshooting

If you encounter issues with your editor not blocking (returning to the prompt immediately), verify that:

1. Your editor supports blocking mode
2. You've included the necessary command-line arguments for blocking mode
3. The editor command is properly quoted if it contains spaces or special characters, e.g.:
   ```bash
   export AIDER_EDITOR="code --wait"
   ```
`````

## File: config/model-aliases.md
`````markdown
---
parent: Configuration
nav_order: 1000
description: Assign convenient short names to models.
---

# Model Aliases

Model aliases allow you to create shorthand names for models you frequently use. This is particularly useful for models with long names or when you want to standardize model usage across your team.

## Command Line Usage

You can define aliases when launching aider using the `--alias` option:

```bash
aider --alias "fast:gpt-4o-mini" --alias "smart:o3-mini"
```

Multiple aliases can be defined by using the `--alias` option multiple times. Each alias definition should be in the format `alias:model-name`.

## Configuration File

Of course,
you can also define aliases in your [`.aider.conf.yml` file](https://aider.chat/docs/config/aider_conf.html):

```yaml
alias:
  - "fast:gpt-4o-mini"
  - "smart:o3-mini"
  - "hacker:claude-3-sonnet-20240229"
```

## Using Aliases

Once defined, you can use the alias instead of the full model name from the command line:

```bash
aider --model fast  # Uses gpt-4o-mini
aider --model smart  # Uses o3-mini
```

Or with the `/model` command in-chat:

```
Aider v0.75.3
Main model: anthropic/claude-3-7-sonnet-20250219 with diff edit format, prompt cache, infinite output
Weak model: claude-3-5-sonnet-20241022
Git repo: .git with 406 files
Repo-map: using 4096 tokens, files refresh
─────────────────────────────────────────────────────────────────────────────────────────────────────
> /model fast

Aider v0.75.3
Main model: gpt-4o-mini with diff edit format
─────────────────────────────────────────────────────────────────────────────────────────────────────
diff> /model smart

Aider v0.75.3
Main model: o3-mini with diff edit format
─────────────────────────────────────────────────────────────────────────────────────────────────────
>
```

## Built-in Aliases

Aider includes some built-in aliases for convenience:

<!--[[[cog
import cog
from aider.models import MODEL_ALIASES

for alias, model in sorted(MODEL_ALIASES.items()):
    cog.outl(f"- `{alias}`: {model}")
]]]-->
- `3`: gpt-3.5-turbo
- `35-turbo`: gpt-3.5-turbo
- `35turbo`: gpt-3.5-turbo
- `4`: gpt-4-0613
- `4-turbo`: gpt-4-1106-preview
- `4o`: gpt-4o
- `deepseek`: deepseek/deepseek-chat
- `flash`: gemini/gemini-2.0-flash-exp
- `haiku`: claude-3-5-haiku-20241022
- `opus`: claude-3-opus-20240229
- `r1`: deepseek/deepseek-reasoner
- `sonnet`: anthropic/claude-3-7-sonnet-20250219
<!--[[[end]]]-->

## Priority

If the same alias is defined in multiple places, the priority is:

1. Command line aliases (highest priority)
2. Configuration file aliases
3. Built-in aliases (lowest priority)

This allows you to override built-in aliases with your own preferences.
`````

## File: config/options.md
`````markdown
---
parent: Configuration
nav_order: 10
description: Details about all of aider's settings.
---

# Options reference
{: .no_toc }

You can use `aider --help` to see all the available options,
or review them below.

- TOC
{:toc}

{% include keys.md %}

## Usage summary

<!--[[[cog
from aider.args import get_md_help
cog.out(get_md_help())
]]]-->
```
usage: aider [-h] [--model] [--openai-api-key] [--anthropic-api-key]
             [--openai-api-base] [--openai-api-type]
             [--openai-api-version] [--openai-api-deployment-id]
             [--openai-organization-id] [--set-env] [--api-key]
             [--list-models] [--model-settings-file]
             [--model-metadata-file] [--alias] [--reasoning-effort]
             [--thinking-tokens] [--verify-ssl | --no-verify-ssl]
             [--timeout] [--edit-format] [--architect]
             [--auto-accept-architect | --no-auto-accept-architect]
             [--weak-model] [--editor-model] [--editor-edit-format]
             [--show-model-warnings | --no-show-model-warnings]
             [--check-model-accepts-settings | --no-check-model-accepts-settings]
             [--max-chat-history-tokens]
             [--cache-prompts | --no-cache-prompts]
             [--cache-keepalive-pings] [--map-tokens]
             [--map-refresh] [--map-multiplier-no-files]
             [--input-history-file] [--chat-history-file]
             [--restore-chat-history | --no-restore-chat-history]
             [--llm-history-file] [--dark-mode] [--light-mode]
             [--pretty | --no-pretty] [--stream | --no-stream]
             [--user-input-color] [--tool-output-color]
             [--tool-error-color] [--tool-warning-color]
             [--assistant-output-color] [--completion-menu-color]
             [--completion-menu-bg-color]
             [--completion-menu-current-color]
             [--completion-menu-current-bg-color] [--code-theme]
             [--show-diffs] [--git | --no-git]
             [--gitignore | --no-gitignore] [--aiderignore]
             [--subtree-only] [--auto-commits | --no-auto-commits]
             [--dirty-commits | --no-dirty-commits]
             [--attribute-author | --no-attribute-author]
             [--attribute-committer | --no-attribute-committer]
             [--attribute-commit-message-author | --no-attribute-commit-message-author]
             [--attribute-commit-message-committer | --no-attribute-commit-message-committer]
             [--commit] [--commit-prompt] [--dry-run | --no-dry-run]
             [--skip-sanity-check-repo]
             [--watch-files | --no-watch-files] [--lint]
             [--lint-cmd] [--auto-lint | --no-auto-lint]
             [--test-cmd] [--auto-test | --no-auto-test] [--test]
             [--analytics | --no-analytics] [--analytics-log]
             [--analytics-disable] [--just-check-update]
             [--check-update | --no-check-update]
             [--show-release-notes | --no-show-release-notes]
             [--install-main-branch] [--upgrade] [--version]
             [--message] [--message-file]
             [--gui | --no-gui | --browser | --no-browser]
             [--copy-paste | --no-copy-paste] [--apply]
             [--apply-clipboard-edits] [--exit] [--show-repo-map]
             [--show-prompts] [--voice-format] [--voice-language]
             [--voice-input-device] [--file] [--read] [--vim]
             [--chat-language] [--yes-always] [-v] [--load]
             [--encoding] [--line-endings] [-c] [--env-file]
             [--suggest-shell-commands | --no-suggest-shell-commands]
             [--fancy-input | --no-fancy-input]
             [--multiline | --no-multiline]
             [--notifications | --no-notifications]
             [--notifications-command]
             [--detect-urls | --no-detect-urls] [--editor] [--opus]
             [--sonnet] [--haiku] [--4] [--4o] [--mini] [--4-turbo]
             [--35turbo] [--deepseek] [--o1-mini] [--o1-preview]

```

## options:

### `--help`
show this help message and exit  
Aliases:
  - `-h`
  - `--help`

## Main model:

### `--model MODEL`
Specify the model to use for the main chat  
Environment variable: `AIDER_MODEL`  

## API Keys and settings:

### `--openai-api-key VALUE`
Specify the OpenAI API key  
Environment variable: `AIDER_OPENAI_API_KEY`  

### `--anthropic-api-key VALUE`
Specify the Anthropic API key  
Environment variable: `AIDER_ANTHROPIC_API_KEY`  

### `--openai-api-base VALUE`
Specify the api base url  
Environment variable: `AIDER_OPENAI_API_BASE`  

### `--openai-api-type VALUE`
(deprecated, use --set-env OPENAI_API_TYPE=<value>)  
Environment variable: `AIDER_OPENAI_API_TYPE`  

### `--openai-api-version VALUE`
(deprecated, use --set-env OPENAI_API_VERSION=<value>)  
Environment variable: `AIDER_OPENAI_API_VERSION`  

### `--openai-api-deployment-id VALUE`
(deprecated, use --set-env OPENAI_API_DEPLOYMENT_ID=<value>)  
Environment variable: `AIDER_OPENAI_API_DEPLOYMENT_ID`  

### `--openai-organization-id VALUE`
(deprecated, use --set-env OPENAI_ORGANIZATION=<value>)  
Environment variable: `AIDER_OPENAI_ORGANIZATION_ID`  

### `--set-env ENV_VAR_NAME=value`
Set an environment variable (to control API settings, can be used multiple times)  
Default: []  
Environment variable: `AIDER_SET_ENV`  

### `--api-key PROVIDER=KEY`
Set an API key for a provider (eg: --api-key provider=<key> sets PROVIDER_API_KEY=<key>)  
Default: []  
Environment variable: `AIDER_API_KEY`  

## Model settings:

### `--list-models MODEL`
List known models which match the (partial) MODEL name  
Environment variable: `AIDER_LIST_MODELS`  
Aliases:
  - `--list-models MODEL`
  - `--models MODEL`

### `--model-settings-file MODEL_SETTINGS_FILE`
Specify a file with aider model settings for unknown models  
Default: .aider.model.settings.yml  
Environment variable: `AIDER_MODEL_SETTINGS_FILE`  

### `--model-metadata-file MODEL_METADATA_FILE`
Specify a file with context window and costs for unknown models  
Default: .aider.model.metadata.json  
Environment variable: `AIDER_MODEL_METADATA_FILE`  

### `--alias ALIAS:MODEL`
Add a model alias (can be used multiple times)  
Environment variable: `AIDER_ALIAS`  

### `--reasoning-effort VALUE`
Set the reasoning_effort API parameter (default: not set)  
Environment variable: `AIDER_REASONING_EFFORT`  

### `--thinking-tokens VALUE`
Set the thinking token budget for models that support it (default: not set)  
Environment variable: `AIDER_THINKING_TOKENS`  

### `--verify-ssl`
Verify the SSL cert when connecting to models (default: True)  
Default: True  
Environment variable: `AIDER_VERIFY_SSL`  
Aliases:
  - `--verify-ssl`
  - `--no-verify-ssl`

### `--timeout VALUE`
Timeout in seconds for API calls (default: None)  
Environment variable: `AIDER_TIMEOUT`  

### `--edit-format EDIT_FORMAT`
Specify what edit format the LLM should use (default depends on model)  
Environment variable: `AIDER_EDIT_FORMAT`  
Aliases:
  - `--edit-format EDIT_FORMAT`
  - `--chat-mode EDIT_FORMAT`

### `--architect`
Use architect edit format for the main chat  
Environment variable: `AIDER_ARCHITECT`  

### `--auto-accept-architect`
Enable/disable automatic acceptance of architect changes (default: True)  
Default: True  
Environment variable: `AIDER_AUTO_ACCEPT_ARCHITECT`  
Aliases:
  - `--auto-accept-architect`
  - `--no-auto-accept-architect`

### `--weak-model WEAK_MODEL`
Specify the model to use for commit messages and chat history summarization (default depends on --model)  
Environment variable: `AIDER_WEAK_MODEL`  

### `--editor-model EDITOR_MODEL`
Specify the model to use for editor tasks (default depends on --model)  
Environment variable: `AIDER_EDITOR_MODEL`  

### `--editor-edit-format EDITOR_EDIT_FORMAT`
Specify the edit format for the editor model (default: depends on editor model)  
Environment variable: `AIDER_EDITOR_EDIT_FORMAT`  

### `--show-model-warnings`
Only work with models that have meta-data available (default: True)  
Default: True  
Environment variable: `AIDER_SHOW_MODEL_WARNINGS`  
Aliases:
  - `--show-model-warnings`
  - `--no-show-model-warnings`

### `--check-model-accepts-settings`
Check if model accepts settings like reasoning_effort/thinking_tokens (default: True)  
Default: True  
Environment variable: `AIDER_CHECK_MODEL_ACCEPTS_SETTINGS`  
Aliases:
  - `--check-model-accepts-settings`
  - `--no-check-model-accepts-settings`

### `--max-chat-history-tokens VALUE`
Soft limit on tokens for chat history, after which summarization begins. If unspecified, defaults to the model's max_chat_history_tokens.  
Environment variable: `AIDER_MAX_CHAT_HISTORY_TOKENS`  

## Cache settings:

### `--cache-prompts`
Enable caching of prompts (default: False)  
Default: False  
Environment variable: `AIDER_CACHE_PROMPTS`  
Aliases:
  - `--cache-prompts`
  - `--no-cache-prompts`

### `--cache-keepalive-pings VALUE`
Number of times to ping at 5min intervals to keep prompt cache warm (default: 0)  
Default: 0  
Environment variable: `AIDER_CACHE_KEEPALIVE_PINGS`  

## Repomap settings:

### `--map-tokens VALUE`
Suggested number of tokens to use for repo map, use 0 to disable  
Environment variable: `AIDER_MAP_TOKENS`  

### `--map-refresh VALUE`
Control how often the repo map is refreshed. Options: auto, always, files, manual (default: auto)  
Default: auto  
Environment variable: `AIDER_MAP_REFRESH`  

### `--map-multiplier-no-files VALUE`
Multiplier for map tokens when no files are specified (default: 2)  
Default: 2  
Environment variable: `AIDER_MAP_MULTIPLIER_NO_FILES`  

## History Files:

### `--input-history-file INPUT_HISTORY_FILE`
Specify the chat input history file (default: .aider.input.history)  
Default: .aider.input.history  
Environment variable: `AIDER_INPUT_HISTORY_FILE`  

### `--chat-history-file CHAT_HISTORY_FILE`
Specify the chat history file (default: .aider.chat.history.md)  
Default: .aider.chat.history.md  
Environment variable: `AIDER_CHAT_HISTORY_FILE`  

### `--restore-chat-history`
Restore the previous chat history messages (default: False)  
Default: False  
Environment variable: `AIDER_RESTORE_CHAT_HISTORY`  
Aliases:
  - `--restore-chat-history`
  - `--no-restore-chat-history`

### `--llm-history-file LLM_HISTORY_FILE`
Log the conversation with the LLM to this file (for example, .aider.llm.history)  
Environment variable: `AIDER_LLM_HISTORY_FILE`  

## Output settings:

### `--dark-mode`
Use colors suitable for a dark terminal background (default: False)  
Default: False  
Environment variable: `AIDER_DARK_MODE`  

### `--light-mode`
Use colors suitable for a light terminal background (default: False)  
Default: False  
Environment variable: `AIDER_LIGHT_MODE`  

### `--pretty`
Enable/disable pretty, colorized output (default: True)  
Default: True  
Environment variable: `AIDER_PRETTY`  
Aliases:
  - `--pretty`
  - `--no-pretty`

### `--stream`
Enable/disable streaming responses (default: True)  
Default: True  
Environment variable: `AIDER_STREAM`  
Aliases:
  - `--stream`
  - `--no-stream`

### `--user-input-color VALUE`
Set the color for user input (default: #00cc00)  
Default: #00cc00  
Environment variable: `AIDER_USER_INPUT_COLOR`  

### `--tool-output-color VALUE`
Set the color for tool output (default: None)  
Environment variable: `AIDER_TOOL_OUTPUT_COLOR`  

### `--tool-error-color VALUE`
Set the color for tool error messages (default: #FF2222)  
Default: #FF2222  
Environment variable: `AIDER_TOOL_ERROR_COLOR`  

### `--tool-warning-color VALUE`
Set the color for tool warning messages (default: #FFA500)  
Default: #FFA500  
Environment variable: `AIDER_TOOL_WARNING_COLOR`  

### `--assistant-output-color VALUE`
Set the color for assistant output (default: #0088ff)  
Default: #0088ff  
Environment variable: `AIDER_ASSISTANT_OUTPUT_COLOR`  

### `--completion-menu-color COLOR`
Set the color for the completion menu (default: terminal's default text color)  
Environment variable: `AIDER_COMPLETION_MENU_COLOR`  

### `--completion-menu-bg-color COLOR`
Set the background color for the completion menu (default: terminal's default background color)  
Environment variable: `AIDER_COMPLETION_MENU_BG_COLOR`  

### `--completion-menu-current-color COLOR`
Set the color for the current item in the completion menu (default: terminal's default background color)  
Environment variable: `AIDER_COMPLETION_MENU_CURRENT_COLOR`  

### `--completion-menu-current-bg-color COLOR`
Set the background color for the current item in the completion menu (default: terminal's default text color)  
Environment variable: `AIDER_COMPLETION_MENU_CURRENT_BG_COLOR`  

### `--code-theme VALUE`
Set the markdown code theme (default: default, other options include monokai, solarized-dark, solarized-light, or a Pygments builtin style, see https://pygments.org/styles for available themes)  
Default: default  
Environment variable: `AIDER_CODE_THEME`  

### `--show-diffs`
Show diffs when committing changes (default: False)  
Default: False  
Environment variable: `AIDER_SHOW_DIFFS`  

## Git settings:

### `--git`
Enable/disable looking for a git repo (default: True)  
Default: True  
Environment variable: `AIDER_GIT`  
Aliases:
  - `--git`
  - `--no-git`

### `--gitignore`
Enable/disable adding .aider* to .gitignore (default: True)  
Default: True  
Environment variable: `AIDER_GITIGNORE`  
Aliases:
  - `--gitignore`
  - `--no-gitignore`

### `--aiderignore AIDERIGNORE`
Specify the aider ignore file (default: .aiderignore in git root)  
Default: .aiderignore  
Environment variable: `AIDER_AIDERIGNORE`  

### `--subtree-only`
Only consider files in the current subtree of the git repository  
Default: False  
Environment variable: `AIDER_SUBTREE_ONLY`  

### `--auto-commits`
Enable/disable auto commit of LLM changes (default: True)  
Default: True  
Environment variable: `AIDER_AUTO_COMMITS`  
Aliases:
  - `--auto-commits`
  - `--no-auto-commits`

### `--dirty-commits`
Enable/disable commits when repo is found dirty (default: True)  
Default: True  
Environment variable: `AIDER_DIRTY_COMMITS`  
Aliases:
  - `--dirty-commits`
  - `--no-dirty-commits`

### `--attribute-author`
Attribute aider code changes in the git author name (default: True)  
Default: True  
Environment variable: `AIDER_ATTRIBUTE_AUTHOR`  
Aliases:
  - `--attribute-author`
  - `--no-attribute-author`

### `--attribute-committer`
Attribute aider commits in the git committer name (default: True)  
Default: True  
Environment variable: `AIDER_ATTRIBUTE_COMMITTER`  
Aliases:
  - `--attribute-committer`
  - `--no-attribute-committer`

### `--attribute-commit-message-author`
Prefix commit messages with 'aider: ' if aider authored the changes (default: False)  
Default: False  
Environment variable: `AIDER_ATTRIBUTE_COMMIT_MESSAGE_AUTHOR`  
Aliases:
  - `--attribute-commit-message-author`
  - `--no-attribute-commit-message-author`

### `--attribute-commit-message-committer`
Prefix all commit messages with 'aider: ' (default: False)  
Default: False  
Environment variable: `AIDER_ATTRIBUTE_COMMIT_MESSAGE_COMMITTER`  
Aliases:
  - `--attribute-commit-message-committer`
  - `--no-attribute-commit-message-committer`

### `--commit`
Commit all pending changes with a suitable commit message, then exit  
Default: False  
Environment variable: `AIDER_COMMIT`  

### `--commit-prompt PROMPT`
Specify a custom prompt for generating commit messages  
Environment variable: `AIDER_COMMIT_PROMPT`  

### `--dry-run`
Perform a dry run without modifying files (default: False)  
Default: False  
Environment variable: `AIDER_DRY_RUN`  
Aliases:
  - `--dry-run`
  - `--no-dry-run`

### `--skip-sanity-check-repo`
Skip the sanity check for the git repository (default: False)  
Default: False  
Environment variable: `AIDER_SKIP_SANITY_CHECK_REPO`  

### `--watch-files`
Enable/disable watching files for ai coding comments (default: False)  
Default: False  
Environment variable: `AIDER_WATCH_FILES`  
Aliases:
  - `--watch-files`
  - `--no-watch-files`

## Fixing and committing:

### `--lint`
Lint and fix provided files, or dirty files if none provided  
Default: False  
Environment variable: `AIDER_LINT`  

### `--lint-cmd`
Specify lint commands to run for different languages, eg: "python: flake8 --select=..." (can be used multiple times)  
Default: []  
Environment variable: `AIDER_LINT_CMD`  

### `--auto-lint`
Enable/disable automatic linting after changes (default: True)  
Default: True  
Environment variable: `AIDER_AUTO_LINT`  
Aliases:
  - `--auto-lint`
  - `--no-auto-lint`

### `--test-cmd VALUE`
Specify command to run tests  
Default: []  
Environment variable: `AIDER_TEST_CMD`  

### `--auto-test`
Enable/disable automatic testing after changes (default: False)  
Default: False  
Environment variable: `AIDER_AUTO_TEST`  
Aliases:
  - `--auto-test`
  - `--no-auto-test`

### `--test`
Run tests, fix problems found and then exit  
Default: False  
Environment variable: `AIDER_TEST`  

## Analytics:

### `--analytics`
Enable/disable analytics for current session (default: random)  
Environment variable: `AIDER_ANALYTICS`  
Aliases:
  - `--analytics`
  - `--no-analytics`

### `--analytics-log ANALYTICS_LOG_FILE`
Specify a file to log analytics events  
Environment variable: `AIDER_ANALYTICS_LOG`  

### `--analytics-disable`
Permanently disable analytics  
Default: False  
Environment variable: `AIDER_ANALYTICS_DISABLE`  

## Upgrading:

### `--just-check-update`
Check for updates and return status in the exit code  
Default: False  
Environment variable: `AIDER_JUST_CHECK_UPDATE`  

### `--check-update`
Check for new aider versions on launch  
Default: True  
Environment variable: `AIDER_CHECK_UPDATE`  
Aliases:
  - `--check-update`
  - `--no-check-update`

### `--show-release-notes`
Show release notes on first run of new version (default: None, ask user)  
Environment variable: `AIDER_SHOW_RELEASE_NOTES`  
Aliases:
  - `--show-release-notes`
  - `--no-show-release-notes`

### `--install-main-branch`
Install the latest version from the main branch  
Default: False  
Environment variable: `AIDER_INSTALL_MAIN_BRANCH`  

### `--upgrade`
Upgrade aider to the latest version from PyPI  
Default: False  
Environment variable: `AIDER_UPGRADE`  
Aliases:
  - `--upgrade`
  - `--update`

### `--version`
Show the version number and exit  

## Modes:

### `--message COMMAND`
Specify a single message to send the LLM, process reply then exit (disables chat mode)  
Environment variable: `AIDER_MESSAGE`  
Aliases:
  - `--message COMMAND`
  - `--msg COMMAND`
  - `-m COMMAND`

### `--message-file MESSAGE_FILE`
Specify a file containing the message to send the LLM, process reply, then exit (disables chat mode)  
Environment variable: `AIDER_MESSAGE_FILE`  
Aliases:
  - `--message-file MESSAGE_FILE`
  - `-f MESSAGE_FILE`

### `--gui`
Run aider in your browser (default: False)  
Default: False  
Environment variable: `AIDER_GUI`  
Aliases:
  - `--gui`
  - `--no-gui`
  - `--browser`
  - `--no-browser`

### `--copy-paste`
Enable automatic copy/paste of chat between aider and web UI (default: False)  
Default: False  
Environment variable: `AIDER_COPY_PASTE`  
Aliases:
  - `--copy-paste`
  - `--no-copy-paste`

### `--apply FILE`
Apply the changes from the given file instead of running the chat (debug)  
Environment variable: `AIDER_APPLY`  

### `--apply-clipboard-edits`
Apply clipboard contents as edits using the main model's editor format  
Default: False  
Environment variable: `AIDER_APPLY_CLIPBOARD_EDITS`  

### `--exit`
Do all startup activities then exit before accepting user input (debug)  
Default: False  
Environment variable: `AIDER_EXIT`  

### `--show-repo-map`
Print the repo map and exit (debug)  
Default: False  
Environment variable: `AIDER_SHOW_REPO_MAP`  

### `--show-prompts`
Print the system prompts and exit (debug)  
Default: False  
Environment variable: `AIDER_SHOW_PROMPTS`  

## Voice settings:

### `--voice-format VOICE_FORMAT`
Audio format for voice recording (default: wav). webm and mp3 require ffmpeg  
Default: wav  
Environment variable: `AIDER_VOICE_FORMAT`  

### `--voice-language VOICE_LANGUAGE`
Specify the language for voice using ISO 639-1 code (default: auto)  
Default: en  
Environment variable: `AIDER_VOICE_LANGUAGE`  

### `--voice-input-device VOICE_INPUT_DEVICE`
Specify the input device name for voice recording  
Environment variable: `AIDER_VOICE_INPUT_DEVICE`  

## Other settings:

### `--file FILE`
specify a file to edit (can be used multiple times)  
Environment variable: `AIDER_FILE`  

### `--read FILE`
specify a read-only file (can be used multiple times)  
Environment variable: `AIDER_READ`  

### `--vim`
Use VI editing mode in the terminal (default: False)  
Default: False  
Environment variable: `AIDER_VIM`  

### `--chat-language CHAT_LANGUAGE`
Specify the language to use in the chat (default: None, uses system settings)  
Environment variable: `AIDER_CHAT_LANGUAGE`  

### `--yes-always`
Always say yes to every confirmation  
Environment variable: `AIDER_YES_ALWAYS`  

### `--verbose`
Enable verbose output  
Default: False  
Environment variable: `AIDER_VERBOSE`  
Aliases:
  - `-v`
  - `--verbose`

### `--load LOAD_FILE`
Load and execute /commands from a file on launch  
Environment variable: `AIDER_LOAD`  

### `--encoding VALUE`
Specify the encoding for input and output (default: utf-8)  
Default: utf-8  
Environment variable: `AIDER_ENCODING`  

### `--line-endings VALUE`
Line endings to use when writing files (default: platform)  
Default: platform  
Environment variable: `AIDER_LINE_ENDINGS`  

### `--config CONFIG_FILE`
Specify the config file (default: search for .aider.conf.yml in git root, cwd or home directory)  
Aliases:
  - `-c CONFIG_FILE`
  - `--config CONFIG_FILE`

### `--env-file ENV_FILE`
Specify the .env file to load (default: .env in git root)  
Default: .env  
Environment variable: `AIDER_ENV_FILE`  

### `--suggest-shell-commands`
Enable/disable suggesting shell commands (default: True)  
Default: True  
Environment variable: `AIDER_SUGGEST_SHELL_COMMANDS`  
Aliases:
  - `--suggest-shell-commands`
  - `--no-suggest-shell-commands`

### `--fancy-input`
Enable/disable fancy input with history and completion (default: True)  
Default: True  
Environment variable: `AIDER_FANCY_INPUT`  
Aliases:
  - `--fancy-input`
  - `--no-fancy-input`

### `--multiline`
Enable/disable multi-line input mode with Meta-Enter to submit (default: False)  
Default: False  
Environment variable: `AIDER_MULTILINE`  
Aliases:
  - `--multiline`
  - `--no-multiline`

### `--notifications`
Enable/disable terminal bell notifications when LLM responses are ready (default: False)  
Default: False  
Environment variable: `AIDER_NOTIFICATIONS`  
Aliases:
  - `--notifications`
  - `--no-notifications`

### `--notifications-command COMMAND`
Specify a command to run for notifications instead of the terminal bell. If not specified, a default command for your OS may be used.  
Environment variable: `AIDER_NOTIFICATIONS_COMMAND`  

### `--detect-urls`
Enable/disable detection and offering to add URLs to chat (default: True)  
Default: True  
Environment variable: `AIDER_DETECT_URLS`  
Aliases:
  - `--detect-urls`
  - `--no-detect-urls`

### `--editor VALUE`
Specify which editor to use for the /editor command  
Environment variable: `AIDER_EDITOR`  

## Deprecated model settings:

### `--opus`
Use claude-3-opus-20240229 model for the main chat (deprecated, use --model)  
Default: False  
Environment variable: `AIDER_OPUS`  

### `--sonnet`
Use anthropic/claude-3-7-sonnet-20250219 model for the main chat (deprecated, use --model)  
Default: False  
Environment variable: `AIDER_SONNET`  

### `--haiku`
Use claude-3-5-haiku-20241022 model for the main chat (deprecated, use --model)  
Default: False  
Environment variable: `AIDER_HAIKU`  

### `--4`
Use gpt-4-0613 model for the main chat (deprecated, use --model)  
Default: False  
Environment variable: `AIDER_4`  
Aliases:
  - `--4`
  - `-4`

### `--4o`
Use gpt-4o model for the main chat (deprecated, use --model)  
Default: False  
Environment variable: `AIDER_4O`  

### `--mini`
Use gpt-4o-mini model for the main chat (deprecated, use --model)  
Default: False  
Environment variable: `AIDER_MINI`  

### `--4-turbo`
Use gpt-4-1106-preview model for the main chat (deprecated, use --model)  
Default: False  
Environment variable: `AIDER_4_TURBO`  

### `--35turbo`
Use gpt-3.5-turbo model for the main chat (deprecated, use --model)  
Default: False  
Environment variable: `AIDER_35TURBO`  
Aliases:
  - `--35turbo`
  - `--35-turbo`
  - `--3`
  - `-3`

### `--deepseek`
Use deepseek/deepseek-chat model for the main chat (deprecated, use --model)  
Default: False  
Environment variable: `AIDER_DEEPSEEK`  

### `--o1-mini`
Use o1-mini model for the main chat (deprecated, use --model)  
Default: False  
Environment variable: `AIDER_O1_MINI`  

### `--o1-preview`
Use o1-preview model for the main chat (deprecated, use --model)  
Default: False  
Environment variable: `AIDER_O1_PREVIEW`  
<!--[[[end]]]-->
`````

## File: config/reasoning.md
`````markdown
---
parent: Configuration
nav_order: 110
description: How to configure reasoning model settings from secondary providers.
---

# Reasoning models

![Thinking demo](/assets/thinking.jpg)

## Reasoning settings

Different models support different reasoning settings. Aider provides several ways to control reasoning behavior:

### Reasoning effort

You can use the `--reasoning-effort` switch to control the reasoning effort
of models which support this setting.
This switch is useful for OpenAI's reasoning models, which accept "low", "medium" and "high".

### Thinking tokens

You can use the `--thinking-tokens` switch to request
the model use a certain number of thinking tokens.
This switch is useful for Sonnet 3.7.
You can specify the token budget like "1024", "1k", "8k" or "0.01M".

### Model compatibility and settings

Not all models support these two settings. Aider uses the 
[model's metadata](/docs/config/adv-model-settings.html)
to determine which settings each model accepts:

```yaml
- name: o3-mini
  ...
  accepts_settings: ["reasoning_effort"]
```

If you try to use a setting that a model doesn't explicitly support, Aider will warn you:

```
Warning: o3-mini does not support 'thinking_tokens', ignoring.
Use --no-check-model-accepts-settings to force the 'thinking_tokens' setting.
```

The warning informs you that:
1. The setting won't be applied because the model doesn't list it in `accepts_settings`
2. You can use `--no-check-model-accepts-settings` to force the setting anyway

This functionality helps prevent API errors while still allowing you to experiment with settings when needed.

Each model has a predefined list of supported settings in its configuration. For example:

- OpenAI reasoning models generally support `reasoning_effort`
- Anthropic reasoning models generally support `thinking_tokens`


### How `accepts_settings` works

Models define which reasoning settings they accept using the `accepts_settings` property:

```yaml
- name: a-fancy-reasoning-model
  edit_format: diff
  use_repo_map: true
  accepts_settings:                  # <---
    - reasoning_effort               # <---
```

This configuration:
1. Tells Aider that the model accepts the `reasoning_effort` setting
2. Indicates the model does NOT accept `thinking_tokens` (since it's not listed)
3. Causes Aider to ignore any `--thinking-tokens` value passed for this model
4. Generates a warning if you try to use `--thinking-tokens` with this model

You can override this behavior with `--no-check-model-accepts-settings`, which will:
1. Force Aider to apply all settings passed via command line
2. Skip all compatibility checks
3. Potentially cause API errors if the model truly doesn't support the setting

This is useful when testing new models or using models through custom API providers.


## Thinking tokens in XML tags

There is also a `reasoning_tag` setting, which takes the name of an XML tag
that the model uses to wrap its reasoning/thinking output.

For example when using DeepSeek R1 from Fireworks, the reasoning comes back inside
`<think>...</think>` tags, so aider's settings
include `reasoning_tag: think`.

```
<think>
The user wants me to greet them!
</think>

Hello!
```

Aider will display the thinking/reasoning output, 
but it won't be used for file editing instructions, added to the chat history, etc.
Aider will rely on the non-thinking output for instructions on how to make code changes, etc.

### Model-specific reasoning tags

Different models use different XML tags for their reasoning:
When using custom or self-hosted models, you may need to specify the appropriate reasoning tag in your configuration.

```yaml
- name: fireworks_ai/accounts/fireworks/models/deepseek-r1
  edit_format: diff
  weak_model_name: fireworks_ai/accounts/fireworks/models/deepseek-v3
  use_repo_map: true
  extra_params:
    max_tokens: 160000
  use_temperature: false
  editor_model_name: fireworks_ai/accounts/fireworks/models/deepseek-v3
  editor_edit_format: editor-diff
  reasoning_tag: think                 # <---
```

## Reasoning model limitations

Many "reasoning" models have restrictions on how they can be used:
they sometimes prohibit streaming, use of temperature and/or the system prompt.
Aider is configured to work properly with popular models
when served through major provider APIs.

If you're using a model through a different provider (like Azure or custom deployment),
you may need to [configure model settings](/docs/config/adv-model-settings.html)
if you see errors related to temperature or system prompt.

Include settings for your new provider in `.aider.model.settings.yml` file
at the root of your project or in your home directory.

### Temperature, streaming and system prompt

Reasoning models often have specific requirements for these settings:

| Setting | Description | Common Restrictions |
|---------|-------------|---------------------|
| `use_temperature` | Whether to use temperature sampling | Many reasoning models require this set to `false` |
| `streaming` | Whether to stream responses | Some reasoning models don't support streaming |
| `use_system_prompt` | Whether to use system prompt | Some reasoning models don't support system prompts |

It may be helpful to find one of the 
[existing model setting configuration entries](https://github.com/Aider-AI/aider/blob/main/aider/resources/model-settings.yml)
for the model you are interested in, say o3-mini:

```yaml
- name: o3-mini
  edit_format: diff
  weak_model_name: gpt-4o-mini
  use_repo_map: true
  use_temperature: false             # <---
  editor_model_name: gpt-4o
  editor_edit_format: editor-diff
  accepts_settings: ["reasoning_effort"]
```

Pay attention to these settings, which must be set to `false`
for certain reasoning models:

- `use_temperature`
- `streaming` 
- `use_system_prompt`

### Custom provider example

Here's an example of the settings to use o3-mini via Azure.
Note that aider already has these settings pre-configured, but they
serve as a good example of how to adapt the main model
settings for a different provider.

```yaml
- name: azure/o3-mini
  edit_format: diff
  weak_model_name: azure/gpt-4o-mini
  use_repo_map: true
  use_temperature: false             # <---
  editor_model_name: azure/gpt-4o
  editor_edit_format: editor-diff
  accepts_settings: ["reasoning_effort"]
```
`````

## File: install/codespaces.md
`````markdown
---
title: GitHub Codespaces
parent: Installation
nav_order: 900
---

# GitHub Codespaces

You can use aider in GitHub Codespaces via the built-in Terminal pane.
See below for an example,
but you can just follow the
[main install instructions](/docs/install.html)
inside your codespace terminal.


<div class="video-container">
  <video controls poster="/assets/codespaces.jpg">
    <source src="/assets/codespaces.mp4" type="video/mp4">
    <a href="/assets/codespaces.mp4">Install aider in GitHub Codespaces</a>
  </video>
</div>

<style>
.video-container {
  position: relative;
  padding-bottom: 101.89%; /* 1080 / 1060 = 1.0189 */
  height: 0;
  overflow: hidden;
}

.video-container video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>
`````

## File: install/docker.md
`````markdown
---
parent: Installation
nav_order: 100
---

# Aider with docker

Aider is available as 2 docker images:

- `paulgauthier/aider` installs the aider core, a smaller image that's good to get started quickly.
- `paulgauthier/aider-full` installs aider will all the optional extras.

The full image has support for features like interactive help, the
browser GUI and support for using Playwright to scrape web pages.  The
core image can still use these features, but they will need to be
installed the first time you access them. Since containers are
ephemeral, the extras will need to be reinstalled the next time you
launch the aider core container.

### Aider core 

```
docker pull paulgauthier/aider
docker run -it --user $(id -u):$(id -g) --volume $(pwd):/app paulgauthier/aider --openai-api-key $OPENAI_API_KEY [...other aider args...]
```

### Full version

```
docker pull paulgauthier/aider-full
docker run -it --user $(id -u):$(id -g) --volume $(pwd):/app paulgauthier/aider-full --openai-api-key $OPENAI_API_KEY [...other aider args...]
```

## How to use it

You should run the above commands from the root of your git repo,
since the `--volume` arg maps your current directory into the
docker container.
Given that, you need to be in the root of your git repo for aider to be able to
see the repo and all its files.

You should be sure your that
git repo config contains your user name and email, since the
docker container won't have your global git config.
Run these commands while in your git repo, before
you do the `docker run` command:

```
git config user.email "you@example.com"
git config user.name "Your Name"
```


## Limitations

- When you use the in-chat `/run` command, it will be running shell commands *inside the docker container*. So those commands won't be running in your local environment, which may make it tricky to `/run` tests, etc for your project.
- The `/voice` command won't work unless you can figure out how to give the docker container access to your host audio device. The container has libportaudio2 installed, so it should work if you can do that.
`````

## File: install/optional.md
`````markdown
---
parent: Installation
nav_order: 20
---

# Optional steps
{: .no_toc }

The steps below are completely optional.

- TOC
{:toc}

## Install git

Aider works best if you have git installed.
Here are
[instructions for installing git in various environments](https://github.com/git-guides/install-git).

## Setup an API key

You need an key from an API provider to work with most models:

- [OpenAI](https://help.openai.com/en/articles/4936850-where-do-i-find-my-secret-api-key) provides o1, o3-mini, gpt-4o and other models. Note that paying for an API key is different than being a "ChatGPT" subscriber.
- [Anthropic](https://docs.anthropic.com/claude/reference/getting-started-with-the-api) provides Claude 3.7 Sonnet and Haiku.
- [DeepSeek](https://platform.deepseek.com/api_keys) provides DeepSeek R1 and DeepSeek Chat V3.
- [OpenRouter](https://openrouter.ai/keys) allows you to access models from many providers using a single key.

You can [store your api keys in configuration or env files](/docs/config/api-keys.html)
and they will be loaded automatically whenever you run aider.

## Enable Playwright 

Aider supports adding web pages to the chat with the `/web <url>` command.
When you add a url to the chat, aider fetches the page and scrapes its
content.

By default, aider uses the `httpx` library to scrape web pages, but this only
works on a subset of web pages.
Some sites explicitly block requests from tools like httpx.
Others rely heavily on javascript to render the page content,
which isn't possible using only httpx.

Aider works best with all web pages if you install
Playwright's chromium browser and its dependencies:

```
playwright install --with-deps chromium
```

See the
[Playwright for Python documentation](https://playwright.dev/python/docs/browsers#install-system-dependencies)
for additional information.


## Enable voice coding 

Aider supports 
[coding with your voice](https://aider.chat/docs/usage/voice.html)
using the in-chat `/voice` command.
Aider uses the [PortAudio](http://www.portaudio.com) library to
capture audio.
Installing PortAudio is completely optional, but can usually be accomplished like this:

- For Windows, there is no need to install PortAudio.
- For Mac, do `brew install portaudio`
- For Linux, do `sudo apt-get install libportaudio2`
  - Some linux environments may also need `sudo apt install libasound2-plugins`

## Add aider to your IDE/editor

You can use 
[aider's `--watch-files` mode](https://aider.chat/docs/usage/watch.html)
to integrate with any IDE or editor.

There are a number of 3rd party aider plugins for various IDE/editors.
It's not clear how well they are tracking the latest
versions of aider,
so it may be best to just run the latest
aider in a terminal alongside your editor and use `--watch-files`.

### NeoVim

[joshuavial](https://github.com/joshuavial) provided a NeoVim plugin for aider:

[https://github.com/joshuavial/aider.nvim](https://github.com/joshuavial/aider.nvim)

### VS Code

You can run aider inside a VS Code terminal window.
There are a number of 3rd party 
[aider plugins for VSCode](https://marketplace.visualstudio.com/search?term=aider%20-kodu&target=VSCode&category=All%20categories&sortBy=Relevance).

### Other editors

If you are interested in creating an aider plugin for your favorite editor,
please let us know by opening a
[GitHub issue](https://github.com/Aider-AI/aider/issues).
`````

## File: install/replit.md
`````markdown
---
parent: Installation
nav_order: 900
---

### Replit

{% include replit-pipx.md %}
`````

## File: llms/anthropic.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 200
---

# Anthropic

To work with Anthropic's models, you need to provide your
[Anthropic API key](https://docs.anthropic.com/claude/reference/getting-started-with-the-api)
either in the `ANTHROPIC_API_KEY` environment variable or
via the `--anthropic-api-key` command line switch.

Aider has some built in shortcuts for the most popular Anthropic models and
has been tested and benchmarked to work well with them:

```
python -m pip install -U aider-chat

export ANTHROPIC_API_KEY=<key> # Mac/Linux
setx   ANTHROPIC_API_KEY <key> # Windows, restart shell after setx

# Aider uses Claude 3.7 Sonnet by default
aider

# Claude 3 Opus
aider --model claude-3-opus-20240229

# List models available from Anthropic
aider --list-models anthropic/
```

{: .tip }
Anthropic has very low rate limits. 
You can access all the Anthropic models via
[OpenRouter](openrouter.md)
or [Google Vertex AI](vertex.md)
with more generous rate limits.

You can use `aider --model <model-name>` to use any other Anthropic model.
For example, if you want to use a specific version of Opus
you could do `aider --model claude-3-opus-20240229`.

## Thinking tokens

Aider can work with Sonnet 3.7's new thinking tokens, but does not ask Sonnet to use
thinking tokens by default.

Enabling thinking currently requires manual configuration.
You need to add the following to your `.aider.model.settings.yml` 
[model settings file](/docs/config/adv-model-settings.html#model-settings).
Adjust the `budget_tokens` value to change the target number of thinking tokens.

```yaml
- name: anthropic/claude-3-7-sonnet-20250219
  edit_format: diff
  weak_model_name: anthropic/claude-3-5-haiku-20241022
  use_repo_map: true
  examples_as_sys_msg: true
  use_temperature: false
  extra_params:
    extra_headers:
      anthropic-beta: prompt-caching-2024-07-31,pdfs-2024-09-25,output-128k-2025-02-19
    max_tokens: 64000
    thinking:
      type: enabled
      budget_tokens: 32000 # Adjust this number
  cache_control: true
  editor_model_name: anthropic/claude-3-7-sonnet-20250219
  editor_edit_format: editor-diff
```

More streamlined support will be coming soon.
`````

## File: llms/azure.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 500
---

# Azure

Aider can connect to the OpenAI models on Azure.

```
python -m pip install -U aider-chat

# Mac/Linux:                                           
export AZURE_API_KEY=<key>
export AZURE_API_VERSION=2023-05-15
export AZURE_API_BASE=https://myendpt.openai.azure.com

# Windows
setx AZURE_API_KEY <key>
setx AZURE_API_VERSION 2023-05-15
setx AZURE_API_BASE https://myendpt.openai.azure.com
# ... restart your shell after setx commands

aider --model azure/<your_deployment_name>

# List models available from Azure
aider --list-models azure/
```

Note that aider will also use environment variables
like `AZURE_OPENAI_API_xxx`.
`````

## File: llms/bedrock.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 560
---

# Amazon Bedrock

Aider can connect to models provided by Amazon Bedrock.
You will need to have an AWS account with access to the Bedrock service.

To configure Aider to use the Amazon Bedrock API, you need to set up your AWS credentials.
This can be done using the AWS CLI or by setting environment variables.

## Select a Model from Amazon Bedrock

Before you can use a model through Amazon Bedrock, you must "enable" the model under the **Model
Access** screen in the AWS Management Console.
To find the `Model ID`, open the **Model Catalog** area in the Bedrock console, select the model 
you want to use, and the find the `modelId` property under the "Usage" heading.

### Bedrock Inference Profiles

Amazon Bedrock has added support for a new feature called [cross-region "inference profiles."](https://aws.amazon.com/about-aws/whats-new/2024/09/amazon-bedrock-knowledge-bases-cross-region-inference/)
Some models hosted in Bedrock _only_ support these inference profiles.
If you're using one of these models, then you will need to use the `Inference Profile ID` 
instead of the `Model ID` from the **Model Catalog** screen, in the AWS Management Console.
For example, the Claude Sonnet 3.7 model, release in February 2025, exclusively supports
inference through inference profiles. To use this model, you would use the 
`us.anthropic.claude-3-7-sonnet-20250219-v1:0` Inference Profile ID.
In the Amazon Bedrock console, go to Inference and Assessment ➡️ Cross-region Inference
to find the `Inference Profile ID` value.

If you attempt to use a `Model ID` for a model that exclusively supports the Inference Profile
feature, you will receive an error message like the following:

> litellm.BadRequestError: BedrockException - b'{"message":"Invocation of model ID
anthropic.claude-3-7-sonnet-20250219-v1:0 with on-demand throughput isn\xe2\x80\x99t supported. Retry your
request with the ID or ARN of an inference profile that contains this model."}'

## AWS CLI Configuration

If you haven't already, install the [AWS CLI](https://aws.amazon.com/cli/) and configure it with your credentials:

```bash
aws configure
```

This will prompt you to enter your AWS Access Key ID, Secret Access Key, and default region.

## Environment Variables

Alternatively, you can set the following environment variables:

```bash
export AWS_REGION=your_preferred_region

# For user authentication
export AWS_ACCESS_KEY_ID=your_access_key
export AWS_SECRET_ACCESS_KEY=your_secret_key

# For profile authentication
export AWS_PROFILE=your-profile
```

You can add these to your 
[.env file](/docs/config/dotenv.html).

### Set Environment Variables with PowerShell

If you're using PowerShell on MacOS, Linux, or Windows, you can set the same AWS configuration environment variables with these commands.

```pwsh
$env:AWS_ACCESS_KEY_ID = 'your_access_key'
$env:AWS_SECRET_ACCESS_KEY = 'your_secret_key'
$env:AWS_REGION = 'us-west-2'   # Put whichever AWS region that you'd like, that the Bedrock service supports.
```

## Install boto3

The AWS Bedrock provider requires the `boto3` package in order to function correctly:

```bash
pip install boto3
```

To use aider installed via `pipx` with AWS Bedrock, you must add the `boto3` dependency to aider's virtual environment by running

```bash
pipx inject aider-chat boto3
```

You must install `boto3` dependency to aider's virtual environment installed via one-liner or uv by running

```bash
uv tool run --from aider-chat pip install boto3
```


## Running Aider with Bedrock

Once your AWS credentials are set up, you can run Aider with the `--model` command line switch, specifying the Bedrock model you want to use:

```bash
aider --model bedrock/anthropic.claude-3-5-sonnet-20240620-v1:0
```

Sometimes it seems to help if you prefix the model name with "us.":

```bash
aider --model bedrock/us.anthropic.claude-3-5-sonnet-20240620-v1:0
```


## Available Models

To see some models available via Bedrock, run:

```bash
aider --list-models bedrock/
```

Make sure you have access to these models in your AWS account before attempting to use them with Aider.

# More info

For more information on Amazon Bedrock and its models, refer to the [official AWS documentation](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html).

Also, see the 
[litellm docs on Bedrock](https://litellm.vercel.app/docs/providers/bedrock).
`````

## File: llms/cohere.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 500
---

# Cohere

Cohere offers *free* API access to their models.
Their Command-R+ model works well with aider
as a *very basic* coding assistant.
You'll need a [Cohere API key](https://dashboard.cohere.com/welcome/login).

To use **Command-R+**:

```
python -m pip install -U aider-chat

export COHERE_API_KEY=<key> # Mac/Linux
setx   COHERE_API_KEY <key> # Windows, restart shell after setx

aider --model command-r-plus-08-2024

# List models available from Cohere
aider --list-models cohere_chat/
```
`````

## File: llms/deepseek.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 500
---

# DeepSeek

Aider can connect to the DeepSeek.com API.
To work with DeepSeek's models, you need to set the `DEEPSEEK_API_KEY` environment variable with your [DeepSeek API key](https://platform.deepseek.com/api_keys).  
The DeepSeek Chat V3 model has a top score on aider's code editing benchmark.

```
python -m pip install -U aider-chat

export DEEPSEEK_API_KEY=<key> # Mac/Linux
setx   DEEPSEEK_API_KEY <key> # Windows, restart shell after setx

# Use DeepSeek Chat v3
aider --model deepseek/deepseek-chat
```
`````

## File: llms/gemini.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 300
---

# Gemini

You'll need a [Gemini API key](https://aistudio.google.com/app/u/2/apikey).

```
python -m pip install -U aider-chat

# You may need to install google-generativeai
pip install -U google-generativeai

# Or with pipx...
pipx inject aider-chat google-generativeai

export GEMINI_API_KEY=<key> # Mac/Linux
setx   GEMINI_API_KEY <key> # Windows, restart shell after setx

aider --model gemini/gemini-1.5-pro-latest

# List models available from Gemini
aider --list-models gemini/
```
`````

## File: llms/groq.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 400
---

# GROQ

Groq currently offers *free* API access to the models they host.
The Llama 3 70B model works
well with aider and is comparable to GPT-3.5 in code editing performance.
You'll need a [Groq API key](https://console.groq.com/keys).

To use **Llama3 70B**:

```
python -m pip install -U aider-chat

export GROQ_API_KEY=<key> # Mac/Linux
setx   GROQ_API_KEY <key> # Windows, restart shell after setx

aider --model groq/llama3-70b-8192

# List models available from Groq
aider --list-models groq/
```
`````

## File: llms/lm-studio.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 400
---

# LM Studio

To use LM Studio:

```
python -m pip install -U aider-chat

# Must set a value here even if its a dummy value
export LM_STUDIO_API_KEY=dummy-api-key # Mac/Linux
setx   LM_STUDIO_API_KEY dummy-api-key # Windows, restart shell after setx

# LM Studio default server URL is http://localhost:1234/v1
export LM_STUDIO_API_BASE=http://localhost:1234/v1 # Mac/Linux
setx   LM_STUDIO_API_BASE http://localhost:1234/v1 # Windows, restart shell after setx

aider --model lm_studio/<your-model-name>
```

**Note:** Even though LM Studio doesn't require an API Key out of the box the `LM_STUDIO_API_KEY` must have a dummy value like `dummy-api-key` set or the client request will fail trying to send an empty `Bearer` token.

See the [model warnings](warnings.html)
section for information on warnings which will occur
when working with models that aider is not familiar with.
`````

## File: llms/ollama.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 500
---

# Ollama

Aider can connect to local Ollama models.

```
# Pull the model
ollama pull <model>

# Start your ollama server
ollama serve

# In another terminal window...
python -m pip install -U aider-chat

export OLLAMA_API_BASE=http://127.0.0.1:11434 # Mac/Linux
setx   OLLAMA_API_BASE http://127.0.0.1:11434 # Windows, restart shell after setx

aider --model ollama_chat/<model>
```

{: .note }
Using `ollama_chat/` is recommended over `ollama/`.


See the [model warnings](warnings.html)
section for information on warnings which will occur
when working with models that aider is not familiar with.

## API Key

If you are using an ollama that requires an API key you can set `OLLAMA_API_KEY`:

```
export OLLAMA_API_KEY=<api-key> # Mac/Linux
setx   OLLAMA_API_KEY <api-key> # Windows, restart shell after setx
```

## Setting the context window size

[Ollama uses a 2k context window by default](https://github.com/ollama/ollama/blob/main/docs/faq.md#how-can-i-specify-the-context-window-size),
which is very small for working with aider.
It also **silently** discards context that exceeds the window. 
This is especially dangerous because many users don't even realize that most of their data
is being discarded by Ollama.
 
By default, aider sets Ollama's context window 
to be large enough for each request you send plus 8k tokens for the reply.
This ensures data isn't silently discarded by Ollama.

If you'd like you can configure a fixed sized context window instead
with an
[`.aider.model.settings.yml` file](https://aider.chat/docs/config/adv-model-settings.html#model-settings)
like this:

```
- name: ollama/qwen2.5-coder:32b-instruct-fp16
  extra_params:
    num_ctx: 65536
```
`````

## File: llms/openai-compat.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 500
---

# OpenAI compatible APIs

Aider can connect to any LLM which is accessible via an OpenAI compatible API endpoint.

```
python -m pip install aider-install
aider-install

# Mac/Linux:
export OPENAI_API_BASE=<endpoint>
export OPENAI_API_KEY=<key>

# Windows:
setx OPENAI_API_BASE <endpoint>
setx OPENAI_API_KEY <key>
# ... restart shell after setx commands

# Prefix the model name with openai/
aider --model openai/<model-name>
```

See the [model warnings](warnings.html)
section for information on warnings which will occur
when working with models that aider is not familiar with.
`````

## File: llms/openai.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 100
---

# OpenAI

To work with OpenAI's models, you need to provide your
[OpenAI API key](https://help.openai.com/en/articles/4936850-where-do-i-find-my-secret-api-key)
either in the `OPENAI_API_KEY` environment variable or
via the `--api-key openai=<key>` command line switch.

Aider has some built in shortcuts for the most popular OpenAI models and
has been tested and benchmarked to work well with them:

```
python -m pip install -U aider-chat

# o3-mini
aider --model o3-mini --api-key openai=<key>

# o1-mini
aider --model o1-mini --api-key openai=<key>

# GPT-4o
aider --model gpt-4o --api-key openai=<key>

# List models available from OpenAI
aider --list-models openai/

# You can also store you API key in environment variables (or .env)
export OPENAI_API_KEY=<key> # Mac/Linux
setx   OPENAI_API_KEY <key> # Windows, restart shell after setx
```

You can use `aider --model <model-name>` to use any other OpenAI model.
For example, if you want to use a specific version of GPT-4 Turbo
you could do `aider --model gpt-4-0125-preview`.

## Reasoning models from other providers

Many of OpenAI's 
"reasoning" models have restrictions on streaming and setting the temperature parameter.
Some also support different levels of "reasoning effort".
Aider is configured to work properly with these models
when served through major provider APIs and
has a `--reasoning-effort` setting.

You may need to [configure reasoning model settings](/docs/config/reasoning.html)
if you are using them through another provider
and see errors related to temperature or system prompt.
`````

## File: llms/openrouter.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 500
---

# OpenRouter

Aider can connect to [models provided by OpenRouter](https://openrouter.ai/models?o=top-weekly):
You'll need an [OpenRouter API key](https://openrouter.ai/keys).

```
python -m pip install -U aider-chat

export OPENROUTER_API_KEY=<key> # Mac/Linux
setx   OPENROUTER_API_KEY <key> # Windows, restart shell after setx

# Or any other open router model
aider --model openrouter/<provider>/<model>

# List models available from OpenRouter
aider --list-models openrouter/
```

In particular, many aider users access Sonnet via OpenRouter:

```
python -m pip install -U aider-chat

export OPENROUTER_API_KEY=<key> # Mac/Linux
setx   OPENROUTER_API_KEY <key> # Windows, restart shell after setx

aider --model openrouter/anthropic/claude-3.7-sonnet
```


{: .tip }
If you get errors, check your
[OpenRouter privacy settings](https://openrouter.ai/settings/privacy).
Be sure to "enable providers that may train on inputs"
to allow use of all models.

## Controlling provider selection

OpenRouter often has multiple providers serving each model.
You can control which OpenRouter providers are used for your requests in two ways:

1. By "ignoring" certain providers in your
[OpenRouter account settings](https://openrouter.ai/settings/preferences).
This disables those named providers across all the models that you access via OpenRouter.

2. By configuring "provider routing" in a `.aider.model.settings.yml` file.

Place that file in your home directory or the root of your git project, with
entries like this:

```yaml
- name: openrouter/anthropic/claude-3.7-sonnet
  extra_params:
    extra_body:
      provider:
        # Only use these providers, in this order
        order: ["Anthropic", "Together"]
        # Don't fall back to other providers
        allow_fallbacks: false
        # Skip providers that may train on inputs
        data_collection: "deny"
        # Only use providers supporting all parameters
        require_parameters: true
```

See [OpenRouter's provider routing docs](https://openrouter.ai/docs/provider-routing) for full details on these settings.

See [Advanced model settings](https://aider.chat/docs/config/adv-model-settings.html#model-settings)
for more details about model settings files.
`````

## File: llms/other.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 800
---

# Other LLMs

Aider uses the [litellm](https://docs.litellm.ai/docs/providers) package
to connect to hundreds of other models.
You can use `aider --model <model-name>` to use any supported model.

To explore the list of supported models you can run `aider --list-models <model-name>`
with a partial model name.
If the supplied name is not an exact match for a known model, aider will
return a list of possible matching models.
For example:

```
$ aider --list-models turbo

Aider v0.29.3-dev
Models which match "turbo":
- gpt-4-turbo-preview (openai/gpt-4-turbo-preview)
- gpt-4-turbo (openai/gpt-4-turbo)
- gpt-4-turbo-2024-04-09 (openai/gpt-4-turbo-2024-04-09)
- gpt-3.5-turbo (openai/gpt-3.5-turbo)
- ...
```

See the [model warnings](warnings.html)
section for information on warnings which will occur
when working with models that aider is not familiar with.

## LiteLLM

Aider uses the LiteLLM package to connect to LLM providers.
The [LiteLLM provider docs](https://docs.litellm.ai/docs/providers)
contain more detail on all the supported providers,
their models and any required environment variables.


## Other API key variables

Here are the API key environment variables that are supported
by litellm. See their docs for more info.

<!--[[[cog
from subprocess import run
lines = run(
    "egrep -ho '[A-Z_]+_API_KEY' ../litellm/litellm/*py | sort -u",
    shell=True,
    capture_output=True,
    text=True,
    ).stdout
lines = ['- ' + line for line in lines.splitlines(keepends=True)]
cog.out(''.join(lines))
]]]-->
- ALEPHALPHA_API_KEY
- ALEPH_ALPHA_API_KEY
- ANTHROPIC_API_KEY
- ANYSCALE_API_KEY
- AZURE_AI_API_KEY
- AZURE_API_KEY
- AZURE_OPENAI_API_KEY
- BASETEN_API_KEY
- CEREBRAS_API_KEY
- CLARIFAI_API_KEY
- CLOUDFLARE_API_KEY
- CODESTRAL_API_KEY
- COHERE_API_KEY
- CO_API_KEY
- DATABRICKS_API_KEY
- DEEPINFRA_API_KEY
- DEEPSEEK_API_KEY
- FIREWORKSAI_API_KEY
- FIREWORKS_AI_API_KEY
- FIREWORKS_API_KEY
- GEMINI_API_KEY
- GROQ_API_KEY
- HUGGINGFACE_API_KEY
- MARITALK_API_KEY
- MISTRAL_API_KEY
- NLP_CLOUD_API_KEY
- NVIDIA_NIM_API_KEY
- OLLAMA_API_KEY
- OPENAI_API_KEY
- OPENAI_LIKE_API_KEY
- OPENROUTER_API_KEY
- OR_API_KEY
- PALM_API_KEY
- PERPLEXITYAI_API_KEY
- PREDIBASE_API_KEY
- PROVIDER_API_KEY
- REPLICATE_API_KEY
- TOGETHERAI_API_KEY
- VOLCENGINE_API_KEY
- VOYAGE_API_KEY
- WATSONX_API_KEY
- WX_API_KEY
- XAI_API_KEY
- XINFERENCE_API_KEY
<!--[[[end]]]-->
`````

## File: llms/vertex.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 550
---

# Vertex AI

Aider can connect to models provided by Google Vertex AI.
You will need to install the
[gcloud CLI](https://cloud.google.com/sdk/docs/install) and [login](https://cloud.google.com/sdk/docs/initializing) with a GCP account
or service account with permission to use the Vertex AI API.

With your chosen login method, the gcloud CLI should automatically set the
`GOOGLE_APPLICATION_CREDENTIALS` environment variable which points to the credentials file.

To configure Aider to use the Vertex AI API, you need to set `VERTEXAI_PROJECT` (the GCP project ID)
and `VERTEXAI_LOCATION` (the GCP region) [environment variables for Aider](/docs/config/dotenv.html).

Note that Claude on Vertex AI is only available in certain GCP regions, 
check [the model card](https://console.cloud.google.com/vertex-ai/publishers/anthropic/model-garden/claude-3-5-sonnet) 
for your model to see which regions are supported.

Example `.env` file:

```
VERTEXAI_PROJECT=my-project
VERTEXAI_LOCATION=us-east5
```

Then you can run aider with the `--model` command line switch, like this:

```
aider --model vertex_ai/claude-3-5-sonnet@20240620
```

Or you can use the [yaml config](/docs/config/aider_conf.html) to set the model to any of the 
models supported by Vertex AI.

Example `.aider.conf.yml` file:

```yaml
model: vertex_ai/claude-3-5-sonnet@20240620
```
`````

## File: llms/warnings.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 900
---

# Model warnings

{% include model-warnings.md %}
`````

## File: llms/xai.md
`````markdown
---
parent: Connecting to LLMs
nav_order: 400
---

# xAI

You'll need a [xAI API key](https://console.x.ai.).

To use xAI:

```
python -m pip install -U aider-chat

export XAI_API_KEY=<key> # Mac/Linux
setx   XAI_API_KEY <key> # Windows, restart shell after setx

aider --model xai/grok-beta

# List models available from xAI
aider --list-models xai/
```
`````

## File: troubleshooting/aider-not-found.md
`````markdown
---
parent: Troubleshooting
nav_order: 28
---

# Aider not found

In some environments the `aider` command may not be available
on your shell path.
This can occur because of permissions/security settings in your OS,
and often happens to Windows users.

You may see an error message like this:

> aider: The term 'aider' is not recognized as a name of a cmdlet, function, script file, or executable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.

Below is the most fail safe way to run aider in these situations:

```
python -m aider
```

You should also consider 
[installing aider using aider-install, uv or pipx](/docs/install.html).
`````

## File: troubleshooting/edit-errors.md
`````markdown
---
parent: Troubleshooting
nav_order: 10
---

# File editing problems

Sometimes the LLM will reply with some code changes
that don't get applied to your local files.
In these cases, aider might say something like "Failed to apply edit to *filename*"
or other error messages.

This usually happens because the LLM is disobeying the system prompts
and trying to make edits in a format that aider doesn't expect.
Aider makes every effort to get the LLM
to conform, and works hard to deal with
LLM edits that are "almost" correctly formatted.

But sometimes the LLM just won't cooperate.
In these cases, here are some things you might try.

## Don't add too many files

Many LLMs now have very large context windows,
but filling them with irrelevant code or conversation 
can confuse the model.
Above about 25k tokens of context, most models start to become distracted and become less likely
to conform to their system prompt.

- Don't add too many files to the chat, *just* add the files you think need to be edited.
Aider also sends the LLM a [map of your entire git repo](https://aider.chat/docs/repomap.html), so other relevant code will be included automatically.
- Use `/drop` to remove files from the chat session which aren't needed for the task at hand. This will reduce distractions and may help the LLM produce properly formatted edits.
- Use `/clear` to remove the conversation history, again to help the LLM focus.
- Use `/tokens` to see how many tokens you are using for each message.

## Use a more capable model

If possible try using GPT-4o, o3-mini, Claude 3.7 Sonnet, DeepSeek V3 or DeepSeek R1.
They are the strong and capable models.

Weaker models
are more prone to
disobeying the system prompt instructions.
Most local models are just barely capable of working with aider,
so editing errors are probably unavoidable.

## Local models: context window and quantization

Be especially careful about the
[Ollama context window](https://aider.chat/docs/llms/ollama.html#setting-the-context-window-size)
when working with local models.
It defaults to be very small and silently discards data if you exceed it.

Local models which have been quantized are more likely to have editing problems
because they are not capable enough to follow aider's system prompts.

## Try the whole edit format

Run aider with `--edit-format whole` if were using a different edit format.
You can see which edit format it is using in the announce lines:

```
Aider v0.50.2-dev
Models: claude-3-5-sonnet-20240620 with ♾️ diff edit format
```

## Try architect mode

Run aider with `--architect` or `/chat-mode architect` to enable [architect mode](../usage/modes.md#architect-mode-and-the-editor-model).
This mode first proposes changes, then uses a separate model to handle the file edits.
This two-step process often produces more reliable edits, especially with models that have trouble
following edit format instructions.

## More help

{% include help.md %}
`````

## File: troubleshooting/imports.md
`````markdown
---
parent: Troubleshooting
nav_order: 28
---

# Dependency versions

Aider expects to be installed with the
correct versions of all of its required dependencies.

If you've been linked to this doc from a GitHub issue, 
or if aider is reporting `ImportErrors`
it is likely that your
aider install is using incorrect dependencies.


## Avoid package conflicts

If you are using aider to work on a python project, sometimes your project will require
specific versions of python packages which conflict with the versions that aider
requires.
If this happens, you may see errors like these when running pip installs:

```
aider-chat 0.23.0 requires somepackage==X.Y.Z, but you have somepackage U.W.V which is incompatible.
```

## Install with aider-install, uv or pipx

If you are having dependency problems you should consider
[installing aider using aider-install, uv or pipx](/docs/install.html).
This will ensure that aider is installed in its own python environment,
with the correct set of dependencies.

## Package managers like Homebrew, AUR, ports

Package managers often install aider with the wrong dependencies, leading
to import errors and other problems.

It is recommended to
[install aider using aider-install, uv or pipx](/docs/install.html).


## Dependency versions matter

Aider pins its dependencies and is tested to work with those specific versions.
If you are installing aider directly with pip
you should be careful about upgrading or downgrading the python packages that
aider uses.

In particular, be careful with the packages with pinned versions 
noted at the end of
[aider's requirements.in file](https://github.com/Aider-AI/aider/blob/main/requirements/requirements.in).
These versions are pinned because aider is known not to work with the
latest versions of these libraries.

Also be wary of upgrading `litellm`, as it changes versions frequently
and sometimes introduces bugs or backwards incompatible changes.

## Replit

{% include replit-pipx.md %}
`````

## File: troubleshooting/models-and-keys.md
`````markdown
---
parent: Troubleshooting
nav_order: 28
---

# Models and API keys

You need to tell aider which LLM to use and provide an API key.
The easiest way is to use the `--model` and `--api-key`
command line arguments, like this:

```
# Work with DeepSeek via DeepSeek's API
aider --model deepseek --api-key deepseek=your-key-goes-here

# Work with Claude 3.7 Sonnet via Anthropic's API
aider --model sonnet --api-key anthropic=your-key-goes-here

# Work with o3-mini via OpenAI's API
aider --model o3-mini --api-key openai=your-key-goes-here

# Work with Sonnet via OpenRouter's API
aider --model openrouter/anthropic/claude-3.7-sonnet --api-key openrouter=your-key-goes-here

# Work with DeepSeek Chat V3 via OpenRouter's API
aider --model openrouter/deepseek/deepseek-chat --api-key openrouter=your-key-goes-here
```

For more information, see the documentation sections:

- [Connecting to LLMs](https://aider.chat/docs/llms.html)
- [Configuring API keys](https://aider.chat/docs/config/api-keys.html)
`````

## File: troubleshooting/support.md
`````markdown
---
parent: Troubleshooting
excerpt: Use "/help <question>" to ask for help about using aider, customizing settings, troubleshooting, using LLMs, etc.
nav_order: 30
---

# Using /help

Type `/help <question>` and
aider will respond with helpful information.
You can ask questions about using aider, 
customizing settings, troubleshooting, using LLMs, etc.

Behind the scenes, aider has indexed all of its
[documentation](https://aider.chat/).
It uses the question to find relevant docs
and share them with the LLM to do
retrieval augmented generation (RAG)
The LLM also has the full context of your ongoing
aider chat,
so it can help with the exact issue you might be facing.


<div class="chat-transcript" markdown="1">

> Aider v0.42.1-dev  
> Models: claude-3-5-sonnet with diff edit format  
> Repo-map: using 1024 tokens  
> Use /help \<question\> to ask for help, run with --help to see cmd line args  

#### /help is the .env file global or for a git repo?

Based on the provided documentation, here's the answer to your question about the .env file in aider:

The .env file in aider can be both global and specific to a git repository. Aider looks for .env files in multiple locations, with the following priority order:

1. Your home directory (global)
2. The root of your git repo (repo-specific)
3. The current directory
4. A file specified with the `--env-file <filename>` parameter

If multiple .env files exist in these locations, they are loaded in the order listed above, with files loaded last taking priority. This means you can have a global .env file in your home directory with general settings, and then override or add to those settings with a repo-specific .env file in your git repository root.

Here are some relevant documentation links that you might find helpful:

- https://aider.chat/docs/config.html
- https://aider.chat/docs/config/dotenv.html

The ability to use both global and repo-specific .env files allows for flexible configuration, where you can set default values globally and override them on a per-project basis as needed.

</div>
<div class="chat-transcript" markdown="1">

#### /help can i use local LLMs?

Yes, you can use local LLMs with Aider. Here's a summary of the options:

1. Ollama: Aider supports using local models through Ollama.

2. OpenAI-compatible API: Aider can access local models that provide an OpenAI-compatible API.

3. Other local models: Aider uses the LiteLLM package to connect to various LLM providers, which may include options for running models locally.

However, it's important to note that Aider works best with more capable models. Less capable models may have difficulty properly returning "code edits," which could result in Aider not being able to edit your files and commit changes effectively.

For more detailed information on using local LLMs and connecting to various models, you might want to check out these relevant documentation pages:

- https://aider.chat/docs/llms.html
- https://aider.chat/docs/llms/ollama.html
- https://aider.chat/docs/llms/openai-compat.html

Keep in mind that the performance and capabilities of local LLMs may vary depending on the specific model and your hardware resources.

</div>


## Getting more help

{% include help.md %}
`````

## File: troubleshooting/token-limits.md
`````markdown
---
parent: Troubleshooting
nav_order: 25
---

# Token limits

Every LLM has limits on how many tokens it can process for each request:

- The model's **context window** limits how many total tokens of
*input and output* it can process.
- Each model has limit on how many **output tokens** it can
produce.

Aider will report an error **if a model responds** indicating that
it has exceeded a token limit.
The error will include suggested actions to try and
avoid hitting token limits.

Here's an example error: 

```
Model gpt-3.5-turbo has hit a token limit!

Input tokens: 768 of 16385
Output tokens: 4096 of 4096 -- exceeded output limit!
Total tokens: 4864 of 16385

To reduce output tokens:
- Ask for smaller changes in each request.
- Break your code into smaller source files.
- Try using a stronger model like DeepSeek V3 or Sonnet that can return diffs.

For more info: https://aider.chat/docs/token-limits.html
```

{: .note }
Aider never *enforces* token limits, it only *reports* token limit errors
from the API provider.
The token counts that aider reports are *estimates*.

## Input tokens & context window size

The most common problem is trying to send too much data to a 
model,
overflowing its context window.
Technically you can exhaust the context window if the input is
too large or if the input plus output are too large.

Strong models like GPT-4o and Sonnet have quite
large context windows, so this sort of error is
typically only an issue when working with weaker models.

The easiest solution is to try and reduce the input tokens
by removing files from the chat.
It's best to only add the files that aider will need to *edit*
to complete your request.

- Use `/tokens` to see token usage.
- Use `/drop` to remove unneeded files from the chat session.
- Use `/clear` to clear the chat history.
- Break your code into smaller source files.

## Output token limits

Most models have quite small output limits, often as low
as 4k tokens.
If you ask aider to make a large change that affects a lot
of code, the LLM may hit output token limits
as it tries to send back all the changes.

To avoid hitting output token limits:

- Ask for smaller changes in each request.
- Break your code into smaller source files.
- Use a strong model like gpt-4o, sonnet or DeepSeek V3 that can return diffs.
- Use a model that supports [infinite output](/docs/more/infinite-output.html).

## Other causes

Sometimes token limit errors are caused by 
non-compliant API proxy servers
or bugs in the API server you are using to host a local model.
Aider has been well tested when directly connecting to 
major 
[LLM provider cloud APIs](https://aider.chat/docs/llms.html).
For serving local models, 
[Ollama](https://aider.chat/docs/llms/ollama.html) is known to work well with aider.

Try using aider without an API proxy server
or directly with one of the recommended cloud APIs
and see if your token limit problems resolve.

## More help

{% include help.md %}
`````

## File: troubleshooting/warnings.md
`````markdown
---
parent: Troubleshooting
nav_order: 20
---

# Model warnings

{% include model-warnings.md %}

## More help

{% include help.md %}
`````

## File: usage/browser.md
`````markdown
---
title: Aider in your browser
highlight_image: /assets/browser.jpg
parent: Usage
nav_order: 800
description: Aider can run in your browser, not just on the command line.
---
{% if page.date %}
<p class="post-date">{{ page.date | date: "%B %d, %Y" }}</p>
{% endif %}

# Aider in your browser

<div class="video-container">
  <video controls loop poster="/assets/browser.jpg">
    <source src="/assets/aider-browser-social.mp4" type="video/mp4">
    <a href="/assets/aider-browser-social.mp4">Aider browser UI demo video</a>
  </video>
</div>

<style>
.video-container {
  position: relative;
  padding-bottom: 101.89%; /* 1080 / 1060 = 1.0189 */
  height: 0;
  overflow: hidden;
}

.video-container video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>

Use aider's new experimental browser UI to collaborate with LLMs
to edit code in your local git repo.
Aider will directly edit the code in your local source files,
and [git commit the changes](https://aider.chat/docs/git.html)
with sensible commit messages.
You can start a new project or work with an existing git repo.
Aider works well with 
GPT-4o, Sonnet 3.7, and DeepSeek Chat V3 & R1.
It also supports [connecting to almost any LLM](https://aider.chat/docs/llms.html).

Use the `--browser` switch to launch the browser version of aider:

```
python -m pip install -U aider-chat

export OPENAI_API_KEY=<key> # Mac/Linux
setx   OPENAI_API_KEY <key> # Windows, restart shell after setx

aider --browser
```
`````

## File: usage/caching.md
`````markdown
---
title: Prompt caching
highlight_image: /assets/prompt-caching.jpg
parent: Usage
nav_order: 750
description: Aider supports prompt caching for cost savings and faster coding.
---

# Prompt caching

Aider supports prompt caching for cost savings and faster coding.
Currently Anthropic provides caching for Sonnet and Haiku,
and DeepSeek provides caching for Chat.

Aider organizes the chat history to try and cache:

- The system prompt.
- Read only files added with `--read` or `/read-only`.
- The repository map.
- The editable files that have been added to the chat.

![Prompt caching](/assets/prompt-caching.jpg)


## Usage

Run aider with `--cache-prompts` or add that setting to your 
[configuration files](/docs/config.html).

Due to limitations in the provider APIs, caching statistics and costs
are not available when streaming responses.
To turn off streaming, use `--no-stream`.

When caching is enabled, it will be noted for the main model when aider launches:

```
Main model: claude-3-5-sonnet-20240620 with diff edit format, prompt cache, infinite output
```

## Preventing cache expiration

Aider can ping the provider to keep your prompt cache warm and prevent
it from expiring.
By default, Anthropic keeps your cache for 5 minutes.
Use `--cache-keepalive-pings N` to tell aider to ping
every 5 minutes to keep the cache warm.
Aider will ping up to `N` times over a period of `N*5` minutes
after each message you send.
`````

## File: usage/commands.md
`````markdown
---
parent: Usage
nav_order: 50
description: Control aider with in-chat commands like /add, /model, etc.
---

# In-chat commands
{: .no_toc }

- TOC
{:toc}

## Slash commands

Aider supports commands from within the chat, which all start with `/`.

<!--[[[cog
from aider.commands import get_help_md
cog.out(get_help_md())
]]]-->

|Command|Description|
|:------|:----------|
| **/add** | Add files to the chat so aider can edit them or review them in detail |
| **/architect** | Enter architect/editor mode using 2 different models. If no prompt provided, switches to architect/editor mode. |
| **/ask** | Ask questions about the code base without editing any files. If no prompt provided, switches to ask mode. |
| **/chat-mode** | Switch to a new chat mode |
| **/clear** | Clear the chat history |
| **/code** | Ask for changes to your code. If no prompt provided, switches to code mode. |
| **/commit** | Commit edits to the repo made outside the chat (commit message optional) |
| **/copy** | Copy the last assistant message to the clipboard |
| **/copy-context** | Copy the current chat context as markdown, suitable to paste into a web UI |
| **/diff** | Display the diff of changes since the last message |
| **/drop** | Remove files from the chat session to free up context space |
| **/editor** | Open an editor to write a prompt |
| **/exit** | Exit the application |
| **/git** | Run a git command (output excluded from chat) |
| **/help** | Ask questions about aider |
| **/lint** | Lint and fix in-chat files or all dirty files if none in chat |
| **/load** | Load and execute commands from a file |
| **/ls** | List all known files and indicate which are included in the chat session |
| **/map** | Print out the current repository map |
| **/map-refresh** | Force a refresh of the repository map |
| **/model** | Switch to a new LLM |
| **/models** | Search the list of available models |
| **/multiline-mode** | Toggle multiline mode (swaps behavior of Enter and Meta+Enter) |
| **/paste** | Paste image/text from the clipboard into the chat.        Optionally provide a name for the image. |
| **/quit** | Exit the application |
| **/read-only** | Add files to the chat that are for reference only, or turn added files to read-only |
| **/reasoning-effort** | Set the reasoning effort level (values: number or low/medium/high depending on model) |
| **/report** | Report a problem by opening a GitHub Issue |
| **/reset** | Drop all files and clear the chat history |
| **/run** | Run a shell command and optionally add the output to the chat (alias: !) |
| **/save** | Save commands to a file that can reconstruct the current chat session's files |
| **/settings** | Print out the current settings |
| **/test** | Run a shell command and add the output to the chat on non-zero exit code |
| **/think-tokens** | Set the thinking token budget (supports formats like 8096, 8k, 10.5k, 0.5M) |
| **/tokens** | Report on the number of tokens used by the current chat context |
| **/undo** | Undo the last git commit if it was done by aider |
| **/voice** | Record and transcribe voice input |
| **/web** | Scrape a webpage, convert to markdown and send in a message |

<!--[[[end]]]-->

{: .tip }
You can easily re-send commands or messages.
Use the up arrow ⬆ to scroll back
or CONTROL-R to search your message history.

## Entering multi-line chat messages

{% include multi-line.md %}

## Interrupting with CONTROL-C

It's always safe to use Control-C to interrupt aider if it isn't providing a useful response. The partial response remains in the conversation, so you can refer to it when you reply to the LLM with more information or direction.

## Keybindings

The interactive prompt is built with [prompt-toolkit](https://github.com/prompt-toolkit/python-prompt-toolkit) which provides emacs and vi keybindings. 

### Emacs

- `Up Arrow` : Move up one line in the current message.
- `Down Arrow` : Move down one line in the current message.
- `Ctrl-Up` : Scroll back through previously sent messages.
- `Ctrl-Down` : Scroll forward through previously sent messages.
- `Ctrl-A` : Move cursor to the start of the line.
- `Ctrl-B` : Move cursor back one character.
- `Ctrl-D` : Delete the character under the cursor.
- `Ctrl-E` : Move cursor to the end of the line.
- `Ctrl-F` : Move cursor forward one character.
- `Ctrl-K` : Delete from the cursor to the end of the line.
- `Ctrl-L` : Clear the screen.
- `Ctrl-N` : Move down to the next history entry.
- `Ctrl-P` : Move up to the previous history entry.
- `Ctrl-R` : Reverse search in command history.


### Vi

To use vi/vim keybindings, run aider with the `--vim` switch.

- `Up Arrow` : Move up one line in the current message.
- `Down Arrow` : Move down one line in the current message.
- `Ctrl-Up` : Scroll back through previously sent messages.
- `Ctrl-Down` : Scroll forward through previously sent messages.
- `Esc` : Switch to command mode.
- `i` : Switch to insert mode.
- `a` : Move cursor one character to the right and switch to insert mode.
- `A` : Move cursor to the end of the line and switch to insert mode.
- `I` : Move cursor to the beginning of the line and switch to insert mode.
- `h` : Move cursor one character to the left.
- `j` : Move cursor down one line.
- `k` : Move cursor up one line.
- `l` : Move cursor one character to the right.
- `w` : Move cursor forward one word.
- `b` : Move cursor backward one word.
- `0` : Move cursor to the beginning of the line.
- `$` : Move cursor to the end of the line.
- `x` : Delete the character under the cursor.
- `dd` : Delete the current line.
- `u` : Undo the last change.
- `Ctrl-R` : Redo the last undone change.
`````

## File: usage/conventions.md
`````markdown
---
parent: Usage
nav_order: 800
description: Tell aider to follow your coding conventions when it works on your code.
---
# Specifying coding conventions

Sometimes you want GPT to be aware of certain coding guidelines,
like whether to provide type hints, which libraries or packages
to prefer, etc.

The easiest way to do that with aider is to simply create
a small markdown file and include it in the chat.

For example, say we want our python code to:

```
- Prefer httpx over requests for making http requests.
- Use types everywhere possible.
```

We would simply create a file like `CONVENTIONS.md` with those lines
and then we can add it to the aider chat, along with the file(s)
that we want to edit.

It's best to load the conventions file with `/read CONVENTIONS.md` 
or `aider --read CONVENTIONS.md`. 
This way it is marked as read-only, and cached if prompt caching
is enabled.

## Community contributed conventions

You can check the [aider conventions repository](https://github.com/Aider-AI/conventions)
to find or contribute conventions files.

## Always load conventions

You can also configure aider to always load your conventions file
in the [`.aider.conf.yml` config file](https://aider.chat/docs/config/aider_conf.html):


```yaml
# alone
read: CONVENTIONS.md

# multiple files
read: [CONVENTIONS.md, anotherfile.txt]
```


## Example

See below for an example of how the conventions can affect the code
that aider writes.

<div class="chat-transcript" markdown="1">
> $ aider --read CONVENTIONS.md useragent.py  
>  
> Aider v0.24.2-dev  
> Added CONVENTIONS.md to the chat.  
> Added useragent.py to the chat.  

#### write a function that uses httpbin to check the user agent on the request. return the ua.  

To fulfill your request, I will:

1. Add an import for `httpx`.
2. Write a function named `check_user_agent` that makes a GET request to `httpbin.org/user-agent` and returns the user agent string.

Here is the diff for the `useragent.py` file:

```diff
--- useragent.py
+++ useragent.py
@@ ... @@
+import httpx
+
+def check_user_agent() -> str:
+    response = httpx.get('https://httpbin.org/user-agent')
+    return response.json()['user-agent']
```

This function uses `httpx` to make a GET request to `httpbin.org/user-agent`, which returns a JSON response containing the user agent of the HTTP client. The function then extracts the user agent string from the JSON response and returns it.

> Applied edit to useragent.py  
</div>

GPT correctly used `httpx` and provided type hints.
See below for the transcript of this same
coding task conducted *without*
using `CONVENTIONS.md`.
In this case, GPT used `requests` and skipped types,
which is perhaps more typical in small python scripts.

<div class="chat-transcript" markdown="1">
> $ aider useragent.py  
>  
> Aider v0.24.2-dev  
> Added useragent.py to the chat.  


#### write a function that uses httpbin to check the user agent on the request. return the ua.  

```diff
--- /dev/null
+++ useragent.py
@@ ... @@
+import requests
+
+def check_user_agent():
+    response = requests.get('https://httpbin.org/user-agent')
+    user_agent = response.json().get('user-agent')
+    return user_agent
```

> Applied edit to useragent.py  
</div>
`````

## File: usage/copypaste.md
`````markdown
---
title: Copy/paste with web chat
#highlight_image: /assets/browser.jpg
parent: Usage
nav_order: 850
description: Aider works with LLM web chat UIs
---

# Copy/paste with web chat

<div class="video-container">
  <video controls loop poster="/assets/copypaste.jpg">
    <source src="/assets/copypaste.mp4" type="video/mp4">
    <a href="/assets/copypaste.mp4">Aider browser UI demo video</a>
  </video>
</div>

<style>
.video-container {
  position: relative;
  padding-bottom: 66.34%; /* 2160 / 3256 = 0.6634 */
  height: 0;
  overflow: hidden;
}

.video-container video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>

## Working with an LLM web chat

[Aider can connect to most LLMs via API](https://aider.chat/docs/llms.html) and works best that way.
But there are times when you may want to work with an LLM via its web chat interface:

- Workplace policies may limit your LLM usage to a proprietary web chat system.
- The web chat LLM may have access to unique context or may have been specially fine tuned for your task.
- It may be cost prohibitive to use some models via API.
- There may not be an API available.

Aider has features for working with an LLM via its web chat interface.
This allows you to use the web chat LLM as the "big brain code architect"
while running aider with a smaller, cheaper LLM to actually make changes
to your local files.

For this "file editor" part of the process 
you can run aider with many open source, free or very inexpensive LLMs.
For example, the demo video above shows aider using DeepSeek to apply the changes
that o1-preview is suggesting in the web chat.

### Copy aider's code context to your clipboard, paste into the web UI

The `/copy-context <instructions>` command can be used in chat to copy aider's code context to your clipboard.
It will include:

- All the files which have been added to the chat via `/add`.
- Any read only files which have been added via `/read`.
- Aider's [repository map](https://aider.chat/docs/repomap.html) that brings in code context related to the above files from elsewhere in your git repo.
- Some instructions to the LLM that ask it to output change instructions concisely.
- If you include `<instructions>`, they will be copied too.

You can paste the context into your browser, and start interacting with the LLM web chat to
ask for code changes.

### Paste the LLM's reply back into aider to edit your files

Once the LLM has replied, you can use the "copy response" button in the web UI to copy
the LLM's response.
Back in aider, you can run `/paste` and aider will edit your files
to implement the changes suggested by the LLM.

You can use a cheap, efficient model like GPT-4o Mini, DeepSeek or Qwen to do these edits.
This works best if you run aider with `--edit-format editor-diff` or `--edit-format editor-whole`.

### Copy/paste mode

Aider has a `--copy-paste` mode that streamlines this entire process:

- Whenever you `/add` or `/read` files, aider will automatically copy the entire, updated
code context to your clipboard. 
You'll see "Copied code context to clipboard" whenever this happens.
- When you copy the LLM reply to your clipboard outside aider, aider will automatically notice
and load it into the aider chat. 
Just press ENTER to send the message
and aider will apply the LLMs changes to your local files.
- Aider will automatically select the best edit format for this copy/paste functionality. 
Depending on the LLM you have aider use, it will be either `editor-whole` or `editor-diff`.

## Terms of service

Be sure to review the Terms Of Service of any LLM web chat service you use with
these features.
These features are not intended to be used in violation of any service's Terms Of Service (TOS).

Aider's web chat features have been designed to be compliant with the 
terms of service of most LLM web chats.

There are 4 copy/paste steps involved when coding with an LLM web chat:

1. Copy code and context from aider.
2. Paste the code and context into the LLM web chat.
3. Copy the reply from the LLM web chat.
4. Paste the LLM reply into aider.

Most LLM web chat TOS prohibit automating steps (2) and (3) where code
is copied from and pasted into the web chat.
Aider's `--copy-paste` mode leaves those as 100% manual steps for the user to complete.
It simply streamlines steps (1) and (4) that are interactions with aider,
and which should not be under the scope of an LLM web chat TOS.

If you are concerned that
the automatic interactions with aider in steps (1) and (4) may be problematic with respect to
your LLM web chat provider's TOS, you can forego `--copy-paste` mode.
Instead, manually use the `/copy-context` and `/paste` commands if that
will keep you in compliance.

Again, do not use these features in violation of any service's Terms Of Service.
`````

## File: usage/images-urls.md
`````markdown
---
parent: Usage
nav_order: 700
description: Add images and web pages to the aider coding chat.
---

# Images & web pages

You can add images and URLs to the aider chat.

## Images

Aider supports working with image files for many vision-capable models
like GPT-4o and Claude 3.7 Sonnet.
Adding images to a chat can be helpful in many situations:

- Add screenshots of web pages or UIs that you want aider to build or modify.
- Show aider a mockup of a UI you want to build.
- Screenshot an error message that is otherwise hard to copy & paste as text.
- Etc.

You can add images to the chat just like you would
add any other file:

- Use `/add <image-filename>` from within the chat
- Use `/paste` to paste an image from your clipboard into the chat.
- Launch aider with image filenames on the command line: `aider <image-filename>` along with any other command line arguments you need.

## Web pages

Aider can scrape the text from URLs and add it to the chat.
This can be helpful to:

- Include documentation pages for less popular APIs.
- Include the latest docs for libraries or packages that are newer than the model's training cutoff date.
- Etc.

To add URLs to the chat:

- Use `/web <url>`
- Just paste the URL into the chat and aider will ask if you want to add it.

You can also scrape web pages from the command line to see the markdown version that aider produces:


```
python -m aider.scrape https://aider.chat/docs/usage/tips.html
```
`````

## File: usage/lint-test.md
`````markdown
---
parent: Usage
nav_order: 900
description: Automatically fix linting and testing errors.
---

# Linting and testing

Aider can automatically lint and test your code
every time it makes changes.
This helps identify and repair any problems introduced
by the AI edits.

## Linting

Aider comes with built in linters for 
[most popular languages](/docs/languages.html)
and will automatically lint code in these languages.

Or you can specify your favorite linter
with the `--lint-cmd <cmd>` switch.
The lint command should accept the filenames
of the files to lint. 
If there are linting errors, aider expects the
command to print them on stdout/stderr
and return a non-zero exit code.
This is how most linters normally operate.

By default, aider will lint any files which it edits.
You can disable this with the `--no-auto-lint` switch.

### Per-language linters

To specify different linters based on the code language, use `--lint "language: cmd"`.

### Code formatting "linters"

Many people use code formatters as linters, to format and pretty their code.
These tools sometimes return non-zero exit codes if they make changes, which will
confuse aider into thinking there's an actual lint error that needs to be fixed.

You can use formatters by wrapping them in a shell script like this and setting
the script as your linter.

```bash
#!/bin/bash

# Run it twice.
#
# First attempt may reformat/modify files, and therefore exit with non-zero status.
#
# Second attempt will not do anything and exit 0 unless there's a real problem beyond
# the code formatting that was completed.

pre-commit run --files $* >/dev/null \
    || pre-commit run --files $*
```

## Testing

You can run tests with `/test <test-command>`.
Aider will run the test command without any arguments.
If there are test errors, aider expects the
command to print them on stdout/stderr
and return a non-zero exit code.

Aider will try and fix any errors
if the command returns a non-zero exit code.

You can configure aider to run your test suite
after each time the AI edits your code
using the `--test-cmd <test-command>` and
`--auto-test` switch.



## Compiled languages

If you want to have aider compile code after each edit, you
can use the lint and test commands to achieve this.

- You might want to recompile each file which was modified
to check for compile errors.
To do this,
provide a `--lint-cmd` which both lints and compiles the file.
You could create a small shell script for this.
- You might want to rebuild the entire project after files
are edited to check for build errors.
To do this,
provide a `--test-cmd` which both builds and tests the project.
You could create a small shell script for this.
Or you may be able to do something as simple as
`--test-cmd "dotnet build && dotnet test"`.

## Manually running code

You can use the `/run` command in the chat to run your code
and optionally share the output with aider.
This can be useful to share error messages or to show aider
the code's output before asking for changes or corrections.

<div class="chat-transcript" markdown="1">
> Aider v0.43.5-dev  

#### /run python myscript.py

```
Traceback (most recent call last):  
 File "myscript.py", line 22, in \<module\ 
   raise ValueError("something bad happened")  
ValueError: something bad happened  
```

> Add the output to the chat? y  

</div>
`````

## File: usage/modes.md
`````markdown
---
parent: Usage
nav_order: 60
description: Using the code, architect, ask and help chat modes.
---

# Chat modes

Aider has a few different chat modes:

- `code` - Aider will make changes to your code to satisfy your requests.
- `architect` - Aider will first propose a solution, then ask if you want it to turn that proposal into edits to your files.
- `ask` - Aider will answer questions about your code, but never edit it.
- `help` - Aider will answer questions about using aider, configuring, troubleshooting, etc.

By default, aider starts in "code" mode. As you are talking, you can
send individual messages in a specific mode using 
`/code`, `/architect`, `/ask`, and `/help` commands:
Using these `/`-commands applies just to that particular message.
Your next message will go back to the active mode (usually "code" mode by default).

You can switch the active mode in a sticky way
with the `/chat-mode <mode>` command:

```
/chat-mode code
/chat-mode architect
/chat-mode ask
/chat-mode help
```

Or you can switch between coding modes using these commands without arguments:

```
/code
/architect
/ask
```

Or you can launch aider in one of the modes with the `--chat-mode <mode>` switch.
There is also a special shortcut `--architect` to launch in `--chat-mode architect`.

## Architect mode and the editor model

When you are in architect mode, aider sends your request to two models:

1. First, it sends your request to the main active model.
The main model is configured with `/model`, `--model` or the shortcut switches like `--sonnet`. 
After the main model replies, aider will offer to edit the files based on the response.

2. To edit the files, aider sends a second LLM request asking for specific code editing instructions.
This request goes to the "editor" model.
Aider has built in defaults to select an editor model based on your main model.
Or, you can choose an editor model yourself with `--editor-model <model>`.

Architect mode produces better results than code mode, but uses two LLM requests.
This probably makes it slower and more expensive than using code mode.

Architect mode is especially useful with OpenAI's o1 models, which are strong at
reasoning but less capable at editing files.
Pairing an o1 architect with an editor model like GPT-4o or Sonnet will
give the best results.

But architect mode is also quite helpful when you use GPT-4o or Sonnet
at both the architect and the editor.
Allowing the model two requests to solve the problem and edit the files
usually provides a better result.

The editor model uses one of aider's edit formats to let the LLM
edit source files.
Aider will pick a suitable edit format by default,
but you can customize it with `--editor-edit-format <format>`.
The 
["editor-diff" and "editor-whole" edit formats](/docs/more/edit-formats.html#editor-diff-and-editor-whole)
are the recommended edit formats when using architect mode.
See this article on 
[aider's architect/editor mode](/2024/09/26/architect.html)
for more details.


## Examples

<div class="chat-transcript" markdown="1">


> Aider v0.47.1-dev  
> Models: claude-3-5-sonnet-20240620 with diff edit format  
> Git repo: .git with 298 files  
> Repo-map: using 1024 tokens  
> Use /help to see in-chat commands, run with --help to see cmd line args  

#### /ask What is this repo?

This is the source code to the popular django package.

#### /help How do I use ollama?

Run `aider --model ollama/<ollama-model>`.
See these docs for more info: https://aider.chat/docs/llms/ollama.html

#### /code Add a function to calculate the factorial of a number

Certainly! I'll add a function to calculate the factorial of a number. Here's the change:

```python
def factorial(n):
    if n == 0 or n == 1:
        return 1
    else:
        return n * factorial(n - 1)

# Example usage
print(factorial(5))  # Output: 120
```

This function uses recursion to calculate the factorial. It's been added to your code.

#### /architect Can we make this simpler?

Yes, you could replace the `factorial()` function with a call to the `math.factorial()`
builtin. 
This way you don't have to maintain a custom factorial implementation,
and the builtin function is well optimized.

> Edit the files? (Y)es/(N)o [Yes]: Yes

```python
<<<<<<< SEARCH
def factorial(n):
    if n == 0 or n == 1:
        return 1
    else:
        return n * factorial(n - 1)
=======
from math import factorial
>>>>>>> REPLACE
```

</div>
`````

## File: usage/not-code.md
`````markdown
---
parent: Usage
nav_order: 901
description: Use aider to edit configuration files, documentation, and other text-based formats.
---


# Editing config & text files

Aider isn't just for code, it can be very helpful when editing
almost any text file.
You can use aider to make changes to your shell & ssh settings, 
Dockerfiles
or pretty much any configuration or documentation file.

Here are some practical examples of modifying common config/text files:

## Shell Configuration

<div class="chat-transcript" markdown="1">
$ aider .bashrc

Added .bashrc to the chat.


#### Add an alias 'll' that lists all files, with all details in human readable format. And update PATH to include uv installed tools.

```
+ alias ll='ls -alh'
+ export PATH="$PATH:$HOME/.local/bin:$PATH"
```
</div>

## SSH Configurations

<div class="chat-transcript" markdown="1">
$ aider ~/.ssh/config

Added config to the chat.

#### Create a Host entry 'my-server' using bastion.example.com as JumpHost

```
+ Host my-server
+     HostName 192.168.1.100
+     User deploy
+     Port 2222
+     IdentityFile ~/.ssh/deploy_key
+     ProxyJump bastion.example.com
```
</div>

## Docker Setup

<div class="chat-transcript" markdown="1">
$ aider Dockerfile docker-compose.yml

Added Dockerfile and docker-compose.yml to the chat.

#### Set non-root user and enable healthchecks

```
+ USER appuser
+ HEALTHCHECK --interval=30s --timeout=3s \
+   CMD curl -f http://localhost:8000/health || exit 1
```

#### Expose port 5432 and add volume for postgres data

```
  services:
    postgres:
      image: postgres:15
+     ports:
+       - "5432:5432"
+     volumes:
+       - pgdata:/var/lib/postgresql/data
```
</div>

## Git Configuration

<div class="chat-transcript" markdown="1">
$ aider .gitconfig

Added .gitconfig to the chat.

#### Set default push behavior to current branch and enable color UI

```
+ [push]
+     default = current
+ [color]
+     ui = auto
```
</div>

## System Configuration
<div class="chat-transcript" markdown="1">
$ aider /etc/hosts  # May need sudo

Added hosts to the chat.

#### Block tracking domains by pointing them to 127.0.0.1

```
+ 127.0.0.1   ads.example.com
+ 127.0.0.1   track.analytics.co
```
</div>


## Editor Configs
<div class="chat-transcript" markdown="1">
$ aider .vimrc

Added .vimrc to the chat.

#### Enable line numbers and set 4-space tabs for Python

```
+ set number
+ autocmd FileType python set tabstop=4 shiftwidth=4 expandtab
```
</div>

## VSCode Configuration
<div class="chat-transcript" markdown="1">
$ aider settings.json

Added settings.json to the chat.

#### Enable auto-format on save and set default formatter

```
+ "editor.formatOnSave": true,
+ "editor.defaultFormatter": "esbenp.prettier-vscode"
```
</div>

## Markdown Documentation
<div class="chat-transcript" markdown="1">
$ aider README.md

Added README.md to the chat.


#### Add installation section with brew and pip options

```
+ ## Installation
+ ```
+ # Homebrew
+ brew install cool-app-10k
+ 
+ # PyPI
+ pipx install cool-app-10k
+ ```
```
</div>

## XML Configuration
<div class="chat-transcript" markdown="1">
$ aider pom.xml

Added pom.xml to the chat.
#### Add JUnit 5 dependency with test scope

```
+ <dependency>
+     <groupId>org.junit.jupiter</groupId>
+     <artifactId>junit-jupiter-api</artifactId>
+     <version>5.9.2</version>
+     <scope>test</scope>
+ </dependency>
```
</div>
`````

## File: usage/notifications.md
`````markdown
---
title: Notifications
highlight_image: /assets/notifications.jpg
parent: Usage
nav_order: 760
description: Aider can notify you when it's waiting for your input.
---

# Notifications

Aider can notify you when it's done working and is
waiting for your input. 
This is especially useful for long-running operations or when you're multitasking.

## Usage

Enable notifications with the `--notifications` flag:

```bash
aider --notifications
```

When enabled, aider will notify you when the LLM has finished generating a response and is waiting for your input.

## OS-Specific Notifications

Aider automatically detects your operating system and uses an appropriate notification method:

- **macOS**: Uses `terminal-notifier` if available, falling back to AppleScript notifications
- **Linux**: Uses `notify-send` or `zenity` if available
- **Windows**: Uses PowerShell to display a message box

## Custom Notification Commands

You can specify a custom notification command with `--notifications-command`:

```bash
aider --notifications-command "your-custom-command"
```

For example, on macOS you might use:

```bash
aider --notifications-command "say 'Aider is ready'"
```

### Remote Notifications

For remote notifications you could use [Apprise](https://github.com/caronc/apprise),
which is a cross-platform Python library for sending notifications to various services.

We can use Apprise to send notifications to Slack

```bash
aider --notifications-command "apprise -b 'Aider is ready' 'slack://your-slack-webhook-token'"
```

or Discord
```bash
aider --notifications-command "apprise -b 'Aider is ready' 'discord://your-discord-webhook-token'"
```

or even to your phone via Pushbullet
```bash
aider --notifications-command "apprise -b 'Aider is ready' 'pbul://your-pushbullet-access-token'"
```

Check more how to use and configure Apprise on their GitHub page.

## Configuration

You can add these settings to your configuration file:

```yaml
# Enable notifications
notifications: true

# Optional custom notification command
notifications_command: "your-custom-command"
```

Or in your `.env` file:

```
AIDER_NOTIFICATIONS=true
AIDER_NOTIFICATIONS_COMMAND=your-custom-command
```
`````

## File: usage/tips.md
`````markdown
---
parent: Usage
nav_order: 25
description: Tips for AI pair programming with aider.
---

# Tips

## Just add the files that need to be changed to the chat

Take a moment and think about which files will need to be changed.
Aider can often figure out which files to edit all by itself, but the most efficient approach is for you to add the files to the chat.

## Don't add lots of files to the chat

Just add the files you think need to be edited.
Too much irrelevant code will distract and confuse the LLM.
Aider uses a [map of your entire git repo](https://aider.chat/docs/repomap.html)
so is usually aware of relevant classes/functions/methods elsewhere in your code base.
It's ok to add 1-2 highly relevant files that don't need to be edited,
but be selective.

## Break your goal down into bite sized steps

Do them one at a time. 
Adjust the files added to the chat as you go: `/drop` files that don't need any more changes, `/add` files that need changes for the next step.

## For complex changes, discuss a plan first

Use the [`/ask` command](modes.html) to make a plan with aider.
Once you are happy with the approach, just say "go ahead" without the `/ask` prefix.

## If aider gets stuck

- Use `/clear` to discard the chat history and make a fresh start.
- Can you `/drop` any extra files?
- Use `/ask` to discuss a plan before aider starts editing code.
- Use the [`/model` command](commands.html) to switch to a different model and try again. Switching between GPT-4o and Sonnet will often get past problems.
- If aider is hopelessly stuck,
just code the next step yourself and try having aider code some more after that.
Take turns and pair program with aider.

## Creating new files

If you want aider to create a new file, add it to the repository first with `/add <file>`.
This way aider knows this file exists and will write to it. 
Otherwise, aider might write the changes to an existing file.
This can happen even if you ask for a new file, as LLMs tend to focus a lot
on the existing information in their contexts.

## Fixing bugs and errors

If your code is throwing an error, 
use the [`/run` command](commands.html)
to share the error output with the aider.
Or just paste the errors into the chat. Let the aider figure out how to fix the bug.

If test are failing, use the [`/test` command](lint-test.html)
to run tests and
share the error output with the aider.

## Providing docs

LLMs know about a lot of standard tools and libraries, but may get some of the fine details wrong about API versions and function arguments.

You can provide up-to-date documentation in a few ways:

- Paste doc snippets into the chat.
- Include a URL to docs in your chat message
and aider will scrape and read it. For example: `Add a submit button like this https://ui.shadcn.com/docs/components/button`. 
- Use the [`/read` command](commands.html) to read doc files into the chat from anywhere on your filesystem.
- If you have coding conventions or standing instructions you want aider to follow, consider using a [conventions file](conventions.html).

## Interrupting & inputting

Use Control-C to interrupt aider if it isn't providing a useful response. The partial response remains in the conversation, so you can refer to it when you reply with more information or direction.

{% include multi-line.md %}
`````

## File: usage/tutorials.md
`````markdown
---
parent: Usage
nav_order: 75
description: Intro and tutorial videos made by aider users.
---

# Tutorial videos

Here are some tutorial videos made by aider users:

- [Using Architect/Editor mode](https://www.youtube.com/watch?v=OPXslklVBZc) -- AICodeKing
- [Using aider to incrementally build a non-trivial app](https://youtu.be/QlUt06XLbJE) -- IndyDevDan
- [Aider and Replit on mobile with your voice](https://x.com/itsPaulAi/status/1830987090617831810) -- Paul Couvert
- [Aider is the OG AI Coding King (Mermaid Diagram AI Agent)](https://www.youtube.com/watch?v=ag-KxYS8Vuw) -- IndyDevDan
- [Installing aider in replit and making a Trello clone](https://x.com/itspaulai/status/1828834199597633724) -- Paul Couvert
- [Step-by-Step Development Environment Setup for AI-Assisted Coding](https://www.youtube.com/watch?v=DnBVgfe6ZQM) -- Coding the Future With AI
- [Generate FULL-STACK Apps with Claude 3.5 Sonnet](https://youtu.be/sKeIZGW8xzg) -- AICodeKing
- [Creating Games with AI from Start-To-End](https://youtu.be/sOd2YYZFMUs) -- AICodeKing
- [Claude 3.5 and aider: Use AI Assistants to Build AI Apps](https://youtu.be/0hIisJ3xAdU) -- Coding the Future With AI
- [Develop a Full-stack App Without Writing ANY Code](https://youtu.be/dzOWn8TI738) -- WorldofAI
- [Generate Games with AI (w/ Local LLMs)](https://youtu.be/DjVJpGzQbSA) -- AICodeKing
- [Aider tips and Example use](https://www.youtube.com/watch?v=OsChkvGGDgw) -- techfren
- [Aider and Claude 3.5: Develop a Full-stack App Without Writing ANY Code!](https://www.youtube.com/watch?v=BtAqHsySdSY) -- Coding the Future With AI
- [Generate application with just one prompt using Aider](https://www.youtube.com/watch?v=Y-_0VkMUiPc&t=78s) -- AICodeKing
- [Aider : the production ready AI coding assistant you've been waiting for](https://www.youtube.com/watch?v=zddJofosJuM) -- Learn Code With JV
- [Holy Grail: FREE Coding Assistant That Can Build From EXISTING CODE BASE](https://www.youtube.com/watch?v=df8afeb1FY8) -- Matthew Berman
- [Aider: This AI Coder Can Create AND Update Git Codebases](https://www.youtube.com/watch?v=EqLyFT78Sig) -- Ian Wootten

Thanks to all these great creators for taking the time
to share their experiences coding with aider!
`````

## File: usage/voice.md
`````markdown
---
parent: Usage
nav_order: 100
description: Speak with aider about your code!
---

# Voice-to-code with aider

Speak with aider about your code! Request new features, test cases or bug fixes using your voice and let aider do the work of editing the files in your local git repo. As with all of aider's capabilities, you can use voice-to-code with an existing repo or to start a new project.

Voice support fits quite naturally into aider's AI pair programming
chat interface. Now you can fluidly switch between voice and text chat
when you ask aider to edit your code.

## How to use voice-to-code

Use the in-chat `/voice` command to start recording,
and press `ENTER` when you're done speaking.
Your voice coding instructions will be transcribed, 
as if you had  typed them into
the aider chat session.

See the [installation instructions](https://aider.chat/docs/install/optional.html#enable-voice-coding) for
information on how to enable the `/voice` command.

<br/>
<div class="chat-transcript" markdown="1">

> Aider v0.11.2-dev  
> Added app.py to the chat.  

#### /voice

<blockquote>
<div class="recording-line">
    <p style="display: inline;">Recording, press ENTER when done... <span id="timer">3.5</span>sec</p>
    <div id="progress-bar">
        <div id="progress"></div>
    </div>
</div>
</blockquote>

<style>
    .recording-line > * {
        display: inline-block;
        vertical-align: middle;
    }
    #progress-bar {
        width: 100px;
        height: 20px;
        background-color: #333;
        margin-left: 10px;
    }
    #progress {
        height: 100%;
        background-color: #f0f0f0;
        animation: progress 3s infinite;
    }
    @keyframes progress {
        0% { width: 10%; }
        20% { width: 90%; }
        50% { width: 30%; }
        60% { width: 90%; }
        80% { width: 20%; }
        100% { width: 10%; }

    }
</style>

<script>
window.onload = function() {
    var counter = 0;
    var timer = document.getElementById('timer');
    setInterval(function() {
        counter = (counter + 0.1) % 9;
        timer.textContent = counter.toFixed(1);
    }, 100);
}
</script>

<div style="display: inline-block; position: relative; width: 200px; background-color: #f0f0f0; border-radius: 10px; padding: 10px; color: #333; font-family: 'Comic Sans MS', 'Comic Sans', 'Marker Felt', sans-serif;">
<span style="position: absolute; top: 0; left: 50%; transform: translate(-50%, -100%); width: 0; height: 0; border-left: 10px solid transparent; border-right: 10px solid transparent; border-bottom: 10px solid #f0f0f0;"></span>
“&nbsp;add a factorial endpoint that uses math factorial&nbsp;”
</div>

#### Add a factorial endpoint that uses math.factorial.

To add a factorial endpoint to the Flask application, we need to modify the `app.py` file.

Here are the steps:

1. Add a new route `/fact/<int:x>` to the Flask application.
2. In the function for this route, call `math.factorial(x)` and return the result as a string.

Here is the *edit block* for these changes:

```python
app.py
<<<<<<< HEAD
if __name__ == '__main__':
    print("Starting...")
    app.run()
=======
@app.route('/fact/<int:x>')
def factorial(x):
    result = math.factorial(x)
    return str(result)

if __name__ == '__main__':
    print("Starting...")
    app.run()
>>>>>>> updated
```

> Applied edit to app.py  
> Commit ef9e3e7 aider: Add a factorial endpoint that uses math.factorial.  

</div>
<br/>
<br/>
<br/>
`````

## File: usage/watch.md
`````markdown
---
title: Aider in your IDE
#highlight_image: /assets/browser.jpg
parent: Usage
nav_order: 750
description: Aider can watch your files and respond to AI comments you add in your favorite IDE or text editor.
---

# Aider in your IDE

<div class="video-container">
  <video controls loop poster="/assets/watch.jpg">
    <source src="/assets/watch.mp4" type="video/mp4">
    <a href="/assets/watch.mp4">Aider browser UI demo video</a>
  </video>
</div>

<style>
.video-container {
  position: relative;
  padding-bottom: 102.7%; /1.027 */
  height: 0;
  overflow: hidden;
}

.video-container video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>

## AI comments

If you run aider with `--watch-files`, it will watch all files in your repo
and look for any AI coding instructions you add using your favorite IDE or text editor.

Specifically, aider looks for one-liner comments (# ... or // ...) that either start or end with `AI`, `AI!` or `AI?` like these:

```python
# Make a snake game. AI!
# What is the purpose of this method AI?
```

Or in `//` comment languages...

```js
// Write a protein folding prediction engine. AI!
```

Aider will take note of all the comments that start or end with `AI`.
Comments that include `AI!` with an exclamation point or `AI?` with a question
mark are special.
They trigger aider to take action to collect *all* the AI comments and use them
as your instructions.

- `AI!` triggers aider to make changes to your code.
- `AI?` triggers aider to answer your question.

See the demo video above that shows aider working with AI comments in VSCode.


## Example

For example, if you included this AI comment in your code:

```js
function factorial(n) // Implement this. AI!
```

Then aider would update the file and implement the function:

```js
function factorial(n) {
  if (n === 0 || n === 1) {
    return 1;
  } else {
    return n * factorial(n - 1);
  }
}
```

## Comment styles

Aider only watches for these types of **one-liner** comments:

```
# Python and bash style
// Javascript style
-- SQL style
```

Aider will look for those comment types in all files.
You can use them into any code file you're editing, even if they aren't the
correct comment syntax for that language.

## Multiple uses

This capability is quite flexible and powerful, and can be used in many ways.

### In-context instructions

You can add an AI comment in the function you want changed,
explaining the change request in-context right where you want the changes.

```javascript
app.get('/sqrt/:n', (req, res) => {
    const n = parseFloat(req.params.n);

    // Add error handling for NaN and less than zero. AI!

    const result = math.sqrt(n);
    res.json({ result: result });
});
```

### Multiple comments

You can add multiple `AI` comments without the `!`,
before triggering aider with a final `AI!`.
Also keep in mind that you can spread the AI comments across
multiple files, if you want to coordinate changes in multiple places.
Just use `AI!` last, to trigger aider.

```python
@app.route('/factorial/<int:n>')
def factorial(n):
    if n < 0:
        return jsonify(error="Factorial is not defined for negative numbers"), 400

    # AI: Refactor this code...

    result = 1
    for i in range(1, n + 1):
        result *= i

    # ... into to a compute_factorial() function. AI!

    return jsonify(result=result)
```

### Long form instructions

You can add a block of comments, with longer instructions.
Just be sure to start or end one of the lines with `AI` or `AI!` to draw
aider's attention to the block.

```python
# Make these changes: AI!
# - Add a proper main() function
# - Use Click to process cmd line args
# - Accept --host and --port args
# - Print a welcome message that includes the listening url

if __name__ == "__main__":
    app.run(debug=True)
```

### Add a file to the aider chat

Rather than using `/add` to add a file inside the aider chat, you can
simply put an `#AI` comment in it and save the file.
You can undo/remove the comment immediately if you like, the file
will still be added to the aider chat.

## Also use aider chat in the terminal

It can be really helpful to get a change started with AI comments.
But sometimes you want to build on or refine those changes.
You can of course continue to do that with AI comments,
but it can sometimes be effective to switch over to the aider terminal chat.
The chat has the history of the AI comments you just made,
so you can continue on naturally from there.

You can also use the normal aider chat in your terminal to work with
many of aider's more advanced features:

- Use `/undo` to revert changes you don't like. Although you may also be able to use your IDE's undo function to step back in the file history.
- Use [chat modes](https://aider.chat/docs/usage/modes.html) to ask questions or get help.
- Manage the chat context with `/tokens`, `/clear`, `/drop`, `/reset`.
Adding an AI comment will add the file to the chat.
Periodically, you may want remove extra context that is no longer needed.
- [Fix lint and test errors](https://aider.chat/docs/usage/lint-test.html).
- Run shell commands.
- Etc.


## You can be lazy

The examples above all show AI
comments with full sentences, proper capitalization, punctuation, etc.
This was done to help explain how AI comments work, but is not needed in practice.

Most LLMs are perfectly capable of dealing with ambiguity and
inferring implied intent.
This often allows you to be quite lazy with your AI comments.
In particular, you can start and end comments with lowercase `ai` and `ai!`,
but you can also be much more terse with the request itself.
Below are simpler versions of some of the examples given above.

When the context clearly implies the needed action, `ai!` might be all you
need. For example, to implement a factorial function
in a program full of other math functions either of these
approaches would probably work:

```js
function factorial(n) // ai!
```

Or...

```js
// add factorial() ai!
```

Rather than a long, explicit comment like "Add error handling for NaN and less than zero,"
you can let aider infer more about the request.
This simpler comment may be sufficient:

```javascript
app.get('/sqrt/:n', (req, res) => {
    const n = parseFloat(req.params.n);

    // add error handling ai!

    const result = math.sqrt(n);
    res.json({ result: result });
});
```

Similarly, this refactor probably could have been requested with fewer words, like this:

```python
@app.route('/factorial/<int:n>')
def factorial(n):
    if n < 0:
        return jsonify(error="Factorial is not defined for negative numbers"), 400

    # ai refactor...

    result = 1
    for i in range(1, n + 1):
        result *= i

    # ... to compute_factorial() ai!

    return jsonify(result=result)
```

As you use aider with your chosen LLM, you can develop a sense for how
explicit you need to make your AI comments.

## Behind the scenes

Aider sends your AI comments to the LLM with the
[repo map](https://aider.chat/docs/repomap.html)
and all the other code context you've added to the chat.

It also pulls out and highlights the AI comments with specific context, showing the LLM
exactly how they fit into the code base.

```
The "AI" comments below marked with █ can be found in the code files I've shared with you.
They contain your instructions.
Make the requested changes.
Be sure to remove all these "AI" comments from the code!

todo_app.py:
⋮...
│class TodoList:
⋮...
│    def __init__(self):
│        """Initialize an empty todo list"""
⋮...
│
│    def list_tasks(self):
│        """Display all tasks"""
█        # Implement this. AI!
│
│def main():
│    todo = TodoList()
│
⋮...
```

--------

#### Credits

*This feature was inspired by
the way [Override](https://github.com/oi-overide) watches for file changes
to find prompts embedded within `//> a specific set of delimiters <//`.*
`````

## File: config.md
`````markdown
---
nav_order: 55
has_children: true
description: Information on all of aider's settings and how to use them.
---

# Configuration

Aider has many options which can be set with
command line switches.
Most options can also be set in an `.aider.conf.yml` file
which can be placed in your home directory or at the root of
your git repo. 
Or by setting environment variables like `AIDER_xxx`
either in your shell or a `.env` file.

Here are 4 equivalent ways of setting an option. 

With a command line switch:

```
$ aider --dark-mode
```

Using a `.aider.conf.yml` file:

```yaml
dark-mode: true
```

By setting an environment variable:

```
export AIDER_DARK_MODE=true
```

Using an `.env` file:

```
AIDER_DARK_MODE=true
```

{% include keys.md %}
`````

## File: faq.md
`````markdown
---
nav_order: 90
description: Frequently asked questions about aider.
---

# FAQ
{: .no_toc }

- TOC
{:toc}

{% include help-tip.md %}

## How can I add ALL the files to the chat?

People regularly ask about how to add **many or all of their repo's files** to the chat.
This is probably not a good idea and will likely do more harm than good.

The best approach is think about which files need to be changed to accomplish
the task you are working on. Just add those files to the chat.

Usually when people want to add "all the files" it's because they think it
will give the LLM helpful context about the overall code base.
Aider will automatically give the LLM a bunch of additional context about
the rest of your git repo.
It does this by analyzing your entire codebase in light of the
current chat to build a compact
[repository map](https://aider.chat/2023/10/22/repomap.html).

Adding a bunch of files that are mostly irrelevant to the
task at hand will often distract or confuse the LLM.
The LLM will give worse coding results, and sometimese even fail to correctly edit files.
Addings extra files will also increase your token costs.

Again, it's usually best to just add the files to the chat that will need to be modified.
If you still wish to add lots of files to the chat, you can:

- Use a wildcard when you launch aider: `aider src/*.py`
- Use a wildcard with the in-chat `/add` command: `/add src/*.py`
- Give the `/add` command a directory name and it will recursively add every file under that dir: `/add src`

## Can I use aider in a large (mono) repo?

Aider will work in any size repo, but is not optimized for quick
performance and response time in very large repos.
There are some things you can do to improve performance.

Be sure to check the
[general usage tips](/docs/usage/tips.html)
before considering this large-repo specific advice.
To get the best results from aider you want to 
be thoughtful about how you add files to the chat,
regardless of your repo size.

You can change into a sub directory of your repo that contains the
code you want to work on and use the `--subtree-only` switch.
This will tell aider to ignore the repo outside of the
directory you start in.

You can also create a `.aiderignore` file to tell aider
to ignore parts of the repo that aren't relevant to your task.
This file conforms to `.gitignore` syntax and conventions.
For example, to focus only on specific directories in a monorepo,
you could create a `.aiderignore` file with:

```
# Ignore everything
/*

# Allow specific directories and their contents
!foo/
!bar/
!baz/

# Allow nested files under these directories
!foo/**
!bar/**
!baz/**
```

You can use `--aiderignore <filename>` to name a specific file
to use for ignore patterns.
You might have a few of these handy for when you want to work on
frontend, backend, etc portions of your repo.

## Can I use aider with multiple git repos at once?

Currently aider can only work with one repo at a time.

There are some things you can try if you need to work with
multiple interrelated repos:

- You can run aider in repo-A where you need to make a change
and use `/read` to add some files read-only from another repo-B.
This can let aider see key functions or docs from the other repo.
- You can run `aider --show-repo-map > map.md` within each
repo to create repo maps.
You could then run aider in repo-A and 
use `/read ../path/to/repo-B/map.md` to share
a high level map of the other repo.
- You can use aider to write documentation about a repo.
Inside each repo, you could run `aider docs.md`
and work with aider to write some markdown docs.
Then while using aider to edit repo-A
you can `/read ../path/to/repo-B/docs.md` to
read in those docs from the other repo.
- In repo A, ask aider to write a small script that demonstrates
the functionality you want to use in repo B.
Then when you're using aider in repo B, you can 
`/read` in that script.

## How do I turn on the repository map?

Depending on the LLM you are using, aider may launch with the repo map disabled by default:

```
Repo-map: disabled
```

This is because weaker models get easily overwhelmed and confused by the content of the
repo map. They sometimes mistakenly try to edit the code in the repo map.
The repo map is usually disabled for a good reason.

If you would like to force it on, you can run aider with `--map-tokens 1024`.

## How do I include the git history in the context?

When starting a fresh aider session, you can include recent git history in the chat context. This can be useful for providing the LLM with information about recent changes. To do this:

1. Use the `/run` command with `git diff` to show recent changes:
   ```
   /run git diff HEAD~1
   ```
   This will include the diff of the last commit in the chat history.

2. To include diffs from multiple commits, increase the number after the tilde:
   ```
   /run git diff HEAD~3
   ```
   This will show changes from the last three commits.

Remember, the chat history already includes recent changes made during the current session, so this tip is most useful when starting a new aider session and you want to provide context about recent work.

You can also use aider to review PR branches:

```
/run git diff one-branch..another-branch

...

Add 6.9k tokens of command output to the chat? (Y)es/(N)o [Yes]: Yes

/ask Are there any problems with the way this change works with the FooBar class?
```

{: .tip }
The `/git` command will not work for this purpose, as its output is not included in the chat. 

## How can I run aider locally from source code?

To run the project locally, follow these steps:

```
# Clone the repository
git clone git@github.com:Aider-AI/aider.git

# Navigate to the project directory
cd aider

# It's recommended to make a virtual environment

# Install aider in editable/development mode, 
# so it runs from the latest copy of these source files
python -m pip install -e .

# Run the local version of aider
python -m aider
```



## Can I change the system prompts that aider uses?

The most convenient way to add custom instructions is to use a
[conventions file](https://aider.chat/docs/usage/conventions.html).

But, aider is set up to support different actual system prompts and edit formats
in a modular way. If you look in the `aider/coders` subdirectory, you'll
see there's a base coder with base prompts, and then there are
a number of
different specific coder implementations.

If you're thinking about experimenting with system prompts
this document about
[benchmarking GPT-3.5 and GPT-4 on code editing](https://aider.chat/docs/benchmarks.html)
might be useful background.

While it's not well documented how to add new coder subsystems, you may be able
to modify an existing implementation or use it as a template to add another.

To get started, try looking at and modifying these files.

The wholefile coder is currently used by GPT-3.5 by default. You can manually select it with `--edit-format whole`.

- wholefile_coder.py
- wholefile_prompts.py

The editblock coder is currently used by GPT-4o by default. You can manually select it with `--edit-format diff`.

- editblock_coder.py
- editblock_prompts.py

The universal diff coder is currently used by GPT-4 Turbo by default. You can manually select it with `--edit-format udiff`.

- udiff_coder.py
- udiff_prompts.py

When experimenting with coder backends, it helps to run aider with `--verbose --no-pretty` so you can see
all the raw information being sent to/from the LLM in the conversation.

You can also refer to the
[instructions for installing a development version of aider](https://aider.chat/docs/install/optional.html#install-the-development-version-of-aider).

## What LLMs do you use to build aider?

Aider writes a lot of its own code, usually about 70% of the new code in each
release.
People often ask which LLMs I use with aider, when writing aider.
Below is a table showing the models I have used recently,
extracted from the 
[public log](https://github.com/aider-ai/aider/blob/main/aider/website/assets/sample-analytics.jsonl)
of my
[aider analytics](https://aider.chat/docs/more/analytics.html).

<!--[[[cog
import sys
sys.path.append(".")
import scripts.my_models as my_models
stats = my_models.collect_model_stats()
html = my_models.format_html_table(stats)
cog.out(html)
]]]-->
<style>
table { border-collapse: collapse; width: 100%; }
th, td { padding: 8px; text-align: left; border-bottom: 1px solid #ddd; }
th { background-color: #f2f2f2; }
tr:hover { background-color: #f5f5f5; }
.right { text-align: right; }
</style>
<table>
<tr><th>Model Name</th><th class='right'>Total Tokens</th><th class='right'>Percent</th></tr>
<tr><td>anthropic/claude-3-7-sonnet-20250219</td><td class='right'>3,055,924</td><td class='right'>95.3%</td></tr>
<tr><td>openrouter/anthropic/claude-3.7-sonnet</td><td class='right'>107,132</td><td class='right'>3.3%</td></tr>
<tr><td>o3-mini</td><td class='right'>38,213</td><td class='right'>1.2%</td></tr>
<tr><td>fireworks_ai/accounts/fireworks/models/deepseek-v3</td><td class='right'>4,855</td><td class='right'>0.2%</td></tr>
</table>
<!--[[[end]]]-->

## How are the "aider wrote xx% of code" stats computed?

[Aider is tightly integrated with git](/docs/git.html) so all
of aider's code changes are committed to the repo with proper attribution.
The 
[stats are computed](https://github.com/Aider-AI/aider/blob/main/scripts/blame.py)
by doing something like `git blame` on the repo,
and counting up who wrote all the new lines of code in each release.
Only lines in source code files are counted, not documentation or prompt files.

## Why does aider sometimes stop highlighting code in its replies?

Aider displays the markdown responses that are coming back from the LLM.
Usually, the LLM will reply with code in a markdown "code block" with
triple backtick fences, like this:

````
Here's some code:

```
print("hello")
```
````

But if you've added files to the chat that contain triple backticks,
aider needs to tell the LLM to use a different set of fences.
Otherwise, the LLM can't safely include your code's triple backticks
inside the code blocks that it returns with edits.
Aider will use fences like `<source>...</source>` in this case.

A side effect of this is that the code that aider outputs may no
longer be properly highlighted.
You will most often notice this if you add markdown files
to you chats that contain code blocks.

## Why is the LLM speaking to me in an unexpected language?

Aider goes to some effort to prompt the model to use the language that is configured
for your system.
But LLMs aren't fully reliable, and they sometimes decide to speak in
an unexpected language.
Claude is especially fond of speaking French.

You can explicitly set the language that aider tells the model to use with
`--chat-language <language>`.
But the LLM may not comply.

## Can I share my aider chat transcript?

Yes, you can now share aider chat logs in a pretty way.

1. Copy the markdown logs you want to share from `.aider.chat.history.md` and make a github gist. Or publish the raw markdown logs on the web any way you'd like.

   ```
   https://gist.github.com/Aider-AI/2087ab8b64034a078c0a209440ac8be0
   ```

2. Take the gist URL and append it to:

   ```
   https://aider.chat/share/?mdurl=
   ```

This will give you a URL like this, which shows the chat history like you'd see in a terminal:

```
https://aider.chat/share/?mdurl=https://gist.github.com/Aider-AI/2087ab8b64034a078c0a209440ac8be0
```

## Can I edit files myself while aider is running?

Yes. Aider always reads the latest copy of files from the file
system when you send each message.

While you're waiting for aider's reply to complete, it's probably unwise to
edit files that you've added to the chat.
Your edits and aider's edits might conflict.

## What is Aider AI LLC?

Aider AI LLC is the company behind the aider AI coding tool.
Aider is 
[open source and available on GitHub](https://github.com/Aider-AI/aider)
under an 
[Apache 2.0 license](https://github.com/Aider-AI/aider/blob/main/LICENSE.txt).


<div style="height:80vh"></div>
`````

## File: git.md
`````markdown
---
parent: More info
nav_order: 100
description: Aider is tightly integrated with git.
---

# Git integration

Aider works best with code that is part of a git repo.
Aider is tightly integrated with git, which makes it easy to:

  - Use the `/undo` command to instantly undo any AI changes that you don't like.
  - Go back in the git history to review the changes that aider made to your code
  - Manage a series of aider's changes on a git branch

Aider uses git in these ways:

- It asks to create a git repo if you launch it in a directory without one.
- Whenever aider edits a file, it commits those changes with a descriptive commit message. This makes it easy to undo or review aider's changes. 
- Aider takes special care before editing files that already have uncommitted changes (dirty files). Aider will first commit any preexisting changes with a descriptive commit message. 
This keeps your edits separate from aider's edits, and makes sure you never lose your work if aider makes an inappropriate change.

## In-chat commands

Aider also allows you to use 
[in-chat commands](/docs/usage/commands.html)
to perform git operations:

- `/diff` will show all the file changes since the last message you sent.
- `/undo` will undo and discard the last change.
- `/commit` to commit all dirty changes with a sensible commit message.
- `/git` will let you run raw git commands to do more complex management of your git history.

You can also manage your git history outside of aider with your preferred git tools.

## Disabling git integration

While it is not recommended, you can disable aider's use of git in a few ways:

  - `--no-auto-commits` will stop aider from git committing each of its changes.
  - `--no-dirty-commits` will stop aider from committing dirty files before applying its edits.
  - `--no-git` will completely stop aider from using git on your files. You should ensure you are keeping sensible backups of the files you are working with.

## Commit messages

Aider sends the `--weak-model` a copy of the diffs and the chat history
and asks it to produce a commit message.
By default, aider creates commit messages which follow
[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).

You can customize the
[commit prompt](https://github.com/Aider-AI/aider/blob/main/aider/prompts.py#L5)
with the `--commit-prompt` option.
You can place that on the command line, or 
[configure it via a config file or environment variables](https://aider.chat/docs/config.html).


## Commit attribution

Aider marks commits that it either authored or committed.

- If aider authored the changes in a commit, they will have "(aider)" appended to the git author and git committer name metadata.
- If aider simply committed changes (found in dirty files), the commit will have "(aider)" appended to the git committer name metadata.

You can use `--no-attribute-author` and `--no-attribute-committer` to disable
modification of the git author and committer name fields.

Additionally, you can use the following options to prefix commit messages:

- `--attribute-commit-message-author`: Prefix commit messages with 'aider: ' if aider authored the changes.
- `--attribute-commit-message-committer`: Prefix all commit messages with 'aider: ', regardless of whether aider authored the changes or not.

Both of these options are disabled by default, but can be useful for easily identifying changes made by aider.
`````

## File: install.md
`````markdown
---
title: Installation
has_children: true
nav_order: 20
description: How to install and get started pair programming with aider.
---

# Installation
{: .no_toc }


## Get started quickly with aider-install

{% include get-started.md %}

This will install aider in its own separate python environment.
If needed, 
aider-install will also install a separate version of python 3.12 to use with aider.

Once aider is installed,
there are also some [optional install steps](/docs/install/optional.html).

See the [usage instructions](https://aider.chat/docs/usage.html) to start coding with aider.

## One-liners

These one-liners will install aider, along with python 3.12 if needed.
They are based on the 
[uv installers](https://docs.astral.sh/uv/getting-started/installation/).

#### Windows

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://aider.chat/install.ps1 | iex"
```

#### Mac & Linux

Use curl to download the script and execute it with sh:

```bash
curl -LsSf https://aider.chat/install.sh | sh
```

If your system doesn't have curl, you can use wget:

```bash
wget -qO- https://aider.chat/install.sh | sh
```


## Install with uv

You can install aider with uv:

```bash
python -m pip install uv  # If you need to install uv
uv tool install --force --python python3.12 aider-chat@latest
```

This will install uv using your existing python version 3.8-3.13,
and use it to install aider.
If needed, 
uv will automatically install a separate python 3.12 to use with aider.

Also see the
[docs on other methods for installing uv itself](https://docs.astral.sh/uv/getting-started/installation/).

## Install with pipx

You can install aider with pipx:

```bash
python -m pip install pipx  # If you need to install pipx
pipx install aider-chat
```

You can use pipx to install aider with python versions 3.9-3.12.

Also see the
[docs on other methods for installing pipx itself](https://pipx.pypa.io/stable/installation/).

## Other install methods

You can install aider with the methods described below, but one of the above
methods is usually safer.

#### Install with pip

If you install with pip, you should consider
using a 
[virtual environment](https://docs.python.org/3/library/venv.html)
to keep aider's dependencies separated.


You can use pip to install aider with python versions 3.9-3.12.

```bash
python -m pip install -U --upgrade-strategy only-if-needed aider-chat
```

{% include python-m-aider.md %}

#### Installing with package managers

It's best to install aider using one of methods
recommended above.
While aider is available in a number of system package managers,
they often install aider with incorrect dependencies.

## Next steps...

There are some [optional install steps](/docs/install/optional.html) you could consider.
See the [usage instructions](https://aider.chat/docs/usage.html) to start coding with aider.
`````

## File: languages.md
`````markdown
---
parent: More info
nav_order: 200
description: Aider supports pretty much all popular coding languages.
---
# Supported languages

Aider should work well with most popular coding languages.
This is because top LLMs are fluent in most mainstream languages,
and familiar with popular libraries, packages and frameworks.

Aider has specific support for linting many languages.
By default, aider runs the built in linter any time a file is edited.
If it finds syntax errors, aider will offer to fix them for you.
This helps catch small code issues and quickly fix them.

Aider also does code analysis to help
the LLM navigate larger code bases by producing
a [repository map](https://aider.chat/docs/repomap.html).
Aider can currently produce repository maps for many popular
mainstream languages, listed below.


## How to add support for another language

Aider should work quite well for other languages, even those
without repo map or linter support.
You should really try coding with aider before
assuming it needs better support for your language.

That said, if aider already has support for linting your language,
then it should be possible to add repo map support.
To build a repo map, aider needs the `tags.scm` file
from the given language's tree-sitter grammar.
If you can find and share that file in a 
[GitHub issue](https://github.com/Aider-AI/aider/issues),
then it may be possible to add repo map support.

If aider doesn't support linting, it will be complicated to
add linting and repo map support.
That is because aider relies on 
[py-tree-sitter-languages](https://github.com/grantjenks/py-tree-sitter-languages)
to provide pre-packaged versions of tree-sitter
parsers for many languages.

Aider needs to be easy for users to install in many environments,
and it is probably too complex to add dependencies on
additional individual tree-sitter parsers.


<!--[[[cog
from aider.repomap import get_supported_languages_md
cog.out(get_supported_languages_md())
]]]-->

| Language | File extension | Repo map | Linter |
|:--------:|:--------------:|:--------:|:------:|
| actionscript         | .as                  |          |   ✓    |
| ada                  | .adb                 |          |   ✓    |
| ada                  | .ads                 |          |   ✓    |
| agda                 | .agda                |          |   ✓    |
| arduino              | .ino                 |    ✓     |   ✓    |
| asm                  | .asm                 |          |   ✓    |
| asm                  | .s                   |          |   ✓    |
| astro                | .astro               |          |   ✓    |
| bash                 | .bash                |          |   ✓    |
| bash                 | .sh                  |          |   ✓    |
| bash                 | .zsh                 |          |   ✓    |
| beancount            | .bean                |          |   ✓    |
| bibtex               | .bib                 |          |   ✓    |
| bicep                | .bicep               |          |   ✓    |
| bitbake              | .bb                  |          |   ✓    |
| bitbake              | .bbappend            |          |   ✓    |
| bitbake              | .bbclass             |          |   ✓    |
| c                    | .c                   |    ✓     |   ✓    |
| c                    | .h                   |    ✓     |   ✓    |
| cairo                | .cairo               |          |   ✓    |
| capnp                | .capnp               |          |   ✓    |
| chatito              | .chatito             |    ✓     |   ✓    |
| clarity              | .clar                |          |   ✓    |
| clojure              | .clj                 |          |   ✓    |
| clojure              | .cljc                |          |   ✓    |
| clojure              | .cljs                |          |   ✓    |
| clojure              | .edn                 |          |   ✓    |
| cmake                | .cmake               |          |   ✓    |
| cmake                | CMakeLists.txt       |          |   ✓    |
| commonlisp           | .cl                  |    ✓     |   ✓    |
| commonlisp           | .lisp                |    ✓     |   ✓    |
| cpon                 | .cpon                |          |   ✓    |
| cpp                  | .cc                  |    ✓     |   ✓    |
| cpp                  | .cpp                 |    ✓     |   ✓    |
| cpp                  | .cxx                 |    ✓     |   ✓    |
| cpp                  | .h++                 |    ✓     |   ✓    |
| cpp                  | .hpp                 |    ✓     |   ✓    |
| cpp                  | .hxx                 |    ✓     |   ✓    |
| csharp               | .cs                  |    ✓     |   ✓    |
| css                  | .css                 |          |   ✓    |
| csv                  | .csv                 |          |   ✓    |
| cuda                 | .cu                  |          |   ✓    |
| cuda                 | .cuh                 |          |   ✓    |
| d                    | .d                   |    ✓     |   ✓    |
| dart                 | .dart                |    ✓     |   ✓    |
| dockerfile           | Dockerfile           |          |   ✓    |
| dtd                  | .dtd                 |          |   ✓    |
| elisp                | .el                  |    ✓     |   ✓    |
| elixir               | .ex                  |    ✓     |   ✓    |
| elixir               | .exs                 |    ✓     |   ✓    |
| elm                  | .elm                 |    ✓     |   ✓    |
| erlang               | .erl                 |          |   ✓    |
| erlang               | .hrl                 |          |   ✓    |
| fennel               | .fnl                 |          |   ✓    |
| firrtl               | .fir                 |          |   ✓    |
| fish                 | .fish                |          |   ✓    |
| fortran              | .f                   |          |   ✓    |
| fortran              | .f03                 |          |   ✓    |
| fortran              | .f08                 |          |   ✓    |
| fortran              | .f90                 |          |   ✓    |
| fortran              | .f95                 |          |   ✓    |
| func                 | .fc                  |          |   ✓    |
| gdscript             | .gd                  |          |   ✓    |
| gitattributes        | .gitattributes       |          |   ✓    |
| gitcommit            | .gitcommit           |          |   ✓    |
| gitignore            | .gitignore           |          |   ✓    |
| gleam                | .gleam               |    ✓     |   ✓    |
| glsl                 | .frag                |          |   ✓    |
| glsl                 | .glsl                |          |   ✓    |
| glsl                 | .vert                |          |   ✓    |
| gn                   | .gn                  |          |   ✓    |
| gn                   | .gni                 |          |   ✓    |
| go                   | .go                  |    ✓     |   ✓    |
| gomod                | go.mod               |          |   ✓    |
| gosum                | go.sum               |          |   ✓    |
| groovy               | .groovy              |          |   ✓    |
| gstlaunch            | .launch              |          |   ✓    |
| hack                 | .hack                |          |   ✓    |
| hare                 | .ha                  |          |   ✓    |
| haskell              | .hs                  |          |   ✓    |
| haxe                 | .hx                  |          |   ✓    |
| hcl                  | .hcl                 |    ✓     |   ✓    |
| hcl                  | .tf                  |    ✓     |   ✓    |
| hcl                  | .tfvars              |    ✓     |   ✓    |
| heex                 | .heex                |          |   ✓    |
| hlsl                 | .hlsl                |          |   ✓    |
| html                 | .htm                 |          |   ✓    |
| html                 | .html                |          |   ✓    |
| hyprlang             | .hypr                |          |   ✓    |
| ispc                 | .ispc                |          |   ✓    |
| janet                | .janet               |          |   ✓    |
| java                 | .java                |    ✓     |   ✓    |
| javascript           | .js                  |    ✓     |   ✓    |
| javascript           | .jsx                 |    ✓     |   ✓    |
| javascript           | .mjs                 |    ✓     |   ✓    |
| jsdoc                | .jsdoc               |          |   ✓    |
| json                 | .json                |          |   ✓    |
| jsonnet              | .jsonnet             |          |   ✓    |
| jsonnet              | .libsonnet           |          |   ✓    |
| julia                | .jl                  |          |   ✓    |
| kconfig              | Kconfig              |          |   ✓    |
| kdl                  | .kdl                 |          |   ✓    |
| kotlin               | .kt                  |    ✓     |   ✓    |
| kotlin               | .kts                 |    ✓     |   ✓    |
| latex                | .cls                 |          |   ✓    |
| latex                | .sty                 |          |   ✓    |
| latex                | .tex                 |          |   ✓    |
| linkerscript         | .ld                  |          |   ✓    |
| llvm                 | .ll                  |          |   ✓    |
| lua                  | .lua                 |    ✓     |   ✓    |
| luadoc               | .luadoc              |          |   ✓    |
| luap                 | .luap                |          |   ✓    |
| luau                 | .luau                |          |   ✓    |
| magik                | .magik               |          |   ✓    |
| make                 | .mk                  |          |   ✓    |
| make                 | Makefile             |          |   ✓    |
| markdown             | .markdown            |          |   ✓    |
| markdown             | .md                  |          |   ✓    |
| matlab               | .m                   |          |   ✓    |
| matlab               | .mat                 |          |   ✓    |
| mermaid              | .mermaid             |          |   ✓    |
| meson                | meson.build          |          |   ✓    |
| ninja                | .ninja               |          |   ✓    |
| nix                  | .nix                 |          |   ✓    |
| nqc                  | .nqc                 |          |   ✓    |
| objc                 | .mm                  |          |   ✓    |
| odin                 | .odin                |          |   ✓    |
| org                  | .org                 |          |   ✓    |
| pascal               | .pas                 |          |   ✓    |
| pascal               | .pp                  |          |   ✓    |
| pem                  | .pem                 |          |   ✓    |
| perl                 | .pl                  |          |   ✓    |
| perl                 | .pm                  |          |   ✓    |
| pgn                  | .pgn                 |          |   ✓    |
| php                  | .php                 |    ✓     |   ✓    |
| po                   | .po                  |          |   ✓    |
| po                   | .pot                 |          |   ✓    |
| pony                 | .pony                |    ✓     |   ✓    |
| powershell           | .ps1                 |          |   ✓    |
| powershell           | .psm1                |          |   ✓    |
| printf               | .printf              |          |   ✓    |
| prisma               | .prisma              |          |   ✓    |
| properties           | .properties          |    ✓     |   ✓    |
| proto                | .proto               |          |   ✓    |
| psv                  | .psv                 |          |   ✓    |
| purescript           | .purs                |          |   ✓    |
| pymanifest           | MANIFEST.in          |          |   ✓    |
| python               | .py                  |    ✓     |   ✓    |
| qmldir               | qmldir               |          |   ✓    |
| qmljs                | .qml                 |          |   ✓    |
| r                    | .R                   |    ✓     |   ✓    |
| r                    | .r                   |    ✓     |   ✓    |
| racket               | .rkt                 |    ✓     |   ✓    |
| re2c                 | .re2c                |          |   ✓    |
| readline             | .inputrc             |          |   ✓    |
| requirements         | requirements.txt     |          |   ✓    |
| ron                  | .ron                 |          |   ✓    |
| rst                  | .rst                 |          |   ✓    |
| ruby                 | .rb                  |    ✓     |   ✓    |
| rust                 | .rs                  |    ✓     |   ✓    |
| scala                | .sc                  |          |   ✓    |
| scala                | .scala               |          |   ✓    |
| scheme               | .scm                 |          |   ✓    |
| scheme               | .ss                  |          |   ✓    |
| scss                 | .scss                |          |   ✓    |
| smali                | .smali               |          |   ✓    |
| smithy               | .smithy              |          |   ✓    |
| solidity             | .sol                 |    ✓     |   ✓    |
| sparql               | .rq                  |          |   ✓    |
| sql                  | .sql                 |          |   ✓    |
| squirrel             | .nut                 |          |   ✓    |
| starlark             | .bzl                 |          |   ✓    |
| starlark             | BUILD                |          |   ✓    |
| starlark             | WORKSPACE            |          |   ✓    |
| svelte               | .svelte              |          |   ✓    |
| swift                | .swift               |    ✓     |   ✓    |
| tablegen             | .td                  |          |   ✓    |
| tcl                  | .tcl                 |          |   ✓    |
| thrift               | .thrift              |          |   ✓    |
| toml                 | .toml                |          |   ✓    |
| tsv                  | .tsv                 |          |   ✓    |
| twig                 | .twig                |          |   ✓    |
| typescript           | .ts                  |    ✓     |   ✓    |
| typescript           | .tsx                 |    ✓     |   ✓    |
| typst                | .typ                 |          |   ✓    |
| udev                 | .rules               |    ✓     |   ✓    |
| ungrammar            | .ungram              |          |   ✓    |
| uxntal               | .tal                 |          |   ✓    |
| verilog              | .sv                  |          |   ✓    |
| verilog              | .v                   |          |   ✓    |
| vhdl                 | .vhd                 |          |   ✓    |
| vhdl                 | .vhdl                |          |   ✓    |
| vim                  | .vim                 |          |   ✓    |
| vim                  | .vimrc               |          |   ✓    |
| vue                  | .vue                 |          |   ✓    |
| wgsl                 | .wgsl                |          |   ✓    |
| xcompose             | .XCompose            |          |   ✓    |
| xml                  | .svg                 |          |   ✓    |
| xml                  | .xml                 |          |   ✓    |
| xml                  | .xsl                 |          |   ✓    |
| yuck                 | .yuck                |          |   ✓    |
| zig                  | .zig                 |          |   ✓    |

<!--[[[end]]]-->
`````

## File: llms.md
`````markdown
---
title: Connecting to LLMs
nav_order: 40
has_children: true
description: Aider can connect to most LLMs for AI pair programming.
---

# Aider can connect to most LLMs
{: .no_toc }

[![connecting to many LLMs](/assets/llms.jpg)](https://aider.chat/assets/llms.jpg)


## Best models
{: .no_toc }

Aider works best with these models, which are skilled at editing code:

- [DeepSeek R1 and V3](/docs/llms/deepseek.html)
- [Claude 3.7 Sonnet](/docs/llms/anthropic.html)
- [OpenAI o1, o3-mini and GPT-4o](/docs/llms/openai.html)


## Free models
{: .no_toc }

Aider works with a number of **free** API providers:

- Google's [Gemini 1.5 Pro](/docs/llms/gemini.html) works with aider, with
code editing capabilities similar to GPT-3.5.
- You can use [Llama 3 70B on Groq](/docs/llms/groq.html) which is comparable to GPT-3.5 in code editing performance.
- Cohere also offers free API access to their [Command-R+ model](/docs/llms/cohere.html), which works with aider as a *very basic* coding assistant.

## Local models
{: .no_toc }

Aider can work also with local models, for example using [Ollama](/docs/llms/ollama.html).
It can also access
local models that provide an
[Open AI compatible API](/docs/llms/openai-compat.html).

## Use a capable model
{: .no_toc }

Check
[Aider's LLM leaderboards](https://aider.chat/docs/leaderboards/)
to see which models work best with aider.

Be aware that aider may not work well with less capable models.
If you see the model returning code, but aider isn't able to edit your files
and commit the changes...
this is usually because the model isn't capable of properly
returning "code edits".
Models weaker than GPT 3.5 may have problems working well with aider.
`````

## File: usage.md
`````markdown
---
nav_order: 30
has_children: true
description: How to use aider to pair program with AI and edit code in your local git repo.
---

# Usage

Run `aider` with the source code files you want to edit.
These files will be "added to the chat session", so that
aider can see their
contents and edit them for you.
They can be existing files or the name of files you want
aider to create for you.

```
aider <file1> <file2> ...
```

At the aider `>` prompt, ask for code changes and aider
will edit those files to accomplish your request.


```
$ aider factorial.py

Aider v0.37.1-dev
Models: gpt-4o with diff edit format, weak model gpt-3.5-turbo
Git repo: .git with 258 files
Repo-map: using 1024 tokens
Use /help to see in-chat commands, run with --help to see cmd line args
───────────────────────────────────────────────────────────────────────
> Make a program that asks for a number and prints its factorial

...
```

{% include help-tip.md %}

## Adding files

To edit files, you need to "add them to the chat".
Do this
by naming them on the aider command line.
Or, you can use the in-chat
`/add` command to add files.


Only add the files that need to be edited for your task.
Don't add a bunch of extra files.
If you add too many files, the LLM can get overwhelmed
and confused (and it costs more tokens).
Aider will automatically
pull in content from related files so that it can
[understand the rest of your code base](https://aider.chat/docs/repomap.html).

You can use aider without adding any files,
and it will try to figure out which files need to be edited based
on your requests.

{: .tip }
You'll get the best results if you think about which files need to be
edited. Add **just** those files to the chat. Aider will include
relevant context from the rest of your repo.

## LLMs

{% include works-best.md %}

```
# o3-mini
$ aider --model o3-mini --api-key openai=<key>

# Claude 3.7 Sonnet
$ aider --model sonnet --api-key anthropic=<key>
```

Or you can run `aider --model XXX` to launch aider with
another model.
During your chat you can switch models with the in-chat
`/model` command.

## Making changes

Ask aider to make changes to your code.
It will show you some diffs of the changes it is making to
complete you request.
[Aider will git commit all of its changes](/docs/git.html),
so they are easy to track and undo.

You can always use the `/undo` command to undo AI changes that you don't
like.
`````
