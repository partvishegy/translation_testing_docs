# Run experiment config template
Here you can find a full template/example for the config file that is used to run an experiment.
For explanation of the parameters, see the Run experiment page (link).

```yaml
# Name of the experiment: str
# This will show up in the UI
# experiment_name: "Baseline-w-ProtoJudge"
experiment_name: "switch-test"

experiment_description: |
  This is the same as the baseline experiment, but with the ProtoJudge LLM enabled,
  So we can observe how well it works and what kind of feedback does it give.


# LLM configuration
llm:
  model: "gpt-4o"         # Model name str
  max_tokens: 1500        # Maximum number of tokens for the output
  temperature: 0.01       # Controls randomness (0.0 - deterministic, 1.0 - highly random)
  top_p: 1                # Cumulative probability threshold for nucleus sampling
  frequency_penalty: 0.0  # Frequency penalty
  presence_penalty: 0.0   # Presence penalty


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
# it must contain the placeholder for target language as well: {target_language}.
user_prompt: |
  Translate the following text to {target_language}:
  {source_text}


# Dataset information
dataset: "Whatever_your_dataset_name_is"  # Predefined datasets in langsmith database: str

# Specify langsmith splits to be used. They have to be defined in advance.
# This is an optional field. If not specified, all splits will be used.
splits: []
  # - "ERROR"
  # - "Hungarian"

# Scores to be computed
scores:
  with_reference: []     # List of scores to be computed WITH reference: list: str

  reference_free:        # List of scores to be computed WITHOUT reference: list: str
    - PROTO_JUDGE

system_fewshot: False
msg_fewshot: True
```


