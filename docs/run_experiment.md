# Run experiment

## Overview
- langsmith uses an experiment to evaluate models.
- the experiments are run on the dataset created by the `create_dataset.py` script.
- The experiment is run by the `run_experiment.py` script.
- It takes a config, where you specify the model, the dataset, and the evaluation metrics.

## Usage

```python
python run_experiment.py --config_path config.yaml
```

## Config params
The config file specifies the dataset creation parameters. Below you can find the parameters that can be specified in the config file and their explanations, and examples of how to use them. You can also find a full template of a run_experiment config file [here](run_experiment_config_template.md).


### Name and description of the experiment
These fields will help you identify the experiment in the UI.
The full description appears when you hover over the experiment name.

Name is a single short string.
Description is a multiline string. Use the `|` character in the first line to indicate a multiline string as shown in the example below.

```yaml
# Name of the experiment: str
# This will show up in the UI
experiment_name: "Baseline"

experiment_description: |
  This experiment tries to capture the current setup of the BC system.
  It uses the 6x50_HUMAN_ref dataset. LLM Params and prompt are set to the current BC setup.
  Few-shot examples (general and language-speicific) are selected from the provided dataset.
  ...

```


### LLM configuration
This is a nested set of parameters that specify the configuration of the LLM model.
Currently only openai models are supported, but support for other models can be added in the future.
You can find more info on these parameters in the [OpenAI API documentation](https://platform.openai.com/docs/api-reference/chat/create).

```yaml
# LLM configuration
llm:
  model: "gpt-4o"         # Model name str
  max_tokens: 1500        # Maximum number of tokens for the output
  temperature: 0.01       # Controls randomness (0.0 - deterministic, 1.0 - highly random)
  top_p: 1                # Cumulative probability threshold for nucleus sampling
  frequency_penalty: 0.0  # Frequency penalty
  presence_penalty: 0.0   # Presence penalty
```

### System and User prompts (+ Few-shot examples)
The system prompt is the prompt used to instruct the LLM model on how to perform the translation.
The user prompt is the prompt requesting the actual translation: it must contain a placeholder for the source text and the target language! (See the example below!)
The few-shot examples are added as messages to the LLM model before the actual translation request.

```yaml
# Prompt for the LLM (keep the Pipe character at the beggining!)
system_prompt: |
  I am going to give you a template text which you need to translate to {target_language} language.
  The template contains parameters that will be replaced by the actual values later.
  You need to preserve the parameters in their original format so that everything within the square brackets needs to remain unchanged.
  The format of the template is as follows: [ broker name] The value in the square brackets is the name of the parameter,
  so you can use it to know what it will be replaced with. Preserve even spaces within the square brackets.
  If you see ', replace it with \" everywhere. If you see the phrase of in [year] in the brackets,
  translate it as in the [year] year. If you see the phrase at [Broker name] in the brackets,
  translate it as at [Broker name] broker. Use generic articles before the parameters in the brackets
  in the translated version. Use informal/casual language for the translated version,
  explain it as you would tell a friend.  You must preserve text in brackets in English.
  Your task is also to preserve the HTML tags in these formats: <tags></tags>.
  Allways return the translation only.


# This prompt is used for the few-shot examples and in the actual translation request
# It must contain the placeholder for the source text: {source_text}.
# If the target_language is not specified elswhere (i.e. the system prompt),
# it also must contain the placeholder for target language: {target_language}.
user_prompt: |
  Translate the following text to {target_language}:
  {source_text}

```
__Fewshot examples:__  
There are two ways to add fewshot examples to the prompts:

- As messages 
- As part of the system prompt

In the first case the fewshot examples are added as messages to the LLM model before the actual translation request.
This makes the examples part of the conversation. 
In the second case the examples are part of the system prompt, and are used to _instruct_ the model on how to perform the translation.

You can control these separately in the config file turning then on or off.
```yaml
system_fewshot: False

msg_fewshot: True
```

You can turn both of them on or off, they are independent.

In both cases the examples are collected from a dataset hosted in google docs.
And There are two types of examples in this dataset:

- General examples: These are examples that are not specific to any language. They are used for all translations.
- Language-specific examples: These are examples that are specific to the target language of the translation. They are used only for translations to that language.

The exmaples are currently hosted [here](https://docs.google.com/spreadsheets/d/1VnOkTNjqDIQy6mF8bifeyXq-i68d2rbNbDyJh-x4q9A/edit?gid=0#gid=0).
In order to access this dataset you need to provide the google service account credentials in a .json file.
You can set the path to this .json file as an environment variable `GOOGLE_APPLICATION_CREDENTIALS`.



### Dataset information
After defining the task for the LLM, you need to specify the dataset to be used.
For this you need to specify the name of the dataset as it appears in the langsmith database.
And you can also choose to run the experiment on a subset of the dataset by specifying the splits to be used.

```yaml

# Dataset information
dataset: "Your_dataset_name"  # Predefined datasets in langsmith database: str

# Specify langsmith splits to be used. They have to be defined in advance.
# This is an optional field. If not specified, all splits will be used.
splits: []
  # - "ERROR"
  # - "Hungarian"

```

### Scores
The scores field is used to specify the evaluation metrics to be computed.
There are two types supported:  

- with_reference: These are metrics that require a reference to compute the score. This will come handy during alignment of your judge models.
- reference_free: These are metrics that do not require a reference to compute the score.

Currently supported metrics are restricted to the PROTO_JUDGE metric and a few length metrics.
You can find the full list and documentation of the metrics in the [evaluators](evaluators.md) page.
This is how you specify the metrics in the config file:

```yaml
# Scores to be computed
scores:
  with_reference: []     # List of scores to be computed WITH reference: list: str
    - JUDGE_ON_REF

  reference_free:        # List of scores to be computed WITHOUT reference: list: str
    - PROTO_JUDGE
```