# HunarBhatia-LangGraph-mat496
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


## *Lesson 2- Simple Graph*

So let’s build a simple graph to introduce the core components of LangGraph.

<img width="868" height="358" alt="image" src="https://github.com/user-attachments/assets/60766bb7-cdca-4cd7-ad49-74e20b53f723" />

The edge between start → node1 is a normal edge. The next edge from node1 to node2 and node3 is called a conditional edge meaning that based on some condition we define, we either choose the branch leading to node2 or node3.
Next, we install the langgraph module and then we define the state that is baiscally the object that we pass between the nodes and edges of our graph. Here, we define the state as a simple dictionary and it is going to have one key graph_state which is shown as follows-:

<img width="872" height="206" alt="image" src="https://github.com/user-attachments/assets/78c31fcc-411b-4ba0-99a0-16a6ad8fbc89" />

Now, next we need to define our nodes we are going to be taking 3 nodes here →

<img width="865" height="404" alt="image" src="https://github.com/user-attachments/assets/45ef73ae-d6f6-4f65-897c-6a5be25c66da" />

Remember that State is a dictionary so here we override the value of "graph_state" and append something new(of our own).

We know for a fact that edges are how we connect nodes.

<img width="871" height="558" alt="image" src="https://github.com/user-attachments/assets/8836513d-8e1a-4af5-bd8e-154720874cd6" />

So here we were using a random function to decide which node to go to next from the conditional edge.

Now, we are going to put all that together into a graph as follows-:
We would be using the `StateGraph` class to do that and we’ll initialize it with `State` that we defined above and we’ll call it `builder` .Then, we add our nodes and name them accordingly. Finally, we define our logic for our starting from START and finishing up at END before we compile our graph and perform the basic checks and display it.

<img width="865" height="676" alt="image" src="https://github.com/user-attachments/assets/a13c2bfb-a4b9-4d9c-a3ee-8940a7a48a03" />

<img width="292" height="422" alt="image" src="https://github.com/user-attachments/assets/a5b72a9f-027a-4b4d-9d39-f2c8b8836f9d" />

Now, the TWEAKING PART→ I tried doing this with 3 nodes in the conditional edge to see how it turns out. In order to do this I changed the conditional statement a bit. This is how it turned out to be→ 

<img width="512" height="469" alt="image" src="https://github.com/user-attachments/assets/a216289e-d6eb-47a8-bc54-bacbbd224d2d" />

Now, graphs implement a runnable protocol. This is just a standard way to execute various langchain components. So, this protocol is a few standard methods which is higly convenient and one of them is invoke().All we need to do is invoke our graph with an initial condition or an initial starting value for our state.So, recall our state is a dictionary and it has one key graph_state and we can just simply invoke it with a starting condition as follows →

<img width="869" height="172" alt="image" src="https://github.com/user-attachments/assets/c8914c70-95d0-4b04-ba2e-1821ece122ce" />

### *Tweakings in Video One*→

1. Passed my own strings in node definitions as we overwrote the value of "graph_state" by appending as State is a dicitonary.
2. I built a graph of my own by changing the condition a bit and providing three nodes to the conditional edge. This is how it turned out to be→

<img width="542" height="482" alt="image" src="https://github.com/user-attachments/assets/a9584a75-b202-4665-bc98-532720b0860d" />

3. I invoked the graph with my own starting condition as follows→

<img width="866" height="147" alt="image" src="https://github.com/user-attachments/assets/1a02d2fa-09d7-4b3e-8539-0934c34e6ab5" />


____________________________________________________________________________________________________________________________________

## ***Lesson 3:LangSmith Studio***

Something went wrong with my OpenAI API key and it took me literal 2 hours to get here→

<img width="868" height="469" alt="image" src="https://github.com/user-attachments/assets/a3d36aae-e656-4ee7-8981-d5e0bf5094c6" />

This is the LangSmith studio interface.

So, we’ll be using simple_graph for now. There’s an input field which allows us to input values to the state just like we did in the notebook in the first video except that it is an IDE this time so we can visually interact with it.
The thread on the right side basically groups different invocations in my graph together.
It is like basically the history of any run of the graph.
On clicking submit, we see that we have a thread which shows us what happened in a brief way where we can see what each node does by expanding on the arrow.

<img width="862" height="498" alt="image" src="https://github.com/user-attachments/assets/49c2c9b4-98ac-43b4-9e1f-f6d6ce252e63" />

So, it is a really nice way to visualize what happens in your graphs and various runs of your graphs.

So, this was a basic introduction to how we’re gonna be using studio.

____________________________________________________________________________________________________________________________________





