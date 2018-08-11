# Xilinx Bitstream Format Library

[![Build Status](https://travis-ci.org/gmholland/libxbf.svg?branch=master)](https://travis-ci.org/gmholland/libxbf)

This library lets you open and read the header of the Xilinx `.bit` format.
It has no knowledge of anything inside of the bit-stream. It only knows
what the header is and what it means, how long is the data section and 
how many bytes there are. But it can print `.bit` information, such as
synthesis date, source `.ncd` file name and others.

# How to build

In order to build the library run:

	make

This will build the shared library, `libxbf.so` and the test program, `xbf`.

You can also run the regression test suite:

	make fetch

It'll fetch several `.bit` files from the official NetFPGA repository (be
warned -- it's 82MB by the time of writing this document). And then:

	make test

Which will verify that your changes didn't bring backward compatibility
problems. The check shows a differences between the output files after your
changes and 'golden model' references.

# API explanation

API calls are pretty self-explanatory and are mentioned below.

- Initialize the `xbf` structure for further operation.

```c
void xbf_init(struct xbf *xbf);
```

-  Open the file under `fname` and initialize the `xbf` context with it.

```c
int xbf_open(struct xbf *xbf, const char *fname);
```

- Just like `xbf_open()`, but take the data of size `mem_size` from `mem`
  pointer.

```c
int xbf_open_mem(struct xbf *xbf, void *mem, size_t mem_size);
```

- Return true/false if the `xbf` pointer has been opened correctly.

```c
int xbf_opened(struct xbf *xbf);
```

- Close the context related with `xbf`

```c
int xbf_close(struct xbf *xbf);
```

- In case of error, this function will return a user-facing error message.
```c
const char *xbf_errmsg(struct xbf *xbf);
```

- All these functions return time, data, part name, `.ncd` file name and file
  name respectively. Passed `xbf` must have been opened correctly.

```c
const char *xbf_get_time(struct xbf *xbf);
const char *xbf_get_date(struct xbf *xbf);
const char *xbf_get_partname(struct xbf *xbf);
const char *xbf_get_ncdname(struct xbf *xbf);
const char *xbf_get_fname(struct xbf *xbf);
```

- Return the length of the data section in opened `xbf` and return the data,
  respectively.

```c
size_t xbf_get_len(struct xbf *xbf);
const unsigned char *xbf_get_data(struct xbf *xbf);
```

- Print debugging data to file pointer `fp`. The `xbf_print` is equivalent to
  `xbf_print_fp(stdout, xbf)`

```c
void xbf_print_fp(FILE *fp, struct xbf *xbf);
void xbf_print(struct xbf *xbf);
```

# Examples

Take a look at `makefile`. It shows how to use `xbf` (the test program). The
Travis badge will show you this use-case in action:

[![Build Status](https://travis-ci.org/gmholland/libxbf.svg?branch=master)](https://travis-ci.org/gmholland/libxbf)

For API example, look into `xbf.c`, function `bf_test`. The `main` of `xbf.c`
has a real-world code snippet. You can steal it to start from.

# Authors

- Wojciech Adam Koszek, [wojciech@koszek.com](mailto:wojciech@koszek.com),
  modifications by Graham Holland
- [http://www.koszek.com](http://www.koszek.com)
