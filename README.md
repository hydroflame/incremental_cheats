## This repo will be deprecated in favor of ext_cheats

# libcheat!

libcheat is a library you can import in your console to help play (cheat) html games.

It does require knowledge of programming to use.

The main mechanic relies on the fact that you can import this library in your console.

	let { libcheat } = await import('https://hydroflame.github.io/incremental_cheats/main.js');

This will be your first line, from there you can use libcheat.

This library will auto-create a UI from where you can control your "cheats".

The main gimmick is that this auto sets up a scheduler every 50 ms (not configurable yet) that will run all your tasks.

We will take [universal paperclip](https://www.decisionproblem.com/paperclips/index2.html) as an example:

Let's say we want to press the "Make paperclip" button.

	libcheat.addTask(() => clipClick(1));

This will click the button every 50ms.

That's great but you may want to be able to disable this sometimes. So use the extra arguments to `addTask`

	libcheat.addTask(() => clipClick(1), true, 'auto make paperclip');

That will add a checkbox on the ui with the label 'auto make paperclip'. When it is checked the task will be executed.

Now we want to setup a skill tree in [calculator evolution](https://spotky1004.com/Calculator-Evolution/). We know we need to buy several skills for this one.

This isn't a task we want executed every 50ms. Only when we click it. So we can use `addButton`

	libcheat.addButton(() => {
		buyQuantumUpgrade(5);
		buyQuantumUpgrade(2);
		buyQuantumUpgrade(3);
	}, 'setup skill tree');

This adds a button on the cheat ui, when clicked that function is executed.

Now I want to automatically ascend for 1e5x more resources than I had before (in an unnamed game). But not always exactly 1e5, sometimes more or sometimes less. So I want a field that I can change.

	libcheat.paramTask(v => {
		if(game.points.mul(x) < calcAscensionGain()) ascend();
	}, 'auto ascension mult');

This will add a label and a field on the UI, the first argument of the function is the value of the field. (be careful about validating that value.

Finally sometimes we just want to be able to see a value that isn't exposed to the user. We can use `watch`

	libcheat.watch(() => {
		return "ascend for: " + calcAscensionGain();
	});

This one is special because it's the only one that returns a value.

# Documentation

## addTask(task[, toggleable, label])
Add a potentially toggleable task to the scheduler
### task: () => void
function takes no arguments and returns nothing, executed every 50ms.
### toggleable: boolean
must be true for the checkbox to appear
### label: string
label of the checkbox associated with this task.


## addButton(onClick, label)
Add a button to the cheat ui.
### onClick: () =>void
function to execute when the button is pressed.
### label: string
label of the button.

## watch(watcher)
Add an auto-updating value to the cheat ui.
### watcher: () => any
function returns the value to watch, updated every 50ms.

## paramTask(task, label)
Add a task to the scheduler that is fed the value of a text field from the cheat ui.
### task: (value: any) => void
function that will be called with the value of the text field, every 50ms.
### label: string
label of the text field.
