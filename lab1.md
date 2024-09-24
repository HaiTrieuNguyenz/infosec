# 22110081 Nguyễn Hải Triều
# LAB 1
### bof1

Run docker image
![image](https://github.com/user-attachments/assets/552b0078-23b4-468c-9d13-0a357afad949)

Stack frame :
![image](https://github.com/user-attachments/assets/574a5843-b5b0-4e53-9744-a091bc62d8f9)



We need to insert the address of the secretFunc into the ret address.

Compile bof1.c :
![image](https://github.com/user-attachments/assets/2aa98ed4-271f-429a-b82a-192b34aed065)





Find address of secretFunc()



gdb bof1.out


disas secretFunc



![image](https://github.com/user-attachments/assets/4e0cd049-de1f-41b8-9a89-4f5441b44d0d)




we can see the address of secretFunc at the first line 0x0804846b

Then filling full buffer with random char and insert the function address
![image](https://github.com/user-attachments/assets/e21f426c-792e-4ebd-8f87-219a2d1f125d)





200 char to fill the buffer and 4 char to fill the ebp address. Then insert address of secretFunc 
in ret address in little endian format.

