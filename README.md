# pylibfst: Handle *Fast Signal Traces* (fst) in Python

Pylibfst is mainly a python cffi wrapper for libfst (*Fast Signal Trace*) from [gtkwave](http://gtkwave.sourceforge.net/).

FST is like [VCD](https://en.wikipedia.org/wiki/Value_change_dump) is an open format for dumpfiles generated by EDA logic simulation tools.
Unlike VCD, FST is a binary format that offers much better performance for very large dumpfiles.
FST was developed as part of gtkwave.
For more details on the format, see [GTKWave 3.3 Wave Analyzer User's Guide](http://gtkwave.sourceforge.net/gtkwave.pdf).



## Build & Install

### Dependencies
Additional packages that need to be installed on the system:
 * cmake build environment (cmake, gcc, ...)
 * zlib-dev

### Install using PyPi (pip)
Pylibfst is available from [PyPi](https://pypi.org/project/pylibfst)!

```
pip install pylibfst --user
```

### Build & Install from Source
The latest development version of pylibfst is available on [github](https://github.com/mschlaegl/pylibfst).

There are various ways to build and/or install pylibfst:
 * Build from Source using *Python*
```
python -m pip install --upgrade build
python -m build
```
 * Build from Source using *make*
```
make all
```
 * Build & Install from Source using *make*
```
make install
```

## Usage
A documentation on how to handle the cffi-style interface (calls, arguments, return values, ...) can be found in the [CFFI documentation](https://cffi.readthedocs.io/en/latest/using.html).

Although the fst format and library are widely used, there is unfortunately no documentation for the libfst library.
(more details on this: [FST API documentation · Issue #70 · gtkwave/gtkwave · GitHub](https://github.com/gtkwave/gtkwave/issues/70)).
However, to support development, pylibfst comes with some helper functions and examples.

 * **Helper functions**:
   * *string(..)* .. Converts ffi cdata to a python string
   * *get_scopes_signals(..)* .. Iterates the hierarchy and returns a list containing all scopes names and a dictionary containing all signal names with corresponding handles.
   * *get_signal_name_by_handle(..)*: Returns the first matching signal name from the given signals dictionary for the given handle.
   * *fstReaderIterBlocks(..)* and *fstReaderIterBlocks2(..)*: Wrapped versions of corresponding libfst functions. Allow the use of any normal Python function as a callback (with slight overhead).
 * **Examples**
   * *dumpfst.py* .. Demonstrates the main calls required to implement an fst reader.
   * *IterBlocks_callback.py* .. Demonstrates the use of *fstReaderIterBlocks* and *fstReaderIterBlocks2* using cffi-style callbacks
     * Advantage: Most efficient
     * Disadvantage: Only one callback function per program possible
   * *IterBlocks_wrapped_callback.py* .. Demonstrates the use of *fstReaderIterBlocks* and *fstReaderIterBlocks2* using pythonic callbacks (helper functions above)
     * Advantage: "Normal" Python functions as callbacks (as many as you want)
     * Disadvantage: Slightly more overhead due to wrapper function

## Known Problems and TODOs
 * Windows and Mac untested

## libfst Sources
 * Location: fst
 * Taken from
   * Repo: https://github.com/gtkwave/gtkwave
   * Path: gtkwave4/src/helpers/fst
   * Commit: 49a2a53caee83890dff503c15815fb53d5ccde74
 * Licenses: see COPYING

### How to upgrade libfst?
 1. Copy most recent sources from gtkwave to directory *fst*
 1. Check and update *fst/CMakeLists.txt* (see comments in file)
 1. Update above section (e.g. commit hash)
 1. Update *pylibfst/libfstapi_build* according to *fst/fstapi.h*
 1. Check and update LICENSE files
 1. Check and update *pylibfst/helpers.py*
 1. Check build and install
 1. Check and update examples
 1. Commit: Must contain the information from section above
