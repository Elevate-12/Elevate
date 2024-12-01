# Case study
In this case study, we select four real world cases of VPA apps.
The first two cases reflect the effectiveness of Elevate's agent on problems detection and state space coverage.
The last two cases show the limitation of Elevate.

## Case 1
### **Generator** generates diverse and context-related inputs which benefit new states coverage and problems detection
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
### **Observer** merges semantic similar outputs to avoid repeated tests, while **Planner** understands semantics to choose the best input
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
### Speeches in audio files are not adequately analyzed
Elevate runs on the simulator, whose outputs are mostly in the form of text.
However, in rare circumstances, the simulator returns an audio file and outputs the text <Short audio\>.
Both Elevate and Vitas do not parse the audio files, but these files do contain information such as speeches or music.
For example, a skill called Butterball only returns an audio file after launching.
The log looks like:

```text
Elevate/Vitas: Alexa open butterball
Skill: <Short audio>.
Elevate/Vitas: help
```

Actually, in the audio file, it says "Welcome back to butterball. I'm Beth. We can help you with planning, preparing, cooking or enjoying a turkey. What can I help with?".
As Elevate and Vitas do not parse the audio file for text information, the context-related inputs ["planning a turkey", "preparing a turkey", "cooking a turkey", "enjoying a turkey"] cannot be generated.
Meanwhile, Observer in Elevate and Vitas merge all audio files into one state because they are all in the text form <Short audio\>.
As a result, the information embeded in the audio file is not adequately used, resulting in insufficient testing of subsequent behaviors.


## Case 4
### Music in audio files is not parsed
Another example is a skill called Song Quiz.
Its functionality is to randomly play music and ask users to guess the title and artist.
The music is uploaded in an audio file.
The simulator plays the audio file and returns the <Short audio\> text.
There is the communication log betweeen Song Quiz and testers.

```text
Skill: Starting your 60s game.  Your opponent is Sam from Edinburgh.  I'll play 5 short clips. Guess the song title or artist, or you can guess both for bonus points.     Question 1 , for 10 points. Name the song title and artist:.<Short audio>.
Elevate: shape of you by ed sheeran / Vitas: artist
```

In such cases, testers must understand the music to generate inputs, which is difficult for current VPA apps testers to handle.
Although Elevate cannot analyze the audio file to get the correct answer, it can learn from the context to acquire the domain of answers.
Consequently, its input is related to the song title and artist, although it is not the right one.
As a comparison, Vitas just inputs "artist", which is not a valid answer.