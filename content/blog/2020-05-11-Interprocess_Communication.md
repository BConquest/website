+++
title = "Project: Interprocess Communication"
date = 2020-05-11
+++

## Description
This is a sample program that demonstrates how process can comunicate on linux based systems. The goal is to read in 2 matrixes stored in plain text and multiply them returning the results. This first part of this is done in package.

Package will read in the matrixes and store them in a data structure that looks like.
```c
typedef struct matrixStruct {
	int rows;
	int cols;
	int **pmatrix;
} matrixStruct;
```
It will then start creating tasks and adding them to the message queue with each task being a separate pthread to wait for the response. Once the threads get a return message they write the answer to an output matrix after also aquiring the appropriate mutex's.

Compute is the other process that first receives from the message queue calculates the answer by finding the dot product and returns that onto the queue. It uses a thread pool to do this and can be of any size.

Both of the programs will also capture the signal SIGINT and print out a status of the current run that follows the form of ` Jobs sent foo Jobs Received bar`. They also print messages when they have sent or recieved a job that contains the job id, type, and size of the job.

## Source
Publicy hosted on [github](https://github.com/BConquest/matrixmults) with MIT license.

## Compiling 
` make all `
Compiles binaries for package and compute with no debug help and an optimization of level 3.

` make debug `
Compiles binaries for package and compute with support for gdb.

` make clean `
Removes compiled binaries 'package' and 'compute'.
