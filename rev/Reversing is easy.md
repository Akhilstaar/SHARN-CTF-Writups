## Reversing is easy

You may use any decompiler of your choice(eg. ghidra, ida or online compiler like dogbolt.org)

I'll be using dogbolt.org to save time.

Search for `main` finction and you'll get something like this 

```
int __fastcall main(int argc, const char **argv, const char **envp)
{
  int v3; // ebx
  const char *v4; // rsi
  const char *v5; // rax
  char v7[32]; // [rsp+20h] [rbp-140h] BYREF
  char v8[32]; // [rsp+40h] [rbp-120h] BYREF
  char s[64]; // [rsp+60h] [rbp-100h] BYREF
  char v10[64]; // [rsp+A0h] [rbp-C0h] BYREF
  char s1[104]; // [rsp+E0h] [rbp-80h] BYREF
  unsigned __int64 v12; // [rsp+148h] [rbp-18h]

  v12 = __readfsqword(0x28u);
  puts("Welcome to the Flag Vault!");
  puts("To access the flag, you must answer three challenges correctly.\n");
  puts("Challenge 1: What is the hex value of the bytes in the string 'hacker'?");
  puts("Hint: Separate each byte by a space (e.g., 68 65 6c 6c 6f).");
  fgets(s, 50, _bss_start);
  s[strcspn(s, "\n")] = 0;
  if ( !strcmp(s, "68 61 63 6b 65 72")
    && (puts("\nChallenge 2: What will be the ROT15 cipher of the string ?"),
        puts("String : g0i_1$_c0i_3crgnei10c "),
        printf("Enter the ROT15 cipher : "),
        fgets(s1, 100, _bss_start),
        s1[strcspn(s1, "\n")] = 0,
        !strcmp(s1, "r0t_1$_n0t_3ncrypt10n")) )
  {
    puts("\nChallenge 3: Crack the password to proceed. \nHint: Think creatively like NSA!");
    printf("Enter the password: ");
    fgets(v10, 50, _bss_start);
    v10[strcspn(v10, "\n")] = 0;
    getstring[abi:cxx11]((__int64)v7);
    v4 = (const char *)std::string::c_str(v7);
    if ( !strcmp(v10, v4) )
    {
      getflag[abi:cxx11]((__int64)v8);
      v5 = (const char *)std::string::c_str(v8);
      printf("\nCongratulations! Access granted. Here is your flag: %s\n", v5);
      v3 = 0;
      std::string::~string(v8);
    }
    else
    {
      puts("Incorrect. Access denied!");
      v3 = 1;
    }
    std::string::~string(v7);
  }
  else
  {
    puts("Incorrect. Access denied!");
    return 1;
  }
  return v3;
}
```

No need to worry since it looks little scary.

We can see that it is taking some inputs from the user and comparing with hardcoded values.
If we take these values from here and paste in the program we'll proceed to the flag.

Ans1 = `68 61 63 6b 65 72`
Ans2 = `r0t_1$_n0t_3ncrypt10n`

for Ans3, we'll need to see the `getflag()` function.

```
__int64 __fastcall getflag[abi:cxx11](__int64 a1)
{
  char v1; // al
  char v3; // [rsp+17h] [rbp-69h]
  __int64 v4; // [rsp+18h] [rbp-68h] BYREF
  __int64 v5; // [rsp+20h] [rbp-60h] BYREF
  char *v6; // [rsp+28h] [rbp-58h]
  __int64 *v7; // [rsp+30h] [rbp-50h]
  __int64 *v8; // [rsp+38h] [rbp-48h]
  char v9[40]; // [rsp+40h] [rbp-40h] BYREF
  unsigned __int64 v10; // [rsp+68h] [rbp-18h]

  v10 = __readfsqword(0x28u);
  v7 = &v5;
  std::string::basic_string<std::allocator<char>>((__int64)v9, "anwfm{c3gcd1yr_15_4x4k1yr_1dye_1e}", (__int64)&v5);
  std::__new_allocator<char>::~__new_allocator();
  v8 = &v5;
  std::string::basic_string<std::allocator<char>>(a1, byte_3033, (__int64)&v5);
  std::__new_allocator<char>::~__new_allocator();
  v6 = v9;
  v4 = std::string::begin(v9);
  v5 = std::string::end(v6);
  while ( __gnu_cxx::operator!=<char *,std::string>((__int64)&v4, (__int64)&v5) )
  {
    v3 = *(_BYTE *)__gnu_cxx::__normal_iterator<char *,std::string>::operator*((__int64)&v4);
    v1 = rot15(v3);
    std::string::push_back(a1, (unsigned int)v1);
    __gnu_cxx::__normal_iterator<char *,std::string>::operator++(&v4);
  }
  std::string::~string(v9);
  return a1;
}
```

The function only returns the string `anwfm{c3gcd1yr_15_4x4k1yr_1dye_1e}` which is the ans3.

If we put all 3 answers, we'll get the flag.