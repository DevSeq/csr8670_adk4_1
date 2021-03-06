Kalimba Matlab Tools
--------------------

Introduction
------------

A set of tools have been developed to enable a level of integration of Matlab and
the Kalimba DSP. These tools are able to connect to a Kalimba DSP over a debug transport
and extract symbol information from ELF or KLO files generated by the Kalimba toolchain.
They can then perform various debug actions, such as querying the state of the processor, 
reading and writing variables in memory, produce stack traces, etc.

Supported Matlab versions
-------------------------

2009a - 2014b inclusive, 32- and 64-bit.

Setup
-----

1. Matlab path

    To use these tools, the directory this file resides in must be added to the Matlab path via
    the "Set Path" command in Matlab. Typically the tools will be located in:

    [BlueLabDir]\tools\matlab\win32
    or
    [BlueLabDir]\tools\matlab\win64

    for 32- and 64-bit variants of the tools, respectively. 

    Once you have added this you may need to refresh the Matlab path, try typing this
    at the Matlab command line:-

       >> rehash path

2. C compiler

    The tools use C++ components to perform debug actions on the Kalimba and read symbol
    information. For Matlab to load these components, a supported C compiler must be available.
    As of Matlab R2013a, the list of supported compilers is available at 
        
        http://www.mathworks.co.uk/support/compilers/R2013a/index.html
        
    32-bit Matlab installations bundle a the "lcc" compiler, which is sufficient (although you
    are free to choose another supported compiler).
    
    64-bit Matlab does not ship "lcc", so a third-party compiler is required. The above page
    links to the Windows SDK 7.1, which is a free download, and includes a suitable C compiler.
    These tools have also been tested to work with Microsoft Visual Studio 2008 SP1.
    
    With a suitable compiler installed, run the following command at the Matlab command line:-
        
        >> mex -setup

    and follow the instructions.
    
3. Test
    
    To test that everything has been set up correctly, run 
    
        >> kalspi
    
    A dialog box should appear, prompting you to connect to an attached Kalimba DSP.
    
Instructions
------------

Note that more complete help on the various functions outlined below is available via the normal 
Matlab help facility. For example,

    >> help(kalloadsym)
    
The tools may be grouped in levels. The lowest level functions are:

KALSPI enumerates attached Kalimba cores via available debug transports, and allows
connection to a particular Kalimba core.

KALLOADSYM loads debugging symbols from a specified ELF or KLO file.

KALSYMFIND searches through the loaded symbol table with the wild cards * and ?
having their usual meanings. These searches are all case INSENSITIVE.

KALREADVAL reads the value of a variable from the Kalimba.

KALWRITEVAL writes the value of a variable to the Kalimba.

KALRUNNING returns and/or modifies the state of the Kalimba - running/stopped.

Other, lower-level, functions exist, for example KALVARPRS and KXS. KALVARPRS is used to
store the symbol table in Matlab. This function should never be called by the user -- it is
called by the other routines to return the currently stored symbol table. However, if users 
wish to develop their own functions which require access to the symbol table, it is available 
through this function. KXS provides direct access to various C++ functions; again, this is 
not intended to be used directly, but is available for new functions to be developed against.

The next level of functions includes:

KALPROFILER returns the usage statistics for the current Kalimba application;
this requires the project to be built with the pre-processor flag DEBUG_ON and
the debug libraries. Optionally, this function can run in the background and
extract, store, display and return the profiling information over periods of time.

KALPCPROFILER returns the usage statistics of the MODULES for the current Kalimba
application. This is done by polling the program counter of Kalimba DSP.

KALCBUFFERS returns levels, sizes, read and write pointers and space and fullness
statistics for the cbuffers within Kalimba. It requires that they have an
associated cbuffer structure and it follow the naming convention *CBUFFER_STRUC.

KALREADCBUFFER reads a number of samples stored in a circular buffer, given its
CBUFFER structure, and updates the read pointer of this structure.

KALWRITECBUFFER writes a number of samples to a circular buffer, given its
CBUFFER structure, and updates the write pointer of this structure.

KALMESSAGES returns a list of the declared message handlers and corresponding
information.

KALTIMERS returns a list of current timers with the offset relative to the first.
NOTE: this function requires the DSP to be stopped when called. If it is not it
will stop the DSP retrieve the information and then restart the DSP.

KALMODNAME returns the module name, text file and line number of the
corresponding program counter location.

KALSTACKTRACE returns a stack trace for the current program counter location.
NOTE this function requires the DSP to be stopped when called. If it is not it
will stop the DSP retrieve the information and then restart the DSP.

KALCODECSTREAMDEBUG provides a GUI to log and plot information about a loaded
codec decoder and encoder including information such as playing state and buffer
levels.

NOTES:

1) Many of these functions are specific to the Kalimba Libraries supplied with
BlueLab. If you modify these libraries the operation of these scripts cannot be
guaranteed.

2) Where capital letters are used above it is for clarity, the Matlab functions
should be written in lower case on the command line.