In this post I'm going to be talking about a data oriented approach to frontend development. 

I will present a conceptual framework first (the philosophy) and then go onto some code examples to illustrate it. The examples are not necessarily code that you would write in the real world. Rather they are meant to illustrate the foundations underlying real world code.

Again, while it is by no means a comprehensive treatment of the topic, I hope to offer a simple foundational understanding of some concepts and techniques present in frontend technologies such as React or Vue.

<a name="#conceptual-framework"></a> 
## Conceptual framework

Let met start by trying to layout the conceptual framework and giving some definitions.

We will be looking at the following model made up of three steps:

1. An **Action** takes place (a user clicks a button, a network response arrives, etc ...)
2. A new **State** is produced as a result
3. A representation of that state is produced by a **View**

Let's look at an example:

-  **Action** ➡ *User presses key 'a'*
-  **State**  ➡ *Current state will be: text typed so far is a*
-  **View**   ➡ *Paint character 'a'*

*Example 1: A user types some text into a text box*

This model is not limited to software or computers, rather it's a way of thinking about processes in general. 

But what are processes?

### Processes

According to Merriam Webster a process is *"a series of actions or operations conducing to an end"* (a final state).

Example of processes are cooking recipes, lacing up your shoes, and as we all know, computer programs.

This definition of a process as a series of sequential steps leading to a final state stands out as quite similar to the model described above.

We say that by repeatedly performing the sequence **Action** ➡ **State update** we can achieve some useful result or results.

Of course, this definition misses the view part. 

But, in fact, a view is not an essential part of a process, rather it's a window for humans to see and understand its result or progress.

We'll talk more above views in the following sections, but a very common example of views is of course user interfaces.

### Actions

Actions are events that take place in the world and that affect its state. 

For example, a user typed some text, a chef put an egg inside a bowl, etc ...

### State

What we mean by *state* is **a truth about the world that we care about**.

Note that we are not concerned with its data representation yet. Rather we are just talking about information.

Of course, in the case of computer processes, at some point this information will be encoded in data. 

For example, **"So far, the user has typed the text ab"** could be encoded like:
```
currentText = 'ab'
```
*Example 2: Encoding state as data*

### View

Finally, a view will take the current state and produce a representation (typically visual) of it.

Views do not normally hold any state themselves, rather they receive the state of the world through their inputs and use it to produce the UI.

**state** ➡ **view** ➡ **UI**

This sequence is repeated throughout the life of the application, and as the state changes as a result of actions, the views will produce different versions of the UI (which themselves might offer new opportunities for users to trigger new actions which will again change the state and so on...)

![](https://raw.githubusercontent.com/adammunoz/blog/master/fig1.svg)

*Fig1: Action, State, View cycle*

### Data-Oriented frontend development

In data oriented frontend development, we focus mostly on describing how actions update the state of the program rather than on mutating the UI.

That is, we focus mainly on this part: **Action** ➡ **State**

And we delegate the work of creating and managing user interfaces to the views to whom we pass the state in order for them to produce a new version of the UI.

Since the views do not normally keep local state, it means that the the UI of our program can be deducted exclusively by knowing the inputs of the views at each point in the program.

This becomes quite handy especially as applications and user interfaces grow in complexity and richness as it frees the developer from having to keep track of UI mutations.

## Functional programming and Frontend Development

In this section, I want to look at the model described so far through a different lens. Namely, the lens of functional programming.

If we consider views to be functions then we can think of our applications' user interface as a **function of state**.

![](https://raw.githubusercontent.com/adammunoz/blog/master/fig2.svg)

*Fig2: User interface is a function of state*


As the variable **state** changes, so will do the **user interface** accordingly. 

We can represent a program as a curve:

![](https://raw.githubusercontent.com/adammunoz/blog/master/fig3.svg)

*Fig3: Representing a program as curve*

By the way, the shape of the curve is not particularly relevant for this example, as neither is the way we choose to numerically encode the y-axis (e.g. hashing the html string).

Of course, this abstraction will only work for as long as the views are pure functions. That is that they respect the contract that *for the same input the same output will always be produced*.

Or, in other words, the views must be able to produce the user interface without access to anything but their inputs.

React is an ideal tool for writing such views, and therefore very well suited to data oriented frontend development.

Let's look at some code samples in React:

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

![](https://raw.githubusercontent.com/adammunoz/blog/master/fig4.svg)

*Fig4: The state of our UI at the start*

Now we need to create some code for action handling.

As we have stated, we will just care about producing the right data and leave it to the view to take care of producing the user interface for us.


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

![](https://raw.githubusercontent.com/adammunoz/blog/master/fig5.svg)

*Fig4: The view will repaint with each state change*

## Immediate Mode vs Retained Mode

As you might have noticed, every state change will trigger a whole repaint of the whole UI. This is called Immediate Mode programming. 

In the game development world, we speak of **immediate mode** and **retained mode**.

While in retained mode programmers manage each change in the screen individually through mutation operations, in **immediate mode**, on the other hand, programmers throw away the current state of the screen and create a new representation with each change.

However, just as DOM operations, graphic display operations are also quite expensive in terms of performance which means that re-painting the whole screen with each state change would quickly become inviable for most programs.

The solution from the game development world to this problem is using a **graphic buffer** to render an in-memory version of the screen, and then diffing this buffer with the current version of the screen in order to automatically calculate the necessary mutation operations. 

Since memory operations are less expensive than graphic display operations, this solves the problem of performance in game programming with immediate mode.

In the web world, we also have our own graphic buffer. That is the **Virtual DOM**.

![](https://raw.githubusercontent.com/adammunoz/blog/master/fig6.svg)

*Fig4: Immediate mode and Virtual DOM*

In a future post I will talk about how to handle state changes in an **atomic transactional** way and how that helps avoid a certain class of bugs.

I will also look at techniques for managing state changes through the use of **persistent data structures** for better performance.

Thank you for reading and I hope you enjoyed it!