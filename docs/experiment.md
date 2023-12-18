# Experiment

* Download [experiment](tool/experiment.zip) to do experiments.
* The elevate_10min, gpt4_chatbot_10min, vitas_10min show the communication logs for Elevate, GPT4(chatbot) and Vitas to test skills for 10 minutes.
* The elevate12+vitas3_10min, elevate13+vitas2_10min, elevate23+vitas1_10min are for ablation studies.
* The elevate+llama2_10min and llama2_chatbot_10min are communication logs of Elevate+Llama2_13b and Llama2-13b(chatbot) respectively.
* The large_scale contains the communciation logs and problem lists of skills in large-scale dataset.
* The user contains the commucation logs of our user study.
* The experiment.py file is the source code to do experiments.

## Study1 and Study2
Figure6-8 show the comparison of state space coverage between Elevate, GPT4(chatbot) and Vitas.
Figure9-10 show the distribution of interaction rounds between Elevate and GPT4(chatbot), Elevate and Vitas respectively.
It is important to note that the results here show the communication logs of each tool testing 10 minutes.
But in the study1 we compare the state space coverage of each tool testing for the same number of interaction rounds.

To get the state coverage and efficiency achieved by Elevate and the baselines, run:

```shell
python experiment.py 1
```

The result will be shown in ``study1&2_coverage_efficiency_gpt4_elevate.xlsx``, ``study1&2_coverage_efficiency_vitas_elevate.xlsx`` and ``study1&2_coverage_efficiency_vitas_gpt4.xlsx``.

## Study3
Table 5 show the number of problems detected by Elevate and the baselines.
The problem lists are shown in the ``result`` directory.
To get the problems detected by Elevate and the baselines, run:

```shell
python experiment.py 3
```

The result will be shown in ``study3_problems.xlsx``.

## Study4
Figure 11-13 show the comparison of state space coverage between Elevate and Elevate(*P<sub>input<sub>*+*P<sub>explr<sub>*), Elevate(*P<sub>state<sub>*+*P<sub>explr<sub>*) and Elevate(*P<sub>state<sub>*+*P<sub>inputsub>*).

To get their state space coverage, run:

```shell
python experiment.py 4
```

The result will be shown in ``study4.1_ablation_1.xlsx``, ``study4.2_ablation_2.xlsx`` and ``study4.3_ablation_3.xlsx``.

## Study5
Figure 14 shows the comparison of state space coverage between Gavin+Llama2-13b and Llama-13b(chatbot).

To get their state space coverage, run:

```shell
python experiment.py 5
```

The result will be shown in ``study5_coverage_llama2_elevate.xlsx``.

## Study6
Table 6 shows the landscape of problems on the large-scale dataset.
The problems are recorded in the ``result`` directory of each category's directory.
To get the distribution of problems, run:

```shell
python experiment.py 6
```

The result will be shown in ``study6_problems.xlsx``.

## Discussion
Figure 15 shows the comparison of state space coverage between user, Elevate and Vitas.
To get their state space coverage and the intersactions, run:

```shell
python experiment.py 7
```

The result will be shown in ``discussion_coverage_user_vitas_elevate.xlsx``.
