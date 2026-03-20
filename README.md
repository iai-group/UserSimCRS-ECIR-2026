# UserSimCRS v2 - Case Study: Movie Recommendation

[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
![Python version](https://img.shields.io/badge/python-3.11-blue)

This repository is a fork of the original [UserSimCRS toolkit](https://github.com/iai-group/UserSimCRS) containing all configuration files to run a case study on simulation-based evaluation of Conversational Recommender Systems (CRS) trained for movie recommendation with multiple user simulators configured/trained with different datasets.

## Installation

UserSimCRS requires Python 3.11+.
The easiest way to install UserSimCRS and all of its dependencies is by using pip:

```shell
python -m pip install -r requirements/requirements.txt
```

To work on the documentation you also need to install other dependencies:

```shell
python -m pip install -r requirements/docs_requirements.txt
```

## Case Study: Movie Recommendation

### Conversational Recommender Systems

  * IAI MovieBot 1.0.1: rules-based CRS
  * CRSs from CRS Arena

IAI MovieBot implementation is available [here](https://github.com/iai-group/MovieBot/releases/tag/v1.0.1), while the implementations of CRSs from the CRS Arena are available [here](https://github.com/iai-group/iEvaLM-CRS).

### User Simulators

  * ABUS: agenda-based user simulator
  * SP-LLMUS: single prompt LLM-based user simulator
  * DP-LLMUS: dual prompt LLM-based user simulator

Configurations of the user simulators for the different datasets are available in the `data/movie_recommendation_case_study/config` folder of each dataset subfolder.

### Running Evaluation

The evaluation is performed in two steps:

1. Generate interaction data between the different CRS and user simulator pairs using the following command:

*IAI MovieBot* (config files starting with `config_iai_crs_`):

```bash
python -m usersimcrs.run_simulation \
    -c <path_to_config_file>
```

*CRSs from CRS Arena* (config files starting with `config_ievalm_`):

```bash
python -m usersimcrs.run_simulation \
    -c <path_to_config_file> \
    --agent_id <agent_id> \
    --agent_uri <agent_uri> \
    --output_name <output_name>
````

1. Compute evaluation metrics using the following command:

*User satisfaction*

```bash
python -m scripts.evaluation.satisfaction_evaluation \
        --dialogues <path_to_synthetic_dialogues>  
```

*Conversation quality aspects*

```bash
python -m scripts.evaluation.quality_evaluation \
    --dialogues <path_to_synthetic_dialogues>  \
    --ollama_config config/llm_interface/config_ollama_information_need.yaml \
    --output <path_to_save_results>
```

*Note*: The commands assume that you have already set up the environment and installed all necessary dependencies as described in the main README file.

### Results

The generated synthetic dialogues for ECIR 2026 submission are saved in the `data/movie_recommendation_case_study/synthetic_dialogues` folder, and the conversation quality aspects' scores are saved in the `data/movie_recommendation_case_study/results` folder.

## Publication

```bibtex
@inproceedings{Bernard:2026:ECIR,
  author = {Bernard, Nolwenn and Balog, Krisztian},
  title = {UserSimCRS v2: Simulation-Based Evaluation for Conversational Recommender Systems},
  year = {2026},
  booktitle = {Proceedings of the 48th European Conference on Information Retrieval},
  series = {ECIR '26}
}
```
