# Lab 05 — Conclusion

<!--
Read this before you write.

This file is graded by your instructor, by hand, and it is worth 6 of the 15 points.

- Type it yourself. Do not use a chatbot, an AI assistant, an AI autocomplete or a "rewrite my
  text" tool for any part of it, not even for grammar. English that is plain and a little
  clumsy but yours is better than polished text that is not.
- Use your own numbers. Every number you quote must be one your own notebook printed. Copy
  the number exactly as it appeared. A number your notebook did not print is marked as wrong.
- Answer in your own words, about 80-200 words per question. A short answer that uses your
  numbers beats a long answer that could have been written by anyone.
- Delete these comment lines if you like. Keep the three headings and the last line.
-->

## 1. Which layer changed your results the most?

Give the numbers before -> after for that layer (correct / wrong / no answer, out of 20) and say
whether it changed what the agent *can do* or only what it *admits*. If no layer changed the
number of correct answers, say so and explain what the layers did change instead.

Answer:
Layer 1 (tool errors go back to the model) changed my results the most.

In the bare version, I got 16 correct answers, 4 wrong answers and 4 questions with no answer. After adding Layer 1, I got 18 correct answers, 6 wrong answers and 0 questions with no answer.

This layer helped the agent continue working after a tool error instead of crashing. It fixed two alias questions (q23 and q24), but q21 and q22 were still incorrect.

Layer 2 did not change the results because the agent never reached the step limit or repeated a tool call. Layer 3 reduced wrong answers from 6 to 4, but two questions (q18 and q22) became UNVERIFIED. The number of correct answers stayed at 18.

## 2. One failure the full harness did not fix

Pick one question that the full harness (layer 3) still gets wrong or refuses. Give its id
(q01 ... q20). Open its trace in the notebook: at which step did it go wrong, and what did
the model do instead? Name one thing you would add to the harness, and what it would cost
(extra model calls, tokens, time, or a new way to fail).

Answer:
Question q18 asked how many percentage points CPI in Jambyl decreased between 2022 and 2025.

The tool returned the correct values: 16.1 in 2022 and 10.6 in 2025. The correct calculation was 16.1 - 10.6 = 5.5 percentage points.

However, the model divided the result by 100 and got 0.055. It then gave 0.55 as the answer. The grounding layer detected the mistake, but the model repeated it, so the final result was UNVERIFIED.

I would add a rule to the system prompt saying that percentage-point changes are calculated by simple subtraction, without dividing by 100. This fixed q18 in my test. However, token usage increased by about 24%, and q14 changed from correct to no answer. So, adding instructions can sometimes create new problems.



## 3. Workflow or agent for the monthly ranking?

Dala Insight wants to publish, every month, "which region is highest and which is lowest on one
indicator in one year". Would you ship the fixed workflow or the agent? Use the accuracy, the
number of model calls, the tokens and the time you measured in Part 1. Then say in one sentence
what kind of question would make you choose the other one.

Answer:
I would choose the fixed workflow for monthly ranking questions.

In my test with 15 questions, the workflow got 13 correct answers, while the bare agent got 9. The workflow used 3,852 tokens and 15 model calls, compared to 19,835 tokens and 26 calls for the agent. It was also faster: 17.4 seconds instead of 47.2 seconds.

The workflow is better because monthly ranking questions always have the same structure: choose an indicator and year, then find the highest and lowest values.

However, I would use an agent for more complicated questions that require several steps, such as calculating percentage changes between years. These questions need more flexibility than a fixed workflow can provide.
---

I wrote this file myself, without any AI tool.
