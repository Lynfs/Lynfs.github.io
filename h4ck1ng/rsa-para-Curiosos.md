# RSA PARA CURIOSOS

Ao pesquisarmos sobre algoritmos de criptografia, com certa frequencia nos deparamos com o famoso _"Algoritmo de Rivest-Shamir-Adleman"_, ou, para os intimos, RSA. Mas, o que seria, como funciona e qual a garantia que temos de que esse algoritmo e realmente seguro? Essas são as perguntas que pretendo dissecar no derrorer desta publicacão.

Antes de prossegirmos, recomendo a leitura da publicacão sobre [diffie-hellman]([https://deadlock.team/wtf/2019/01/23/Diffie-Hellman/](https://deadlock.team/wtf/2019/01/23/Diffie-Hellman/)) no site do time qual faco parte, onde algumas duvidas que podem surgir ao decorrer desta leitura são esclarecidas,
tais como o funcionamento de chaves simetricas e assimetricas.

Dito isso, prossigamos com algumas explicacoes necessarias e antecedentes ao algoritmo em si. Essas explicacoes servem para o esclarecimento dos processos envolvidos no decorrer da realizacão de criptografia de um conteudo qualquer.

## Teorema fundamental da aritmetica

O **Teorema Fundamental da Aritmetica** sustenta que:

Se **n** e um numero natural, **n>1**, então existem numeros primos **p1,p2,p3,⋯,pr**, com **r≥1**, tais que:
	**n=p1p2p3⋯pr.**

A menos da ordem dos fatores primos, essa representacão e unica. Vejamos:

Sendo **x** um numero qualquer, queremos provar que existe uma decomposicão de x em fatores primos. Para isso, usaremos um recurso indutivo:  
-   Comecando com **x=2**, vemos que ele possui uma decomposicão em fatores primos, ja que ele proprio e um numero primo, e podemos chamar essa decomposicão de  **decomposicão trivial**.
-   Sendo **x > 2**, se **x** for um numero primo, então admite a decomposicão trivial, da mesma forma que o numero 2. Se x não for primo, então **x** possui um divisor positivo d qualquer, tal que **x=dq**. De forma analoga ao que foi feito com **x**, caso _d_ ou _q_ sejam primos, eles admitem decomposicão trivial. 
   
   
Uma vez provada a existencia da decomposicão, devemos provar que essa e unica:  
Vamos supor que o numero **x** admite duas decomposicoes em primos, de modo que _x = p1 e x = q1q2...qs_ . Chamaremos s de comprimento de x.  
Então  _p1 = q1q2...qs_ como _q1 divide  q1q2...qs, q1_ divide _p1_  e  _p1_ e primo, devemos ter  _p1 = q1_. Cancelando dos dois lados da igualdade, teremos  _1 = q2...qs_. Assim, vemos que  _s = 1_, pois se  _s>1_, teriamos um produto de numeros primos resultado em 1, o que e um absurdo.  
Assim, todo o numero de comprimento 1 admite uma unica decomposicão.  
Agora, suponha que, para algum k natural, numeros de comprimento k admita uma unica decomposicão. Sendo x tal que  _x = pqp2...pk pk+1 = q1q2...qs_, em que os fatores estão dispostos em ordem crescente, provemos que numeros de comprimento  _K=1_ admitem uma unica decomposicão.  
_q1_  divide  _p1p2...pk+1_, então  q1  divide algum  _Pi_. Como ambos são primos, qi = pi  , e sabemos que  _q1 ≥ p1_, pois os numeros estão em ordem crescente. Da mesma forma,  _p1_ divide _q1q2...qs_, então _p1 = qj_, para algum **j**.  
Desse modo, _ p1 ≥ q1_. Cancelando os dois valores na igualdade, teremos que _p2...pk+1 = qs...qs _.  
Temos, assim, um numero de comprimento  _k(p2...pk+1)_, que, pela hipotese de inducão, possui uma unica decomposicão.  
Visualmente, temos:  
  ![enter image description here](http://clubes.obmep.org.br/blog/wp-content/uploads/2015/11/primos2.png)![enter image description here](http://clubes.obmep.org.br/blog/wp-content/uploads/2015/11/primos1.png)  
## Os numeros primos  
Um numero e classificado como primo se ele e maior do que um e e divisivel apenas por um e por ele mesmo. Apenas numeros naturais são classificados como primos.  
Eliminando alguns numeros com as seguintes regras de divisibilidade:  
**Divisibilidade por 2:** todo numero par e divisivel por 2. Os numeros pares são aqueles terminados em 0, 2, 4, 6 e 8.  
**Divisibilidade por 3**: um numero e divisivel por 3 se a soma dos seus algarismos der um numero divisivel por 3.  
**Divisibilidade por 4**: um numero e divisivel por 4 se ele for divisivel duas vezes por 2 ou, então, se seus dois ultimos algarismos forem divisiveis por 4.  
**Divisibilidade por 5**: todo numero terminado em 0 ou 5 e divisivel por cinco.  
**Divisibilidade por 6:**  se um numero for par e tambem divisivel por 3, sera divisivel por 6.  
**Divisibilidade por 7:**  um numero e divisivel por 7 se a diferenca entre o dobro do ultimo algarismo e o restante do numero resultar em um numero multiplo de 7.  
teremos, para N numeros primos onde N<100:  
![crivo](https://s1.static.brasilescola.uol.com.br/img/2015/11/numeros-primos-.jpg)  
onde os numeros destacados em amarelo não se enquadram nas regras de divisibilidade supracitadas.  
## Aritmetica modular e relacão de congruencias  
Uma congruencia e a relacão entre dois numeros que, divididos por um terceiro - chamado modulo de congruencia - deixam o mesmo resto. Por exemplo, _32_ e congruente com _8_ modulo _12 (32 = 2 x 12 + 8 e 32 = 0 x 12 + 8)._   
Sejam **a** e **b** dois inteiros, e **m** um inteiro positivo. Então **a ≡ b (mod m)** se e somente se **a mod m = b mod m**.  
Ex: _379 ≡ 3 (mod 8)_    
## O Algoritmo de criptografia RSA  
O Algoritmo de rsa e um algoritmo de criptografia assimetrica e toma como base a matematica dos numeros primos. Vejamos:  
1.  Escolhe-se dois numeros primos **p** e **q**, distintos entre si.
2. Define-se _N = pq e φ(N) = (p - 1) (q - 1)_.
3. Deve-se escolher um numero _e_, que faz parte da chave publica, de forma que _MDC( e, φ(N) ) = 1_: _( e,φ(N) ) = 1, e 1 < e < φ(N)_.
4. Resolvendo a congruencia _ed ≡ 1 ( mod φ(N) )_ encontra-se _d_, que faz parte da chave privada.
5. De acordo com uma tabela rpe formulada e de dominio publico( como a tabela ASCII), e feita a transformacão de todos os caracteres da mensagem em numeros, obtendo-se a mensagem numerica em um unico bloco que sera dividida em blocos _b_, de forma que:  _1 ≤ b < N_. Isso garante que, ao utilizar congruencia, obtenha-se um unico resultado na decodificacão.
6. De posse da Chave Publica (e,N) criptografa-se os blocos _b_ de acordo com a congruencia (b^e) ≡ C(b) (mod N) onde C(b) e a mensagem criptografada.
7. De posse da chave privada (d, N) descriptografa-se de acordo com a congruencia: _C(b)^d ≡ D(C(b)) (mod N)_, onde _D(c(b))_ e a mensagem descriptografada, _1 ≤ D(C(b)) < N._
8. Cada bloco D(C(b)) deve ser colocado em sequencia e de acordo com a mesma tabela usada na **5** parte desta sequencia  os numeros devem ser convertidos em caracteres.  
Vejamos um exemplo para φ(697):  
_φ(n) = (**p** - 1)  (**q** - 1)_  
_φ(697) = (**17** - 1) * (**41** - 1)_  
_φ(697) = **640**_  
*logo*:  
_MDC(640, 3) = 1_ e a _Chave publica = (697, 13)._  
*Para "TURING" teremos:*  
_T = 19 ^ 13 mod 697_  
_U = 20 ^ 13 mod 697_  
_R = 17 ^ 13 mod 697_  
_I = 09 ^ 13 mod 697_  
_N = 13 ^ 13 mod 697_  
_G = 07 ^ 13 mod 697_  
Que resulta em:  
`15 692 391 501 421 176`   
## Mas, por que esse metodo funciona?  
A Mensagem b deve ser igual a mensagem decodificada _D(C(b))_ de acordo com o processo:  
_(b^e)^ ≡ C(b) mod N_  
_C(b)^d ≡D(C(b)) mod N_  
_ed ≡ 1 mod φ(N)_  
Como _b < N D(C(b)) < N_ temos que:  
(b^e)^d^ ≡ D(C(b)) mod N. E, se p | b implica que _b≡0 mod p_, então b^e^d^≡ 0 mod p, implicando assim que _b^e^d^≡ b mod p, provando que:  
_D(C(b)) ≡b mod p_  
O que implica _(b^(q-1))^(p-1)^ ≡ 1^(p-1) mod q_, que, por transitividade, obtem-se que _b ≡D(C(b)) mod N._ Logo, a mensagem **b** e igual a mensagem decodificada _D(C(b))_.  
## Por que e seguro?  
Para quebrar o codigo e preciso ter a chave de decodificacão _(d, N)_, acontece que a dhace publica _(e, N)_ ja fornece parte do que e preciso, a saber, o numero N. Assim, e apenas preciso encontrar _d_ e ter a mensagem em mãos.  
Tendo em vista o  numero  _N = pq e φ(N) = (p - 1) (q - 1)_, e preciso encontrar a fatoracão de N para que _p e q_ sejam encontrrados e assim encontrar φ(N), para prosseguir com _ed≡1 mod φ(N)_ e por fim **d**, o que e inviavel haja vista a incapacidade computacional para fatoracão de Inteiros grandes sem pequenos fatores.  
Se uma chave possui 3 bits, com **n** teremos _k = 2^n_ podemos dizer que k = 2^3^, que e a quantidade de possibilidades da representacão binaria elevada a quantidade de bits. Nesse caso teremos 8 possibilidades: **000, 001, 010, 011, 100, 101, 110, 111**. Assim sendo, a cada bit acrescentado ao expoente, dobra-se a quantidade de combinacoes possiveis e, consequentemente, esforco computacional e tempo requeridos para exaurimento das possibilidades.  
Em uma situacão onde tivessemos uma chave de 256 bits, ou, k= 2^256 para n=256, e um computador capaz de realizar 33.86 petaflops (quadrilhoes de calculos por segundo), teriamos a seguinte situacão:  
1 Petaflot = 2^50 operacoes por segundo.  Logo, nosso supercomputador pode processar _33.86 x 2^50_ operacoes por segundo, o que nos resulta em:  
2^256^ / (3386x2^50) x 350 x 24 x 60 x 60, que resulta em 9.63x10^52 anos.  
Deixo a sua imaginacão a tarefa de imaginar o numero de possibilidades/tempo para k = 2^4096^.  
## Referencias  
[Criptografia RSA, por Daniele Helena Bonfim](https://upload.wikimedia.org/wikipedia/commons/1/1b/ASCII-Table-wide.svg)  
[A guide for teachers – Years 11 and 12 b y Maths delivers! RSA Encryption](https://www.amsi.org.au/teacher_modules/pdfs/Maths_delivers/Encryption5.pdf)  
[Teorema_fundamental_da_aritmetica](https://pt.wikipedia.org/wiki/Teorema_fundamental_da_aritm%C3%A9tica)  
[Introducão a aritmetica Modular](https://www.cin.ufpe.br/~gdcc/matdis/aulas/aritmeticaModular.pdf)  