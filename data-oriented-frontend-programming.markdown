# Data-Oriented Frontend Development
By Adam Munoz
January 2020

## Table of contents
1. [Introduction](#introduction)
1. [Conceptual framework](#conceptual-framework)
1. [Functional programming and frontend development](#functional-programming-frontend-development)
1. [Immediate Mode](#immediate-mode)
1. [React](#react)
1. [Persistent Data Structures](#persisten-data-structures)
1. [STM: Transactional Memory](#stm)
1. [Redux](#redux)


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
