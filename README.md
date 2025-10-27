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

## ***Lesson 7:Agent with Memory***

Previously, we built an agent that can act, observe and reason. In this video, we get introduced to the idea of memory.

***I am not making any new tools of my own in this video as this follows the format of the last video where we already did this and hence it would only be repetitive.***

Yet again, we define tools and have a system message and get a graph that represents our agent.

Here, we want our agent to have memory i.e. for it to be able to access the conversation history for even more complex computations and deeper conversations.

To address the problem of the state being transient to a single graph execution to access the previous message, persistence comes in play as it introduces the idea of memory.

So, all we need to do is just import MemorySaver and then compile our graph with checkpointer set to memory in this case, MemorySaver() →

<img width="685" height="168" alt="image" src="https://github.com/user-attachments/assets/eaa0d456-f0fe-4fb7-aebd-53a218d93827" />

What checkpointers actually do is that they save the state of the graph at each point as this checkpoint. So, the checkpoint contains things like the state but it also has other stuff like the next node to go to and some metadata and has a checkpoint_id.

Now these checkpoints can be associated together in what we call a thread.

So, when we specify a thread, specify an input and run → it works normally except for now when we run the follow-up question with the same thread_id, it answers it they way it should be answering by referencing.

<img width="869" height="445" alt="image" src="https://github.com/user-attachments/assets/65191a04-1a84-42c8-baf3-d0ba52183558" />

Tweakings in Video Seven→ There wasn't much to do to be honest instead of changing the tools to our custom tools which we already have been doing for the last few lessons. I did try a thread of my own in the LangGraph studio but nothing else.

_____________________________________________________________________________________________________________________________________
_____________________________________________________________________________________________________________________________________

# Module-2

## *Lesson-1 : State Schema*

We built the foundation in module-1 and now we’ll go a little deeper in state and memory in the second module.

Fist, let’s talk a little bit more about schema. It is just the structure and types of data that the graph will use.

The schema is just a structure and the types of data a graph will use.

So, we’ve largely been using typedDict which is quite convenient and often recommended.
*Tweaking#1→*

Changed the code a little to experiment on my own.I changed the number of nodes and conditions according to those nodes and also I changed the mood literal to car.

Now, note that TypedDict is not the only way to establish schema for your graph. Python’s dataclasses provide another way to define structured data. Dataclasses offer a concise syntax for creating classes that are primarily used to store data.
We only modify the graphs slightly like for TypedDict it was state[”name”] but here it is just state.name*.*

*Tweaking#2→*

Changed mood to car for DataClass example and it ran smoothly as before.

One of the problems with Dataclasses and TypedDicts is that they provide type hints but they don’t enforce types at runtime.
So, Pydantic is a very nice solution to this problem as it provides data validation and is quite popular. So, in the code, we define a validator on car that confirms that car is either pagani or Benz otherwise it throws an error.

*Tweaking#3→*

On changing the car to Lamborghini it showed an error which is a good sign as it was not a part of the car list.

The Module 2 Lesson-1 teaches us that defining our **LangGraph state schema** carefully is crucial for reliability of agent behavior. TypedDict and DataClass approaches are fast and flexible only for prototypes, but **Pydantic** is preferred when you need runtime safety or strict format checking for production-level graphs.

__________________________________________________________________________________________________________________________________________

## *Lesson-2 : State Reducers*

Now, we are going to dive down on reducers which define how state updates are performed on specific keys or channels in your schema.

For the first example, we use a TypedDict as our state schema.
As discussed before, LangGraph does not know the preferred way to update the state.

Next, on invoking the graph and using try, when we execute it we get an InvalidUpdateError. This happens because if we are incrementing that same shared state key in both places at the same time, it’s ambiguous as to which one I keep.

This is where reducers come into play as they give us a general way to address this problem.So, all we need to do when we define our state schema is we need to supply the annotated type to our key and include a reducer function.So performing the same and changing the state code for foo into a list of integers and then executing the code gives us and appended list instead of having the old one modified.

*Tweaking#1* 

To verify the implementation, I created a **graph with three nodes**, where each node adds values 1, 3, and 2 to the shared state. Finally, I ran a series of **test cases** to confirm that the custom reducer correctly accumulated results from all nodes instead of replacing previous values.

In some cases we are required to define custom reducers rather than using the inbuit reducers in python for edge cases such as null. 
Next,we also learned about the message state reducers and add_reducer method. We got to know about adding messages, overwriting them using id and deleting them using the inbuilt RemoveMessage reducers.

Using add_messages also enables message removal for which we simply use RemoveMessage from the langchain_core.

*Tweaking#2*

I tried providing my own custom messages instead of the ones they had and verified its working when it came to using the RemoveMessage method.

Module 2 Lesson 2 of the LangChain Academy focuses on state reducers in the LangGraph. It explains how the reducers manage state updates across the nodes by specifying how multiple updates to the same key should be handled. By default, LangGraph overwrites values, but reducers allow customized merging logic—such as adding, concatenating, or appending data—making concurrent updates possible during branching. The lesson also introduces annotating state keys with reducer functions, handling errors from parallel state updates, and creating custom reducers for special use cases like handling null values. Finally, it demonstrates the built-in reducers such as add_messages and how they manage message histories efficiently.

____________________________________________________________________________________________________________________________________________________

   ## *Lesson-3 : Multiple Schemas*

Typically all graph nodes operate on a single schema and a single schema operates on all graph’s input or output channels. But, there are cases where we want more control over this. Case 1 would be when the internal graph nodes may pass info that ain’t required in the graph’s input/output.

So, first we’ll explore Private State.This is useful for intermediate working logic but not overall impactful for graph i/o.

Next, we use the classes InputState and OutputState and whatever params we provide to these two, only those would be provided as the respective input or output. Later, in the graph we assign input as InputState and output as the OutputState and that is what filters them both.

*Tweaking#1*

I used my custom input and output for the InputState and OutputState and experimented with a few changes in the code to see how it works I also tried swapping the OutputState and InputState for input and output schema and observed that it did not work properly.

this lesson teaches us how to define and control multiple input/output schemas in LangGraph, allowing our graph to expose only certain keys to users while still managing internal details during execution.

_____________________________________________________________________________________________________________________________________________________



## *Lesson-4 : Trim and Filter Messages*

We quickly set up our environment variables and pass a few custom messages→ 

*Tweaking#1*

I passed a few custom messages in the conversation and provided it to the LLM and made sure that everything was working correctly.

A practical issue working with messages is managing long running conversations that can be token intensive.

So, we’ll be learning a trick that involves usage of RemoveMessage that we explored earlier. Basically, what we’ll do is we will remove all the messages except the mentioned last few.

*Tweaking#2*

I tried various different numbers to check if the last few messages corresponding to those number were showing up on my custom inputs.I also added a few messages so that this process would go smoother.

Next, we go ahead and learn Filterng messages. If you don’t need or want to modify the graph state,you can just filter the messages you pass to the chat model. For example, just pass in a filtered list-:`llm.invoke(messages[-1:])` to the model.

On invoking the graph using message filtering, we see that the state has all of the messages but when we go ahead and look at the langsmith trace we observe that only the last message has been used.

So another alternative approach would be trimming as we can trim messages based on the number of specified tokens and this is relevant because LLMs have a context window with a specified token length, and we may want to specify the input accordingly.There are multiple different types of approaches here for example we can use the strategy=LAST to specify the final message in the list and you can make allow_partial=true to cut messages in the middle.

*Tweaking#3*

I played around a bit and tried out different uses of strategy keyword such as first, accumulate, skip and replace.Also, I used allow_partial as true in some cases to see how it ended up.

______________________________________________________________________________________________________________________________________________________

## *Lesson-5: Chatbot with Summarizing Messages and Memory*

We’ve already covered how to customise graph, state schema and reducers.
Now, we’ll take this one step closer and see a trick that actually uses LLMs to produce a running summary of conversation.
It’s another means of compression that tries to preserve information better than just filtering or trimming old messages.

We focus on enhancing chatbot capabilities by implementing long-term memory through conversation summarization, addressing the challenges of managing extended dialogues efficiently.

The lesson introduces a method where the chatbot generates a running summary of the conversation once it exceeds a predefined number of messages—six in the example provided—thereby compressing the dialogue history and reducing token usage.

*Tweaking1→*

I changed the messages in the notebook to that of my own to see how the results turn out to be.

This summarization process is automated within the chatbot's workflow, where after surpassing the message threshold, a dedicated node produces a summary that encapsulates key details such as user introductions and expressed interests, which is then stored in the chatbot's state. 

This approach not only preserves essential information but also allows the chatbot to maintain context over prolonged interactions without overwhelming the system with extensive message histories.

*Tweaking2→*

Adjusted the message count threshold for summarization (e.g., changed from 6 to another number) to see how it impacts conversation length and token usage.Modified the summary prompt text or formatting to improve the quality or focus of the running summary.

It amazingly presented a summary to us at the end.

____________________________________________________________________________________________________________________________________________

## *Lesson-6: Chatbot with Summarizing messages and External Memory*

In Lesson 6 of Module 2, you learn how to take all the powerful LangGraph agents and chatbots you’ve built so far and deploy them so real users can interact with them. 

This involves packaging your graph, running a server, and handling user interactions in a robust production environment. You get to explore features like streaming responses live, human-in-the-loop control, and managing complex scenarios such as multiple user requests racing in. 

*Tweakings→*

I updated the chatbot by changing the character’s name from Lance to Hunar and shifted the focus entirely to badminton. The chatbot now engages in conversations about badminton, remembering details and interactions related to the sport. During testing, Hunar introduced himself and mentioned his friend Aryan is a passionate follower of Indian badminton. Hunar asked the chatbot about top badminton players, and it successfully recalled information about prominent athletes like P.V. Sindhu, Kidambi Srikanth, and their key achievements. Across different messages, the chatbot remembered and responded accurately with rich details about the players and badminton history, showcasing that the memory feature works effectively with the new badminton-themed content.

The lesson introduces the key backend pieces like Redis and Postgres that make this possible and shows how to build customizable assistants tailored for different tasks—all running smoothly and persistently as deployed services. 

It’s an essential step to take your LangGraph projects from notebooks into the real world.

*Tweakings→*

I developed a customer service chatbot by integrating its memory with an external SQLite database for persistent state management. I specifically tested whether this setup allowed the chatbot to save and retrieve conversation history from the SQLite database continuously over extended periods. To validate the functionality, I utilized LangGraph Studio to observe the automatic persistence layers built into the system. I conducted experiments by ending and restarting chat sessions to confirm that past interactions were correctly restored from the database. Additionally, I documented this process by capturing and embedding screenshots within my project notebook.

__________________________________________________________________________________________________________________________________________________________

# *Module 3*

## *Lesson 1-:*

In this lesson, I learned about the different ways LangGraph handles **streaming results** during graph execution. There are two main methods — `.stream` for synchronous streaming and `.astream` for asynchronous streaming.

LangGraph also supports different **streaming modes** that determine what kind of data we receive while the graph runs:

- **`values`** → Streams the **entire graph state** after each node finishes running.
- **`updates`** → Streams only the **changes** (or updates) to the graph state after each node runs.

We also explored **`.astream_events`**, which allows us to stream **events** happening inside nodes in real time. Each event includes details like the event type, name, data, and metadata (such as which node emitted it). This is especially useful for **chat models**, where we can stream tokens as they’re generated instead of waiting for the whole output.

Overall, this lesson clarified how LangGraph provides fine control over streaming — whether we want full state updates, incremental changes, or token-level events — making it powerful for building interactive and responsive applications.

Changes I made-:
I altered the prompts to test custom output for both the types of streaming modes and observed that it was working fine. With this, I got to learn and experiment more with the streaming modes.

<img width="1426" height="388" alt="image" src="https://github.com/user-attachments/assets/25891f12-f151-45ba-b3bf-d96acfb70dc1" />




______________________________________________________________________________________________________________________________________________________________


## *Lesson 2-:*

Lesson 2 of Module 3 in the LangGraph course explains how to use **human-in-the-loop** controls — basically, how to let a person step in and guide the AI agent when needed.

It focuses on three main reasons for doing this:

1. **Approval** – letting a human review or approve actions before the agent executes them.
2. **Debugging** – allowing developers to pause, rewind, or replay parts of the process to fix issues.
3. **Editing** – making it possible to manually adjust the agent’s data or decisions mid-run.

LangGraph makes this possible by giving access to the agent’s state at any time. The lesson uses a tool approval example — where the agent’s tool use is monitored — showing how you can use `interrupt_before` to pause the workflow before certain tools run. When executed, these interruptions show up as flagged steps in the graph, indicating points where human input is required before continuing.

Changes I made-: I changed the tools and added some of my own to see how it worked with custom examples and how we could provide permission to the LLM in order to call a tool.TJhis helped me explore breakpoints in a better way.

_________________________________________________________________________________________________________________________________________________________________

## *Lesson 3-:*

In this video, we learned how to edit the graph state after interrupting its flow. While breakpoints are primarily used for human approval, they also allow us to modify the state of the graph. This is achieved through the default message reducer, "add_message", which updates the graph’s state.
We also explored how to add and work with interrupts in LangGraph Studio, as well as how to edit the graph state using the LangGraph API.
Additionally, I learned how to explicitly modify the graph state based on user input by introducing a dummy node. This node accepts user feedback and injects it into the graph at a specific point, simulating the execution of that node. We can achieve this by using the "as_node" field in the update_state function call.

<img width="849" height="360" alt="image" src="https://github.com/user-attachments/assets/8647341c-a483-44bc-807d-1f2bb1af2983" />


Changes I made-:I added additional tools such as square, greet, and random number. I then interrupted and modified the graph’s state using both hardcoded values and user input, and continued the graph’s execution from that point. I also updated the graph state using the LangGraph API, as demonstrated in the tutorial.I also wrote custom inputs into LangGraph studio to see how interrupt works.

_________________________________________________________________________________________________________________________________________________________________

## *Lesson 4-:*



