# Argument Parser

A lightweight, arena based command line argument parser for C. This is part of my custom standard library [ylib](https://github.com/yukittynya/ylib) :3   


### Features

- Simple and lightweight
- Arena based
- Memory safe

### Usage

### Basic Setup

First, include the headers: **arena.h** and **args.h** 

```
#include "arena.h"
#include "args.h"
```

From there define your list of valid arguments/flags **in** args.h in `valid_flags[] = {}` (Line 44)  

```
static const ValidFlag valid_flags[] = {
    {"-h", false},  // Help flag, no input
    {"-a", true},   // Add flag, expects input
}
```

Then parse your arguments in your program!  
```
#include "arena.h"
#include "args.h"

static Arena arena = {0};

int main(int argc, char* argv[]) {
    ParseResult parsed = parse_args(&arena, argc, argv);

    for (int i = 0; i < parsed.count; i++) {
        Arg arg = parsed.args[i];
        
        if (strcmp(arg.literal, "-h") == 0 || strcmp(arg.literal, "--help") == 0) {
            print_help();
        } else if (strcmp(arg.literal, "-v") == 0) {
            enable_verbose();
        } else if (strcmp(arg.literal, "-o") == 0) {
            set_output_file(arg.input);
        } else if (strcmp(arg.literal, "-f") == 0) {
            process_file(arg.input);
        }
    }
    
    arena_free(&arena);
    return 0;
}
```

### Example Usage

```
# Simple flags
./program -h -v

# Flags with inputs
./program -o output.txt -f input.txt

# Mixed usage
./program -v -f data.txt -o results.txt
```
