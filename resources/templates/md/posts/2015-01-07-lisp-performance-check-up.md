{:title "lisp performance check up"
:layout :post
 :tags ["old-post"]}



Came across [pixie-lang](https://github.com/pixie-lang/pixie), and it had this bit about "_the loop compiles down to six instructions!_"



So I decided to go and satisfy my curiosity about just how many instructions a loop like this _should_ be expected to compile down to.



```lisp

CL-USER> (defun add-fn (args)

           (reduce #'+ args))

```



which obviously runs as



```lisp

CL-USER> (add-fn '(3 4 5))

12

```



Now, a look at what's going on "under the hood"



```lisp

CL-USER> (disassemble #'add-fn)

; disassembly for ADD-FN

; Size: 42 bytes. Origin: #x1004FBC3F8

; 3F8:       498B4C2460       MOV RCX, [R12+96]               ; thread.binding-stack-pointer

                                                              ; no-arg-parsing entry point

; 3FD:       48894DF8         MOV [RBP-8], RCX

; 401:       488B1598FFFFFF   MOV RDX, [RIP-104]              ; #<FUNCTION +>

; 408:       488BFE           MOV RDI, RSI

; 40B:       488B0596FFFFFF   MOV RAX, [RIP-106]              ; #<FDEFINITION for REDUCE>

; 412:       B904000000       MOV ECX, 4

; 417:       FF7508           PUSH QWORD PTR [RBP+8]

; 41A:       FF6009           JMP QWORD PTR [RAX+9]

; 41D:       CC0A             BREAK 10                        ; error trap

; 41F:       02               BYTE #X02

; 420:       19               BYTE #X19                       ; INVALID-ARG-COUNT-ERROR

; 421:       9A               BYTE #X9A                       ; RCX

NIL

```



Ok, let's see if we can slightly improve that



```lisp

CL-USER> (defun add-fn (args)

           (declare (optimize (speed 3) (safety 0)))

           (reduce #'+ args))

```



```lisp

CL-USER> (disassemble #'add-fn)

; disassembly for ADD-FN

; Size: 25 bytes. Origin: #x10051BDB12

; 12:       488B15A7FFFFFF   MOV RDX, [RIP-89]                ; #<FUNCTION +>

                                                              ; no-arg-parsing entry point

; 19:       488B05A8FFFFFF   MOV RAX, [RIP-88]                ; #<FDEFINITION for REDUCE>

; 20:       B904000000       MOV ECX, 4

; 25:       FF7508           PUSH QWORD PTR [RBP+8]

; 28:       FF6009           JMP QWORD PTR [RAX+9]

```



Pretty sweet, eh? _You don't get that kind of feedback every day_.



This was all run on 64-bit SBCL:



```lisp

CL-USER> (lisp-implementation-type)

"SBCL"

CL-USER> (lisp-implementation-version)

"1.2.7"

```
