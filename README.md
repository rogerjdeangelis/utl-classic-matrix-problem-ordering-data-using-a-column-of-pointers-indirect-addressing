# utl-classic-matrix-problem-ordering-data-using-a-column-of-pointers-indirect-addressing
Classic matrix problem ordering data using a column of pointers indirect addressing
    %let pgm=utl-classic-matrix-problem-ordering-data-using-a-column-of-pointers-indirect-addressing;

    Classic matrix problem ordering data using a column of pointers indirect addressing

    This kind of processing is done under the covers in many applications ie hash

          Solutions
              1  sas dataset
              2. r matrix

      Related Repos on end

    github
    https://tinyurl.com/yzf66w8z
    https://github.com/rogerjdeangelis/utl-classic-matrix-problem-ordering-data-using-a-column-of-pointers-indirect-addressing

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*                 INPUT                          PROCESS                                   OUTPUT                        */
    /*                 =====                          =======                                   ======                        */
    /*                                                                                                                        */
    /* 1  SAS DATASET                                                                                                         */
    /* ==============                                                                                                         */
    /*                                                                                                                        */
    /*   POINTER  LTR                          data want;                               LTR                                   */
    /*                                            retain pointer;                                                             */
    /*      4      B                              do until (dne);                        A    have[row 4] = A                 */
    /*      1      D                                 set sd1.have                        B    have[row 1] = B                 */
    /*      3      C                                   (rename=pointer=pt)               C    have[row 3] = C                 */
    /*      2      A                                  end=dne;                           D    hvae[row 2] = D                 */
    /*                                               set sd1.have point=pt;                                                   */
    /* ROW TARGET ROW                              output;                                                                    */
    /*  1      4      B    have[row 4] = A       end;                                                                         */
    /*          \   /                            stop;                                                                        */
    /*           \ /                           run;quit;                                                                      */
    /*  2      1 /\ / D    have[row 2] = B                                                                                    */
    /*             /                                                                                                          */
    /*  3      3 -/---C    have[row 3] = C                                                                                    */
    /*           /   \                                                                                                        */
    /*  4      2      A    have[row 2] = D                                                                                    */
    /*                                                                                                                        */
    /*------------------------------------------------------------------------------------------------------------------------*/
    /*                                                                                                                        */
    /* 2  R MATRIX                                                                                                            */
    /* ===========                                                                                                            */
    /*                                                                                                                        */
    /*         SAME INPUT                      %utl_rbegin;                             ROWNAMES    POINTER    LTR            */
    /*                                         parmcards4;                                                                    */
    /*                                         library(haven)                              1          2        A              */
    /*                                         have<-read_sas("d:/sd1/have.sas7bdat")      2          4        B              */
    /*                                         want<-have[have$POINTER,]                   3          3        C              */
    /*                                         source("c:/temp/fn_tosas9.R")               4          1        D              */
    /*                                         fn_tosas9(dataf=want)                                                          */
    /*                                         ;;;;                                                                           */
    /*                                         %utl_rend;                                                                     */
    /*                                                                                                                        */
    /*                                         libname tmp "c:/temp";                                                         */
    /*                                         proc print data=tmp.want;                                                      */
    /*                                         run;quit;                                                                      */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                       _       _            _
    / |  ___  __ _ ___    __| | __ _| |_ __ _ ___| |_ ___ _ __
    | | / __|/ _` / __|  / _` |/ _` | __/ _` / __| __/ _ \ `_ \
    | | \__ \ (_| \__ \ | (_| | (_| | || (_| \__ \ ||  __/ |_) |
    |_| |___/\__,_|___/  \__,_|\__,_|\__\__,_|___/\__\___| .__/
     _                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */


    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    input pointer ltr $1.;
    cards4;
    4 B
    1 D
    3 C
    2 A
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* SD1.HAVE total obs=4                                                                                                   */
    /*                                                                                                                        */
    /* Obs    POINTER    LTR                                                                                                  */
    /*                                                                                                                        */
    /*  1        4        B                                                                                                   */
    /*  2        1        D                                                                                                   */
    /*  3        3        C                                                                                                   */
    /*  4        2        A                                                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    data want;
       retain pointer;
       do until (dne);
          set sd1.have
            (rename=pointer=pt)
           end=dne;
          set sd1.have point=pt;
        output;
      end;
      stop;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Obs    POINTER    LTR                                                                                                  */
    /*                                                                                                                        */
    /*  1        2        A                                                                                                   */
    /*  2        4        B                                                                                                   */
    /*  3        3        C                                                                                                   */
    /*  4        1        D                                                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                           _        _
    |___ \   _ __   _ __ ___   __ _| |_ _ __(_)_  __
      __) | | `__| | `_ ` _ \ / _` | __| `__| \ \/ /
     / __/  | |    | | | | | | (_| | |_| |  | |>  <
    |_____| |_|    |_| |_| |_|\__,_|\__|_|  |_/_/\_\

    */

    %utl_rbegin;
    parmcards4;
    library(haven)
    have<-read_sas("d:/sd1/have.sas7bdat")
    want<-have[have$POINTER,]
    source("c:/temp/fn_tosas9.R")
    fn_tosas9(dataf=want)
    ;;;;
    %utl_rend;

    libname tmp "c:/temp";
    proc print data=tmp.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Obs    ROWNAMES    POINTER    LTR                                                                                     */
    /*                                                                                                                        */
    /*  1         1          2        A                                                                                       */
    /*  2         2          4        B                                                                                       */
    /*  3         3          3        C                                                                                       */
    /*  4         4          1        D                                                                                       */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    REPOS

    https://github.com/rogerjdeangelis/utl-a-nice-example-of-indirect-addressing
    https://github.com/rogerjdeangelis/utl-indirect-addressing-to-access-variable-names
    https://github.com/rogerjdeangelis/utl-scraping-a-single-indirect-html-reference-using-python-beautiful-soup-and-request-packages

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
