# Create Dataset config template

Here you can find a full template/example for the config file that is used to create a dataset.
For explanation of the parameters, see the [Create Dataset](create_dataset.md) page.

```yaml
# Name of the dataset: str
# This will show up in the UI along with the description
dataset_name: "6x50_HUMAN_ref"

dataset_description: |
  This is the current best dataset with reference for Hungarian, Arabic, Serbian, Croatian, Russian, Danish.
  It contains nearly 50 translation expressions for each language.

source_file: "data/raw/gold_data/HUMAN_UNIFIED_6x50_long.csv"

# Name of the input field in the dataset: str (Original text to be translated)
input_field: "source_text"

# Name of the field in the dataset that conatins target language for each example: str
target_language_field: "target_language"

# Name of the output field in the dataset: str (Reference translation - write "" if not available)
reference_output: "target_text"

# If you want to experment with a subset of the whole dataset. I takes the top n rows.
# Setting it to 0 will use the whole dataset.
limit: 0

```





