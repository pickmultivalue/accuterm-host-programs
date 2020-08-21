Object Bridge 4.0.1 Release Notes

To use this facility, install using the AccuTerm Host Programs Installer
by typing "RUN FTBP LOAD-ACCUTERM-PROGS" and selecting the ObjectBridge option.
Alternatively, upload the contents of the OBJBP sub-folder to a file on the
host (we recommend OBJBP, but any source code file will suffice). You can use
the FT utility to upload the programs. You will need to have the file transfer
utilities installed to use Object Bridge.

After uploading the OBJBP programs, compile and catalog them.

A sample program, CREATE.WORD.LETTER, for creating a Word letter document from
a Pick/BASIC program is included in the SAMPLES folder.

You may need some reference material in order to understand the object model of
the application you are trying to control (Word, Excel, VB, etc). Also, the
Object Browser tool available in the Macro -> Visual Basic Editor or in the
VB IDE is very useful.

The basic idea is this:

 1) Initialize the Object Bridge environment
 2) Create an object
 3) Set property values, get property values or call
    subruotines or functions (invoke methods) on the object
 4) Respond to desired events produced by the object
 5) Release the object


The subroutines included in Object Bridge are:

	ATINITOBJMGR(ERRMSG, OPTS)

Call this subroutine before using any other Object Bridge routine. This verifies
that a compatible version of AccuTerm is connected, and initializes some
important state information in the OPTS variable. You must maintain and pass
the OPTS variable to all other Object Bridge routines.

If any of the Object Bridge routines encounter an error, the ERRMSG arugment will
be set to indicate the error, otherwise it will be null. Sometimes you can ignore
an error, such as when attempting to set a property to an invalid value.


	ATRESETOBJMGR

This routine may be called if you want to release all objects and clean up the
Windows object manager. This is useful if an error occurs and you want to abort
all further use of any objects. You do not need to call ATRELEASEOBJECT if you
call ATRESETOBJMGR.


	ATCREATEOBJECT(CLSID, OBJID, ERRMSG, OPTS)

This routine will attempt to create an object of the desired class (CLSID). Use
the returned OBJID to refer to this object. When you no longer require access
to the object, call ATRELEASEOBJECT passing the same OBJID, otherwise the object
remains present in Windows memory.


	ATRELEASEOBJECT(OBJID, ERRMSG, OPTS)

This routine releases the specified object, as well as any objects "derived" from
the specified object (OBJID). A dereived object may be an object returned by a
call to ATGETPROPERTY or ATINVOKEMETHOD when the return value is itself an object.


	ATSETPROPERTY(OBJID, PROPNAMES, PROPVALUES, ERRMSG, OPTS)

Use this routine to set the values of one or more properties of an object. Specify
the property names you desire, separated by attribute marks. The corresponding
property values to set are passed in the PROPVALUES argument, also separated by
attribute marks. If an error occurs, the error message will be in the corresponding
attribute of the ERRMSG argument.


	ATGETPROPERTY(OBJID, PROPNAMES, PROPVALUES, ERRMSG, OPTS)

Use this routine to get the values of one or more properties of an object. Specify
the property names you desire, separated by attribute marks. The corresponding
property values are returned in the PROPVALUES argument, also separated by
attribute marks. If an error occurs, the error message will be in the corresponding
attribute of the ERRMSG argument.

Some properties are actually objects themselves. In this case, you can use the
returned property value as an object ID in other Object Bridge calls. When the
object which returned the "derived" object is released, all of its derived objects
will also be released. Releasing the derived object will not release its creator.


	ATINVOKEMETHOD(OBJID, METHNAMES, METHARGS, RESULT, ERRMSG, OPTS)

Call this routine to invoke a "method" on the object. A method is a subroutine or
function. A subroutine will not return a value (RESULT will be null). A function
will return a result in the RESULT argument. Multiple methods may be called.
Separate method names in the METHNAMES argument by attribute marks. If a method
requires arguments, the arguments are passed in the METHARGS parameter, with
arguments for each method separated by attribute marks. Within the method
arguments for one method, multiple arguments are separated by value marks.
Results from multiple methods are returned in the RESULTS argument separated
by attribute marks.

Some methods return objects. In this case, you can use the returned value as an
object ID in other Object Bridge calls. When the object which returned the
"derived" object is released, all of its derived objects will also be released.
Releasing the derived object will not release its creator.


	ATSETEVENT(OBJID, EVENTNAMES, ENABLE, ERRMSG, OPTS)

Many objects support events, which notify the object owner of certain actions,
such as closing the program or clicking a button. Object Bridge only notifies
the Pick application of those events which have been enabled by calling ATSETEVENT.
Specify the event names in the EVENTNAMES arguments, separated by attribute marks.
Specify whether the event is enabled (1) or disabled (0) in the corresponding
ENABLE argument, also separated by attribute marks.


	ATGETEVENT(OBJID, EVENTNAME, ARGS, TIMEOUT, ERRMSG, OPTS)

Call this routine to wait for an event from any objects for which you have enabled
events. You do not pass OBJID to this routine, it is passed back to your program
when an event occurs so you can identify the source of the event. EVENTNAME and
ARGS are also passed back to your routine. Some events allow you to modify one or
more of thier arguments, for example, to cancel an operation. You must preserve
OBJID, EVENTNAME and ARGS (except for modified values in ARGS) when processing the
event so they can be passed to ATENDEVENT when you complete processing the event.
Event argument values are separated by value marks. You must always call ATENDEVENT
after processing the event. You can call other routines in between, but no more
events can be processed, and the Windows object may be roadblocked until you call
ATENDEVENT.

If you want ATGETEVENT to return to your program even if an event did not occur,
you can specify the number of milliseconds to wait for the event in the TIMEOUT
parameter. Set TIMEOUT to zero to wait forever for the event.


	ATENDEVENT(OBJID, EVENTNAME, ARGS, ERRMSG, OPTS)

Call this routine after handling an event. Pass back the OBJID, EVENTNAME and ARGS
as they were returned by the ATGETEVENT routine, except for any argument values you
have modified.

