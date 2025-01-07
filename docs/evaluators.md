# Evaluators

Evaluators are functions used by the langsmith evaluator to evaluate the performance of a model on a dataset. The evaluator functions are defined in the `src/evaluators.py` module. 

Here we will list the evaluator functions and explain their goals.

## Usage
Using an evaluator function is simple. Just add the name of the evaluator function to the `scores` field in the config file.
Under the `scores` field, there are two subfields: `with_reference` and `reference_free`.
The `with_reference` field is a list of evaluator functions that require a reference to compute the score. The `reference_free` field is a list of evaluator functions that do not require a reference to compute the score.

Here are the list of currently supported optional evaluators:
```yaml
scores:
  with_reference:        # List of scores to be computed with reference: list: str
    - REF_CHAR
    - JUDGE_ON_REF
  reference_free:        # List of scores to be computed without reference: list: str
    - PROTO_JUDGE
    - CHAR_COUNT
    - CHAR_DIFF
```
You can copy-paste this into your config file and add/remove the names of the evaluator functions you want to use.

There are also some mandatory evaluators that are always computed. These are:

- `html_retention`
- `placeholder_retention`

Youn don't need to add these to the config file, they are always computed.
In fact adding them to your config will throw an error during config validation.
 

## Evaluator functions
For each evaluator function you can find the implementation in the `evaluators.py` module.
Each evaluator function has a docstring that explains what the evaluator does.
Each evaluator function has an alias in an ENUM class that defines the name of the evaluator. The class is used to identify the evaluator in the config file.

### Optional evaluators
Here is the mapping between the evaluator functions and their aliases used in the config file:

```python
SCORE_FUNCTIONS = {
    ReferenceFreeScore.PROTO_JUDGE: prototype_LLM_judge,
    WithReferenceScore.JUDGE_ON_REF: JUDGE_on_reference,
    WithReferenceScore.REF_CHAR: evaluate_reference_length,
    ReferenceFreeScore.CHAR_COUNT: evaluate_length,
    ReferenceFreeScore.IN_CHAR_COUNT: evaluate_input_length,
    ReferenceFreeScore.CHAR_DIFF: evaluate_length_diff,
}
```

#### PROTO_JUDGE
This evaluator computes the PROTO_JUDGE score. The PROTO_JUDGE score is a reference-free metrics defined in the `judges.py` module. It is currently a prototpye, not fully alligned to the preferences of BC. There is work to be done here.
The prototype currently uses the gpt-4o model to compute the PROTO_JUDGE score. It should support other models in the future. For this support to be added in a scalable way, the factory or buillder pattern should be used. Currently this is just a wrapper that is imported directly from the judges.py module into an evaluator function. This is not the most flexible way to do this, but it is an OK start.


#### JUDGE_ON_REF
This evaluator computes the same as the PROTO_JUDGE score, but on the reference translation. The same statements are true for this evaluator as for the PROTO_JUDGE evaluator. It should be used during alignment of the PROTO_JUDGE metric.

#### REF_CHAR
This evaluator computes the number of characters in the reference translation. It is a with-reference evaluator, and requires a reference to compute the score.

#### CHAR_COUNT
This evaluator computes the number of characters in the translation. It is a reference-free evaluator, and does not require a reference to compute the score.

#### IN_CHAR_COUNT
This evaluator computes the number of characters in the input text. It is a reference-free evaluator, and does not require a reference to compute the score.

#### CHAR_DIFF
This evaluator computes the difference in the number of characters between the translation and the original input. It is a reference-free evaluator, and does not require a reference to compute the score.



#### Mandatory evaluators

The mandatory evaluators are always computed and are not optional. They are:

- Placeholder retention: This evaluator checks if the placeholders in the source text are retained in the translation.
- HTML retention: This evaluator checks if the HTML tags in the source text are retained in the translation.

They both look for exact matches.

