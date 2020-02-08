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

Chances are many if not most of the applications you have encountered so far will perform the following dance:

1. Some **Action** takes place (a user clicks a button, a network response arrives, etc ...)
2. Some new **State** is produced as a result
3. Some **View** is produced to represent that **State**

Let's look at an example.

** Example 1: A user types some text into a text box **

-  **Action**  ➡ *User presses key 'A'*
-  **State**  ➡ *Current state will be: **keys typed so far are A***
-  **View**    ➡ *Paint character 'A'*

-  **Action** ➡ *User presses again key 'B'*
-  **State**  ➡ *Current State will be: **keys typed so far are A and B***
-  **View**   ➡ *Paint characters 'AB'*

Before delving into the above example futher, **TODO** ...

### Process

The model that we are describing is not really limited to software applications,
Rather it can be used to model **any process** in real life including but not limited to a process running on a computer. 

According to Merriam Webster a process is *"a series of actions or operations conducing to an end (a final state)"*.

Example of processes are cooking recipes, lacing up your shoes, and of course, computer programs.

### Action

**TODO**

### State

What we mean by *state* is **a truth about the world that we care about**.

Note that we are not concerned about its data representation yet.  Rather, we say that an action has produced some new information.

For example, we encode this *information*: "So far, the user has typed the letters A and B into the text box" like this:
```
currentText = 'ab'
```

### View

Finally, a view will take the current state and paint a visual representation of that state on the screen. So the full cycle looks like this:


### Relation between State and View

A useful analogy to think about the relation between State and View is that of your image in the mirror being the representation of your self.

As you change your State by for example waving your arms or putting different clothes on your self, the image in the mirror will faithfully represent those changes.

In similar fashion, as the state of a frontend application encoded as data changes its view will output different visual artificats (typically on a computer screen) to represent the current state of the application.

As we will show, this realisation will allow us to create applications whose UIs evolve and respond to different events just by manipulating data.

Thus we call this style of programming Data-Oriented.