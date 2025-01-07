# BC Translation Evaluator Documentation Overview

There are two main entry points for the two main functions of the tool:

- `create_dataset.py` creates a dataset for the translation evaluation task.
- `run_experiment.py` runs the translation evaluation experiment.

## Usage
__Currently the tool is run form a poetry environment. But will be moved to docer shortly.__

### Set up environment
- To set up the environment, run the following command in your terminal:
```bash
$ poetry install
```
If you have poetry installed just move to the main forlder of the repo, and run the command:
```bash
$ poetry shell
```
Now your environment is set up. And you can start creating a dataset or running an experiment.

### Create Dataset
- To create a dataset, run the following command:
```bash
$ python create_dataset.py --config_path config.yaml
```
For more information on the config file, see the [Create Dataset](create_dataset.md) page.
You can also find a template for the config file in the git repository under the `configs/dataset_configs/` folder,
or in the [create_dataset config template](create_dataset_config_template.md) page of the documentation.


### Run Experiment
- To run an experiment, run the following command:
```bash
$ python run_experiment.py --config_path config.yaml
```
For more information on the config file, see the [Run Experiment](run_experiment.md) page.
You can also find a template for the config file in the git repository under the `configs/experiment_configs/` folder,
or in the [run_experiment config template](run_experiment_config_template.md) page of the documentation.

The results will be collected on BC's langsmith server, and you will be able to see them in the UI.



## Project layout
    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.

    Usage:
        Create_Dataset.md
        Run_Experiment.md

    Configuration templates:
        create_dataset_config_template.yaml
        run_experiment_config_template.yaml

    Modules:
        evaluators.md  # Details of evaluator functions
        validators.md  # Details of validators
        helper.md  # Details of helper functions