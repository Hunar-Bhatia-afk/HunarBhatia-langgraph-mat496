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

### *Tweakings in Video Two*→

I tried different inputs of my own to see how the appending worked step by step and what changes were made at what step throughout the entire process.

____________________________________________________________________________________________________________________________________

## ***Lesson 4:Chain***

So we previously built a normal graph with nodes, normal edges and conditional edges.

Now, let’s build up to a simple chain that combines 4 key concepts-:

- Using chat messages in our graph.
- Using chat models.
- Binding tools to our LLM.
- Executing tool calls in our graph.

Getting started, first is messages → So, chat models interact with messages. Here’s an example -: I can create a list of messages which in our given case was the conversation between an AI and a human. We can assign a name to each message as well as content and just print it out. We change this as follows →

<img width="861" height="371" alt="image" src="https://github.com/user-attachments/assets/5898f8ad-b96f-48c3-bf49-c72450268f3f" />

I changed the conversation in the part where we learned about messages into a funny conversation on apples and doctors and observed the output.

Going further, I can take this list of messages and pass it directly to a chat model. First we make sure our OpenAI key is set, then we make some imports and then we specify the LLM model to work with which in this case was GPT 4o.

<img width="727" height="260" alt="image" src="https://github.com/user-attachments/assets/e158aa63-bd21-4564-9c82-9219aa316828" />

We then get the result and it is basically an AI message and the content which is a string from the LLM and then in the next cell we get the response metadata.

Now, let’s introduce the idea of tools which is another way to use chat models. The idea is simple→ Sometimes, we want to connect our chat model with some external tool like an API that requires particular payload to run. For example-: We take a function and define it and bind it to the LLM and now we’ll have an LLM that would have access to awareness of that function.

As shown in the diagram below, we can take natural language in and it can produce the payload necessary to actually run that tool or function out.

<img width="865" height="241" alt="image" src="https://github.com/user-attachments/assets/e4358f28-5947-4216-ae78-7f55e110da61" />

This is what we get from our own function that we defined in the notebook-:

<img width="866" height="390" alt="image" src="https://github.com/user-attachments/assets/b783129f-9854-4b20-8c77-b6940567e035" />

Here, we saw how to bind tools to chat models to produce tool calls outputs.

Now, let’s start rolling these pieces all into Langgraph. The first idea is how we can use messages as a graph state.

Now, we define this class messages_state and it is a typed_dict and it has one key messages which is just a list of messages. Do not forget that we overwrite the value of this key when we perform state updates by default and LangGraph but in our particular case we don’t wanna do that. Instead, we wanna append this list everytime. For example our chat model produces an output, we wanna append to a state so it preserves a full history of the conversation. This is what motivates the idea of reducer functions.

So, when we define our state in LangGraph, we can have a single key messages, we can have  it but we can actually annotate it with what we call reducer function, which tells LangGraph to append to this messages list when it recieves a new message.
Since having a list of messages in graph state is so common, LangGraph has a pre-built [`MessagesState`](https://langchain-ai.github.io/langgraph/concepts/low_level/#messagesstate)!

`MessagesState` is defined:

- With a pre-build single `messages` key.
- This is a list of `AnyMessage` objects.
- It uses the `add_messages` reducer.

Tested the reducer with my custom messages based on a bollywood joke to see how the reducer function appends the response. I also tried adding two messages to the function which gave an error where I learned that the reducer function takes 0-2 arguments and 3 were given to it.

<img width="863" height="286" alt="image" src="https://github.com/user-attachments/assets/7bcd2861-ae15-4ef6-b068-fc92e78622d1" />

So, instead of doing this, I wrote two functions and then tried executing it which led me to the result that it showed output for only the most recently called function.

<img width="866" height="272" alt="image" src="https://github.com/user-attachments/assets/01d36856-4a07-401c-b611-f38b090ed29c" />

Finally, we roll this into a graph as follows→

<img width="866" height="318" alt="image" src="https://github.com/user-attachments/assets/e1803370-e47e-4c3b-a140-46e02c07cef3" />

Now, let’s try invoking out graph with 2 different inputs(of my own) →

<img width="866" height="170" alt="image" src="https://github.com/user-attachments/assets/569351ac-22f7-4b2b-b766-faba956ca84e" />

In the second invoke function, we try tool calling with our custom function and it worked as follows→

<img width="864" height="254" alt="image" src="https://github.com/user-attachments/assets/c0abed9a-5496-4e58-b3cf-f04bc55cba6d" />

### *Tweakings in Video four*→

1. I changed the conversation in the part where we learned about messages into a funny conversation on apples and doctors and observed the output.
2. While tool calling, we changed the function to multiply_squares function that take 3 inputs and multiplies the squares of each of the given inputs to get the final result. We also alter the next 2-3 blocks accordingly.
3. Tested the reducer with my custom messages based on a bollywood joke to see how the reducer function appends the response. I also tried adding two messages to the function which gave an error where I learned that the reducer function takes 0-2 arguments and 3 were given to it.
   
<img width="865" height="283" alt="image" src="https://github.com/user-attachments/assets/0b8836e6-c108-4e79-8592-40d84b1984b1" />

So, instead of doing this, I wrote two functions and then tried executing it which led me to the result that it showed output for only the most recently called function.

<img width="865" height="272" alt="image" src="https://github.com/user-attachments/assets/dc740399-1351-4939-8fac-23162d820b20" />

4. Played around with the content a bit while invoking the graph.In the second invoke function, we try tool calling with our custom function and it worked as follows→

<img width="868" height="257" alt="image" src="https://github.com/user-attachments/assets/32b0a8a7-077e-4d24-a018-2805a3baa0b2" />

____________________________________________________________________________________________________________________________________

## ***Lesson 5:Router***

We built a grpah that uses messages as state and a chat model with bound tools.

To do either of the two things-:

- Return a tool call.
- Return a natural language response.

If an input is relevant to the tool, it returns a tool call otherwise just responds directly.Now, you can think of this as a general router as the chat model is routing between a tool call or a direct response.

This is what a typical router looks like where the LLM chooses one or two potential paths based on the input-:

<img width="870" height="277" alt="image" src="https://github.com/user-attachments/assets/ac65da97-8697-4f37-b35d-0cef39262572" />

So, let’s actually extend what we did using two new ideas-:

1. We’ll add a node that’ll actually call our tool so if the model responds with a tool call we actually can execute that call in a separate node.
2. We can add a conditional edge that let’s you look at the chat model output and make a decision.

Starting with the notebook→

We first call the API and make the imports required and then we set our custom function for tool calling.I altered the function that we were calling as a tool and changed it from multiply to weighted random number chooser for 3 inputs as follows→

<img width="831" height="699" alt="image" src="https://github.com/user-attachments/assets/3b2b68f1-6ce2-4951-8204-c8c262e0b8e7" />

We have the built-in ToolNode of LangGraph which is what we use to call our tool and all we need to do is just pass that function to our ToolNode.
We also have the pre-built tools_condition and basically it is a conditional edge, so it’s gonna look at the output of our LLM, and if that output is a tool call, it’ll route to our ToolNode.

<img width="860" height="767" alt="image" src="https://github.com/user-attachments/assets/67c17b69-f181-40b0-9957-e0aa3d1562ff" />

And instead of having the tool call, if we provide it with a direct message inside the content we’ll see that it responds normally.

<img width="867" height="511" alt="image" src="https://github.com/user-attachments/assets/60fe6fbc-2b1b-438b-9e6c-a9e73dcf9fd8" />

So, basically, the below graph depicts the the control flow of the entire process where the LLM decides whether to make the tool call or end the process.

<img width="229" height="446" alt="image" src="https://github.com/user-attachments/assets/1e98db1f-4f97-4000-a700-0e69f543cdbe" />

Now, we’ll see how it works in the studio as follows →

So, we’ll open our studio and go over to router and the interface should be looking something like this→

<img width="860" height="450" alt="image" src="https://github.com/user-attachments/assets/638493e6-c8b9-45f8-84fc-76686bfbbca6" />

So, we write a new message of our own to test how it works with a router via threads→

Here we see that tool_calling_llm does not use the tools part from the threads as it was a direct response and the tool wasn’t needed.

<img width="862" height="429" alt="image" src="https://github.com/user-attachments/assets/227c2af3-4e66-4faa-a069-253d9cf45533" />

On the other hand, when we pass it a message having something to do with the tool, it uses the tools part in the thread shown as follows→

<img width="856" height="412" alt="image" src="https://github.com/user-attachments/assets/6e9fb6a6-16fa-465c-bf25-a3b036ce0724" />

### *Tweakings in Video five*→

1. I altered the function that we were calling as a tool and changed it from multiply to weighted random number chooser for 3 inputs.
2. I used the built-in ToolNode and tools_condition commands on my own custom made tool and it was working perfectly.

<img width="868" height="763" alt="image" src="https://github.com/user-attachments/assets/3fcccb0d-61e8-44a8-8b74-fa0042fe5b2a" />

3. In LangSmith studio, I added two custom messages one having the need for the tool based function and on being a direct question that has nothing to do with the tool to see how the router worked in a step by step elaborated way via threads. 

____________________________________________________________________________________________________________________________________

## ***Lesson 6:Agent***

We are already done with the basics of a router. We can make one simple modification to this router to turn it into one of the more famous generic agent architectures.

So, from the router if we send the tools based information back to the LLM it would turn it into ReAct architecture which has 3 components →

1. act→ let the model call specific functions.
2. observe→ pass the tool output back to the model.
3. reason→let the model reason about the tool output to decide what to do next.

<img width="866" height="455" alt="image" src="https://github.com/user-attachments/assets/f0113a47-e6fb-49ea-8425-f2d718726b8b" />

This general purpose architecture can be applied to many types of tools.

So, basically the model can continue to call tool until it sees a solution fit for the purpose or determines that the problem is solved and then it can just return natural language response and then end. This does have cases where we can apply a max limit for recursion but this was the intuition behind the whole concept and let’s go further now.

After the installations and imports, we create 3 tools (of our own) and bind them to the model.

After this, we work with the system messages and go ahead and build our graph but we see that this time it looks a little bit different→

<img width="328" height="348" alt="image" src="https://github.com/user-attachments/assets/ba610f94-5dd7-441d-a3df-174afbdc62a8" />

We can clearly see that here our assistant is the chat model and whenever it goes to access tools, its output gets redirected back towards the assistant.

I changed the content in the human message and tailored it according to my self-built tools.

For the rest of the video, we examine the traces in LangSmith. This allowed us to go a little bit further under the hood and look at the various steps in the process.

### *Tweakings in Video six*→

1. I created 5 tools of my own and bound them to the model before the message part to check how well it works with custom inputs.
2. I changed the content in the human message and tailored it according to my self-built tools.

<img width="868" height="430" alt="image" src="https://github.com/user-attachments/assets/50d33c28-8f81-42bf-a392-8a23a443697d" />

____________________________________________________________________________________________________________________________________












