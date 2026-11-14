---
tags: 
- emacs
- LI
MOC: Technology
source: "https://chatgpt.com/c/6af8bc4e-2f64-83e9-9ca7-3a67270b501b"
---
[[_0000 Home|Home]] | [[_0001 Linux MOC|Back to Linux MOC]] 
# Emacs Calc & GNUplot

## Algebraic mode

- Calc doesn't require you to use RPN (Reverse Polish Notation) exclusively
- You can switch to algebraic mode using `m a` (`calc-algebraic-mode`)
- This allows you to write expressions in conventional mathematical notation, such as:
    - `x + 2*y`
    - `3 - x/2`
- You can also use `'` to enter an algebraic expression regardless of the current mode

## Plotting functions

- Calc has a built-in graphing interface that uses GNUplot
    
- The main commands are:
    
    - `g a` (`calc-graph-add`): Adds a curve to the current graph
    - `g p` (`calc-graph-plot`): Plots all accumulated curves
    - `g d` (`calc-graph-delete`): Deletes the most recently added curve
    - `g v` (`calc-graph-view`): Views the generated GNUplot commands

### Adding a function

- To plot a function, we need to supply an interval and the function itself
- For example, if we want to plot `y = 3 - x/2`:

```
2: [-10..10]
1: 3 - x/2
```

- Notice that we don't actually enter `y =`, only the right-hand side of the equation
- We then use `g a` to add the curve
- We can repeat the same process to add more functions to the same graph

For example, `y = 2x + 8`:

```
2: [-10..10]
1: 2*x+8
```

- After adding both functions, `g p` will plot them together

### Sampling and resolution

There are two different sampling mechanisms to keep in mind:

- Calc's `g N` (`calc-graph-num-points`)
    - Controls the number of points Calc computes when adding a formula curve
- GNUplot's `set samples`
    - Controls the number of points GNUplot computes when plotting functions directly

The difference is that Calc often evaluates the function itself and passes the resulting data to GNUplot, rather than passing the original function.

In other words, `set samples` won't necessarily affect curves that Calc has already computed.

The interval determines the domain of the function, while the number of samples determines how many points are calculated within that domain.

## GNUplot configuration

- Calc exposes some common graph settings through its own commands, but it doesn't expose every GNUplot configuration
- We can access the generated GNUplot commands through the `*Gnuplot Commands*` buffer
- This allows us to customize the graph directly using GNUplot's configuration language
- After changing the commands, we can use `g p` again to regenerate the plot

### Some useful Calc commands

| Command | Description                              |
| ------- | ---------------------------------------- |
| `g g`   | Toggle grid                              |
| `g b`   | Toggle border                            |
| `g k`   | Toggle legend                            |
| `g N`   | Set graph resolution                     |
| `g h`   | Set graph title                          |
| `g n`   | Set the most recently added curve's name |
| `g t`   | Set x-axis title                         |
| `g T`   | Set y-axis title                         |
| `g r`   | Set x-axis range                         |
| `g R`   | Set y-axis range                         |
| `g l`   | Toggle logarithmic x-axis                |
| `g L`   | Toggle logarithmic y-axis                |
| `g z`   | Toggle horizontal zero axis              |
| `g Z`   | Toggle vertical zero axis                |
| `g s`   | Configure line style                     |
| `g S`   | Configure point style                    |

### Making the graph look like a Cartesian coordinate system

- GNUplot's default graph has a rectangular border, and the axes aren't necessarily positioned at the origin
- We can use `xzeroaxis` and `yzeroaxis` to draw the axes at the origin

```
set xzeroaxis
set yzeroaxis
```

- `xzeroaxis` draws the horizontal axis at `y = 0`
- `yzeroaxis` draws the vertical axis at `x = 0`

To make them thicker and solid instead of dotted:

```
set xzeroaxis lt -1 lw 2
set yzeroaxis lt -1 lw 2
```

- `lt -1` selects a solid foreground line
- `lw 2` sets the line width to 2

We can also remove the surrounding border and move the tick marks onto the axes:

```
set border 0
set xtics axis
set ytics axis
```

This gives us a more traditional Cartesian coordinate system, with two perpendicular axes intersecting at the origin.

### A note about `g z` and `g Z`

- These commands toggle the visibility of the zero axes
- According to the Calc manual, zero-axis lines are on by default
- They don't control the thickness or line style, only whether the axes are displayed

## GNUplot terminals

- GNUplot supports different output terminals, depending on how we want the graph to be displayed
- One of them is the `dumb` terminal, which displays graphs using ASCII characters directly in the terminal
- A graphical terminal uses an actual graphics backend instead

In our case, we needed to make sure GNUplot could access the Wayland display to use the graphical output.

## Emacs daemon and emacsclient

- An interesting thing we discovered is that we can interact with the running Emacs daemon directly from the terminal using `emacsclient`
- This means we don't necessarily need to open another Emacs instance or interact with it through a graphical frame

### Evaluating Emacs Lisp

We can use `emacsclient -e` to evaluate an Emacs Lisp expression inside the running Emacs server.

For example:

```
emacsclient -e '(emacs-version)'
```

We can also inspect environment variables:

```
emacsclient -e '(getenv "WAYLAND_DISPLAY")'
```

And modify them:

```
emacsclient -e '(setenv "WAYLAND_DISPLAY" "wayland-1")'
```

- This is what we used to configure the Emacs daemon's Wayland environment
- Processes subsequently launched by Emacs can inherit that environment
- This was important for allowing GNUplot to access the graphical display

### Invoking Emacs commands remotely

We can also invoke interactive Emacs commands:

```
emacsclient -e "(call-interactively 'calc-graph-grid)"
```

Or evaluate other Lisp expressions:

```
emacsclient -e '(default-directory)'
emacsclient -e '(garbage-collect)'
```

There are essentially three ways to interact with Emacs:

1. Interactive commands through `M-x` and keybindings
2. Evaluating Lisp directly through `M-:`
3. Evaluating Lisp remotely through `emacsclient -e`

The important thing is that `emacsclient` communicates with the already running Emacs server. It doesn't need to start another Emacs instance.

This means that practically anything we can express in Emacs Lisp can potentially be invoked remotely, provided the relevant function is loaded.

## References

- [GNU Emacs Calc Manual](https://www.gnu.org/software/emacs/manual/html_mono/calc.html)
    - Section 14: Graphics
    - Section 14.4: Graphics Options
    - Algebraic-style calculations
- [GNUplot Documentation](https://gnuplot.sourceforge.net/documentation.html)
- [GNU Emacs Manual — Emacs Server](https://www.gnu.org/software/emacs/manual/html_node/emacs/Emacs-Server.html)