I'm a hobbyist programmer with an interest in bringing retro computing to the browser by inverting the Web stack:

+ Use WebAssembly like a modern 6502, programming it directly in assembly.
+ Use an engine instance on the main thread as the CPU.
+ Use zero or more engines, each in its own worker thread, to implement generic co-processors.
+ Use worklets to implement specialized co-processors (audio co-processor, paint co-processor et cetera), with up
  to one engine for each type of worklet.
+ Use a shared array buffer for the RAM (so every core can access the same memory), bringing all of the processors
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

This is obviously just for fun, but there is nothing toyish about the technology. Chrome (with a few flags enabled)
already offers a very powerful platform, and the WebAssembly Binary Toolkit includes everything else I need to get
started. It's really just a novel paradigm for web development, based on old school microcomputing.

Ideally, I would like a better assembler than the text format, with nicer syntax, better error messages, and macros
*et cetera*. It would also be nice to curate a collection of helper chips, each with its own little 'datasheet'.
