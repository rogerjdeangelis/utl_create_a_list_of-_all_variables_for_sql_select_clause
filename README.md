# utl_create_a_list_of-_all_variables_for_sql_select_clause
    Create a list of  all variables for sql select clause;

    see github
    https://tinyurl.com/ybkw542y
    https://github.com/rogerjdeangelis/utl_create_a_list_of-_all_variables_for_sql_select_clause

    I was asked in an interview haw to get a list of all the variables in a SAS
    dataset.

    SQL dictionaries are often too slow on EG servers, especially
    when the server caters to non-programmers.

       Seven Solutions

           1. SQL feedback (best)
           2. SQL dictionaries (write a program andoften too slow on EG servers 10 to 30 minutes)
           3. SQL describe (write separate a program)
           4. proc contents (write separate a program)
           5. proc datasets (write separate a program)
           6. Proc report   (write separate a program)
           7. datastep put (write separate a program  - put (_all_) (= $ +(-1) ',' /);)

    INPUT
    =====

       proc sql;
         select
            *
         from
            sashelp.baseball
       ;quit;


    EXANPLE OUTPUT
    ==============

     proc sql;
      select
         NAME,
         TEAM,
         NATBAT,
         NHITS,
         NHOME,
         NRUNS,
         NRBI,
         NBB,
         YRMAJOR,
         CRATBAT,
         CRHITS,
         CRHOME,
         CRRUNS,
         CRRBI,
         CRBB,
         LEAGUE,
         DIVISION,
         POSITION,
         NOUTS,
         NASSTS,
         NERROR,
         SALARY,
         DIV,
         LOGSALARY
      from
        sashelp.baseball


    PROCESS
    =======

     1. SQL feedback (best)
     ----------------------

        proc sql feedback ;
          reset inobs=1;
          create
             table class1 as
        select
             *
        from
             Sashelp.baseball as _
        ;quit;

        Copy and paste from log into classic editor.
        Yype 'TF11<space>' in the prefix area (classic editor) you get this.

         select
         _.NAME,
         _.TEAM,
         _.NATBAT,
         _.NHITS,
         _.NHOME,
         _.NRUNS,
         _.NRBI,
         _.NBB,
         _.YRMAJOR,
         _.CRATBAT,
         _.CRHITS,
         _.CRHOME,
         _.CRRUNS,
         _.CRRBI,
         _.CRBB,
         _.LEAGUE,
         _.DIVISION,
         _.POSITION,
         _.NOUTS,
         _.NASSTS,
         _.NERROR,
         _.SALARY,
         _.DIV,
         _.LOGSALARY

        Now highlight '_.' and 'cntl x; to remove the '_.' or
        type '((3<space>' in _.NAME prefix area and '((<space>' in '_.LOGSALARY' prefix area
        and the '_.' will go into the bit bucket.


       2. SQL dictionaries (often too slow on EG servers 10 to 30 minutes)
       --------------------------------------------------------------------

          * have to wrire a program;
          proc sql;
            select
                cats(',',name) as nam
            from
                sashelp.vcolumn
            where
                libname="SASHELP" and
                memname = "BASEBALL"
         ;quit;


         * copy and paste from log;

           ,NAME      * remove first comma;
           ,TEAM
           ,NATBAT
           ,NHITS
          ....
           ,SALARY
           ,DIV
           ,LOGSALARY


      3. SQL describe (write separate a program)
      --------------------------------------------

         options nolabel;
         proc sql;
          describe table sashelp.baseball
         ;quit;
         options label;

         create table SASHELP.BASEBALL( label='1986 Baseball Data' bufsize=65536 )
           (
            NAME char(18),
            TEAM char(14),
            NATBAT num,
            NHITS num,
            NHOME num,
            NRUNS num,
            NRBI num,
            NBB num,
            YRMAJOR num,
            CRATBAT num,
            CRHITS num,
            CRHOME num,
            CRRUNS num,
            CRRBI num,
            CRBB num,
            LEAGUE char(8),
            DIVISION char(8),
            POSITION char(8),
            NOUTS num,
            NASSTS num,
            NERROR num,
            SALARY num,
            DIV char(16),
            LOGSALARY num
           );

    4. proc contents (write separate a program)
    --------------------------------------------

        proc contents data=sashelp.baseball position;
        run;quit;

        The CONTENTS Procedure

          Variables in Creation Order

         #    Variable     Type    Len

         1    NAME         Char     18
         2    TEAM         Char     14
         3    NATBAT       Num       8
         4    NHITS        Num       8
         5    NHOME        Num       8
         6    NRUNS        Num       8
         7    NRBI         Num       8
         8    NBB          Num       8
         9    YRMAJOR      Num       8
        10    CRATBAT      Num       8
        11    CRHITS       Num       8
        12    CRHOME       Num       8
        13    CRRUNS       Num       8
        14    CRRBI        Num       8
        15    CRBB         Num       8
        16    LEAGUE       Char      8
        17    DIVISION     Char      8
        18    POSITION     Char      8
        19    NOUTS        Num       8
        20    NASSTS       Num       8
        21    NERROR       Num       8
        22    SALARY       Num       8
        23    DIV          Char     16
        24    LOGSALARY    Num       8


    5. proc datasets (write separate a program)
    -------------------------------------------

        proc datasets lib=sashelp;
        contents data=baseball position;
        run;quit;

        The DATASETS Procedure

          Variables in Creation Order

         #    Variable     Type    Len

         1    NAME         Char     18
         2    TEAM         Char     14
         3    NATBAT       Num       8
         4    NHITS        Num       8
         5    NHOME        Num       8
         6    NRUNS        Num       8
         7    NRBI         Num       8
         8    NBB          Num       8
         9    YRMAJOR      Num       8
        10    CRATBAT      Num       8
        11    CRHITS       Num       8
        12    CRHOME       Num       8
        13    CRRUNS       Num       8
        14    CRRBI        Num       8
        15    CRBB         Num       8
        16    LEAGUE       Char      8
        17    DIVISION     Char      8
        18    POSITION     Char      8
        19    NOUTS        Num       8
        20    NASSTS       Num       8
        21    NERROR       Num       8
        22    SALARY       Num       8
        23    DIV          Char     16
        24    LOGSALARY    Num       8


    6. Proc report   (write separate a program)
    --------------------------------------------

        proc report data=sashelp.baseball(obs=1) nowd missing LIST;
        run;quit;

        PROC REPORT DATA=SASHELP.BASEBALL LS=171 PS=65  SPLIT="/" NOCENTER MISSING ;
        COLUMN  NAME TEAM NATBAT NHITS NHOME NRUNS NRBI NBB YRMAJOR CRATBAT CRHITS CRHOME
       CRRUNS CRRBI CRBB LEAGUE DIVISION POSITION NOUTS NASSTS NERROR SALARY DIV LOGSALARY;

        DEFINE  NAME / DISPLAY FORMAT= $18. WIDTH=18    SPACING=2   LEFT "NAME" ;
        DEFINE  TEAM / DISPLAY FORMAT= $14. WIDTH=14    SPACING=2   LEFT "TEAM" ;
        DEFINE  NATBAT / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "NATBAT" ;
        DEFINE  NHITS / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "NHITS" ;
        DEFINE  NHOME / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "NHOME" ;
        DEFINE  NRUNS / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "NRUNS" ;
        DEFINE  NRBI / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "NRBI" ;
        DEFINE  NBB / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "NBB" ;
        DEFINE  YRMAJOR / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "YRMAJOR" ;
        DEFINE  CRATBAT / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "CRATBAT" ;
        DEFINE  CRHITS / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "CRHITS" ;
        DEFINE  CRHOME / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "CRHOME" ;
        DEFINE  CRRUNS / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "CRRUNS" ;
        DEFINE  CRRBI / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "CRRBI" ;
        DEFINE  CRBB / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "CRBB" ;
        DEFINE  LEAGUE / DISPLAY FORMAT= $8. WIDTH=8     SPACING=2   LEFT "LEAGUE" ;
        DEFINE  DIVISION / DISPLAY FORMAT= $8. WIDTH=8     SPACING=2   LEFT "DIVISION" ;
        DEFINE  POSITION / DISPLAY FORMAT= $8. WIDTH=8     SPACING=2   LEFT "POSITION" ;
        DEFINE  NOUTS / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "NOUTS" ;
        DEFINE  NASSTS / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "NASSTS" ;
        DEFINE  NERROR / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "NERROR" ;
        DEFINE  SALARY / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "SALARY" ;
        DEFINE  DIV / DISPLAY FORMAT= $16. WIDTH=16    SPACING=2   LEFT "DIV" ;
        DEFINE  LOGSALARY / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "LOGSALARY" ;
        RUN;

    7. Datastep put (write separate a program)
    -------------------------------------------

       data _null_;
          set sashelp.class(obs=1);
          put (_numeric_ _character_) ( $ +(-1) ',' /);
       run;quit;

       data _null_;
          set sashelp.class(obs=1);
          call missing(of _all_);
          put (_all_) (= $ +(-1) ',' /);
       run;quit;

       NAME= ,
       SEX= ,
       AGE=.,
       HEIGHT=.,
       WEIGHT=.

