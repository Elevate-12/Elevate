# Case study
In this case study, we select four real world cases of VPA apps.
The first two cases reflect the effectiveness of Elevate's agent on problems detection and state space coverage.
The last two cases show the limitation of Elevate.

## Case 1
This is an Alexa skill called My Horoscope.
The skill's main functionality is to show insights and forecasts.
Here is a piece of communication log between My Horoscope and Elevate.

```text
...
Skill: My Horoscope provides your daily horoscope along with insights into love, career, and money. For example, to get the horoscope for Capricorn, say, "Alexa, what is the horoscope for Capricorn?". To change your default sign, say, "Alexa, change my default zodiac sign.". Which zodiac sign your need the horoscope for?
Elevate: alexa, change my default zodiac sign.
Skill: Which zodiac sign do you want to set as default?
Elevate: aries
Skill: Oh an Aries, a beautiful sign for a beautiful soul....Would you like to start your sevenday free trial?
Elevate: yes
Alexa: Sorry, I am unable to fulfill your request on this device. <unexpected exit>
```

This is the corresponding communication log between My Horoscope and Vitas.

```text
...
Skill: My Horoscope provides your daily horoscope along with insights into love, career, and money. For example, to get the horoscope for Capricorn, say, "Alexa, what is the horoscope for Capricorn?". To change your default sign, say, "Alexa, change my default zodiac sign.". Which zodiac sign your need the horoscope for?
Vitas: change my default zodiac sign.
Skill: Which zodiac sign do you want to set as default?
Vitas: transform
Skill: Sorry. I missed that....What sign do you want the horoscope for?
Vitas: confidence
Skill: Sorry. I missed that....What sign do you want the horoscope for?
Vitas: transform
Skill: Sorry. I missed that....What sign do you want the horoscope for?
Vitas: reinforces
Skill: Sorry. I missed that....What sign do you want the horoscope for?
...
```

In this case, Vitas stucks on the state "What sign do you want the horoscope for?" because it cannot generate context-related inputs.
Consequently, Vitas cannot discover new states or problems after that.
As a comparison, the **Generator** agent generates several context-related inputs (["Aries", "Taurus", "Gemini", "Cancer", "Leo", "Virgo", "Libra", "Scorpio", "Sagittarius", "Capricorn", "Aquarius", "Pisces"]) for this state , and the new state "Would you like to start your sevenday free trial?" is discovered.
After that, the unexpected exit problem is found.

## Case 2
This is an Alexa skill called Action movies.
The skill's main functionality is to recommend different movies.
Here is a piece of communication logs between Action movies and Elevate.

```text
...
Skill: You can ask me to recommend a movie answering yes or no. Do you want me to recommend a movie?
Elevate: yes
Skill: Terminator. I tell you another movie.
Elevate: yes
Skill: Terminator. I tell you another movie.
Elevate: action
Skill: James Bond. I tell you another one.
Elevate: title
Alexa: I missed something. Can you say it again. <unexpected exit>
```

This is the corresponding communication log between Action movies and Vitas.

```text
...
Skill: You can ask me to recommend a movie answering yes or no. Do you want me to recommend a movie?
Vitas: recommend a movie answering yes
Skill: Black Panther. I tell you another movie.
Vitas: recommend a movie answering yes
Skill: Rambo. I tell you another one.
Vitas: no
Skill: Bye.
<Skill exits normally>

...
Skill: Total Recall. I tell you another movie.
Vitas: recommend a movie answering yes
Skill: Die Hard. I tell you another one.
...
```

In this case, **Observer** merges "I tell you another movie." and "I tell you another one" as the same semantic state.
**Planner** learns from the behavior model that the inputs "yes" and "action" cannot help discover new states or new problems.
Also, **Planner** understands the potential results of the input "no", which is to end the communication.
Finally, it chose "title" as input when visiting the same state the third times.
After that, a new unexpected exit problem is found.
As a comparison, Vitas considered "I tell you another movie." and "I tell you another one" as two different states.
So "recommend a movie answering yes" was considered as a good input as it helped discover new states.
We can see from the log that "recommend a movie answering yes" was chosen three times, which means that the same functionality was tested three times.
Despite that, Vitas chose the input without considering semantic relevance.
That is the reason why it chose "no" as the input and the communication ended.

This case proves our assumptions.
Firstly, ignoring sematic relevance when extracting states will affect the testing efficiency.
This is the motivation of designing **Observer**.
Secondly, semantic relevance is important when choosing inputs.
This drawback is handled by **Planner**, who considers context-relevance and history testing results when making testing decisions. 

## Case 3
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