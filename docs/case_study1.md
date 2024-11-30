# Case study
In this case study, we select three real world cases of VPA apps.
Each of the cases demonstrates the effectiveness of Elevate's one agent.

## Case 1
This is an Alexa skill called Action movies.
The skill's main functionality is to recommend different movies.
Here is a piece of communication logs between Action movies and Elevate.

```text
Skill: Ok, Here's Action movies.Welcome to action movies. Do you want me to tell you a movie?
Elevate: help
Skill: You can ask me to recommend a movie answering yes or no. Do you want me to recommend a movie?
Elevate: yes
...
Skill: Welcome to action movies. Do you want me to tell you a movie?
Elevate: no
```

Below shows the recording of **Observer**'s prompts and outputs related to the above communication log.
Only \*Short Prompt\* is displayed.

```text
Short Prompt:
    Input: sentence: "Do you want me to tell you a movie?", state list: ['<START>']
    Output:
Observer:
    Do you want me to tell you a movie?
Short Prompt:
    Input: sentence: "Do you want me to recommend a movie?", state list: ['<START>', 'Do you want me to tell you a movie?']
    Output:
Observer:
    Do you want me to tell you a movie?
```

In this case, "Do you want me to tell you a movie?" and "Do you want me to recommend a movie?" are semantically similar outputs.
Observer correctly maps the output "Do you want me to recommend a movie?" to an existing state "Do you want me to tell you a movie?".
Therefore, Elevate recognizes the three skill's outputs as one state, so it uses different input events to efficiently trigger skill's different functionalities. 


## Case 2
This is an Alexa skill called Age Calculator.
The skill's main functionality is to calculate the number of days between two dates.
Here is a piece of communication logs between Age Calculator and Elevate.

```text
Skill: Handing off to Age Calculator by sayak.Age Calculator. You can ask me to calculate the amount of time between now and a specific date.What date would you like to use?
Elevate: tommorrow
Skill: The length of time between now and xxx is about xxx.
...
```

Below shows the recording of **Generator**'s prompts and outputs related to the above communication log.
Only \*Short Prompt\* is displayed.

```text
Short Prompt:
    Input: skill: "What date would you like to use?"
    Output:
Generator:
    ["today", "tomorrow", "July 4", "December 25", "next Monday"]
```

The output "What date would you like to use?" expects a specifc date as inputs.
Otherwise, this skill will return information like "I can only calculate based on exact dates.".
Without domain-specifc knowledge of correct "date", invalid test cases will be generated.
However, most NLP based testers have difficulty in generating valid test cases in such scenarios.
As a comparison, Generator correctly generates five valid dates, benefited from the LLM.
Therefore, Elevate's testing based on such high quality test cases is much more effective. 

## Case 3
This is an Alexa skill called Asthma Device Helper.
The skill's main functionality is to provide usage instructions.
Here is a piece of communication logs between Asthma Device Helper and Elevate.

```text
Skill: Welcome to Asthma Device Helper. You can ask a question like, what are the instructions for using a proair inhaler. ... Now, what can I help you with?
Elevate: help
Skill: You can ask questions such as, what's the instructions, or, you can say exit...Now, what can I help you with?
Elevate: what are the instructions for using a proair inhaler
Skill: When first using your proair inhaler...
```

Below shows the recording of **Planner**'s prompts and outputs related to the above communication log.

```text
Short Prompt:
    Input: <current state>="You can ask questions such as, what's the instructions, or, you can say exit...",FSM={Σ={"what's the instructions":0,"exit":0,"what are the instructions for using a proair inhaler":0,"help":1,"pause":0,"resume":0,"stop":0,"what's the time":0},δ=([<current_state>,"help",<current_state>])}.
    Thought:
Planner:
    ...
    Output: "what are the instructions for using a proair inhaler"
```

The state "You can ask questions such as, what's the instructions, or, you can say exit..." has many candidate input events, including semantic relevant inputs like "what's the instructions", "what are the instructions for using a proair inhaler" and "help", and semantic irrelevant inputs like "exit", "pause", "stop", etc..
Planner considers the semantic relevance, history invocation times and transitions to make exploration decisions.
Consequently, semantic irrelevant inputs have lower priority.
Among semantic relevant inputs, "help" was chosen once and is invalid, so it is removed.
From the left two choices, Planner selects "what are the instructions for using a proair inhaler" as the final input.
Without considering semantic relevance, "exit" will have the same priority as "what are the instructions for using a proair inhaler" and "what's the instructions".
If "exit" is selected, the skill will terminate, affecting the testing continuity.