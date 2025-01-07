# Create Dataset

## Overview
- langsmith uses a dataset to evaluate models.
- The dataset is created by the `create_dataset.py` script which is also an entry point.
- It takes a config

## Usage
- `python create_dataset.py --config_path config.yaml`

## Config params
The config file specifies the dataset creation parameters.
Below you can find the parameters that can be specified in the config file and their explanations, and exampels of how to use them. You can also find a full template of a dataset creation config file [here](create_dataset_config_template.md).


### Name and description of the dataset
These fields will help you identify the dataset in the UI.
The full description appears when you hover over the dataset name.

Name is a single short string.
Description is a multiline string. Use the `|` character in the first line to indicate a multiline string as shown in the example below.

```yaml
dataset_name: "GoodDataset"

dataset_description: |
  This is indeed a very good dataset.
  It contains many examples of good translations.
  And this is a multiline string. Add any details you may need in the future!

```

### Source file
The path to the source file that contains the dataset. Also provided as a string.
The source file can be in .csv or .tsv format.
```yaml
source_file: "data/raw/gold_data/HUMAN_UNIFIED_6x50_long.csv"
```

### Mandatory fields
These fields are required for the dataset creation process.
You should provide the field names as they appear in the source .csv/.tsv file.
They are:

- __input_field__
- __target_language_field__
- __reference_output__ (you can write an empty string "" if not available, this indicates no-reference dataset)
```yaml
# Name of the input field in the dataset: str (Original text to be translated)
input_field: "source_text"

# Name of the field in the dataset that conatins target language for each example: str
target_language_field: "target_language"

# Name of the output field in the dataset: str (Reference translation - write "" if not available)
reference_output: "target_text"
```

### Limit
Sometimes you may want to experiment with a subset of the whole dataset. For faster iteration without spending time on selecting sepcific examples.
The limit field is an integer. Set it to a number n to use the top n rows of the dataset. If you set it to 0, the whole dataset will be used, using a number larger then the dataset size will also use the whole dataset.

```yaml
limit: 0
```
The limit field came handy during early development and testing. But you can also use existing splits for a slower but more controlled workflow.

[Create Dataset Template](create_dataset_config_template.md)


