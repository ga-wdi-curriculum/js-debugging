# JS Debugging

## Learning Objectives
*After this lesson, students will be able to:*

- Identify and resolve common and uncommon "logical errors" that affect the results of your program
- Use logs to troubleshoot errors in an application (console log in Dev Tools)
- Conduct real-time debugging in the browser (start small, triangulation, remove code)

## Preparation
*Before this lesson, students should already be able to:*

- Use chrome dev tools
- Use a text editor

## Framing

This class is about Javascript errors and what to do when you get one.

First, we'll discuss:

# I Do: How errors are made (10/10)

## Throwing an error

Errors don't just happen. Chrome doesn't just "break". In fact, it's not Chrome that's throwing the error at all: it's Javascript.

Whenever you run into an error, it's because somewhere in the code you're running or in Javascript's source code there's a line that says, "When this happens, throw an error."

I'm going to show you how to throw an error. **Please close your computers and just watch me.** I'm asking you to do this because I don't want anyone to worry about throwing custom errors when working on Javascript projects in this class. We're showing this to you simply so you know what "magic" is going on under the surface.

Here's a piece of HTML/JS that "throws" an error:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>JS Errors</title>
    <script>
    throw(new Error("Oh, noes."));
    console.log("Such is life.");
    </script>
  </head>
  <body></body>
</html>
```

The result:

```
Uncaught Error: Oh, noes.       index.html:6
```

The `console.log` doesn't happen. This is because **when an error is thrown, Javascript stops whatever the current function is doing, just like `return`**.

## Error handling

You probably don't want an error to stop whatever your app's doing. You can tell Javascript, "If an error like such-and-such occurs, then do this specific thing instead of simply breaking and stopping." This is called **error handling**.

To handle an error you have to "catch" it (errors are "thrown", and must be "caught"). An "uncaught" error will cause the app to stop.

Here's an example of catching an error:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>JS Errors</title>
    <script>
    try{
      throw(new Error("Oh, noes."));
    }catch(err){
      console.log("Oops! There was an error. It says: " + err.message);
    }
    console.log("Such is life.");
    </script>
  </head>
  <body></body>
</html>
```

The result:

```
Oops! There was an error. It says: Oh, noes.
Such is life.
```

This time the `console.log` *does* happen. When you "catch" an error Javascript does whatever you tell it to do in response to the error, and then keeps on going. When you don't catch an error, Javascript stops.

Javascript is intentionally "trying" some code, and if the code throws an error, Javascript is waiting to "catch" it.

This `try...catch` system is something most programming languages have although it may use different verbs.

### And that's all we'll say about error handling for now

Again, error handling is an advanced concept. Generally we don't recommend getting into it until everything else on your app is very well-polished.


# Chrome Developer Tools

So you have an uncaught error. Now what?

### Set up (5/15)

If you haven't already, [please download Chrome](https://www.google.com/chrome/).

All browsers have developer tools that are very similar. But just for consistency's sake, we'll ask that you use Chrome for this lesson.

Chrome has its own combined command line and REPL called the Console. To open it press `Command` + `Option` + `J`.

You can also open it from the View menu as shown here:

![View menu](chrome_menu.jpg)

#### Disable Caching

Click the little dots at the top-right of the console and select "settings".

![Console settings](settings-1.jpg)

Check the "Disable cache" box. This makes sure Chrome reloads your Javascript whenever you refresh the page.

![Console settings two](settings-2.jpg)

## Keyboard Shortcuts (5/25)

Notice those weird characters to the right of all the things on the menu? Each is the keyboard shortcut for that menu item. Knowing the keyboard shortcut will make your life a million times easier.

> "View Source" is `Command` + `Option` + `U`. It shows the HTML for a page.

Here's a table of what those weird characters mean:

| Symbol | Name |
| --- | --- |
| &#8984; | Command |
| &#8997; | Option |
| &#8679; | Shift |
| ^ | Control |
| &#9166; | Return |
| &#9003; | Delete |
| &#9099; | Escape |
| &#8682; | Caps lock |

## Task Manager (5/30)

Whatever page you're looking at in Chrome right now, open the console and enter:

```js
while(true){}
```

Press enter.

#### Q. What just happened? Why can you no longer click anything on the page?
> You just put Javascript into an infinite loop. This prevents anything else from happening on the page.

To make this stop, you can quit Chrome, wait for Chrome to throw an error, **or** stop this Javascript using Chrome's Task Manager in the "Window" menu.

This is similar to the Task Manager in Windows.

![Task manager](task_manager.png)

This shows you all the tabs, windows, and extensions Chrome is currently running. You should see the tab with the infinite loop taking up about 99% of your computer's CPU (central processing unit).

![Task manager window](task_manager_window.jpg)

That's a problem! Click the row containing that tab, and click "End process".

## Open files in Chrome using Terminal (5/20)

Copy and paste the following into Terminal. **It must be `>>` instead of `>`.** Running this will add a shortcut to your Bash profile for opening stuff in Chrome:

```sh
$ echo "alias chrome='open -a \"Google Chrome.app\"'" >> ~/.bash_profile
```

#### For Linux users

```sh
$ echo "alias chrome='google-chrome'" >> ~/.bash_profile
```

**Close and re-open your Terminal window.** Then, just type `$ chrome filename.html`.

# Console Debugging

### Preserve log

If the "Preserve log" checkbox is checked in your Chrome console, uncheck it. When it's checked it doesn't clear error messages when you refresh your page. This can make your console get *really* ugly *really* quickly.

## Reading error messages (10/40)

Let's say I get this error message:

```
Uncaught SyntaxError: Unexpected token {      controller.js:8
```

#### Q. In what file and on what line is the error?
> Line 8 of controller.js

Click on `controller.js:8` and it'll show you that specific line of code.

### Stack traces

If you haven't already, **clone down this lesson plan's repo.** Then, open `stacktrace.html` in Chrome and open the Chrome console. **Refresh the page.** You should see two different lines starting with "User", followed by an error.

![Error](script_errors.jpg)

Click the little black/grey triangle on the left edge of the error to expand it.

![Stack trace](stack_trace.jpg)

Code is a chain of events called a "stack". Sometimes an error can be caused by something that happened earlier in the stack.

This is a **stack trace**. It shows all the functions that were called in order to call the function in which this error happened: `User` calls `getFirstAndLastName` which calls `greetEnthusiastically`.

Looking back through the stack trace should reveal the place at which the error was set in motion.

## Break (10/50)

## Console logging (10/60)

Errors are usually called by some variable not having the value you expect. The easiest way to debug code is to see how the value of a variable changes over time. The easiest way to do *this* is with `console.log` and `console.dir`.

In the console, note how there are two different "user" lines before the error. The first is the result of `console.log(user);`, the second of `console.dir(user);`.

In the console, type:

```
> console.log(document.body);
> console.dir(document.body);
```

#### Q. With your partner: What's the difference between `console.log` and `console.dir`?
> `console.dir` prints everything as an object, showing you all its properties and methods. `console.log` prints everything however Chrome sees fit: objects look like JSON, and DOM elements look like HTML.

#### Q. So what's the problem with this piece of code?
> Juan needs a last name.

When I need to debug stuff, I just put `console.log` everywhere, and look for the place my variable stops having the value I expect it to have. That's where my error is.

## Debugger (10/70)

**Replace** the `console.log` and `console.dir` in the script with the word `debugger;`. Refresh the page.

Now, in the console type each of the following. **What values do they have?**

```
user
names
myName
myAge
```

`debugger` stops a script at its line of code and lets you "look around". You can see what variables are available at the line of code where `debugger;` is run.

To make your code continue, press the little eject-looking button.

![Resume execution](resume_execution.jpg)

You can have as many `debugger;` lines as you want. The script will stop at each one and wait for you to tell it to continue.

## You Do: Debugger Jokes (10/80)

https://github.com/ga-wdi-exercises/debugger-jokes

## BREAK (10/90)

# Common errors

### Framing (10/100)

Javascript has [7 error types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error#Error_types). 3 of them will account for 99% of the errors you encounter in this class, so we're going to focus on those.

#### Q. What might these 3 words mean in the context of Javascript? *Syntax*, *reference*, and *type*.
> Syntax: The way the code is actually written.

> Reference: The process of calling variables and functions.

> Type: The different kinds of data Javascript can handle, like strings and numbers.

### You Do: Getting acquainted with error messages (20/120)

https://github.com/ga-wdi-exercises/js-errors-practice

# How To Find Answers (15/135)

If you can't fix an error within a reasonable amount of time &mdash; for instance, if the console says the error's on a line like this...

![Ugly error](ugly_error.jpg)

...turn to Google. Definitely do NOT try to slog through minified code.

## Google Fu

Tips for Googling:
- Copy and paste the exact text of your error into Google, and then remove any words that are specific to your script.
  - For example, instead of:
  ```
  Uncaught ReferenceError: robins_spatula is not defined
  ```
  ...search for:
  ```
  Uncaught ReferenceError: is not defined
  ```

- If you're looking for a specific phrase, put it in quotes.
  - `is not defined` will return any page with the words `is`, `not`, and `defined`.
  - `"is not defined"` will return any page with the exact phrase `is not defined`.

- Use `-` to exclude stuff.
  - `ReferenceError -jquery` will return any page with `ReferenceError` and **without** `jquery`

- Use `site:sitename.com` to search within a site
  - `site:stackoverflow.com ReferenceError` will search for pages with `ReferenceError` inside Stack Overflow only

## Stack Overflow

When I look at things on Stack Overflow I almost never read the actual question; I skip straight to the answers.

If the answer doesn't look promising, I go to the next. I repeat until the answers have very few upvotes.

If none of the answers are promising, I go on to the next thing that turned up on Google.

You can get [badges](stackoverflow.com/help/badges) and [special privileges](stackoverflow.com/help/privileges) on Stack Overflow by asking good questions and giving good answers! [I have 1066 points as I write this.](http://stackoverflow.com/users/2053389/robertakarobin) Catch me!

## Instructors

Failing all that, ask an instructor. Generally the first question we'll ask *you* is, "Did you get an error?" Having an error message makes things *much* easier to fix. We likely don't need to know anything at all about your app to fix the problem... but would love to learn about it if you have the time! :)

## You do: Debugging Practice (15/150)

https://github.com/ga-wdi-exercises/javascript-debugging

# References

- Screencasts
  - WDI8
    - [Part 1](https://youtu.be/dCukspxmNDs)
    - [Part 2](https://youtu.be/VbfB1qB20Yk)
