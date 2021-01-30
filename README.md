# Stubgen - a member function stub generator for C++

Stubgen is a C++ development tool that keeps code files in sync with their associated headers.  When it finds a member function declaration in a header file that doesn't have a corresponding implementation, it creates an empty skeleton with descriptive comment headers.  stubgen has several options, but see the "Brief Example" section below for an idea of what it can do.

C++ Compatibility
-----------------

stubgen's parser does not conform to the latest C++ standard. It was developed back in 1998 as a gigantic hack that I created when I was teaching myself lex/yacc. Hacking the yacc grammar further probably isn’t a good idea, since C++ isn’t an LALR(1) language anyways. 

At the time it was written it handled C++98 pretty well - but it may not handle C++03 or C++11 well at all.

Brief Example
-------------

Suppose you have the following header file Point.h:

``` 
class Point {
public:
    Point(int x, int y);
    void addTo(const Point& other);
 
    int xValue, yValue;
};
```
 
Running "stubgen -s Point.h" would produce the following file:

``` 
/***********************************************
 * AUTHOR: Michael J. Radwin <mjr@acm.org>
 *   FILE: Point.cpp
 *   DATE: Mon Apr 20 17:39:05 1998
 *  DESCR:
 ***********************************************/
#include "Point.h"
 
/*
 *  Method: Point::Point()
 *   Descr:
 */
Point::Point(int x, int y)
{
}
 
/*
 *  Method: Point::addTo()
 *   Descr:
 */
void
Point::addTo(const Point& other)
{
}
```

Files
-----
The following files are included in this distribution:

    LICENSE        -  the BSD License
    ChangeLog      -  a listing of changes made on various versions.
    Makefile       -  a makefile for building stubgen on unix
    README         -  this file
    etc/ -  debugging routines for use with the -d option
    lexer.l        -  flex source, generates tokens
    main.c         -  code generation routines
    parser.y       -  yacc source, parses header and code files
    stubgen.1      -  nroff-able man page
    table.[ch]     -  data structures used in parsing
    util.[ch]      -  utilities, logging routines, used in parsing
    test/-  test header files for stubgen


Building
--------

Prerequisites: gcc, flex, bison, cmake

```
mkdir build
cd build
cmake ..
make
```

Acknowledgments
----------------
stubgen borrows code from:

Jutta Degener's 1995 ANSI C grammar (based on Jeff Lee's 1985
implementation):
ftp://ftp.uu.net/usenet/net.sources/ansi.c.grammar.Z
http://www.lysator.liu.se/c/ANSI-C-grammar-l.html
http://www.lysator.liu.se/c/ANSI-C-grammar-y.html

Graham D. Parrington's Stub Generator for the Arjuna project at the
University of Newcastle upon Tyne:
http://arjuna.ncl.ac.uk/

Raphael Assenat <raph@raphnet.net> contributed the -N and -l flags.

Dushara <nidujay@gmail.com> contributed the -p and -I flags to make it
easier to make stubs for 3rd party tools (e.g. #include <wx/string.h>)

