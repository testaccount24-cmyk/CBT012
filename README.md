# CBT012
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. GitHub repos will be deleted and created during this period...

```
//***FILE 012 IS FROM JOHN HANCOCK MUTUAL LIFE INSURANCE COMPANY    *   FILE 012
//*           AND IS THEIR ISPF BACKGROUND JOBS DRIVER.             *   FILE 012
//*                                                                 *   FILE 012
//*       THIS FILE CONSISTS OF OPTIONS DESIGNED TO BE USED WITH    *   FILE 012
//*       THE INTERACTIVE SYSTEM PRODUCTIVITY FACILITY/PROGRAM      *   FILE 012
//*       DEVELOPMENT FACILITY (ISPF/PDF).                          *   FILE 012
//*                                                                 *   FILE 012
//*       NOTE:  SOME OF THESE OPTIONS WILL WORK ONLY UNDER ISPF    *   FILE 012
//*       VERSION 2.  PANELS/SKELETONS/MESSAGES PROVIDED FOR USE    *   FILE 012
//*       WITH JH#PDF8 ARE INTENDED ONLY AS SAMPLES.  SOME          *   FILE 012
//*       MODIFICATION (E.G. ACCOUNTING FIELDS) WOULD BE            *   FILE 012
//*       REQUIRED AT YOUR INSTALLATION.  PANELS THAT CONTAIN A     *   FILE 012
//*       "VOLUME SERIAL" FIELD DETERMINE A UNIT NAME IN THEIR      *   FILE 012
//*       )PROC SECTION.  THIS SHOULD BE CHECKED FOR                *   FILE 012
//*       INSTALLATION COMPATIBILITY.                               *   FILE 012
//*                                                                 *   FILE 012
//*       CHANGES 03/04/85:  MODIFICATIONS TO EXPLOIT ISPF          *   FILE 012
//*              VERSION 2:  MACRO ISPCALL NOW SUPPORTS ISPEXEC     *   FILE 012
//*              FORMAT (SEE NEW JH#PDF8 FOR EXAMPLE); JH#PDF8      *   FILE 012
//*              MODIFIED TO USE LM SERVICES TO ENABLE VIO          *   FILE 012
//*              ISPCTLN DATA SETS TO BE EDITED (UNDER V1           *   FILE 012
//*              JH#PDF8 WORKS AS BEFORE); PANEL JHAEFR01 ADDED     *   FILE 012
//*              (USED BY JH#PDF8 UNDER V2); JH ISR¬PRIM            *   FILE 012
//*              REPLACED FOR V2; MSGS JHA¬M04 ADDED; MANY          *   FILE 012
//*              PANELS/SKELETONS/MESSAGES FORMERLY PREFIXED Z*     *   FILE 012
//*              ARE NOW PREFIXED JHA*; CLIST ISRCTBL RENAMED       *   FILE 012
//*              JH#YCTBL; JH EDIT INTERFACE ALTERED FOR V2;        *   FILE 012
//*              CLIST TSEDITPR RENAMED JH#TEDPR AND CHANGED TO     *   FILE 012
//*              PROCESS PDF V2 FIELDS; ADDED JH#GTDSN (A DIALOG    *   FILE 012
//*              PROGRAM THAT RETURNS VOLUME SERIAL AND DATA SET    *   FILE 012
//*              NAME WHEN PROVIDED A DDNAME - USED BY OTHER        *   FILE 012
//*              DIALOGS); ADDED JH#TEDRT TO DISPLAY AND EDIT A     *   FILE 012
//*              USER'S PDF EDIT RECOVERY TABLE; ADDED ISRUOLJH,    *   FILE 012
//*              IBM'S ISRUOL (PDF 3.8) MODIFIED TO RUN FASTER      *   FILE 012
//*              BY USING TEMPORARY DATA SETS; ADDED JH#TPRGM TO    *   FILE 012
//*              INVOKE IEHPROGM FOREGROUND; ADDED JH#EDTMP TO      *   FILE 012
//*              ALLOW EDITING OF VIO ISPCTLN DATA SETS.            *   FILE 012
//*                                                                 *   FILE 012
//*       CHANGES 10/18/83: TABLE DISPLAY UTILITY REWRITTEN AND     *   FILE 012
//*              ENHANCED.  CLIST RENAMED JH#YDTBL FROM             *   FILE 012
//*              ISRYDTBL.                                          *   FILE 012
//*                                                                 *   FILE 012
//*       CHANGES 10/3/83: JOHN HANCOCK EDIT INTERFACE IS           *   FILE 012
//*              ENHANCED TO ALLOW USER-DEFINED ABBREVIATIONS       *   FILE 012
//*              (SEE #3 BELOW); SMALL CHANGES TO JH#PDF8 AND       *   FILE 012
//*              ASSOCIATED SAMPLE PANELS; ADDED #MAXTOP #8, #9.    *   FILE 012
//*                                                                 *   FILE 012
//*       1. JOHN HANCOCK BATCH JOBS DRIVER (JH#PDF8) IS A          *   FILE 012
//*          GENERAL PURPOSE ISPF FUNCTION FOR THE GENERATION OF    *   FILE 012
//*          JCL BASED ON DATA ENTERED ON PANELS.  PROCESSING IS    *   FILE 012
//*          CONTROLLED BY AN INITIAL PARM AND BY ISPF              *   FILE 012
//*          VARIABLES.  SEE THE COMMENTS AT THE BEGINNING OF       *   FILE 012
//*          THE JH#PDF8 SOURCE FOR ADDITIONAL INFORMATION.         *   FILE 012
//*                                                                 *   FILE 012
//*          SOME DIFFERENCES BETWEEN PDF OPTION 5 AND JH#PDF8:     *   FILE 012
//*          JH#PDF8 DOES NOT ALLOCATE DATA SETS FOR                *   FILE 012
//*          VERIFICATION, WHERE OPTION 5 OPTIONALLY ALLOCATES      *   FILE 012
//*          THE INPUT DATA SET (ONLY); JH#PDF8 ALLOWS A USER TO    *   FILE 012
//*          EDIT THE TEMPORARY GENERATED JCL IN ADDITION TO        *   FILE 012
//*          SUBMITTING OR CANCELING THE JOB; JH#PDF8 ALLOWS        *   FILE 012
//*          INITIAL AND FINAL SKELETONS TO BE TAILORED FOR EACH    *   FILE 012
//*          INVOCATION; JH#PDF8 ALLOWS TWO OR MORE PANELS TO       *   FILE 012
//*          PROVIDE INPUT TO ONE TAILORING OPERATION; WITH         *   FILE 012
//*          JH#PDF8 PROCESSING SUCH AS JOB CHARACTER               *   FILE 012
//*          INCREMENTATION IS DONE IN THE PANELS (SEE SAMPLE       *   FILE 012
//*          JHABP¬B) INSTEAD OF IN THE DRIVER PROGRAM.             *   FILE 012
//*                                                                 *   FILE 012
//*          SAMPLE PANELS, SKELETONS, AND MESSAGES ARE PROVIDED    *   FILE 012
//*          FOR USE WITH JH#PDF8.  IT IS POSSIBLE, HOWEVER, TO     *   FILE 012
//*          CREATE TOTALLY DIFFERENT ISPF COMPONENTS FOR USE       *   FILE 012
//*          WITH THIS DRIVER.  THE ONLY REQUIREMENTS ARE THAT      *   FILE 012
//*          THERE BE A PSEUDO-SELECTION PANEL WHOSE NAME IS        *   FILE 012
//*          PASSED VIA A PARM TO JH#PDF8 (SAMPLE IS JHABP¬A)       *   FILE 012
//*          AND THAT THE COMPONENTS SET ISPF VARIABLES TO          *   FILE 012
//*          DICTATE PROCESSING (AGAIN, SEE THE COMMENTS).          *   FILE 012
//*                                                                 *   FILE 012
//*          JH#PDF8 CODE IS REENTRANT; THE MODULE MAY BE           *   FILE 012
//*          PLACED IN LPALIB WITH OTHER ISPF MODULES.              *   FILE 012
//*                                                                 *   FILE 012
//*       2. CLIST JH#YDTBL WILL DISPLAY THE CONTENTS               *   FILE 012
//*          (NON-EXTENSION VARIABLES) OF ANY TABLE IN TABLE        *   FILE 012
//*          DISPLAY (SCROLLABLE) FORMAT.  AS PROVIDED HERE IT      *   FILE 012
//*          SUPPORTS FIVE DIFFERENT TABLE DISPLAY FORMATS.  IT     *   FILE 012
//*          MAY BE INVOKED FROM ISPF/PDF OPTION 6, VIA THE ISPF    *   FILE 012
//*          TSO COMMAND, OR FROM A SELECTION PANEL (E.G.,          *   FILE 012
//*          ISRYXD1).                                              *   FILE 012
//*                                                                 *   FILE 012
//*       3. PANEL JHTEPE01 IS A JOHN HANCOCK EDIT INTERFACE.       *   FILE 012
//*          TO USE IT, ADD THE FOLLOWING ENTRY TO A SELECTION      *   FILE 012
//*          PANEL:                                                 *   FILE 012
//*                N,'PGM(ISREDIT) PARM(P,JHTEPE01)                 *   FILE 012
//*                  NEWAPPL(ISR)'                                  *   FILE 012
//*          NOTE: FUTURE RELEASES OF PDF MAY NOT SUPPORT THIS      *   FILE 012
//*          METHOD OF IMPLEMENTATION.                              *   FILE 012
//*                                                                 *   FILE 012
//*       4. CLIST JH#TEDPR WILL DISPLAY THE CONTENTS OF A          *   FILE 012
//*          USER'S CURRENT EDIT PROFILE (FOR THE APPLICATION       *   FILE 012
//*          HE HAS ENTERED).  IT MAY BE INVOKED FROM ISPF/PDF      *   FILE 012
//*          OPTION 6, VIA THE ISPF TSO COMMAND, OR FROM A          *   FILE 012
//*          SELECTION PANEL.  THE CLIST MUST BE MODIFIED FOR       *   FILE 012
//*          YOUR INSTALLATION'S ISPF PROFILE NAMING                *   FILE 012
//*          CONVENTION.                                            *   FILE 012
//*                                                                 *   FILE 012
//*       5. CLIST TSCMDTB WILL DISPLAY THE CONTENTS OF THE         *   FILE 012
//*          CURRENT SYSTEM COMMAND TABLE.  THIS MAY BE USED BY     *   FILE 012
//*          END-USERS, SINCE THE "DESCRIPTION," NOT THE            *   FILE 012
//*          "ACTION," IS DISPLAYED.  IF THIS CLIST IS TO BE        *   FILE 012
//*          USED, IT IS RECOMMENDED THAT A COPY OF THE SYSTEM      *   FILE 012
//*          COMMAND TABLE (ISPCMDS) BE MADE UNDER A DIFFERENT      *   FILE 012
//*          NAME (SYSCMDS IS USED IN THE CLIST).  STRANGE          *   FILE 012
//*          THINGS WILL HAPPEN IF YOU ATTEMPT TO OPEN AND CLOSE    *   FILE 012
//*          A COMMAND TABLE THAT ISPF HAS ALREADY OPENED.          *   FILE 012
//*          TSCMDTB MAY BE INVOKED FROM ISPF/PDF OPTION 6, VIA     *   FILE 012
//*          THE ISPF TSO COMMAND, OR FROM A SELECTION PANEL.       *   FILE 012
//*                                                                 *   FILE 012
//*       6. CLIST JH#YCTBL DRIVES THE TABLE RECONSTRUCTION         *   FILE 012
//*          UTILITY.  THIS ALLOWS FIELDS TO BE ADDED TO/REMOVED    *   FILE 012
//*          FROM TABLES WITHOUT LOSING THE TABLE DATA.  IT MAY     *   FILE 012
//*          BE INVOKED FROM ISPF/PDF OPTION 6, OR FROM A           *   FILE 012
//*          SELECTION PANEL (E.G., ISRYXD1).                       *   FILE 012
//*                                                                 *   FILE 012
//*       7. PANEL JHAYP14¬ PROVIDES ENTRY TO A FOREGROUND          *   FILE 012
//*          INTERFACE TO THE IBM-SUPPLIED SELECTION PANEL          *   FILE 012
//*          UPDATE UTILITY (ISPPUP).  THIS MAY BE ENTERED FROM     *   FILE 012
//*          A HIGHER-LEVEL SELECTION PANEL (XX,'PANEL(ZYPUP¬)')    *   FILE 012
//*          OR FROM TSO READY (ISPSTART PANEL(ZYPUP¬)).            *   FILE 012
//*                                                                 *   FILE 012
//*       8. CLIST ISRALTK ALLOWS A USER TO DEFINE AND ACTIVATE     *   FILE 012
//*          A SECOND SET OF PROGRAM FUNCTION KEY DEFINITIONS.      *   FILE 012
//*          READ HELP PANEL XALTK BEFORE USING.  IT MAY BE         *   FILE 012
//*          INVOKED FROM A SELECTION PANEL (E.G.,ISPOPTA) BY       *   FILE 012
//*          "'XX,CMD(%ISRALTK)'."  AN ENTRY IN A COMMAND TABLE     *   FILE 012
//*          IS ALSO A GOOD IDEA:                                   *   FILE 012
//*                    VERB    ACTION                               *   FILE 012
//*                     K2     SELECT CMD(%ISRALTK PARM('&ZPARM'))  *   FILE 012
//*                                                                 *   FILE 012
//*       9. PANEL PANELID IS A EXAMPLE OF HOW "PANELID ON" MAY     *   FILE 012
//*          BE SET WITHOUT THE USER HAVING TO ENTER THE ISPF       *   FILE 012
//*          COMMAND.  THE CODE IN THIS PANEL COULD BE USED IN      *   FILE 012
//*          ANY SELECTION PANEL, INCLUDING ISR¬PRIM AND            *   FILE 012
//*          ISP¬MSTR.                                              *   FILE 012
//*                                                                 *   FILE 012
//*      10. CLIST JH#TEDRT ALLOWS THE DISPLAYING AND               *   FILE 012
//*          MODIFICATION OF A USER'S EDIT RECOVERY TABLE.  IT      *   FILE 012
//*          MAY BE INVOKED FROM ISPF/PDF OPTION 6, OR FROM A       *   FILE 012
//*          SELECTION PANEL.  THE CLIST MUST BE MODIFIED FOR       *   FILE 012
//*          YOUR INSTALLATION'S ISPF PROFILE NAMING CONVENTION.    *   FILE 012
//*                                                                 *   FILE 012
//*      11. CLIST ISRUOLJH IS JOHN HANCOCK'S MODIFIED ISRUOL,      *   FILE 012
//*          THE CLIST THAT DRIVES PDF OPTION 3.8.  IT HAS BEEN     *   FILE 012
//*          MODIFIED TO RUN FASTER BY USING TEMPORARY DATA         *   FILE 012
//*          SETS, BYPASSING CATALOGING AND DELETION.  IT USES      *   FILE 012
//*          PROVIDED DIALOG PROGRAM JH#GTDSN (WHICH CAN BE IN      *   FILE 012
//*          LPA IF HEAVILY USED).  (THIS CLIST HAS ALSO BEEN       *   FILE 012
//*          MODIFIED TO USE THE SAME JOB CARDS AS OPTION 3.6.)     *   FILE 012
//*                                                                 *   FILE 012
//*      12. CLIST JH#TPRGM PROVIDES A PANEL TO RUN IEHPROGM IN     *   FILE 012
//*          FOREGROUND.  TO USE IT ADD THE FOLLOWING ENTRY TO A    *   FILE 012
//*          SELECTION PANEL:                                       *   FILE 012
//*             NN,'CMD(%JH#TPRGM PANEL(JHTEP0M)                    *   FILE 012
//*                PANEL2(JHTEP0M2) SKEL(JHTES0M))'                 *   FILE 012
//*                                                                 *   FILE 012
//*      13. PROGRAM JH#EDTMP PUTS THE USER INTO EDIT OF AN ISPF    *   FILE 012
//*          TEMPORARY CONTROL DATA SET (ISPCTLN).  IT SUPPORTS     *   FILE 012
//*          VIO AS WELL AS DASD DATA SETS.  IF NO PARM IS          *   FILE 012
//*          SUPPLIED, THE DATA SET ASSOCIATED WITH THE CURRENT     *   FILE 012
//*          LOGICAL SCREEN IS USED.  TO ACCESS THE DATA SET        *   FILE 012
//*          USED BY THE PDF SUBMIT COMMAND, USE A PARM OF 0        *   FILE 012
//*          (ZERO).  THIS PROGRAM MAY BE INVOKED VIA THE ISPF      *   FILE 012
//*          SELECT SERVICE FROM ANOTHER DIALOG FUNCTION.  AN       *   FILE 012
//*          ENTRY MAY ALSO BE PLACED IN A COMMAND TABLE:           *   FILE 012
//*                                                                 *   FILE 012
//*            VERB      T  ACTION                                  *   FILE 012
//*            EDTEMP    3  SELECT PGM(JH#EDTMP) PARM(&ZPARM)       *   FILE 012
//*                         NEWAPPL(ISR)                            *   FILE 012
//*                                                                 *   FILE 012
//*          THEN A USER CAN ENTER THE EDTEMP COMMAND ON ANY        *   FILE 012
//*          SCREEN AND EDIT THE DATA SET.                          *   FILE 012
//*                                                                 *   FILE 012
//*       CONTENTS OF THIS PDS:                                     *   FILE 012
//*                                                                 *   FILE 012
//*          SOURCE:    JH#EDTMP                                    *   FILE 012
//*                     JH#GTDSN                                    *   FILE 012
//*                     JH#PDF8                                     *   FILE 012
//*                                                                 *   FILE 012
//*          MACROS:    ENTER                                       *   FILE 012
//*                     ISPCALL                                     *   FILE 012
//*                     LEAVE                                       *   FILE 012
//*                     REQUS                                       *   FILE 012
//*                     SCANLINE                                    *   FILE 012
//*                                                                 *   FILE 012
//*          JCL:       $INSTALL (COPY ISPF COMPONENTS AND          *   FILE 012
//*                     ASSEMBLE PROGRAMS)                          *   FILE 012
//*                                                                 *   FILE 012
//*                     $LOAD    (SAMPLE JCL TO LOAD                *   FILE 012
//*                     DISTRIBUTION LIB FROM TAPE)                 *   FILE 012
//*                                                                 *   FILE 012
//*          CLISTS:    SEE IEBCOPY STATEMENTS IN $INSTALL          *   FILE 012
//*                     MEMBER                                      *   FILE 012
//*                                                                 *   FILE 012
//*          PANELS:    SEE IEBCOPY STATEMENTS IN $INSTALL          *   FILE 012
//*                     MEMBER PLUS MODIFIED ISR¬PRIM               *   FILE 012
//*                                                                 *   FILE 012
//*          SKELETONS: SEE IEBCOPY STATEMENTS IN $INSTALL          *   FILE 012
//*                     MEMBER                                      *   FILE 012
//*                                                                 *   FILE 012
//*          MESSAGES:  SEE IEBCOPY STATEMENTS IN $INSTALL          *   FILE 012
//*                     MEMBER                                      *   FILE 012
//*                                                                 *   FILE 012
```
