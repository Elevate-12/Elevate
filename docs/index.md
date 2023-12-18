# Elevate: model-enhanced LLM driven VUI testing on VPA apps

## Files Tree

```text
├── code
│   ├── main.py
│   ├── step1_process_document.py
│   ├── step2_test_skill.py
│   ├── step3_detect_problem.py
│   ├── model
│   │   └── ...
│   ├── skill
│   │   └── ...
│   └── util
│       └── ...
├── chrome
│   └── chromedriver_103
├── config
│   └── config000.ini
├── cookie
│   └── ...
├── corpus
│   └── ...
├── dataset_2022
│   ├── benchmark2022.xlsx
│   └── large_scale.xlsx
├── README.md
└── requirement.txt
```

* code: contains the source code of Elevate
* chrome: contains the chromedriver with version 103
* config: configuration files.
* dataset_2022: benchmark skills and large-scale skills
* requirement.txt: requirements of python packages

## Requirement

### python package
`pip install -r requirement.txt`

`python -m spacy download en_core_web_sm`

### environment

`export OPENAI_API_KEY=<YOUR OPENAI API KEY>`

### Other
1. the **chromedriver** that matches your chrome version
2. an **amazon developer account** that can use the simulator (change the config000.ini in config directory to add your acount)
3. an **Azure account** that can access to the openAI API (change the config000.ini in config directory to add the apibase and apiversion)
4. We need to login to the amazon developer account to start using the simulator. In most of the cases amazon will send an email to your linked email address for verifications. We use pop to read the emails, so make sure the **110 port** is open.

## How to run Elevate

```
cd code

python main.py 
    -i <begin index of skill, default as 1> 
    -ei <end index of skill, default as 100> 
    -c <configuration file name in the config directory, default as config000.ini> 
    -e <input skills file in the dataset_2022 directory, default as benchmark2022.xlsx> 
    -l <path to save communication logs and results, default as ../../output/benchmark_elevate> 
    -d <chromedriver name in the chrome directory, default as chromedriver_103>
```

## Result
* problem.txt: name of skills with problems
* problem<x\>.txt: name of skills with p<x\>


## Download
* [Elevate](tool/Elevate.zip) See the Elevate directory for running Elevate. Dataset can also be found here.
* [Output & Experiment](tool/experiment.zip) The data for experiments can all be found here. Approaches to run the experiments can be found [here](experiment.md).