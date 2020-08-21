Zumasys Smart User Interface Library

Version 1.0.3 - March 1, 2006

If you encounter any problems with the library, please report the problems
to accuterm@zumasys.com and we will try to resolve them as soon as possible.

The documentation for the library is included in Microsoft Word format in
the ...\Atwin\Pickbp\SUIBP\Doc folder. If you have difficulty understanding
the documentation, or find errors, please report these to support@asent.com.

To install the library, run the AccuTerm host program installer by typing
"RUN FTBP LOAD-ACCUTERM-PROGS" from TCL, and choose the "Smart User Interface"
option. Alternatively, create the SUIBP source file on the host, then
use the LOAD-ACCUTERM-SUIBP utility to upload the library to the SUIBP
file. If you do not have the LOAD-ACCUTERM-SUIBP utility in your FTBP
file, you should upgrade your FTBP file. LOAD-ACCUTERM-SUIBP customizes
any platform-specific code for your host platform.

If the library does not work or compile correctly for your platform,
the problem is probably in one of the platform-specific INCLUDE items:

	SUI.INPCHR  - raw single character input - return character in Q,
                      ASCII value in SQ.

	SUI.INPELSE - raw single character timed input - if a character
                      is already in the typeahead buffer, or if one is
                      received in a very short time (200-300ms), return
                      character in Q, ASCII value in SQ. If no character
                      received within timeout interval, return NULL in Q,
	 	      -1 in SQ. This is necessary to detect the ESC key.

	SUI.TACLEAR - clear the typeahead buffer (if available on your platform).

	SUI.CASING -  force case-sensitive string compares - required
                      for AP & D3.

Please send any necessary modifications for your platform to support@asent.com.

Edit the SUI.CONFIG configuration file to customize various behaviors
See the documentation for specific items that can be customized.

Compile and catalog the library:

SETUP.ACCUTERM.COLORS
SUI.CLEAR.RECT
SUI.CLOSE.WINDOW
SUI.DISPLAY.BTNBAR
SUI.DISPLAY.BUTTON
SUI.DISPLAY.FRAME
SUI.DISPLAY.LABEL
SUI.DISPLAY.LIST
SUI.DISPLAY.TEXT
SUI.FOLD.TEXT
SUI.GET.TERM
SUI.INPUT
SUI.INPUT.BTNBAR
SUI.INPUT.BUTTON
SUI.INPUT.CHAR
SUI.INPUT.LIST
SUI.INPUT.MLTEXT
SUI.INPUT.TEXT
SUI.MESSAGE.BOX
SUI.MOUSE.HIT
SUI.OPEN.WINDOW
SUI.PRINT
SUI.TEST
SUI.TEST.ATTRIBUTES
SUI.TEST.KEYBOARD
SUI.VERSION

If you are running AccuTerm with Visual Styles, you may want
to run the SETUP.ACCUTERM.COLORS utility program included with
the library to setup AccuTerm's colors to work with best with these
routines.

Use the SUI.TEST.KEYBOARD program to check if the library is recognizing
all of the function, editing and cursor keys properly. Use the
SUI.TEST.ATTRIBUTES program to test the visual attribute defined for
your terminal. See the SUI.TEST program for examples of how to use the
Smart User Interface Library functions.

You need accurate terminal definitions for the SUI input and display functions
to work correctly. The library includes definitions for Wyse 50 & 60; ADDS
Viewpoint, Viewpoint A2 Enhanced & 4000; VT100, VT220, VT320 & VT420; and
Pick PC console. All included definitions support AccuTerm's emulation of the
terminal and take advantage of AccuTerm features such as mouse, windowing, etc.
The SUI library uses the SYSTEM(7) function to obtain the terminal type,
forming a terminal defintion ID by concatenating 'TERMDEF.' to the result of
SYSTEM(7). You may need to create synonym terminal definitions for use with
your specific host platform. Look at the definition of TERMDEF.I in the SUIBP
file for an example.




