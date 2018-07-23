{:title "fooling around with array memory dot dot dot"
:layout :post
 :tags ["old-post"]}



Something I saved in a scratch buffer once ...



```lisp

* (room)



Dynamic space usage is: 43,629,232 bytes.

Read-only space usage is: 6,736 bytes.

Static space usage is: 4,000 bytes.

Control stack usage is: 2,976 bytes.

Binding stack usage is: 800 bytes.

Control and binding stack usage is for the current thread only.

Garbage collection is currently enabled.



Breakdown for dynamic space:

13,392,016 bytes for 12,550 code objects.

7,629,616 bytes for 87,920 instance objects.

7,108,736 bytes for 444,296 cons objects.

7,093,664 bytes for 59,224 simple-vector objects.

2,268,928 bytes for 35,452 symbol objects.

6,136,272 bytes for 115,793 other objects.

43,629,232 bytes for 755,235 dynamic objects (space total.)









* (defparameter *a* (make-array '(3000 3000) :element-type '(unsigned-byte 8) :initial-element 0))



*A*

* (room)



Dynamic space usage is: 52,662,000 bytes.

Read-only space usage is: 6,736 bytes.

Static space usage is: 4,000 bytes.

Control stack usage is: 2,976 bytes.

Binding stack usage is: 800 bytes.

Control and binding stack usage is for the current thread only.

Garbage collection is currently enabled.



Breakdown for dynamic space:

13,392,016 bytes for 12,550 code objects.

9,221,424 bytes for 1,914 simple-array-unsigned-byte-8 objects.

7,644,768 bytes for 88,074 instance objects.

7,123,088 bytes for 445,193 cons objects.

7,096,224 bytes for 59,245 simple-vector objects.

8,217,248 bytes for 149,969 other objects.

52,694,768 bytes for 756,945 dynamic objects (space total.)







* (gc)



NIL

* (room)



Dynamic space usage is: 43,810,864 bytes.

Read-only space usage is: 6,736 bytes.

Static space usage is: 4,000 bytes.

Control stack usage is: 2,976 bytes.

Binding stack usage is: 800 bytes.

Control and binding stack usage is for the current thread only.

Garbage collection is currently enabled.



Breakdown for dynamic space:

13,392,016 bytes for 12,550 code objects.

7,687,200 bytes for 88,515 instance objects.

7,144,528 bytes for 59,470 simple-vector objects.

7,134,528 bytes for 445,908 cons objects.

2,269,312 bytes for 35,458 symbol objects.

6,183,280 bytes for 116,551 other objects.

43,810,864 bytes for 758,452 dynamic objects (space total.)

```
