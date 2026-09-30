# Lec 22 : Interpreting LLMs (CoT)

**CoT aka Chain of Thought Prompting**
Was intoduced in a paper from 2022.

## Introductions

Scaling up LLMs improvesd many NLP tasks but not multi-step reasoning
- arithmetic reasoning
- common sense
- symbolic

>[!NOTE]
> All the above steps required models to execute a sequence of steps.

## Methods

- Change what the `few-shot examplars` look like:
instead of $<Questions , Answers>$.
Use $<Questions ,Chain of Thought, Answers>$

> More detail method description in slide.
> with examples.

---

## Key findings

#1
- CoT prompting works well in large models and not in small models
- ie benefits only appear for above 100+ billion parameters.

- CoT does not give a model a reasonign ability that it doesn't already possess. It provides a way to elict and externalize multi-step reasoing.

#2
- Bigger gains on Harder Problems
- Performs well in multistep cases.

>[!IMPORTANT]
> CoT makes these intermediate results explicit

#3
- Beats Fine-Tuned Models, with Zero Training
- Strinking because the fine-tuned competetiors needed labeled training set and extra memory.

---

## Robustness Check

How much the way of people writing the prompt on CoT ?

From result they note that overall performance still better than normal prompting and even though there is minor difference based on people.

> [!NOTE]
> The effect comes from step by step reasoning , not the prompt quality.

---

## Limitations

- Doesn't resolves whether the model is "actually reasoning" : explicitly open questions.
- "No guarentee of correct reasoning paths" - a chain of thought can look plausible and still wrong.
- Only works at large model scale - costly to serve in practice.
- Annotation cost is low for prompting (~8 examples) but wouldn't scale cheaply.

---
