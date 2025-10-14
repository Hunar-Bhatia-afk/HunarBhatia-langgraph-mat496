# HunarBhatia-langgraph-mat496
Here we'll be doing the Intro To LangGraph course module by module.
Module 1-Langgraph by Hunar Bhatia
## ***Lesson 1: Motivation***

A solitary model alone is fairly limited as it does not have any access to tools, external content and multi-step workflows. So many applications use control flow before and after LLM calls which could be tool calls, retrieval steps and so forth. This control flow forms a chain which you can think of as some set of steps before and after an LLM call.
Now, the nice thing about chains is that they are very reliable so the same set of steps occurs whenever a chain is invoked. But, we do want LLM systems that can pick their very own control flow for certain kinds of problems. This is really what an agent is →
<img width="864" height="293" alt="image" src="https://github.com/user-attachments/assets/3de0a905-d487-455e-a9ed-a6e51bf4322f" />
Agent→ Its control flow that is actually defined by an LLM.

So, you have chains which are fixed control flows set by developers and you have agents which are LLM defined control flows.
Now, the thing to think about is that there are many different kinds of workflows so you can think about dialing the control you give to the LLM from low to high. In a router an LLM chooses to perform a single step in a flow and it may choose between a narrow set of options.
On the other hand, you have a fully autonomous agent that can choose a sequence of steps through some set of given options, or even it can generate its own steps it can take which is basically auto-generate its next own move based on some potentially available resources.
<img width="866" height="467" alt="image" src="https://github.com/user-attachments/assets/302d9250-92d8-4a04-aff4-43a5d8efd1d0" />
But, we do have some practical challenges here→ 

We’ve seen as you ramp up the level of control you give to the LLM, the reliability drops so going from something simpler like a router to going to something much more complicated like a fully autonomous agent, the application does degrade in terms of reliability.
<img width="769" height="563" alt="image" src="https://github.com/user-attachments/assets/0edbfb3f-cf29-46f0-abb0-9dcee57c45d4" />
So, this is where Langgraph comes into use → It helps use bend the curve by allowing us to build agents that maintain reliability, even as you push out the level of control you actually give to the LLM or Agent.

<img width="768" height="574" alt="image" src="https://github.com/user-attachments/assets/35c8b285-670a-49dd-86e3-4c3cdeb9406b" />

Let’s get started with → 

<img width="404" height="105" alt="image" src="https://github.com/user-attachments/assets/00dd192b-016b-49b7-8436-74f8435b78de" />
In many applications, we want to kind of combine developer intuitions with LLM control, and so you can very easily specify in your application certain steps that you always want to be fixed. We always start at step one and end at step two but we can also inject LLM at certain points turning it into an agent. These are expressed as graphs which contain nodes that represent the steps in our application and edges are just the connectivity between the nodes and there’s a lot of flexibilty for how you lay out nodes and edges. 
<img width="862" height="207" alt="image" src="https://github.com/user-attachments/assets/3fc47130-131b-458b-a125-90850543f88b" />
So, there’s certain pillars in Langgraph that help us achieve the goals that we are talking about here→

- Persistence
- Streaming
- Human-in-the-loop
- Controllability

These pillars are going to be the cornerstones of the modules in this course and we’ll be going through these a lot as we proceed in this code.
<img width="719" height="112" alt="image" src="https://github.com/user-attachments/assets/e08fcc7b-6d4e-4499-85f5-830885299da8" />
____________________________________________________________________________________________________________________________________
