# Data-Oriented Frontend Development
By Adam Munoz
January 2020

<a name="#introduction"></a> 
## Introduction

**TODO**

<a name="#conceptual-framework"></a> 
## Conceptual framework

Let met start by trying to layout the conceptual framework and giving some definitions.

In this article I will be looking at the following model for software applications:

1. Some **Action** takes place (a user clicks a button, a network response arrives, etc ...)
2. Some new **State** is produced as a result
3. Some representation of that state is produced by a **View**

Let's look at an example:

-  **Action** ➡ *User presses key 'a'*
-  **State**  ➡ *Current state will be: **text typed so far is a***
-  **View**   ➡ *Paint character 'a'*

-  **Action** ➡ *User presses again key 'b'*
-  **State**  ➡ *Current State will be: **text typed so far is ab***
-  **View**   ➡ *Paint characters 'ab'*

*Example 1: A user types some text into a text box*

This model is not something that it's limited to software or computers, rather it's a way of thinking about processes in general, but what are processes?

### Processes

According to Merriam Webster a process is *"a series of actions or operations conducing to an end"* (a final state).

Example of processes are cooking recipes, lacing up your shoes, and of course, computer programs.

This definition of process as a series of sequential steps leading to a final state is something that seems quite similar to the model presented above.

We say that repeatedly performing the sequence **Action** ➡ **State update** leads to some useful result.

Of course, this definition misses the view part. 

In fact, a view is not an essential part of performing a process, rather it's a window for humans to see the result of the process or its progress.

We'll talk more above views in the following sections, but a very common example of a view is a User Interface.

### Actions

Actions are events that take place in the world and that affect its state. 

For example, a user typed some text, the cook put an egg inside a bowl, etc ...

### State

What we mean by *state* is **a truth about the world that we care about**.

Note that we are not concerned about its data representation yet. Rather we are talking about information about the world.

For example, we can encode **"So far, the user has typed the text ab"** like this:
```
currentText = 'ab'
```
*Example 2: Encoding information as data*

### View

Finally, a view will take the current state and produce a representation (typically visual) of it.

Views do not normally hold any state themselves, rather they receive the state of the world through their inputs and use it to produce the UI.

**state of the world** ➡ **view** ➡ **UI**

![](fig1.svg)

*Fig1: Action, State, View cycle*

### Data-Oriented frontend development

In data oriented frontend development, thus we focus mostly on describing how actions update the state of the program as a whole rather than on mutating the UI.

**Action** ➡ **State update**

We then pass this state to the view who will use it to produce the UI in an deterministic and reproducible way.

Since the views do not keep local state, it means that the the UI of our program can be deducted exclusively by knowing the inputs of the views at each point in the program.

This becomes quite handy especially as applications grow in complexity, and UIs more rich.

## Functional programming and Frontend Development

In this section, I want to look at the model described so far through a different lenses. Namely, the lenses of functional programming.

If we consider the views to be functions then we can think of as frontend development as a function of data.

![](fig2.svg)

*Fig2: User interface is a function of state*


As we change the variable **state** the value of **user interface** changes. 

Therefore, we can represent our program as a line like this:

![](fig3.svg)

*Fig3: Representing a program as curve*

This of course relies on views being pure functions. That is that they respect the contract that for the same input the same output will always be produced.

In other words, as we said in the previous section, the views must be able to produce the UI without access to anything but their inputs.

This is actually the current trend in frameworks such as React and that's why React is a very good tool to do data oriented frontend development.

Let's look at our previous example in React:

```
// state
currentText = ''

// view
function view({text}) {
  return 
    <div>
      <p>You have typed {text.length} characters</p>
      <textarea value={text} />
    </div>
}

// user interface 
ui = <view text={currentText} />

// paint
ReactDOM.render(ui, document.getElementById('root'))

```

The output of this would be something like this:

![](fig4.svg)

*Fig4: The state of our UI at the start*

Now we need to create some code for action handling.

As we said in the previous section, we will just care about producing the right data and let the view take care of producing the user interface.


```
// state
currentText = ''

// update state
function updateCurrentText(text) {
  currentText = text;
  render();
}

// view
function view({text, onTextChangeAction}) {
  return 
    <div>
      <p>You have typed {text.length} characters</p>
      <textarea value={text} onchange={onTextChangeAction} />
    </div>
}

// paint
function render() {
  ui = <view text={currentText} onTextChangeAction={updateCurrentText}/>
  ReactDOM.render(ui, document.getElementById('root'))
}
```

Every time the user generates a typing action, the state will be updated and the view will produce a new version of the user interface.

![](fig5.svg)

*Fig4: The view will repaint with each state change*

## Immediate Mode vs Retained Mode

As you might have noticed, every state change will trigger a whole repaint of all the screen. 

In fact, this is not something new. 

In the game programming world, we speak of **immediate mode** and **retained mode**.

In retained mode programmers manage each change in the screen individually through mutation operations. 

In **immediate mode**, on the other hand, programmers throw away the current state of the screen and create a new representation it with each frame just as we explained in the previous section.

Just like DOM operations, graphic display operations are also quite expensive which means that re-painting the whole screen with each state change quickly become inviable from the point of view of performance.

The solution from the game world to this problem is using a **graphic buffer** to render an in-memory version of the screen, and then diffing this buffer with the actual state of the screen in order to automatically calculate the necessary mutation operations. 

Since memory operations are less expensive than graphic display operations, this solves the problem of performance in game programming with immediate mode.

In the web world, we solve the problem of performance with the DOM using a similar technique. Namely, the **Virtual DOM**.

![](fig6.svg)

*Fig4: Immediate mode and Virtual DOM*

In a future post I will talk about how to handle state changes in an **atomic transactional** way and why that is important.

I will also look at techniques for managing these changes in performant ways such as **persistent data structures**.

Thank you for reading, I hope you enjoy it!