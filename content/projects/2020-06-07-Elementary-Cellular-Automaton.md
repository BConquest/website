+++
title = "Elementary Cellular Automaton"
date = 2020-06-06
+++

## What are Elementary Cellular Automaton

They are a group of one-dimensional automaton that have binary states. It is considered to be  one-dimensional since the cell's state depends on what it was in the previous state
plus its two neighbors.  This leads to a nice property of having 255 states that can be represent every combination of states. So for example the number 78 when converted to binary
is 0b01001110 and the rule set for 78 looks like:

| 111 | 110 | 101 | 100 | 011 | 010 | 001 | 000 |
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
| 0   | 1   | 0   | 0   | 1   | 1   | 1   | 0   |

So if you have an input of `01010` and assume that the input is cyclical you can split the string into sets of 3 and then use those sets to get the output of `11010`. Example:

| Neighbors | Output Cell |
|:---------:|:-----------:|
|    001    |      1      |
|    010    |      1      |
|    101    |      0      |
|    010    |      1      |
|    100    |      0      |

## Creating a program to visualize these different rules

### Generating a Rule
Since the way that rules are encoded can be represented as a binary number you can store
the conversion table in an array. This array is indexed by the converting the neighbors
into a decimal number.

```
Example:

0b001 -> 1
rule[1] = 1
```

You would do this for the entire current generation and either exit the program or go to
the next generation of cells.

### Efficient Printing
In an effort to find an efficient way to print out each generation. I started out with an
an unbuffered printing statement.

```c
for (int i = 0; i < size; i++) {
	if (array[i] == 0) {
		printf(" ");
	} else {
		printf("#");
	}
}
```

This was taking too long to print out longer sets of generations I switched to a buffered
printing method that allocates a character space for each cell. This greatly improved the
amount of time that it would take to print out each generation of cells. With my final
print function look like this.

```c
char *buffer = calloc(sizeof(char), (size+1));

for (int i = 0; i < size; i++)
	buffer[i] = (array[i] == 1) ? ' ' : '#';

printf("%s\n", buffer);
free(buffer);
```

### What I would change
1. Arguments
Right now the arguments are hard coded static options always in the form of
`./a.out <size> <iterations> <rule: 0-255>`. If you know of a better way to handle these
arguments than this file an issue or pull request on
[github](https://github.com/BConquest/automata). I know there is argparse and getopt.

2. Different Ways Neighbor can be handled
There are a couple of ways that I can think of handling neighbors on the edges of a
generation. The first is how I have it right now in which the *"world"* acts as a circle.
Another way to handle the generations is to have a hard cut off limit for each side of the
world. One other way is to allow the world to grow at each generation. I would like to
implement all the two new ways to do this ,but I do not want to until I am able to parse
arguments.

3. Allowing different methods of seed generation
This goes along with issue number 2 in which I can implement allowing the first generation
to be randomly generated or reading it in from a file.

## Examples
[![asciicast](https://asciinema.org/a/bxqzQ6hrapltHs5FoxQaJ4r3G.svg)](https://asciinema.org/a/bxqzQ6hrapltHs5FoxQaJ4r3G)
[![asciicast](https://asciinema.org/a/M3W7tJuSsPkPPXHYfpf3PlvXF.svg)](https://asciinema.org/a/M3W7tJuSsPkPPXHYfpf3PlvXF)
