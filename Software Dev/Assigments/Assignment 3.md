**For every code snippet, first, explain the vulnerability of the code and how you can exploit it (1 point for each question). Second, write at least one solution to remove the vulnerability (1 point for each question). (10 points in total)**

1. *C Code*
```C
int main(int argc, char** argv) {  
  char *buf;  
  char *buf2;  
  buf=(char*)malloc(1024);  
  buf2=(char*)malloc(1024);  
  printf("buf=%p buf2=%p\n",buf,buf2);  
  strcpy(buf,argv[1]);  
  free(buf2);  
}
```
- **Vulnerability:** The vulnerability in this code is a buffer overflow. The function `strcpy` can write more bytes into `buf` than is allocated if `argv[1]` is longer than `1024` (in this case).
- **Exploit:** An attacker can supply a *LONG* argument to overflow said buffer. This may overwrite the return address on the stack, which would redirect program execution to malicious code.
- **Fix:** Use `strncpy` in place of `strcpy`, as to limit the number of bytes copied into the buffer. This will also ensure null termination of the string.
	- `strncpy(buf, argv[1], 1023);`
	- `buf[1023] = '\0';`
2. *C Code*
```C
#include <stdio.h>  
int main(void) {        
	int authorized = 0;  
    char sys_pass[16] = "54S$P";  
    char usr_pass[16];      
    printf("Enter your password: ");  
    scanf("%s", usr_pass);        
    if (strcmp(sys_pass, usr_pass) == 0) {  
        authorized = 1;  
    }    
    
    if (authorized) {  
        printf("You are authenticated!\n");  
    }  
    else{  
        printf("Wrong password!\n");  
    }  
}
```
- **Vulnerability:** The vulnerability in this code is a buffer overflow. The function `scanf` used in conjunction with the `%s` format specifier has *no bounds checking*, so it can write more data than allocated into `usr_pass` (max 16 bytes). 
- **Exploit:** A long password input can overflow `usr_pass`, which can potentially corrupt the stack or variables within the stack, which would enable an attacker to control the program's execution.
- **Fix:** Use the function `fgets` instead of `scanf` to read input, which would allow you to specify the maximum input size.
	- `fgets(usr_pass, 16, stdin);`
3. *C Code (w/ `libpng`)*
```C
if (!(png_ptr->mode & PNG_HAVE_PLTE)) {
  /* Should be an error, but we can cope with it */
  png_warning(png_ptr, "Missing PLTE before tRNS");
}
else if (length > (png_uint_32)png_ptr->num_palette) {
  png_warning(png_ptr, "Incorrect tRNS chunk length");
  png_crc_finish(png_ptr, length);
  return;
}
...
png_crc_read(png_ptr, readbuf, (png_size_t)length);  
```
- **Vulnerability:** We covered this one in class 😉. This one is an integer overflow. If `length` is larger than `png_ptr->num_palette` and then converted to `png_size_t` (which is likely unsigned), it could wrap around to a smaller value than originally anticipated. This would allow a malicious PNG to *underflow* the buffer in `png_crc_read`.
- **Exploit:** An attacker could specially craft a PNG image to trigger the integer overflow, which may lead to heap corruption, or arbitrary code execution. **Not good.**
- **Fix:** Add additional checks to ensure that `length` does not exceed the size of the palette before the call to the function `png_crc_read`.
4. *C Code*
```C
int main() {
    int my_secret = 0x2fbd000b;

    char name[64] = {0};
    read(0, name, 64); //ask user for name
    printf("Hello ");
    printf(name);
    printf("Goodbye!\n");
    return 0;
}
```
- **Vulnerability:** This code has a Format String vulnerability. In the function `printf(name)`, the `name` variable, is directly used as a format string.
- **Exploit:** An attacker could include format specifiers such as `%x` or `%n` in their `name` input (as it is user-controlled), making it read arbitrary memory locations, which could leak sensitive data, or straight up cause the program to crash.
- **Fix:** ***NEVER*** pass user input directly to `printf` as a format string. ***ALWAYS*** use a safe format, like `printf("Hello %s, and goodbye~!\n", name);`.
5. *C Code (Networking)*
```C
a = packet_get_int(); //get integer
if (a > 0) {
 response = xmalloc(a*sizeof(char*));
 for (i = 0; i < a; i++)
  response[i] = packet_get_string(NULL);
}
```
- **Vulnerability:** This has two potential vulnerabilities. The first is an integer overflow, and the second is a possible out-of-bounds memory access. The calculation of `a*sizeof(char*)` could overflow if `a` is too large, which would lead to a ***MUCH*** smaller `malloc` request than originally intended, due to an integer underflow. Following the smaller memory allocation, it would loop and write `'a'` strings to the buffer.
- **Exploit:** An attacker could craft a malicious packet that sends an extremely large value for `a` to trigger the overflow. This is where the potential resides where critical memory areas could be overwritten. 
- **Fix:** Perform careful bounds checking on the value of `a` before proceeding with the `malloc` and the loop. You could also introduce limits on them maximum allowable size of `a`.

## Bonus Points
**Check the site [https://tryhackme.com/room/bufferoverflowprep](https://tryhackme.com/room/bufferoverflowprep) , there are 10 buffer overflow practices. You can practice each of them and get 1 bonus point per each. You can make videos to put on your YouTube channel or write in your weblog. For getting bonus points I need you to present them to me on your computer. You have time till end of the study break for earning these bonus points.**
