Anuraag Rijal
@02765122
Ink to project on Github:


https://github.com/anuraagrijal3138/SyntaxAnalysisProgrammingAssignment


Instructions to compile:


* In the command line navigate to the location where “parser.c” and “Makefile” are stored
* type “make” in the command line
* After the code is compiled type “./parser <file_name>” in the command line


*Here file_name is the location of input file


Link to 5 most significant commits:


1. https://github.com/anuraagrijal3138/SyntaxAnalysisProgrammingAssignment/commit/9aa5e0fa0a7e6c642e94a806dec4e66409c79183
2. https://github.com/anuraagrijal3138/SyntaxAnalysisProgrammingAssignment/commit/a0a852dc0493fc73cd2afd867d3b98ee5149fbb4
3. https://github.com/anuraagrijal3138/SyntaxAnalysisProgrammingAssignment/commit/952b431f02e1d7ae4fae2c7cce9c9d79df83ed8d
4. https://github.com/anuraagrijal3138/SyntaxAnalysisProgrammingAssignment/commit/53d52d7a8fb40892750606d3e994f0fa11041331
5. https://github.com/anuraagrijal3138/SyntaxAnalysisProgrammingAssignment/commit/0d7a7c5fa113757b2dee4e8e72e0eed012c60deb




Five most significant commit logs:




commit 9aa5e0fa0a7e6c642e94a806dec4e66409c79183
Author: Anuraag Rijal <Anuraag Rijal>
Date:   Thu Mar 23 23:30:30 2017 -0400


    Error function main function and getchar function revised


diff --git a/main.c b/main.c
index b0921af..4332a2d 100644
--- a/main.c
+++ b/main.c
@@ -14,6 +14,12 @@ int token;
 int nextToken;
 FILE *in_fp, *fopen();
 
+
+/* Variables required to handle multiple lines*/
+char * line = NULL;
+int lineIndex;
+char errChar;
+
 /* Function declarations */
 void addChar();
 void getChar();
@@ -22,6 +28,7 @@ int lex();
 void expr();
 void term();
 void factor();
+void error();
 
 /* Character classes */
 #define LETTER 0
@@ -41,18 +48,36 @@ void factor();
 
 /******************************************************/
 /* main driver */
-main() {
+int main(int argc, char* argv[]) {
+        ssize_t readTest;
+        size_t length = 0;
 
 /* Open the input data file and process its contents */
-        if ((in_fp = fopen("front.in", "r")) == NULL)
+
+        /* File name not provided or illegal characters*/
+
+        if(argc != 2){
+                printf("Invalid command");
+                exit(0);
+        }
+
+        if ((in_fp = fopen(argv[1], "r")) == NULL)
                 printf("ERROR - cannot open front.in \n");
         else {
-                getChar();
-                do {
-                        lex();
-                        expr();
-                }while (nextToken != EOF);
+                readTest = getline(&line, &length, in_fp);
+                while(readTest != -1){
+                        lineIndex = 0;
+                        getChar();
+                        if(line != NULL){
+                                do {
+                                        lex();
+                                        expr();
+                                }while (nextToken != EOF);
+                        }
+                        readTest = getline(&line, &length, in_fp);                
+                }
         }
+        return 0;
 }
 
 /*****************************************************/
@@ -108,7 +133,8 @@ void addChar() {
 /* getChar - a function to get the next character of
 input and determine its character class */
 void getChar() {
-        if ((nextChar = getc(in_fp)) != EOF) {
+        if (line[lineIndex] != '\n' && line[lineIndex] != '\0') {
+                nextChar = line[lineIndex ++];
                 if (isalpha(nextChar))
                         charClass = LETTER;
         else if (isdigit(nextChar))
@@ -127,13 +153,13 @@ void getNonBlank() {
                 getChar();
 }
 
-/
-*****************************************************/
+/*****************************************************/
 /* lex - a simple lexical analyzer for arithmetic
 expressions */
 int lex() {
         lexLen = 0;
         getNonBlank();
+        errChar = nextChar;
         switch (charClass) {
                 /* Parse identifiers */
                 case LETTER:
@@ -243,6 +269,6 @@ printf("Exit <factor>\n");;
 } /* End of function factor */
 
 void error(){
-        printf("Syntax error! ");
-        exit(0);
+        printf("%s\n", line);
+        printf("Error! occurs at %c\n", errChar);
 }
\ No newline at end of file


commit a0a852dc0493fc73cd2afd867d3b98ee5149fbb4
Author: Anuraag Rijal <Anuraag Rijal>
Date:   Thu Mar 23 14:47:55 2017 -0400


    error function


diff --git a/main.c b/main.c
index be2b215..b0921af 100644
--- a/main.c
+++ b/main.c
@@ -2,6 +2,7 @@
 
 #include <stdio.h>
 #include <ctype.h>
+#include <stdlib.h>
 
 /* Global declarations */
 /* Variables */
@@ -18,6 +19,9 @@ void addChar();
 void getChar();
 void getNonBlank();
 int lex();
+void expr();
+void term();
+void factor();
 
 /* Character classes */
 #define LETTER 0
@@ -236,4 +240,9 @@ void factor() {
         } /* End of else */
 
 printf("Exit <factor>\n");;
-} /* End of function factor */
\ No newline at end of file
+} /* End of function factor */
+
+void error(){
+        printf("Syntax error! ");
+        exit(0);
+}
\ No newline at end of file


commit 952b431f02e1d7ae4fae2c7cce9c9d79df83ed8d
Author: Anuraag Rijal <Anuraag Rijal>
Date:   Thu Mar 23 01:40:32 2017 -0400


    Minor changes to main function


diff --git a/main.c b/main.c
index 3c42a77..be2b215 100644
--- a/main.c
+++ b/main.c
@@ -46,6 +46,7 @@ main() {
                 getChar();
                 do {
                         lex();
+                        expr();
                 }while (nextToken != EOF);
         }
 }


commit 53d52d7a8fb40892750606d3e994f0fa11041331
Author: Anuraag Rijal <Anuraag Rijal>
Date:   Thu Mar 23 00:47:23 2017 -0400


    Code from 4.4.1 added and run once


diff --git a/main.c b/main.c
index 8ec7ef7..3c42a77 100644
--- a/main.c
+++ b/main.c
@@ -122,7 +122,8 @@ void getNonBlank() {
                 getChar();
 }
 
-/*****************************************************/
+/
+*****************************************************/
 /* lex - a simple lexical analyzer for arithmetic
 expressions */
 int lex() {
@@ -169,3 +170,69 @@ int lex() {
         printf("Next token is: %d, Next lexeme is %s\n", nextToken, lexeme);
         return nextToken;
 } /* End of function lex */
+
+/* expr
+        Parses strings in the language generated by the rule:
+        <expr> -> <term> {(+ | -) <term>}
+*/
+void expr() {
+        printf("Enter <expr>\n");
+/* Parse the first term */
+        term();
+/* As long as the next token is + or -, get
+the next token and parse the next term */
+        while (nextToken == ADD_OP || nextToken == SUB_OP) {
+                lex();
+                term();
+        }
+        printf("Exit <expr>\n");
+} /* End of function expr */
+
+/* term
+        Parses strings in the language generated by the rule:
+        <term> -> <factor> {(* | /) <factor>)
+*/
+void term() {
+        printf("Enter <term>\n");
+/* Parse the first factor */
+        factor();
+/* As long as the next token is * or /, get the
+        next token and parse the next factor */
+        while (nextToken == MULT_OP || nextToken == DIV_OP) {
+                lex();
+                factor();
+        }
+        printf("Exit <term>\n");
+} /* End of function term */
+
+/* factor
+        Parses strings in the language generated by the rule:
+        <factor> -> id | int_constant | ( <expr )
+*/
+void factor() {
+        printf("Enter <factor>\n");
+/* Determine which RHS */
+        if (nextToken == IDENT || nextToken == INT_LIT)
+/* Get the next token */
+                lex();
+/* If the RHS is ( <expr>), call lex to pass over the
+        left parenthesis, call expr, and check for the right
+        parenthesis */
+        else {
+                if (nextToken == LEFT_PAREN) {
+                        lex();
+                        expr();
+                        if (nextToken == RIGHT_PAREN)
+                                lex();
+                        else
+                                error();
+                } /* End of if (nextToken == ... */
+
+/* It was not an id, an integer literal, or a left
+        parenthesis */
+        else
+                error();
+        } /* End of else */
+
+printf("Exit <factor>\n");;
+} /* End of function factor */
\ No newline at end of file