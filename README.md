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

## Implementation details

See [internals documentation](doc/internals.md).
