**Student: Duong Duc Khai**
**ID: 21110775**

# Lab 3 - Delete file using return-to-libc attack with environment variable

1. Set up dummyfile directory <br>
![image](https://github.com/user-attachments/assets/79a3216e-8202-41ed-b058-8df010387d55)
2. Set environment variable for dummyfile path <br>
![image](https://github.com/user-attachments/assets/83a9849a-a64a-49da-9559-e3e327195335)
3. Analyze vuln.c stack frame for function main <br>
![image](https://github.com/user-attachments/assets/b17ea0fd-c18f-4f86-b41b-49cf1103018a)
<br>
The function main takes in 2 parameters: int argc, char* argv[], and it has a local variable: char buf[64], the analyzed stack frame is: <br>
![lab3 stack frame drawio (1)](https://github.com/user-attachments/assets/39d30c65-b0d9-4a8e-8654-1cf1cb5688a8)
We want to start buffer overflow from variable buf, overwrite eip with the function remove's address, following by the function exit's address and the dummyfile environment variable's address. <br>
4. Compile and start gdb <br>
![image](https://github.com/user-attachments/assets/efb21707-27b1-4488-9908-7c14b99c5ae1)
I turned on the NX bit here so stack is not executable, we're forced to use exploits such as return-to-libc to attack the program.
6. Find addresses of remove, exit, and dummyfile envrionment variable <br>
![image](https://github.com/user-attachments/assets/a7ae1dbe-4e1f-4bbd-b1b1-133a30c67a08)
Because I need the value from the dummyfile env varaible only, I incremented the addresses by 6 to remove "DUMMY=". Therefore, when we pass in the address 0xffffdf1e, we should get "./tmp/dummy/dummyfile" as our value. <br>
8. Conduct attack
exit - 0xf7e449e0 -> \xe0\x49\xe4\xf7
remove - 0xf7e71f30 -> \x30\x1f\xe7\xf7
DUMMY - 0xffffdf1e ->  \x1f\xdf\xff\xff
&DUMMY+6 = FFFFDF24 -> \x24\xdf\xff\xff
Now I have had all the required information for the attack. <br>
![image](https://github.com/user-attachments/assets/4d786373-f459-4610-99e9-0ad56ac8521b)
I added a break point so I can check if the stack frame is correct after I run the program with a vunerable snippet.
![image](https://github.com/user-attachments/assets/43de3a41-c38e-401a-ad27-2835c4b0acb1)
![image](https://github.com/user-attachments/assets/42ba28e7-8832-4cc0-99d3-1aea415f3aea)
We can see that the first 64 bytes of the stack frame is the letter 'a' or 61 in hex following by 4 bytes of the letter 'b' or 62 in hex. The next 3 next 4 bytes are the addresses of remove, exit, and DUMMY respectively.
I resumed the program and the dummyfile is deleted. <br>
![image](https://github.com/user-attachments/assets/0bb01ed9-e0fc-4b45-8dae-c0d89a0895cb)





