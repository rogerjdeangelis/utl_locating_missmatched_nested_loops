# utl_locating_missmatched_nested_loops
Locating missmatched nested loops.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Locating missmatched nested loops

    github
    https://github.com/rogerjdeangelis/utl_locating_missmatched_nested_loops

    The NESTING argument instructs SAS to print a note to the log
    at the beginning and end of every DO/END and SELECT/END pairing.

    This is jolly handy for debugging recalcitrant DATA steps with unbalanced nesting.


    INPUT
    =====

    Note three do statements and only 2 end statements

    data class/nesting;
      do i=1 to 10;
        do j=1 to 10;
          if i=1 then do;
             x=j;
          end;
        end;
       stop;
    run;quit;


    EXAMPLE OUTPUT (slightly ediited)
    ==================================

    665   data class/nesting;

    666     do i=1 to 10;
    NOTE  *** DO begin level 1 ***.  ** not matched;

    667       do j=1 to 10;
    NOTE  *** DO begin level 2 ***.

    668         if i=1 then do;
    NOTE  *** DO begin level 3 ***.

    669            x=j;
    670         end;

    NOTE  *** DO end level 3 ***.
    671       end;

    NOTE *** DO end level 2 ***.

                                      ** no level 1;
    672      stop;
    673   run;


    ACTUAL LOG
    ==========

    674   data class/nesting;
    675     do i=1 to 10;
               -
               719
    NOTE 719-185: *** DO begin level 1 ***.

    676       do j=1 to 10;
                 -
                 719
    NOTE 719-185: *** DO begin level 2 ***.

    677         if i=1 then do;
                              -
                              719
    NOTE 719-185: *** DO begin level 3 ***.

    678            x=j;
    679         end;
                ---
                720
    NOTE 720-185: *** DO end level 3 ***.

    680       end;
              ---
              720
    NOTE 720-185: *** DO end level 2 ***.

    681      stop;
    682   run;

