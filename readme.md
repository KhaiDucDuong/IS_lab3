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
![lab3 stack frame drawio (1)](https://github.com/user-attachments/assets/f7d152be-7edc-4b82-b8cf-6d38de516037)
We want to start buffer overflow from variable buf, overwrite eip with the function remove's address, following by the function exit's address and the dummyfile environment variable's address.
4. Compile and start gdb <br>
![image](https://github.com/user-attachments/assets/efb21707-27b1-4488-9908-7c14b99c5ae1)
6. Find addresses of remove, exit, and dummyfile envrionment variable
7. Conduct attack

