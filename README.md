# CBT731
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. GitHub repos will be deleted and created during this period...

```
//***FILE 731 is from Sam Golob, and contains TSO commands and      *   FILE 731
//*           programs, related to most TSO/E PARMLIB UPDATE        *   FILE 731
//*           functions, TEST command auxiliary programs, also      *   FILE 731
//*           Broadcast Dataset switching, and commands related to  *   FILE 731
//*           the XMIT command (change OUTLIM and WARNING values    *   FILE 731
//*           on the fly, or to display all INMXPARM values).       *   FILE 731
//*                                                                 *   FILE 731
//*           There is a lot of unique stuff here, relating to TSO. *   FILE 731
//*                                                                 *   FILE 731
//*           YOU PROBABLY WILL NOT FIND MANY OF THESE TOOLS        *   FILE 731
//*           OR THEIR EQUIVALENT, ANYWHERE ELSE...!!!              *   FILE 731
//*                                                                 *   FILE 731
//*           Among other areas, many commands relating to the TPVT *   FILE 731
//*           and TCAS control blocks can be found here.            *   FILE 731
//*                                                                 *   FILE 731
//*           Programs relating to several other areas of TSO are   *   FILE 731
//*           also included in this file.                           *   FILE 731
//*                                                                 *   FILE 731
//*           email:  sbgolob@cbttape.org                           *   FILE 731
//*                                                                 *   FILE 731
//*           Recently added, was a command to display the          *   FILE 731
//*           non-default TEST settings from IKJTSOxx (called       *   FILE 731
//*           DTEST).  Added as well, was a program to replace      *   FILE 731
//*           new TEST settings using control cards.  This batch    *   FILE 731
//*           program is called LOADTEST.                           *   FILE 731
//*                                                                 *   FILE 731
//*           This file also contains TSO commands to show and      *   FILE 731
//*           change quantities having to do with the TSOKEYxx      *   FILE 731
//*           PARMLIB member.  (See also CBT File 797)              *   FILE 731
//*                                                                 *   FILE 731
//*           Many programs may be found here, which relate         *   FILE 731
//*           to control blocks that are chained off the TPVT       *   FILE 731
//*           (TSO Parmlib Vector Table), a control block that      *   FILE 731
//*           is not publicly documented by IBM.  See also,         *   FILE 731
//*           commands SHOWTPVT and SHOWTCAS, SHOWTSVT, and         *   FILE 731
//*           SHOWTSB (which has to be run APF-authorized).         *   FILE 731
//*                                                                 *   FILE 731
//*           (Look carefully through this file, because it         *   FILE 731
//*           contains programs that do a lot of stuff in TSO,      *   FILE 731
//*           and you probably can't find other programs like       *   FILE 731
//*           these, anywhere else.)                                *   FILE 731
//*                                                                 *   FILE 731
//*       - - - - - - - - - - - - - - - - - - - - - - - - - - - -   *   FILE 731
//*                                                                 *   FILE 731
//*  **NEW**  We have added several commands designed to change     *   FILE 731
//*  *     *  TCAS parameters on the fly.  They have been named     *   FILE 731
//*  *     *  to match the parameters in the TSOKEYxx PARMLIB       *   FILE 731
//*  *     *  member.  They must be APF-authorized, because the     *   FILE 731
//*  **NEW**  TCAS control block is in fetch-protected storage.     *   FILE 731
//*                                                                 *   FILE 731
//*  **NEW**  These are called (same as TSOKEYxx keywords):         *   FILE 731
//*  *     *  BUFRSIZE, CHNLEN, LOBFREXT, HIBFREXT, RECONLIM,       *   FILE 731
//*  *     *  SCRSIZE, USERMAX, and ENGTRANS.                       *   FILE 731
//*  *     *    ---   These commands are a set.  ---                *   FILE 731
//*  *     *  The command SHOWTCAS summarizes the values in all     *   FILE 731
//*  *     *  of these fields. "SHOWTCAS ALL" formats the entire    *   FILE 731
//*  **NEW**  TCAS control block (macro IKTTCAST).                  *   FILE 731
//*                                                                 *   FILE 731
//*  **NEW**  Since the TCAS parameters are reflected by macro      *   FILE 731
//*  *     *  IKTTCAST in SYS1.MACLIB, which describes the TCAS     *   FILE 731
//*  *     *  control block, which is in Subpool 231 and is         *   FILE 731
//*  *     *  fetch-protected, these programs all must be run       *   FILE 731
//*  *     *  APF-authorized as TSO commands.  You can't even       *   FILE 731
//*  *     *  look at the data without being APF-authorized.        *   FILE 731
//*  *     *  To use these commands, they must be named in the      *   FILE 731
//*  *     *  TSO table IKJEFTE2, or they must be authorized by     *   FILE 731
//*  *     *  some other means.  Without any other operands,        *   FILE 731
//*  *     *  each of these commands tells you the current state    *   FILE 731
//*  *     *  of the setting.  If you enter a number afterward,     *   FILE 731
//*  *     *  then the command changes the setting to that number,  *   FILE 731
//*  *     *  provided that it is in the correct range.             *   FILE 731
//*  **NEW**                                                        *   FILE 731
//*                       ***  SHOWTCAS  ***.                       *   FILE 731
//*                                                                 *   FILE 731
//*           See member $$NOTE01 for some information about        *   FILE 731
//*           the programs, classified by type of use and           *   FILE 731
//*           type of information shown.                            *   FILE 731
//*                                                                 *   FILE 731
//*  **NEW**  More programs in this file mainly pertain to control  *   FILE 731
//*  *     *  blocks chained off the TPVT ("TSO PARMLIB Vector      *   FILE 731
//*  *     *  Table") so we have added a program to display the     *   FILE 731
//*  *     *  general contents and layout of the TPVT, which is     *   FILE 731
//*  *     *  not documented for external use by IBM.  Our program  *   FILE 731
//*  *     *  to display the entire layout of the TPVT is called    *   FILE 731
//*  **NEW**                                                        *   FILE 731
//*                       ***  SHOWTPVT  ***.                       *   FILE 731
//*                                                                 *   FILE 731
//*           *****  FURTHER DESCRIPTION OF MORE PROGRAMS  *****    *   FILE 731
//*                                                                 *   FILE 731
//*           With the recent introduction of the IKJTSOxx LOGON    *   FILE 731
//*           parameters, the authorized LOGOPTS command was        *   FILE 731
//*           written to display and control those options,         *   FILE 731
//*           without having to change IKJTSOxx in PARMLIB and      *   FILE 731
//*           doing a PARMLIB UPDATE(xx) or a SET IKJTSOxx          *   FILE 731
//*           command.                                              *   FILE 731
//*                                                                 *   FILE 731
//*           This class of programs derives from control blocks    *   FILE 731
//*           that are chained off the TPVT control block, which    *   FILE 731
//*           stands for "TSO Parmlib Vector Table".  Its           *   FILE 731
//*           descriptive macro, IKJTPVT, is not in an IBM macro    *   FILE 731
//*           library, but we have provided our own version of      *   FILE 731
//*           IKJTPVT.  IBM's internal version of macro IKJTPVT is  *   FILE 731
//*           only PL/X.  Our version is Assembler.                 *   FILE 731
//*                                                                 *   FILE 731
//*           See member MODGEN in this file, which contains our    *   FILE 731
//*           versions of macros that were not publicly provided    *   FILE 731
//*           by IBM.  Actually, the TPVT is pointed to by a        *   FILE 731
//*           public IBM macro, IKJTSVT, which describes the "TSO   *   FILE 731
//*           Vector Table", and which is in SYS1.MACLIB.  But      *   FILE 731
//*           the details of the TPVT have not been publicly        *   FILE 731
//*           provided by IBM, and that is the kind of work that    *   FILE 731
//*           we are doing here.  (See also, CBT Files 185 and      *   FILE 731
//*           797 for related information.)                         *   FILE 731
//*                                                                 *   FILE 731
//*           Our new program SHOWTPVT displays all your current    *   FILE 731
//*           fields and addresses in the TPVT, showing the         *   FILE 731
//*           layout of the entire TPVT, together with each         *   FILE 731
//*           field's values, on your system.                       *   FILE 731
//*                                                                 *   FILE 731
//*           Added more programs, have to do with another two      *   FILE 731
//*           categories:                                           *   FILE 731
//*                                                                 *   FILE 731
//*           1-  Programs controlling or showing the contents      *   FILE 731
//*               of the TSO Relogon Buffer.  This is an important  *   FILE 731
//*               control block, but its study is often neglected.  *   FILE 731
//*                                                                 *   FILE 731
//*           2-  Programs having to do with 8-character TSO userid *   FILE 731
//*               support, which was introduced with z/OS 2.3.      *   FILE 731
//*                                                                 *   FILE 731
//*        Additional programs:  (even more stuff)                  *   FILE 731
//*                                                                 *   FILE 731
//*        These programs make use of the actual UCB Lookup Table,  *   FILE 731
//*        whose format is hidden by IBM, because if you do a       *   FILE 731
//*        dynamic I/O reconfiguration, this table gets completely  *   FILE 731
//*        reconstructed.  We do not figure that this occurrence    *   FILE 731
//*        will happen often, or in everyday processing.  We        *   FILE 731
//*        figure that most shops will do this during off hours.    *   FILE 731
//*        So during other times, these programs will run fine.     *   FILE 731
//*                                                                 *   FILE 731
//*        1. UCBDASD is a TSO command to display online DASD.      *   FILE 731
//*           This program uses an unusual and undocumented         *   FILE 731
//*           technique to obtain the REAL online DASD UCBs.        *   FILE 731
//*           The main idea of this technique is that you can       *   FILE 731
//*           do a UCB scan of all real UCB's, NOT copies,          *   FILE 731
//*           without being APF-authorized.                         *   FILE 731
//*                                                                 *   FILE 731
//*           (Tested and valid:  ESA 5.2.2 thru z/OS 1.13.)        *   FILE 731
//*           (New version of all 3 programs, tested thru           *   FILE 731
//*           z/OS 3.1.)                                            *   FILE 731
//*                                                                 *   FILE 731
//*           The UCBDASD program can be used as a model to         *   FILE 731
//*           create your own similar programs, and to find         *   FILE 731
//*           real-time UCB's and DCE's without the need of         *   FILE 731
//*           APF-authorization.                                    *   FILE 731
//*                                                                 *   FILE 731
//*           Caveat - If you are not doing a one-time display      *   FILE 731
//*           of the ULUT (UCB Lookup Table), but are changing      *   FILE 731
//*           something or need it for a long time, then you        *   FILE 731
//*           need to serialize the ULUT, and that takes APF        *   FILE 731
//*           authorization.  But for just looking, you should      *   FILE 731
//*           be OK. (SBG)                                          *   FILE 731
//*                                                                 *   FILE 731
//*        2. UCBTAPE is a TSO command to display online tape       *   FILE 731
//*           drives and pending tape mounts.  UCBTAPE# is a        *   FILE 731
//*           help member for UCBTAPE and shows you how to use      *   FILE 731
//*           it.                                                   *   FILE 731
//*                                                                 *   FILE 731
//*           (Tested and valid:  ESA 5.2.2 thru z/OS 1.13.)        *   FILE 731
//*           (New version of all 3 programs, tested thru           *   FILE 731
//*           z/OS 3.1.)                                            *   FILE 731
//*                                                                 *   FILE 731
//*        3. UCBTYPE is a TSO command to display device class      *   FILE 731
//*           totals, based on the ULUT header fields, and on       *   FILE 731
//*           an actual scan of all the UCB's on the system.        *   FILE 731
//*                                                                 *   FILE 731
//*           (Tested and valid:  ESA 5.2.2 thru z/OS 1.13.)        *   FILE 731
//*           (New version of all 3 programs, tested thru           *   FILE 731
//*           z/OS 3.1.)                                            *   FILE 731
//*                                                                 *   FILE 731
//*        Commands are: (see the pds member list for all of them)  *   FILE 731
//*                                                                 *   FILE 731
//*           SHOWTPVT - Non-authorized program to display and      *   FILE 731
//*                      format the ENTIRE TPVT, which is an        *   FILE 731
//*                      undocumented control block (outside of     *   FILE 731
//*                      IBM).  So you won't get this information   *   FILE 731
//*                      pretty much anywhere else.  It's very      *   FILE 731
//*                      useful stuff too, and pretty essential     *   FILE 731
//*                      to know about, if you're dealing with      *   FILE 731
//*                      TSO.                                       *   FILE 731
//*                                                                 *   FILE 731
//*           SHOWTCAS - APF-authorized TSO command to display all  *   FILE 731
//*                      the fields of the TCAS control block,      *   FILE 731
//*                      mapped by macro IKTTCAST, with parameter   *   FILE 731
//*                      ALL.                                       *   FILE 731
//*                                                                 *   FILE 731
//*           SHOWTSB  - Of course this has to run APF authorized.  *   FILE 731
//*                      Displays (in detail) the fields of the     *   FILE 731
//*                      Terminal Status Block (TSB).  Does not     *   FILE 731
//*                      include the TSBX.  (Use DUMPTSB from CBT   *   FILE 731
//*                      File 566 to get an idea about the TSBX.)   *   FILE 731
//*                                                                 *   FILE 731
//*           ALPL     - Authorized TSO/E command to change the     *   FILE 731
//*                      default disposition (system wide) of the   *   FILE 731
//*                      TSO/E ALLOCate command, from OLD to SHR,   *   FILE 731
//*                      or SHR to OLD, without a PARMLIB switch.   *   FILE 731
//*                                                                 *   FILE 731
//*           ADIS     - Non-authorized TSO/E command to display    *   FILE 731
//*                      the incore values of all the TSO/E "auth"  *   FILE 731
//*                      tables generated from the IKJTSOxx PARMLIB *   FILE 731
//*                      member, if these tables exist.  These      *   FILE 731
//*                      tables are lists of program names in       *   FILE 731
//*                      IKJEFTE2 (AUTHCMD), IKJEFTE8 (AUTHPGM),    *   FILE 731
//*                      IKJEFTNS (NOTBKGND), IKJEFTAP (AUTHTSF),   *   FILE 731
//*                      and even PCVE (PLATCMD) and PPVE (PLATPGM) *   FILE 731
//*                      if they exist.                             *   FILE 731
//*                                                                 *   FILE 731
//*           ALLIDS   - Shows your TSO userid in many different    *   FILE 731
//*                      places.  There are situations where your   *   FILE 731
//*                      userid can be one thing in one place, and  *   FILE 731
//*                      something else in another place.  This     *   FILE 731
//*                      command detects a lot of that.  Also it is *   FILE 731
//*                      fully compatible with 8-character userids. *   FILE 731
//*                  >>  THIS IS A VERY IMPORTANT PROGRAM FOR       *   FILE 731
//*                  >>  DOING 8-CHARACTER USERID CONVERSIONS.      *   FILE 731
//*                      Running ALLIDS non-authorized will give    *   FILE 731
//*                      a message saying the TSBX userid cannot    *   FILE 731
//*                      be displayed. (The id is in SP 231, and    *   FILE 731
//*                      needs authorization.)                      *   FILE 731
//*                                                                 *   FILE 731
//*           BYE      - Allows you to put arbitrary data into      *   FILE 731
//*                      the TSO Relogon Buffer, and setup a        *   FILE 731
//*                      logoff as soon as the session reaches      *   FILE 731
//*                      READY mode.  The data in the Relogon       *   FILE 731
//*                      Buffer will then be executed by the        *   FILE 731
//*                      TSO LOGON processor.                       *   FILE 731
//*                                                                 *   FILE 731
//*                      BYE with no operations, initializes the    *   FILE 731
//*                      Relogon Buffer, and reverses the effects   *   FILE 731
//*                      of BYE with operands.  BYE works together  *   FILE 731
//*                      with the SHOWRLGB command.                 *   FILE 731
//*                                                                 *   FILE 731
//*           CPFX     - 8-character userid support to change the   *   FILE 731
//*                      prefix of your TSO session.  Like          *   FILE 731
//*                      PROFILE PREFIX(prefix) IBM command, but    *   FILE 731
//*                      fully supports 8-character prefixes.       *   FILE 731
//*                      Compatible with z/OS 2.3 8-character id    *   FILE 731
//*                      support.                                   *   FILE 731
//*                                                                 *   FILE 731
//*           DACEE    - Non-authorized TSO command to display      *   FILE 731
//*                      the entire contents of this userid's       *   FILE 731
//*                      ACEE, interpreting the flag fields and     *   FILE 731
//*                      dumping the entire control block in HEX    *   FILE 731
//*                      and EBCDIC.  Optionally format the UTOKEN  *   FILE 731
//*                      and dump it.  Dump the ACEX as well.       *   FILE 731
//*                                                                 *   FILE 731
//*           DTEST    - Non-authorized TSO command to display      *   FILE 731
//*                      the IKJTSOxx values in the TEST listing.   *   FILE 731
//*                      Defaults to blank entries if TEST is       *   FILE 731
//*                      not coded.                                 *   FILE 731
//*                                                                 *   FILE 731
//*           LOADTEST - This is a batch program to reload the      *   FILE 731
//*                      IKJTSOxx values in the TEST listing,       *   FILE 731
//*                      using control cards.  APF-authorized       *   FILE 731
//*                      if reloading the TEST parameters from      *   FILE 731
//*                      control cards.  Not APF if reporting       *   FILE 731
//*                      only.                                      *   FILE 731
//*                      If you want to run LOADTEST under TSO,     *   FILE 731
//*                      then allocate DD SYSPRINT to the terminal  *   FILE 731
//*                      and then execute the program LOADTEST.     *   FILE 731
//*                                                                 *   FILE 731
//*           LOADT*** - Members providing support for the          *   FILE 731
//*                      LOADTEST program.                          *   FILE 731
//*                                                                 *   FILE 731
//*           DVAT     - Display original VATLSTxx settings after   *   FILE 731
//*                      IPL time.  Caveats mentioned in the        *   FILE 731
//*                      program.  A useful display.                *   FILE 731
//*                                                                 *   FILE 731
//*           EESCB    - Display all fields relating to the         *   FILE 731
//*                      status of TSO/E Broadcast Dataset          *   FILE 731
//*                      switching.                                 *   FILE 731
//*                                                                 *   FILE 731
//*           INMXD    - Display all fields relating to the         *   FILE 731
//*                      INMXPARM control block containing all      *   FILE 731
//*                      the parameters affecting the TSO XMIT      *   FILE 731
//*                      command.  (Use INMXD to test the           *   FILE 731
//*                      results of the CINMX command.)             *   FILE 731
//*                                                                 *   FILE 731
//*           CINMX    - Change the XMIT OUTLIM, Wait Threshold,    *   FILE 731
//*                      and Wait Interval quantities in storage.   *   FILE 731
//*                      Changes are effective immediately, and     *   FILE 731
//*                      do not depend on any PARMLIB settings.     *   FILE 731
//*                      (This command has to be APF authorized.)   *   FILE 731
//*                                                                 *   FILE 731
//*           HIBFREXT - A special-purpose command to change the    *   FILE 731
//*                      numeric value of the HIBFREXT field in     *   FILE 731
//*                      the IKJTCAST (VTAM/TSO) control block.     *   FILE 731
//*                      This program can be adapted to change      *   FILE 731
//*                      any fullword numeric value in a different  *   FILE 731
//*                      numeric field.  Must be APF-authorized.    *   FILE 731
//*                      (Change it in two places. 1- where the     *   FILE 731
//*                      old value is recorded, and 2- where the    *   FILE 731
//*                      new value gets substituted in.  All the    *   FILE 731
//*                      numeric checks are already in place.)      *   FILE 731
//*                                                                 *   FILE 731
//*           LOBFREXT - Works the same as HIBFREXT except that     *   FILE 731
//*                      it changes the LOBFREXT value instead      *   FILE 731
//*                      of the HIBFREXT value.  Now you can        *   FILE 731
//*                      adjust both of them.                       *   FILE 731
//*                                                                 *   FILE 731
//*           LOGOPTS  - Allows complete control of settings        *   FILE 731
//*                      in the IKJTSOxx LOGON parameters, on       *   FILE 731
//*                      the fly.  Without operands, LOGOPTS        *   FILE 731
//*                      tells you the current state of the         *   FILE 731
//*                      flags.  With operands (APF-authorized)     *   FILE 731
//*                      LOGOPTS can switch off or on, any of       *   FILE 731
//*                      the IKJTSOxx LOGON settings.               *   FILE 731
//*                                                                 *   FILE 731
//*                      The effect is global, for the whole LPAR,  *   FILE 731
//*                      so be careful.                             *   FILE 731
//*                                                                 *   FILE 731
//*                      Bits are set as follows for the            *   FILE 731
//*                      following options:                         *   FILE 731
//*                                                                 *   FILE 731
//*                      X'08' -  Password Phrase Support           *   FILE 731
//*                      X'04' -  Applid Verification               *   FILE 731
//*                      X'02' -  LOGONHERE Support                 *   FILE 731
//*                      X'01' -  Password Preprompt Support        *   FILE 731
//*                                                                 *   FILE 731
//*                      LOGOPTS PARMS (to set the bit on/off)      *   FILE 731
//*                      ------- -----                              *   FILE 731
//*                       S -  Password Phrase Support    SF - off  *   FILE 731
//*                       A -  Applid Verification        AF - off  *   FILE 731
//*                       H -  LOGONHERE Support          HF - off  *   FILE 731
//*                       P -  Password Preprompt Support PF - off  *   FILE 731
//*                                                                 *   FILE 731
//*           LOKSES   - Lock your terminal session.                *   FILE 731
//*                      Based on an ancient program by JR Broido.  *   FILE 731
//*                                                                 *   FILE 731
//*           MEMBER   - Program from Bill Godfrey to list          *   FILE 731
//*                      attributes of a pds member.                *   FILE 731
//*                                                                 *   FILE 731
//*           MYIDP    - This was a little ditty on File 247 just   *   FILE 731
//*                      to show your userid, but now it shows you  *   FILE 731
//*                      a lot more about your TSO session so we    *   FILE 731
//*                      are bringing it here.                      *   FILE 731
//*                                                                 *   FILE 731
//*           RELOGON  - Generates a LOGON command for your         *   FILE 731
//*                      current session, puts it into the TSO      *   FILE 731
//*                      Relogon Buffer, and sets the switch to     *   FILE 731
//*                      LOGOFF when you reach READY mode.  If      *   FILE 731
//*                      the session has a TSB password, then it    *   FILE 731
//*                      is included, as: LOGON userid/password.    *   FILE 731
//*                                                                 *   FILE 731
//*           SYSLV    - Quick and dirty display of z/OS (MVS)      *   FILE 731
//*                      System Level and CPU model.  Might be      *   FILE 731
//*                      quick and dirty, but it uses PUTLINE.      *   FILE 731
//*                                                                 *   FILE 731
//*           SHOWRLGB - A complete display of information about    *   FILE 731
//*                      the TSO Relogon Buffer, including whether  *   FILE 731
//*                      the ECTLOGF switch is set, to force        *   FILE 731
//*                      LOGOFF at READY mode.                      *   FILE 731
//*                                                                 *   FILE 731
//*           TPRV/NPRV  Give/take TSO privileges (must run APF-    *   FILE 731
//*                      authorized)                                *   FILE 731
//*                                                                 *   FILE 731
//*           TRMSZRPT - TSO command to report the terminal size    *   FILE 731
//*                      for all logged-on TSO users.  APF-auth.    *   FILE 731
//*                                                                 *   FILE 731
//*           TSUINFO  - TSO command to display some unusual        *   FILE 731
//*                      information about all logged-on TSO        *   FILE 731
//*                      users.  APF-authorized.                    *   FILE 731
//*                                                                 *   FILE 731
//*           TSVT8    - Turns 8-character userid support on/off    *   FILE 731
//*                      for testing purposes.  Works with z/OS     *   FILE 731
//*                      2.3 or higher.                             *   FILE 731
//*                                                                 *   FILE 731
//*           TSVTTMO  - For z/OS 2.4 and higher:                   *   FILE 731
//*                      TSO LOGON screen timeout is now           .*   FILE 731
//*                      adjustable.  It used to be fixed at        *   FILE 731
//*                      5 minutes, but IBM was told of the need    *   FILE 731
//*                      to make it adjustable for some shops.      *   FILE 731
//*                      The TSVTTMO TSO command can display or     *   FILE 731
//*                      change the LOGON screen's timeout value.   *   FILE 731
//*                      (i.e. If you type LOGON userid without     *   FILE 731
//*                      typing a password, the system gets a       *   FILE 731
//*                      TSO "STARTING" session going, but it does  *   FILE 731
//*                      not stay there--it times out.  So how      *   FILE 731
//*                      long does the screen stay there before     *   FILE 731
//*                      timing out?  This is what we are adjus-    *   FILE 731
//*                      ting.  The default value is 5 minutes.)    *   FILE 731
//*                                                                 *   FILE 731
//*           UCBDASD  - Display all online DASD volumes, in        *   FILE 731
//*                      real-time, without being APF-authorized.   *   FILE 731
//*                      Gives real UCB's.                          *   FILE 731
//*                                                                 *   FILE 731
//*           UCBPUBL  - APF-Authorized TSO commands to make        *   FILE 731
//*           UCBPRIV    a DASD volume PUBLIC, PRIVATE, or          *   FILE 731
//*           UCBSTOR    STORAGE.  With these commands, you         *   FILE 731
//*                      don't need to run a MOUNT command from     *   FILE 731
//*                      the console.  Works by zapping the         *   FILE 731
//*                      real UCB of the pack.                      *   FILE 731
//*                                                                 *   FILE 731
//*           UCBTAPE  - Display all online tape drives and         *   FILE 731
//*                      pending tape mounts.  See help member      *   FILE 731
//*                      UCBTAPE#.                                  *   FILE 731
//*                                                                 *   FILE 731
//*           ULUONLN  - Displays all online devices attached       *   FILE 731
//*                      to the system.  Needs ULU**** macros:      *   FILE 731
//*                      ULUINIT, ULUDSECT, ULUSCAN.                *   FILE 731
//*                      Not APF-authorized.  Gives REAL UCB's.     *   FILE 731
//*                                                                 *   FILE 731
//*           USERMAX  - Another way of doing F TSO,USERMAX=nnnn    *   FILE 731
//*                      Good if you can't get to a console.        *   FILE 731
//*                      This TSO command must be APF-authorized.   *   FILE 731
//*                                                                 *   FILE 731
//*           XOR      - A simple program to display the XOR of     *   FILE 731
//*                      up to 8 characters that have been input.   *   FILE 731
//*                                                                 *   FILE 731
//*           ZAPLWA   - Use this program for experimental          *   FILE 731
//*                      purposes to alter your own TSO session's   *   FILE 731
//*                      logon characteristics.  (Probably should   *   FILE 731
//*                      not be used for "production use")          *   FILE 731
//*                                                                 *   FILE 731
//*           A load library containing all of these load modules   *   FILE 731
//*           is contained in this pds, in XMIT format, as member   *   FILE 731
//*           LOADLIB.  If you want to assemble all these           *   FILE 731
//*           commands, you will need the EESCB.MODGEN macro        *   FILE 731
//*           library, which can be generated from the MODGEN       *   FILE 731
//*           member of this pds, using the $PDSLOAD member.        *   FILE 731
//*           The PDSLOAD program is also included in the load      *   FILE 731
//*           library containing the programs from this file,       *   FILE 731
//*           so if you TSO RECEIVE the member called LOADLIB,      *   FILE 731
//*           you'll have the PDSLOAD program too.                  *   FILE 731
//*                                                                 *   FILE 731
//*     These commands have been tested and found to work on MVS    *   FILE 731
//*     systems back to MVS/ESA 5.2.2 and all the OS/390 releases.  *   FILE 731
//*     Also tested on z/OS thru 3.1. These commands contain dual   *   FILE 731
//*     coding paths to accommodate the older TSO/E releases.       *   FILE 731
//*                                                                 *   FILE 731
//*           email:  sbgolob@cbttape.org                           *   FILE 731
//*                                                                 *   FILE 731
//*     Control Block Notes:                                        *   FILE 731
//*     ------- ----- -----                                         *   FILE 731
//*                                                                 *   FILE 731
//*     1.  The IKJTPVT control block is chained off the TSO Vector *   FILE 731
//*         Table (mapped by IKJTSVT in SYS1.MACLIB), and is OCO    *   FILE 731
//*         officially.  But if you look in the IKJTSVT macro,      *   FILE 731
//*         the pointer to the TPVT control block is officially     *   FILE 731
//*         documented.  And if you look at the TPVT in storage,    *   FILE 731
//*         it is pretty obvious that X'20' off the beginning,      *   FILE 731
//*         points to the IKJEESCB control block, which contains    *   FILE 731
//*         the BROADCAST and USERLOG dataset status, and (in       *   FILE 731
//*         TSO/E Release 3 and higher, where BROADCAST dataset     *   FILE 731
//*         switching is supported,) the BROADCAST switching        *   FILE 731
//*         status too.  Macro IKJEESCB is in SYS1.MODGEN.          *   FILE 731
//*                                                                 *   FILE 731
//*     2.  The TPVT control block was mapped, by guesswork, in     *   FILE 731
//*         the SHOWzOS work (on CBT Tape File 492).  I have        *   FILE 731
//*         therefore included the necessary macros from the        *   FILE 731
//*         SHOWzOS maclib, called IKJTPVT and IKJCTLT, for         *   FILE 731
//*         assembling the EESCB command.                           *   FILE 731
//*                                                                 *   FILE 731
//*     3.  Older versions of the IKJEESCB control block are        *   FILE 731
//*         supported here, too.  These are TSO/E Release 2 and     *   FILE 731
//*         lower, going up to z/OS 1.2.  At those levels,          *   FILE 731
//*         BROADCAST dataset switching was not yet supported,      *   FILE 731
//*         so the extension to the IKJEESCB control block that     *   FILE 731
//*         supports an alternate BROADCAST dataset is not yet      *   FILE 731
//*         there.  But in IKJEESCB level 2, all of the PARMLIB     *   FILE 731
//*         UPDATE stuff IS there, except for the dataset switch-   *   FILE 731
//*         ing stuff.  In IKJEESCB level 1, less detail about      *   FILE 731
//*         the TSO Userlogs is retained in the control block.      *   FILE 731
//*                                                                 *   FILE 731
//*     4.  It seems that the purpose of the IKJTPVT control        *   FILE 731
//*         block is to point to all the information that is        *   FILE 731
//*         involved in either a PARMLIB UPDATE(xx) TSO command     *   FILE 731
//*         execution, or (in TSO/E Release 3) a SET IKJTSO=xx      *   FILE 731
//*         operator command, which does the same thing.  So any    *   FILE 731
//*         new control blocks that are involved with the           *   FILE 731
//*         IKJTSOxx parameters, are pointed to by the TPVT.  It    *   FILE 731
//*         seems to me, that IBM wants to keep some flexibility    *   FILE 731
//*         with regard to new development of TSO-based system      *   FILE 731
//*         controls, so that is why they are keeping the IKJTPVT   *   FILE 731
//*         control block OCO.  (Also, the pointers to the "auth"   *   FILE 731
//*         tables are there.)  Nevertheless, we are using it,      *   FILE 731
//*         because we have to.                                     *   FILE 731
//*                                                                 *   FILE 731
//*     5.  Details from both the IKJEESCB control block and the    *   FILE 731
//*         TPVT control block which are relevant to the BROADCAST  *   FILE 731
//*         dataset, are shown by the EESCB TSO command.  For       *   FILE 731
//*         TSO/E Release 3, I have pulled the stops out, and I     *   FILE 731
//*         have tried to show just about everything related to     *   FILE 731
//*         the BROADCAST dataset that there is.  (With the         *   FILE 731
//*         exception of the second set of timings.)                *   FILE 731
//*                                                                 *   FILE 731
//*     6.  A TSO command to show the contents of all the fields    *   FILE 731
//*         in your TPVT control block, together with their         *   FILE 731
//*         explanations, is called SHOWTPVT.  Most of the          *   FILE 731
//*         addresses shown by SHOWTPVT point to the different      *   FILE 731
//*         control blocks that govern each IKJTSOxx parameter.     *   FILE 731
//*         The complete control block, with every field specified, *   FILE 731
//*         is shown and explained.                                 *   FILE 731
//*                                                                 *   FILE 731
//*     TPUT and PUTLINE Notes:                                     *   FILE 731
//*     ---- --- ------- -----                                      *   FILE 731
//*                                                                 *   FILE 731
//*     The output of the EESCB command, especially for TSO/E       *   FILE 731
//*     Release 3, is quite long, so rather than having it          *   FILE 731
//*     overflow several screens, I have tried to use the PUTLINE   *   FILE 731
//*     interface to the TSO screen, which is "capturable" by the   *   FILE 731
//*     SYSOUTTRAP service, rather than using the TPUT interface    *   FILE 731
//*     to the screen, which is not capturable.                     *   FILE 731
//*                                                                 *   FILE 731
//*     However, in coding the EESCB command, I used the TPUT       *   FILE 731
//*     interface first, which is much simpler to code and set      *   FILE 731
//*     up.  This made for easier development.                      *   FILE 731
//*                                                                 *   FILE 731
//*     After having coded EESCB using TPUT, I then converted it    *   FILE 731
//*     to PUTLINE using Howard Dean and Jim Elsworth's interface   *   FILE 731
//*     to set up PUTLINE, which is included here, too, for your    *   FILE 731
//*     edification and easy access (originally from CBT File 136). *   FILE 731
//*                                                                 *   FILE 731
//*     This PUTLINE interface works as follows:                    *   FILE 731
//*                                                                 *   FILE 731
//*     A program called EPUTL, which can be separately assembled   *   FILE 731
//*     and linkedited, sets up the PUTLINE interface externally    *   FILE 731
//*     from the TSO command which calls it.  The call to EPUTL     *   FILE 731
//*     is then set up by a macro called APUT, which has the same   *   FILE 731
//*     coding rules as a single line TPUT.  Therefore, if you      *   FILE 731
//*     linkedit the EPUTL routine into your TSO command load       *   FILE 731
//*     module, and you convert all coded TPUTs in the source code  *   FILE 731
//*     to APUTs, then the TSO screen interface is automagically    *   FILE 731
//*     transformed from TPUT to PUTLINE.                           *   FILE 731
//*                                                                 *   FILE 731
//*     The EPUTL program assumes that the calling TSO command      *   FILE 731
//*     had been set up properly, using standard IBM linkage        *   FILE 731
//*     conventions.  Once that has been assumed, and if it is      *   FILE 731
//*     indeed true, EPUTL can nose around in the caller's saved    *   FILE 731
//*     registers and pull out the caller's CPPL, so that it can    *   FILE 731
//*     set up the PUTLINE environment and issue the PUTLINE        *   FILE 731
//*     macro, independently linkedited from the calling program.   *   FILE 731
//*     It is a nice idea.  Most TSO commands are properly coded,   *   FILE 731
//*     and therefore it is possible to set up PUTLINE service in   *   FILE 731
//*     this easy manner.                                           *   FILE 731
//*                                                                 *   FILE 731
//*     This system makes TSO command coding (that has terminal     *   FILE 731
//*     outputs) much easier too, because you can code the outputs  *   FILE 731
//*     using TPUT and convert them to PUTLINE later.               *   FILE 731
//*                                                                 *   FILE 731
//*     The EESCB source code already includes the APUT and the     *   FILE 731
//*     EPUTL source, together with it inline.  But I have          *   FILE 731
//*     nevertheless put separate copies of them into this pds,     *   FILE 731
//*     so you don't have to go to the trouble of extracting        *   FILE 731
//*     them, if you want to use them for other applications.       *   FILE 731
//*                                                                 *   FILE 731
//*     SYSOUTTRAP Notes:                                           *   FILE 731
//*     ---------- -----                                            *   FILE 731
//*                                                                 *   FILE 731
//*     To make it easier to view the entire EESCB output on the    *   FILE 731
//*     screen, I have made it possible (using Mark Zelden's TSOE,  *   FILE 731
//*     TSOB, TSOV, and TSOR execs) to make the output scrollable.  *   FILE 731
//*                                                                 *   FILE 731
//*     These are Mark's EXECs:                                     *   FILE 731
//*                                                                 *   FILE 731
//*     TSOE SYSOUTTRAPs the TSO command output, and allocates a    *   FILE 731
//*     temporary file, which it EDITs.                             *   FILE 731
//*                                                                 *   FILE 731
//*     TSOB SYSOUTTRAPs the TSO command output, and allocates a    *   FILE 731
//*     temporary file, which it BROWSEs.                           *   FILE 731
//*                                                                 *   FILE 731
//*     TSOV SYSOUTTRAPs the TSO command output, and allocates a    *   FILE 731
//*     temporary file, which it VIEWs.                   ..        *   FILE 731
//*                                                                 *   FILE 731
//*     TSOR SYSOUTTRAPs the TSO command output, and allocates a    *   FILE 731
//*     temporary file, which it displays using the REVIEW command  *   FILE 731
//*     from CBT File 134 (load modules on File 135).  REVIEW       *   FILE 731
//*     works either from within ISPF, or from TSO READY mode.      *   FILE 731
//*     So TSOR EESCB works from TSO READY mode as well as from     *   FILE 731
//*     an ISPF command line, as TSO TSOR EESCB.                    *   FILE 731
//*                                                                 *   FILE 731
//*     To use the TSOE, TSOB, TSOV, and TSOR execs, just copy      *   FILE 731
//*     them to a SYSPROC or SYSEXEC library, and issue:            *   FILE 731
//*                                                                 *   FILE 731
//*        TSO TSOE EESCB         or                                *   FILE 731
//*        TSO TSOB EESCB         or                                *   FILE 731
//*        TSO TSOV EESCB         or                                *   FILE 731
//*        TSO TSOR EESCB         from an ISPF command line,        *   FILE 731
//*     or     TSOR EESCB from TSO READY mode, all of which         *   FILE 731
//*     will produce scrollable output.                             *   FILE 731
//*                                                                 *   FILE 731
//* --------------------------------------------------------------- *   FILE 731
//*                                                                 *   FILE 731
//*   I added another command called CINMX, which will reset        *   FILE 731
//*   the TRANSREC OUTLIM number as a TSO command.  (It will be     *   FILE 731
//*   helpful to remember the old number before running this        *   FILE 731
//*   command.)  You just run the TSO command:                      *   FILE 731
//*                                                                 *   FILE 731
//*   CINMX nnnnnn     where nnnnnn has to be numeric, up to 10     *   FILE 731
//*                    digits, and the OUTLIM is reset in core,     *   FILE 731
//*                    to this value.                               *   FILE 731
//*                                                                 *   FILE 731
//*   CINMX W nnnnnn   adjusts the TRANSREC Warn Threshold          *   FILE 731
//*                                                                 *   FILE 731
//*   CINMX I nnnnnn   adjusts the TRANSREC Warn Interval           *   FILE 731
//*                                                                 *   FILE 731
//*   Of necessity this command has to be APF authorized.           *   FILE 731
//*                                                                 *   FILE 731
//*   Reason for this command:                                      *   FILE 731
//*                                                                 *   FILE 731
//*   If you want to transmit a large file, and your installation   *   FILE 731
//*   won't allow it because the TRANSREC OUTLIM value in PARMLIB   *   FILE 731
//*   was too low, you can just adjust it up, transmit your file,   *   FILE 731
//*   and adjust it back.  Now, with the W and I parameters, you    *   FILE 731
//*   can use CINMX to adjust the Warn Threshold and Warn Interval  *   FILE 731
//*   values, too.  You can test the results of the CINMX command   *   FILE 731
//*   by running the INMXD command, which displays the INMXPARM     *   FILE 731
//*   control block values currently in storage.                    *   FILE 731
//*                                                                 *   FILE 731
//*   Note: -                                                       *   FILE 731
//*   The CINMX program will also work at the OS/390 1.3 level and  *   FILE 731
//*   before, even though the displacements into the INMXPARM       *   FILE 731
//*   control block were 4 bytes earlier.  (CINMX Ver 1.2)          *   FILE 731
//*   All of these commands were tested on MVS systems back to      *   FILE 731
//*   ESA 5.2.2 and up through z/OS 3.1.                            *   FILE 731
//*                                                                 *   FILE 731
//*   Another Note: -                                               *   FILE 731
//*   When using the XMIT command just to reformat a pds to make    *   FILE 731
//*   it sequential, or just to reformat a sequential dataset to    *   FILE 731
//*   convert it to recfm-lrecl FB-80, the OUTLIM parameter in      *   FILE 731
//*   PARMLIB member IKJTSOxx used to cut it off at the OUTLIM      *   FILE 731
//*   size.  This was before z/OS 1.9.  So if you are running an    *   FILE 731
//*   older operating system and you want just to XMIT a very       *   FILE 731
//*   large file, then you have to use CINMX to increase the        *   FILE 731
//*   OUTLIM parameter to a large enough value.  At z/OS 1.9 and    *   FILE 731
//*   later, if you are just reformatting a file and not sending    *   FILE 731
//*   it to another system, then the OUTLIM parameter is ignored    *   FILE 731
//*   and you don't have to do anything.  The OUTLIM parameter      *   FILE 731
//*   was intended to prevent the clogging of NJE transmission      *   FILE 731
//*   lines between different MVS (z/OS) systems.  So if you are    *   FILE 731
//*   just reformatting a file on the same system, then it is not   *   FILE 731
//*   necessary.  (Nevertheless before z/OS 1.9 it used to ALWAYS   *   FILE 731
//*   cut off an XMIT file in the middle, if OUTLIM was not big     *   FILE 731
//*   enough.  Yecch.)                                              *   FILE 731
//*                                                                 *   FILE 731
```
