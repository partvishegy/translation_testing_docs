# Validators

Validations are used to check if the input configs are correct.
In case they don't match the expected format, the validator will raise an error providing a helpful description of the error.

Validators are organized into a separate module under `src/validators.py`. They are built using the `pydantic` python library. Validators are applied to dataset creation and experiment configuration files.

#### Adding new evaluators

If you want to add new evaluators, after defining them in `src/evaluators.py` you will also need to add them to the `src/validators.py` module.
```python
# Enums for restricted list of scores
class WithReferenceScore(str, Enum):
    JUDGE_ON_REF = "JUDGE_ON_REF"
    REF_CHAR = "REF_CHAR"


class ReferenceFreeScore(str, Enum):
    PROTO_JUDGE = "PROTO_JUDGE"
    CHAR_COUNT = "CHAR_COUNT"
    IN_CHAR_COUNT = "IN_CHAR_COUNT"
    CHAR_DIFF = "CHAR_DIFF"
```

The enum keywords need to be connected to function names at the end of `src/evaluators.py` module.  

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