# Experiment

* Download [experiment](tool/experiment.zip) to do experiments.
* The elevate_10min, gpt4_chatbot_10min, vitas_10min show the communication logs for Elevate, GPT4(chatbot) and Vitas to test skills for 10 minutes.
* The ablation1, ablation2, ablation are for ablation studies.
* The elevate+llama2-70b-chat-hf_10min and llama2-70b-chat-hf_chatbot_10min are communication logs of Elevate-Llama2-70b-chat and Llama2-70b-chat(chatbot) respectively.
* The large_scale contains the communciation logs of skills in large-scale dataset.
* The user contains the commucation logs of our user study.
* The experiment.py file is the source code to do experiments.

## Study1
Figure 7 shows the comparison of sentence states and semantic states covered by Elevate, GPT4(chatbot) and Vitas.
Figure 8 shows the average semantic state coverage rate with varying interaction rounds between Elevate and baselines.
It is important to note that the results here show the communication logs of each tool testing 10 minutes.
But in the study 1 we compare the semantic state coverage rate of each tool testing for the same number of interaction rounds.

To get the semantic/sentence states and average semantic state coverage rate achieved by Elevate and the baselines, run:

```shell
python experiment.py 1
```

The results will be shown in ``study1_1_.xlsx`` and ``study1_2.xlsx``.

## Study2
Figure 9 shows the average semantic state coverage rate by Elevate, w/o States extraction, w/o Input events generation and w/o State space exploration.
To get the raw data of the ablation study, run:

```shell
python experiment.py 2
```

The result will be shown in ``study2.xlsx``.

## Study3
Figure 10 shows the average semantic state coverage rate achieved by Elevate-Llama2-70b-chat, Vitas and Llama2-70b-chat(chatbot).

To get their state coverage rate on the benchmark, run:

```shell
python experiment.py 3
```

The result will be shown in ``study3.xlsx``.

## Study4
Figure 11 shows the comparison of coverage rate achieved by Elevate and Vitas on 4000 skills from all categories.

To get the coverage rate of Elevate and Vitas on these 4000 skills, run:

```shell
python experiment.py 4
```

The result will be shown in ``study4.xlsx``.

