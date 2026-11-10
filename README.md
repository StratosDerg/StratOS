# StratOS
My own little custom OS (Side project to avoid burnout)

## Progress
-# Based on the process described in [OSDev.org](https://wiki.osdev.org/Creating_an_Operating_System)
### Phase 0: Setup

#### Building GCC <-- I am here
Compiling a compiler that compiles the uncompiled code on the compiled development OS so that I can compile my own OS.

### Phase 1: Basics

#### Setting up a Cross-Toolchain
Connecting GCC to my new OS

#### Creating a Hello World Kernel
This is where everything finally starts! This is where the very very basics are finally complete and we can begin adding interactivity.

#### Setting up infrastructure
Allowing building of programs? Idk this one.

#### Calling Global Constructors
The first step of running a program, setting up where objects in memory and such are initialized.

#### Terminal Support
Basically allow debugging by giving me a print, printf, println, etc. Maybe allow colours here?

#### Stack Smash Protector
Like the basic-est of the basic cyberattack. Easy to avoid, but the guiderails do not come out of the box.

#### Multiboot
Information and features offered to the kernel. Memory maps? Video Moes? Kernel Symbol Tables?

#### Global Descriptor Table
Idk what this is, I will research it more when I get closer to the step.

#### Memory Management
One of the most important functions. Having reusable functions and just a groundwork allows for the easiest way of building anything without having to define the memory every time.

#### Interrupts
Asynchronous events happen in hardware, it needs to be handled.

#### Multithreaded Kernel
Starting the multithreading early prevents having to do it later, where I'd have to rewrite major parts to include it.

#### Keyboard
The very first type of input. Allows for reading off of it to allow interactivity.

#### Internal Kernel Debugger
Stops kernel and dumps all memory for debugging.

#### Filesystem Support
I will be creating a custom filesystem, though I may have a working placeholder to start.

### Phase 2: User-Space

#### User-Space
This is where permissions are added, pretty much.

#### Program Loading
The first step of running any program is holding it into memory.

#### System Calls
This is how programs communicate with the base kernel.

#### OS Specific Toolchain
This step is where we define the conventions so that we can set up the tools to cross-compile.

#### Creating a C Library
This is the bridge for connecting C to the kernel.

#### Fork and Execute
Allow processes to create new processes.

#### Shell
This is the command line, so that finally there is a working way to choose what to do and when. Basic command line interface :3

### Phase 3: Extending

#### Time
A lot of things require keeping time, more than one would think. Creating functions to allow timers and system clocks to exist.

#### Threads
This allows multiple operations to happen simultaneously. Exposing this extends the life of the OS for higher-end machines and upgrading.

#### Thread Local Storage
At the point of writing this, I'm actually unsure of this one.

#### Symmetric Multiprocessing
Allow multiple CPUs, basically.

#### Secondary Storage
Now adding non-volitile memory support, of reading and writing.

#### Real filesystems
Connecting secondary storage to the filesystem.

#### Graphics
Finally allowing for something other than text on a screen.

#### UI
An actually usable interface, allowing a desktop and such.

#### Networking
Let's go online!

#### Sound
Sound is sound. Otherwise it's just flashing colours.

#### USB
Modern peripherals!

### Phase 4: Porting

#### Software
I intend on adding the capability of running executable files for both Unix and Windows. This is likely a pipe dream.

#### GCC
Evil ass Compiler compiling.

#### StratOS
I have to compile my own OS on my own OS, and all of the build systems that require it.

#### Self-hosting
After everything else is complete, this should allow development and porting of other programs.

### Phase 5: Profit

#### StratOS is finally complete.

If I've gotten here within a year. I am extremely proud of myself. I severely doubt I will get here within five, but one can dream.
