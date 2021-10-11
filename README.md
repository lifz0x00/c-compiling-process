## How GCC compiles a C/C++ program into executable:
* `preprocessing` Step 1, Source Code (.c, .cpp, .h), Preprocessor (cpp)
* `compilation` Step 2, Include Header, Expand Macro (.i, .ii) Compiler (gcc, g++)
* `assemble` Step 3, Assembly Code (.s) Assembler (as) to Machine Code (.o, .obj)
* `linking` Step 4, Static Library(.lib, .a) Linker (ld)
* `execute` Step 5, Executable Machine Code (.exe) "./executable"

For example, a `gcc main.c -o executable`

## Filename:
* `.c` C language file (SourceCode)
* `.i` Preprocessed C language file
* `.s` Compiled assembly file
* `.o` Compiled object file (ObjectCode/ObjectFile/objfile)

## GCC Commands:
* `-E` Preprocessing only
* `-S` Only preprocess and compile
* `-c` Only preprocess, compile and assemble
* `-o` file 	Specify the generated output file name as file
* `-c` Create Object File
* `-v` Display the programs invoked by the compiler.

## Key Terms:
* `Source Code?`
* `Object Code?`
* `Machine Code?`
* `Binary Code?`
* `Executable Code?`
* `Assembly Code?`

Catatan tentang *objfile* dan *executable file*

Perhatikan bahwa hasil akhir, file `foo` adalah file yang dapat diekekusi 
file `foo` disebut juga {"program biner terkompilasi", "program", "(gabungan) objfile" "executable", "binary", atau hanya "machine code", atau "satu dan nol" (10101010).

`*.o` adalah `obj file` pada `format obj file`, 
untuk menghasilkan `output` individu `*.o` atau `objfile` digabungkan menjadi satu oleh `linker`.
`Linker` secara garis besar menggabungkan semua *objfile* `*.o`
untuk menjadikan `executable file`, studi kasus disini `foo` adalah `executable file` yang merupakan gabungan dari `obj file`

## Notes
Object File can't be read immediately, please disassembly before reading `objfile`.

__Commands to Display assembler__
```zsh
$ objdump -d <objfile.o> // Display assembler contents of executable sections
$ objdump -d <objfile.o> // Display assembler contents of all sections
```

## Source Code .c
`main.c`
```c
#include <stdio.h>

int funcAdded(int num1, int num2);

int main(){
	int total = funcAdded(5, 3);
	printf("5 + 3 = %d", total);
	
	return 0;
}
```

`add.c`
```c
int funcAdded(int num1, int num2) {
	return (num1 + num2);
}
```

<br>

## Instant Compilation C/C++ Code
__Commands to Compile C Code__
```zsh
$ gcc hello.c -o executable; ./executable
> Output (executable)
```

<br>

## Understanding Compilation Process with Linking {main, add}.c to executable!

<br>

## Step 1
__Pre-processing__: via the GNU C Preprocessor (cpp.exe), which includes the headers (#include) and expands the macros (#define).

```txt
cpp {source.c} > {pre-processor.i}
```

__Create Preprocessed file__

```zsh
$ cpp main.c > main.i
> Output main.i // Output <pre-processor>
```

<br>

## Step 2
__Compilation__: The compiler compiles the `pre-processed` *Source Code* into `assembly code` for a __specific processor__.

```txt
gcc -S pre-processing
```

__Create Assembly file__

```zsh
$ gcc -S main.i
> Output main.s // Output <assembly.s> 
```

<br>

## Step 3 
__Assembly__: The assembler (as.exe) converts the assembly code into machine code in the `objfile` "main.o".

```txt
gcc <assembly.s> -o <objfile.o> 
```

__Create Object file__

```zsh
$ as main.s -o main.o
> Output main.o // Output <objfile.o>
```

<br>

## Step 4 
Create objfile from add.c with default name.

```txt
gcc -c <source.c>
```

__Compile Source file `.c` to Object file `.o`__ 

```zsh
$ gcc -c add.c
> Output add.o
```

<br>

## Step 5
__Linker__: links the *objfile* `main.o` and `add.o` to produce an executable file `executable`.  

__Linking `main.o` and `add.o` to `executable`__ 

```zsh
$ gcc main.o add.o -o executable
```

<br>

## Step 6
__Execute__: Show this output from `executable file`

```zsh
$ ./executable
> (Output)
```

## Running this Code step-by-step and Check this Output with Terminal

__Commands__:
```zsh
cpp main.c > main.i;
ls;
gcc -S main.i;
ls;
as main.s -o main.o;
ls;
gcc -c add.c;
ls;
gcc main.o add.o -o executable;
ls;
./executable;
```

__Output__:
```zsh
➜  c-low-level ls
total 16
-rw-r--r-- 1 kali kali  62 11 Apr   10:59   add.c 
-rw-r--r-- 1 kali kali 143 11 Apr   10:59   main.c 
➜  c-low-level cpp main.c > main.i;
➜  c-low-level ls
total 56
-rw-r--r-- 1 kali kali    62 11 Apr   10:59   add.c 
-rw-r--r-- 1 kali kali   143 11 Apr   10:59   main.c 
-rw-r--r-- 1 kali kali 16398 11 Apr   11:10   main.i 
➜  c-low-level gcc -S main.i;
➜  c-low-level ls
total 64
-rw-r--r-- 1 kali kali    62 11 Apr   10:59   add.c 
-rw-r--r-- 1 kali kali   143 11 Apr   10:59   main.c 
-rw-r--r-- 1 kali kali 16398 11 Apr   11:10   main.i 
-rw-r--r-- 1 kali kali   594 11 Apr   11:10   main.s 
➜  c-low-level as main.s -o main.o;
➜  c-low-level ls
total 72
-rw-r--r-- 1 kali kali    62 11 Apr   10:59   add.c 
-rw-r--r-- 1 kali kali   143 11 Apr   10:59   main.c 
-rw-r--r-- 1 kali kali 16398 11 Apr   11:10   main.i 
-rw-r--r-- 1 kali kali  1632 11 Apr   11:10   main.o 
-rw-r--r-- 1 kali kali   594 11 Apr   11:10   main.s 
➜  c-low-level gcc -c add.c;
➜  c-low-level ls
total 80
-rw-r--r-- 1 kali kali    62 11 Apr   10:59   add.c 
-rw-r--r-- 1 kali kali  1240 11 Apr   11:11   add.o 
-rw-r--r-- 1 kali kali   143 11 Apr   10:59   main.c 
-rw-r--r-- 1 kali kali 16398 11 Apr   11:10   main.i 
-rw-r--r-- 1 kali kali  1632 11 Apr   11:10   main.o 
-rw-r--r-- 1 kali kali   594 11 Apr   11:10   main.s 
➜  c-low-level gcc main.o add.o -o executable;
➜  c-low-level ls
total 120
-rw-r--r-- 1 kali kali    62 11 Apr   10:59   add.c 
-rw-r--r-- 1 kali kali  1240 11 Apr   11:11   add.o 
-rwxr-xr-x 1 kali kali 16672 11 Apr   11:11   executable 
-rw-r--r-- 1 kali kali   143 11 Apr   10:59   main.c 
-rw-r--r-- 1 kali kali 16398 11 Apr   11:10   main.i 
-rw-r--r-- 1 kali kali  1632 11 Apr   11:10   main.o 
-rw-r--r-- 1 kali kali   594 11 Apr   11:10   main.s
➜  c-low-level ./executable; 
5 + 3 = 8
➜  c-low-level objdump -d add.o

add.o:     file format elf64-x86-64


Disassembly of section .text:

0000000000000000 <funcAdded>:
   0:   55                      push   %rbp
   1:   48 89 e5                mov    %rsp,%rbp
   4:   89 7d fc                mov    %edi,-0x4(%rbp)
   7:   89 75 f8                mov    %esi,-0x8(%rbp)
   a:   8b 55 fc                mov    -0x4(%rbp),%edx
   d:   8b 45 f8                mov    -0x8(%rbp),%eax
  10:   01 d0                   add    %edx,%eax
  12:   5d                      pop    %rbp
  13:   c3                      retq   
➜  c-low-level objdump -d main.o

main.o:     file format elf64-x86-64


Disassembly of section .text:

0000000000000000 <main>:
   0:   55                      push   %rbp
   1:   48 89 e5                mov    %rsp,%rbp
   4:   48 83 ec 10             sub    $0x10,%rsp
   8:   be 03 00 00 00          mov    $0x3,%esi
   d:   bf 05 00 00 00          mov    $0x5,%edi
  12:   e8 00 00 00 00          callq  17 <main+0x17>
  17:   89 45 fc                mov    %eax,-0x4(%rbp)
  1a:   8b 45 fc                mov    -0x4(%rbp),%eax
  1d:   89 c6                   mov    %eax,%esi
  1f:   48 8d 3d 00 00 00 00    lea    0x0(%rip),%rdi        # 26 <main+0x26>
  26:   b8 00 00 00 00          mov    $0x0,%eax
  2b:   e8 00 00 00 00          callq  30 <main+0x30>
  30:   b8 00 00 00 00          mov    $0x0,%eax
  35:   c9                      leaveq 
  36:   c3                      retq 
```

## References
* [GCC and Make: Compiling, Linking and Building C/C++ Applications - www3.ntu.edu.sg](https://www3.ntu.edu.sg/home/ehchua/programming/cpp/gcc_make.html)
* [Solve the mystery of the three lives and three generations of HelloWorld through the ELF file of Linux (from now on, your youth will not be lost) - programmersought](https://www.programmersought.com/article/79215069083/)
* [Preprocessed .i file - pvs-studio](https://pvs-studio.com/en/t/0076/)
* [4.3 Preprocessing source files - linuxtopia](https://www.linuxtopia.org/online_books/an_introduction_to_gcc/gccintro_36.html)
* [Introduction to the ELF Format Part II : Understanding Program Headers - k3170makan](http://blog.k3170makan.com/2018/09/introduction-to-elf-format-part-ii.html)
