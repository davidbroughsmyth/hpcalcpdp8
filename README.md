# HP-35 and HP-45 Simulator for PDP-8 #
### Kyle Owen - 4 September 2016 ###
### kylevowen@gmail.com ###

This program is a simulator using the original HP-35 and HP-45 microcode. Thus,
the same key sequences on this simulator will result in the same answer as an
original calculator.

This work was heavily inspired by Eric Smith's `casmsim` and `nonpareil`
simulators. `nonpareil` can be found at <http://nonpareil.brouhaha.com/>.

### TERMS OF USE ###

This software is provided as-is, without a warranty of any kind, either express
or implied. 

### REQUIREMENTS ###

This has been tested and works fine under `SimH`. Previous versions have run
well on Omnibus hardware. Slight modifications would be required to convert this
program to run on older pre-Omnibus hardware. It requires two fields (8k words)
to run, though running just the HP-35 portion of the simulator can be done in
one field, likely without the help text as well. This program relies on the
over-typing of a single line of text; hence, a video terminal is recommended.
Modifications could be made to support a printing terminal, though this may be
more challenging than initially anticipated. 

`palbart` is used to assemble the file for use under `SimH`. `gcc` or `clang` is
used to compile `obj2oct`, which is a simple utility used to convert a
`nonpareil` object file to an octal syntax used in the program. A simple
`Makefile` is provided in order to assemble the binary for `SimH` use, and can
be used assuming `palbart` is installed as `pal` in your path. 

### BASIC USAGE ###

Starting the program at `0200` will run the simulator; switching SR0 (the
left-most switch) will toggle between HP-35 (0) and HP-45 (1) modes. Typing `?`
will print the valid keys used to interact with the simulator.

### OS/8 USAGE ###

Ensure the `OS8` flag is uncommented. Transfer the file to the OS/8 image of
your choice using PIP, but first, ensure that the assembly file has DOS line
endings. In VIM, you can do this with `:e ++ff=dos` followed by a `:w`. 

If you're using SimH, you'll need to ensure you've attached the file to the
reader using `at ptr [file]`. 

The basic PIP usage in OS/8 is:

`R PIP`

`DSK:HPCALC.PA<PTR:`

PIP will print a `^`; press `return`, then `ctrl-c`, and then assemble it using
PAL: `PAL DSK:HPCALC`. No errors should be generated.

To run the program, you can `LOAD DSK:HPCALC.BN /G`. Or, if you are so inclined,
you can save it as a `.SV` file by first running `LOAD DSK:HPCALC.BN`, then:

`SAVE DSK HPCALC 00000-04777,10200-17377;00200=0000`

This saves locations 0-4777 in the first field, 200-7377 in the second field,
sets the starting address to 200, and sets the job status word to the
appropriate value (page 1-37 in the OS/8 Handbook). 

Now, you can `RUN DSK HPCALC` to start the program. Assuming you have enough
room on `SYS:`, you can `COPY SYS:HPCALC.SV<DSK:HPCALC.SV` and now start the
program with `R HPCALC`. A ctrl-c should return you to the OS/8 prompt.

### TO-DO ###

Currently...
* Handle HP-45 error display (slightly challenging)
* Macro support for HP-35 only and HP-45 only versions, single field if possible

### SPECIAL THANKS ###

Thanks to Eric Smith for his inspiration in creating `casmsim` followed by
`nonpareil`. 
