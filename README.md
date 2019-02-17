# utl-run-highlighted-code-sas-batch-and-have-log-pop-up
Run highlighted code sas batch and have log pop up.
    Run highlighted code sas batch and have log pop up

     Three options to run code batch

          1. Highlight and type sasbat on the 1980s
             classic editor clean command line (not EG,EE,UE,SAS Studio...)
          2. Highlight and hit a function key
          3. Highlight and hit mouse button

    This requires a  one time setup

    SETUP
    =====

     Setup Not as bad as you think

      1. Need a directory for the program, log and list. I use d:/log
      2. Need to build the SAS command line invocation. I use
          sas  -sysin d:/log/__clp.sas
            -log d:/log/__clp.log
            -print d:/log/__clp.lst
            -config c:\cfg\sasv9.cfg
            -autoexec c:\oto\tut_oto.sas
            -sasautos c:/oto"

      ---- Only if you want to use a function key or mouse action ----

      3. If you want to use a function key. Puth this text on a key.
         ALT F1 store;note;notesubmit '%sasbath;';
      4. Get yourself a programmable mouse like Logitect G203 Prodigy Gaming mouse
         After doing #3
         Map 'ALT F1"  key combination to whatever mouse action you want


    INPUT
    =====

    Highlight lines 1 to 4 and then type sasbat on the command line

    Command ====>
    00001 data _null_;
    00002   set sashelp.class;
    00003   put (_all_) (= $ /);
    00004 run;quit;

    Command ====> sasbat

    --------------
    EXAMPLE OUTPUT
    --------------

    A notepad window will pop up when the batch program completes with the log

    +-------------------------------------------------------------------------+
    | d:/log/__clp.sas                                                        |
    +-------------------------------------------------------------------------+
    | File Edir Format View Help                                              |
    +-------------------------------------------------------------------------+
    |                                                                         |
    | The SAS System            15:26 Sunday, February 17, 2019               |
    |                                                                         |
    | NOTE: Copyright (c) xxxxxxxxx by SAS Institute Inc., Cary, NC, USA.     |
    | NOTE: SAS (r) Proprietary Software 9.4 (TS1M2)                          |
    |       Licensed to xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx    |
    | NOTE: This session is executing on the X64_7PRO  platform.              |
    |                                                                         |
    | NOTE: AUTOEXEC processing completed.                                    |
    |                                                                         |
    | 1   data _null_;                                                        |
    | 2     set sashelp.class(obs=1);                                         |
    | 3     put (_all_) (= $ /);                                              |
    | 4   run;                                                                |
    |                                                                         |
    | NAME=Alfred                                                             |
    | SEX=M                                                                   |
    | AGE=14                                                                  |
    | HEIGHT=69                                                               |
    | WEIGHT=112.5                                                            |
    |                                                                         |
    | NOTE: There were 1 observations read from the data set SASHELP.CLASS.   |
    | NOTE: DATA statement used (Total process time):                         |
    |       real time           0.00 seconds                                  |
    |       cpu time            0.00 seconds                                  |
    |                                                                         |
    | NOTE: SAS Institute Inc., SAS Campus Drive, Cary, NC USA 27513-2414     |
    | NOTE: The SAS System used:                                              |
    |       real time           0.47 seconds                                  |
    |       cpu time            0.26 seconds                                  |
    |                                                                         |
    |                                                                         |
    +-------------------------------------------------------------------------+


    SOLUTION
    ========

    Put this code in your performance package(perpac.sas) or put
    both macros in one member,sasbat.sas, in your autocall library.

    %macro sasbat /cmd des="highlight code type sas7bat on command line";
       store;note;notesubmit '%sasbath;';
    %mend sasbat;

    %macro sasbath;
      /* use your own delete file code or
         %utlfkil(d:/log/__clp.sas);
         %utlfkil(d:/log/__clp.log);
         %utlfkil(d:/log/__clp.lst);
      */
      FILENAME clp clipbrd ;
      DATA _NULL_;
        INFILE clp;
        INPUT;
        file "d:/log/__clp.sas";
        put _infile_;
      RUN;quit;
      filename sin pipe %sysfunc(compbl("sas -sysin d:/log/__clp.sas
            -log d:/log/__clp.log -autoexec c:\oto\tut_oto.sas
           -print d:/log/__clp.lst -config \cfg\sasv9.cfg -sasautos c:/oto"));
       data _null_;
         infile sin;
         input;
         put _infile_;
       run;quit;
       x notepad.exe d:/log/__clp.log;;
    %mend sasBath;


    TEST CASE
    =========

    %utlfkil(d:/log/__clp.sas);
    %utlfkil(d:/log/__clp.log);
    %utlfkil(d:/log/__clp.lst);

    data _null_;
      set sashelp.class(obs=1);
      put (_all_) (= $ /);
    run;quit;




