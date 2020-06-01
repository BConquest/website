+++
title = "Creating a Simple Network Monitor"
date = 2020-05-31
+++

# Introduction
While trying to get more information about what is happing on without having a terminal open. I decided to use a bar to display this information and instead of using other people's scripts, I have recreated the wheel. This was a fun challenge reading from files and finding the information that I would need.

# How it works
This is a relatively simple program but it reads from three files. The first two are the rx_bytes and tx_bytes for the interface that is passed in. The other file is reads from is a log file that contains the previous values of the transmitted and recieved bytes. It will then subtract the current bytes from the previous bytes and output that in a truncated way, thanks to this nifty bit of code.

```c
#define DIM(x) (sizeof(x)/sizeof(*(x)))
static const char *sizes[] = { "EiB", "PiB", "TiB", "GiB", "MiB", "KiB", "B"};
static const u_int64_t exbibytes = 1024ULL * 1024ULL * 1024ULL * 1024ULL * 1024ULL * 1024ULL;

char *calculateSize(u_int64_t size) {
	char *result = (char *) malloc(sizeof(char)*20);
	u_int64_t multiplier = exbibytes;
	int i;

	for (i = 0; i < DIM(sizes); i++, multiplier /= 1024) {
		if (size < multiplier) continue;
		sprintf(result, "%.1f %s", (float) size / multiplier, sizes[i]);
		return result;
	}

	strcpy(result, "0");
	return result;
}
```

What this code does is takes in the number of bytes that you have and converts it into a formatted shorter version of it.
