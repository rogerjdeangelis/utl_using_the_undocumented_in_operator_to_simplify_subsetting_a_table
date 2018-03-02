# utl_using_the_undocumented_in_operator_to_simplify_subsetting_a_table
Using the undocumented in operator to simplify subsetting a table.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverfl SAS community.
    Using the undocumented in operator to simplify subsetting a table

    github
    https://tinyurl.com/ycz7pqn8
    https://github.com/rogerjdeangelis/utl_using_the_undocumented_in_operator_to_simplify_subsetting_a_table

    Not exactly the same problem as
    https://communities.sas.com/t5/Base-SAS-Programming/Array-Syntax-Question/m-p/441082

    This feature seems to simplify comparisons when using an arrays.

     Does not work produces an error
      diag1 in (of ds[*])

     Undocumented but works;
      in(diag1,of ds[*]);  * where is checks to see if first arg is in the array. Keep lengths the same though;

     Undocumented and no error but does not do anything useful
      in(of xs[*],of ds[*]) only compares xs[1] withe xs[2:], ds[*]


    INPUT
    ====

      RULE

       Only output records from HAVE where DIAG1 OR DIAG2 is in the the array of diagnosis codes
       Set a flag indicating which DIAG1 or DIAG2 was matched

      array ds[5] $5 d1-d5 ('1' '2' 'E' '5' '9');

      and a list of two diagnosis codes


      WORK.HAVE total obs=8

        DIAG1    DIAG2

          A        1   * 1 is in list
          B        2   * 2 is in list

          3        C
          D        4

          E        E   * E is in list

          6        6
          G        7
          H        8

      What we want
      WORK.WANT total obs=3

         DIAG1    DIAG2  X1    X2

           A        1     0     1  * A is not in list(X1=0) 1 is(X2=1)
           B        2     0     1
           E        E     1     1

    PROCESS
    =======

       * note diag1 in (of ds[*]) is not supported;

       data want;

        retain x1 x2 .;
        array ds[5] $5 d1-d5 ('1' '2' 'E' '5' '9');

        set have;

        * is the first argument any whate in the array;
        x1=in(diag1,of ds[*]);
        x2=in(diag2,of ds[*]);

        if in(diag1,of ds[*]) or in(diag2,of ds[*]) then output;

        drop d1-d5;

       run;quit;

    OUTPUT
    ======

      WORK.WANT total obs=3

         DIAG1    DIAG2  X1    X2

           A        1     0     1  * A is not in list(X1=0) 1 is(X2=1)
           B        2     0     1
           E        E     1     1

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    data have;
     length diag1 diag2 $1;
     input diag1$ diag2$;
    cards4;
    A 1
    B 2
    3 C
    D 4
    E E
    6 6
    G 7
    H 8
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    data want;

     retain x1 x2 .;
     array ds[5] $5 d1-d5 ('1' '2' 'E' '5' '9');

     set have;

     * is the first argument any whate in the array;
     x1=in(diag1,of ds[*]);
     x2=in(diag2,of ds[*]);

     if in(diag1,of ds[*]) or in(diag2,of ds[*]) then output;

     drop d1-d5;

    run;quit;

