# malc

mal (Make A Lisp) compiler

## Installation

The executables generated by `malc` are dynamically linked with the Boehm
Garbage Collection shared library (`libgc.so`).

To install the Boehm GC library on Debian/Ubuntu:

    sudo apt-get install libgc-dev

To install the Boehm GC library on RedHat/CentOS:

    sudo yum install gc-devel

## Usage

Run the `malc` wrapper script with the mal program file as the argument:

    ./malc myprogram.mal

If successful, this will generate the executable `myprogram`.

## Implementation internal details

### Object structure

`mal_val_t` is a 64-bit value which can be one of the following:

* Integer
* Constant (nil, false, true)
* Pointer to object

### Integers

Integer are 63-bit integers shifted to the left by one bit. The least
significant is set to 1:

    (the_int_value << 1) | 0x1

### Constants

The three constant values are represented by the following 64-bit values:

* `nil`: 0x02
* `false`: 0x04
* `true`: 0x06

### Objects

12 bytes header:

```
struct mal_obj_t {
  uint32_t type;
  uint32_t len;
  byte* data;
}
```

type:

17 - 0x11 - symbol
18 - 0x12 - string
19 - 0x13 - keyword

 len - N number of chars in data
 data - points to char array of length N

33 - 0x21 - list
34 - 0x22 - vector
35 - 0x23 - hash-map

  len = N number of elements
  data - points to array of N `mal_val_t` entries

49 - 0x31 - atom - implemented as a vector of size 1.

65 - 0x41 - Env - implemented as a vector with two elements:

* Index 0: `outer` - an outer environment, or `nil` if this is the root environment
* Index 1: `data` - a hash-map from variables names to their values

66 - 0x42 - Func - implemented as a vector with three elements:

* Index 0: `arg_names` - Vector of symbols (argument names)
* Index 1: `env` - Env
* Index 2: `func_ptr` - Function pointer (of type: `%mal_obj fn(%mal_obj env)`)

67 - 0x43 - NativeFunc - implemented as a vector with three elements:

* Index 0: `arg_names` - Vector of symbols (argument names)
* Index 1: `name` - Symbol (function name)
* Index 2: `func_ptr` - Function pointer, of one of the following types (according to length of `arg_names`):
  - 0 arguments: `%mal_obj()`
  - 1 arguments: `%mal_obj(%mal_obj)`
  - 2 arguments: `%mal_obj(%mal_obj,%mal_obj)`
  - 3 arguments: `%mal_obj(%mal_obj,%mal_obj,%mal_obj)`
