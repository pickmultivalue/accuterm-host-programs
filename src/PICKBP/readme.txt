AccuTerm Host Programs

08/30/2019

With AccuTerm 7 and 8, you can normally use MultiValue -> Host Programs -> Install
from the main AccuTerm menu to install the host programs. If the automated
install script invoked by the menu fails, you can use the procedure from
AccuTerm 2K2 outlined below.

Beginning with AccuTerm 2K2 release 5.3, you can use a simplified installation
procedure to install the AccuTerm host programs.

To install the host programs using release 5.3 or later, identify the correct
installation script from the list below. You need to be logged in to the
ACCUTERM account (or another account, but Zumasys strongly recommends
loading the host programs into the ACCUTERM account). With your session
at TCL (ECL, command prompt), use Edit->Paste From in the AccuTerm menu, and
choose the script corresponding to your host platform. When you click the
Open button, the FTBP file will be created, then a short "bootstrap" program
will be uploaded to the host. Once the initial paste completes, the bootstrap
program will be compiled, and it will automatically load the base file transfer
(FTBP) programs. You will then be presented with a dialog box where you can
select optional host programs (GUI, ObjectBridge, Smart User Interface,
samples, demos and examples).

For earlier versions of AccuTerm, installation instructions can be found in the
User Guide (File Transfer section) or in the online help (click on "Index" and
type "Install" in the search box).


The following MultiValue platforms are now supported:

FTPINST.TXT 	Generic Pick (some features are not supported for generic systems)
FTPINSADDS.TXT	Mentor, Mentor Pro, MOE
FTPINSAP.TXT	Advanced Pick
FTPINSAP_PRO.TXT Advanced Pick / Pro
FTPINSCACHE.TXT Cache MultiValue (2008 release)
FTPINSD3.TXT	D3 (NT, Linux, Unix except AIX)
FTPINSENH.TXT	Enhanced R83 (Altos, Fujitsu, R91, etc.)
FTPINSJB.TXT	jBase 3.x
FTPINSJB4.TXT	jBase 4.x
FTPINSJB5.TXT	jBase 5.x
FTPINSMD.TXT	McDonnell Douglas, RealityX
FTPINSMV.TXT	mvBase
FTPINSNR.TXT	Northgate Reality 9+
FTPINSOAS.TXT   OASYS
FTPINSOI.TXT	Revelation OpenInsight CTO
FTPINSON.TXT	ONWare MVON
FTPINSPWR95.TXT Power95
FTPINSQM.TXT	QM 2.x MV Database
FTPINSQM3.TXT	QM 3.x MV Database
FTPINSR83.TXT	Pick R83 (FTBP must be a DC file: use "ED MD FTBP" to change)
FTPINSSEQ.TXT	Sequoia, mvEnterprise (except Ultimate emulation)
FTPINSUD.TXT	UniData
FTPINSULT.TXT	Ultimate Plus (all except SCO Unix)
FTPINSULT_SCO.TXT Ultimate Plus on SCO Unix
FTPINSULTX.TXT	Early Ultimate (as implemented on mvEnterprise)
FTPINSUV.TXT	UniVerse
FTPINSVIS.TXT	UniVision


Note: the following versions are used with any platform
running on IBM RS6000 AIX based systems:

FTPINSAP_AIX.TXT Advanced Pick on AIX
FTPINSCACHE_AIX.TXT Cache MultiValue on AIX
FTPINSD3_AIX.TXT D3 on AIX
FTPINSJB_AIX.TXT jBase 3.x on AIX
FTPINSJB4_AIX.TXT jBase 4.x on AIX
FTPINSJB5_AIX.TXT jBase 5.x on AIX
FTPINSNR_AIX.txt Northgate Reality 9+ on AIX
FTPINSON_AIX.TXT ONWare MVON on AIX
FTPINSSEQ_AIX.TXT mvEnterprise on AIX
FTPINSUD_AIX.TXT UniData on AIX
FTPINSUV_AIX.TXT UniVerse on AIX
FTPINSULT_AIX.TXT Ultimate Plus on AIX
FTPINSULTX_AIX.TXT Early Ultimate on AIX (as implemented on mvEnterprise)


Experimental UTF-8 support for platforms without Unicode support (D3)
---------------------------------------------------------------------
To enable UTF-8 on platforms without Unicode support, set line 63
of item 'KMTCFG' in the ACCUTERMCTRL file to 1. This flag modifies
packet length and checksum calculations, converting UTF-8 sequences
into Unicode values when performing calculations. Use of this feature
requires that any characters above CHAR(127) in the data being exchanged
MUST FORM A VALID UTF-8 SEQUENCE! Also, the terminal INPUT must accept
characters above CHAR(127) - for D3, this is handled by executing the
XCS-ON command. AccuTerm must be configured with the host character
set encoding as "Unicode (UTF-8)".


Callable file transfer subroutines
----------------------------------
The FTS subroutine can be called to send or receive data to or from a
dynamic array variable. See source code for FTS for more information.

New subroutines FTSEND & FTRECV provide callable versions of FT. See subroutine
source code for more information.

New subroutines FTEXPORT & FTIMPORT provide callable versions of FTD. See
subroutine source code for more information.


Kermit long packet support
--------------------------
The current Kermit implementation supports long packets. To enable long packets
use the KERMIT command from TCL and enter "C" to configure. The "Maximum packet
size" can range from 35 to 2000 bytes. This setting must be less than the host
input buffer size (auto-CR threshhold for INPUT statement), and affects both
incoming and outgoing packets. The "Source buffer index" further limits the
size of outgoing packets and should be at least as large as the "Maximum packet
size" setting.

NOTE: using long packets does not necessarily mean that file transfer speed will
increase! Empirical testing for D3 and UniVerse suggests best performance with
"maximum packet size" set to 500 and source buffer index" set to 700.
See platform notes below for known size limits.


Client/server protocol settings
-------------------------------
AccuTerm uses a proprietary client/server protocol to provide exchange data
between the host and external Windows applications, such as the wED editor
and GUI designer. This protocol is also used by AccuTerm GUI applications.

The following client/server protocol settings can be adjusted by editing the
KMTCFG item in the ACCUTERMCTRL file:

line 52: incoming packet maximum size (80-9999, default is 80; must be less than
         the auto-CR threshhold for INPUT statement on host platform.) Increasing
         this to 2000 on AIX-based host systems greatly improves performance. See
         platform notes below regarding known size limits.

line 53: outgoing packet maximum size (80-9999, default is 2000.)

line 54: maximum number of attempts to send or get error-free packet (1 to 20,
         default is 4.)

line 55: 1 to disable client/server streaming protocol (reverts to ACK/NAK
         protocol if set.) This is forced ON for AIX-based systems; for other
         platforms, setting this to 1 may improve data integrity if data errors
         are suspected.

line 56: 1 to disable client/server message length integrity check. Setting this
         to 1 may improve performance slightly at the cost of possible undetected
         data loss.

line 57: 1 to disable client/server checksum integrity check. Setting this to 1
         may improve performance, especially on slower hosts, at the cost of
         possible undetected data corruption.

line 58: 1 to disable strict item locks in FTSERVER (WED, GED, etc.) When WED and
         GED have record locking enabled (default), the AccuTerm session will set
         a system item lock for the locked record. This prevents other users from
         updating the same item. However, it may not prevent the same user from
         opening the same item multiple times. For this reason, the FTSERVER program
         maintains its own local lock table which is checked in addition to the
         system level lock. The local lock mechanism is disabled when this line
         is set to 1.

line 59: 1 to disable use of tagged delimiters in client/server protocol.
         Traditionally, MultiValue systems use CHAR(254), CHAR(253) and CHAR(252)
         as dynamic array delimiters. In some languages these characters are
         important, so AccuTerm uses sepcial tags to represent them when
         exchanging. Using tags increases the encoding overhead by 2 or 3
         characters for each delimiter in the data. Set this line to 1 to disable
         this feature.

line 63: 1 to enable UTF-8 on non-Unicode MultiValue installations. See note above
         for more information about this feature.


Platform notes:
---------------

   mVBase: max INPUT line length (packet size) is 1990.

   jBase 4.1: max INPUT line length (packet size) is 1000. 
   jBase 5.x: max INPUT line length (packet size) is 1024. 

   Cache 2008: for best overall performance, edit the KMTCFG item in
   the ACCUTERMCTRL file and change the following lines as follows:
     line 1: 2000
     line 2: 250
     line 52: 2000 (non-AIX) or 250 (AIX)
     line 53: 9999



