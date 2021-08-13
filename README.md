My name is Carl Smith, but I started using `7ombie` when I stopped programming professionally.

I'm currently working on the Ellex 80, and hope to continue doing so indefinitely.

ELLEX 80
--------

The Ellex 80 is a planned retro computing platform for hobbyist programmers, based around the modern browser. The
Ellex 80 is heavily inspired by 1980s home microcomputers, and borrows ideas from fantasy consoles like PICO 8.

Holistic Standardization
------------------------

The Ellex 80 has some simple specifications for its physical hardware that people can follow to build a *clone*.
These specs are never more specific than they need to be. For example, the platform requires a 1080p 60Hz display
that the user is comfortable coding and gaming on.

The platform relies on its physical constraints to define software specifications that ensure that everything is
standardized for all users. For example, defining a set of graphics modes that each have resolutions that support
pixel-perfect scaling to 1080p. This allows users to target a graphics mode that is a good fit for their project,
and share their code, confident that it well render perfectly for all other users of the platform.

The Ellex 80 design philosophy rejects the popular misconception that gimping the hardware inspires creativity. It
is certainly true that artists are often more creative when forced to work under tight constraints, but they can
simply opt into whatever constraints they find interesting, without the platform enforcing them.

The Ellex 80 only employs constraints to minimize complications, ensuring that code written for one clone will
execute perfectly on every other clone.

Virtualization
--------------

Virtual hardware is created on top of clone systems by inverting the Web stack:

+ Use WebAssembly like a modern 6502, programming in assembly.
+ Use an engine instance on the main thread as the central processing unit.
+ Use zero or more engines, each in its own worker thread, to implement generic co-processors.
+ Use an audio worklet to create an optional audio co-processor, which can generate audio or process the output
  from audio chips (see below) when both are present.
+ Use a shared array buffer as RAM (so every processor can access the same memory), bringing all of the processors
  together into a multicore SoC.
+ Use shared functions to implement interrupts, where imported functions (*handles*) are invoked by the user once
  any data is ready, passing any offsets as arguments, and exported functions (*handlers*) are written by the user,
  and return any offsets once they have finished handling the interrupt.
+ Use JavaScript and HTML5 (WebGL, WebAudio, WebUSB, WebMIDI, the Keyboard, Mouse and Gamepad APIs et cetera) to
  implement helper chips for rendering sprites, background tiles and audio, for raising interrupts in response to
  user input, animation frames and timers, as well as providing access to the network, storage *et cetera*.
+ Combine application-specific collections of helper chips with custom SoCs to implement bespoke, virtual arcade
  system boards that can be programmed in assembly.
+ Use memory mapped IO with dynamic offsets to communicate with helper chips using interrupts.

The paradigm summarized above will feel much more concrete to users of the platform, due to the way that packaging
abstracts the components, providing fully specified (albeit bespoke and virtual) hardware for the user to program.

Ellex Cartridges
----------------

Ellex projects are packaged into *cartridges*, which are fundamentally just directories, zipped to form a single
distributable file, that is also compressed.

Each Ellex project is developed within a directory that can contain anything the user likes, with whatever file
structure they want, just so long as the root directory contains a file named `ellex.meta.json`, which is known
as the *meta file*.

The meta file contains a hash that sets out the usual metadata, and contains a nested hash that describes the
cartridge filesystem in terms of the project filesystem. This nested hash is known as the *directory mapping*.

The directory mapping represents the root directory of the cartridge. Each key in the hash represents the name of
a file or directory. Each nested directory is represented by a nested hash, using the same representation as that
used by the root directory. Each file is expressed by a *command*, stored as a string. Commands can copy, compile
and concatenate files from the project directory to produce the content that is written to the corresponding file
in the cartridge directory.

The cart can be loaded by the Ellex 80 runtime (a small JavaScript library), which will use the metadata to setup
the SoC, copy the correct binaries to the various processors, resolve any imports and exports, and install all of
the helper chips.

PHANTASM
--------

The need for a modern, virtual 6502 is perfectly met by the WebAssembly Engine. However, the Text Format (WAT) is
a poor fit. It was not designed to be used as a source language, and support for using it that way is limited. It
also has serious (albeit subjective) aesthetic issues, with many users strongly disliking the syntax.

The PHANTASM assembler was created specificially to permit web assembly programming with a modern grammar, better
error messages, and source-level debugging in DevTools, with a look and feel that is much closer to conventional
assembly languages (which were historically inspired by languages like 6502 Assembly).

Note: While PHANTASM will always be developed as its own project, unrelated to the Ellex 80, the Ellex 80 platform
depends on PHANTASM as its official assembler and programming language (though WAT is still supported, as there are
those that enjoy using it).

The Ellex Chip Set
------------------

Once the Ellex 80 platform is being used, it would make sense to curate a collection of helper chips, each with its
own little *datasheet* that explains how the chip works, how its registers are mapped internally (within the larger
address space required by the chip) *et cetera*.

A gamerfied version of the Mouser and Farnell websites, with a pixel-art aesthetic, would be ideal for sharing and
documenting generic Ellex helper chips.
