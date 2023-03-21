# RSA PARA CURIOSOS

Ao pesquisarmos sobre algoritmos de criptografia, com certa frequência nos deparamos com o famoso _"Algoritmo de Rivest-Shamir-Adleman"_, ou, para os intimos, RSA. Mas, o que seria, como funciona e qual a garantia que temos de que esse algoritmo é realmente seguro? Essas são as perguntas que pretendo dissecar no derrorer desta publicação.

Antes de prossegirmos, recomendo a leitura da publicação sobre [diffie-hellman]([https://deadlock.team/wtf/2019/01/23/Diffie-Hellman/](https://deadlock.team/wtf/2019/01/23/Diffie-Hellman/)) no site do time qual faço parte, onde algumas dúvidas que podem surgir ao decorrer desta leitura são esclarecidas,
tais como o funcionamento de chaves simétricas e assimétricas.

Dito isso, prossigamos com algumas explicações necessárias e antecedentes ao algoritmo em si. Essas explicações servem para o esclarecimento dos processos envolvidos no decorrer da realização de criptografia de um conteúdo qualquer.

## Teorema fundamental da aritmética

O **Teorema Fundamental da Aritmética** sustenta que:

Se **n** é um número natural, **n>1**, então existem números primos **p1,p2,p3,⋯,pr**, com **r≥1**, tais que:
	**n=p1p2p3⋯pr.**

A menos da ordem dos fatores primos, essa representação é única. Vejamos:

Sendo **x** um número qualquer, queremos provar que existe uma decomposição de x em fatores primos. Para isso, usaremos um recurso indutivo:  
-   Começando com **x=2**, vemos que ele possui uma decomposição em fatores primos, já que ele próprio é um número primo, e podemos chamar essa decomposição de  **decomposição trivial**.
-   Sendo **x > 2**, se **x** for um número primo, então admite a decomposição trivial, da mesma forma que o número 2. Se x não for primo, então **x** possui um divisor positivo d qualquer, tal que **x=dq**. De forma análoga ao que foi feito com **x**, caso _d_ ou _q_ sejam primos, eles admitem decomposição trivial. 
   
   
Uma vez provada a existência da decomposição, devemos provar que essa é única:  
Vamos supor que o número **x** admite duas decomposições em primos, de modo que _x = p1 e x = q1q2...qs_ . Chamaremos s de comprimento de x.  
Então  _p1 = q1q2...qs_ como _q1 divide  q1q2...qs, q1_ divide _p1_  e  _p1_ é primo, devemos ter  _p1 = q1_. Cancelando dos dois lados da igualdade, teremos  _1 = q2...qs_. Assim, vemos que  _s = 1_, pois se  _s>1_, teríamos um produto de números primos resultado em 1, o que é um absurdo.  
Assim, todo o número de comprimento 1 admite uma única decomposição.  
Agora, suponha que, para algum k natural, números de comprimento k admita uma única decomposição. Sendo x tal que  _x = pqp2...pk pk+1 = q1q2...qs_, em que os fatores estão dispostos em ordem crescente, provemos que números de comprimento  _K=1_ admitem uma única decomposição.  
_q1_  divide  _p1p2...pk+1_, então  q1  divide algum  _Pi_. Como ambos são primos, qi = pi  , e sabemos que  _q1 ≥ p1_, pois os números estão em ordem crescente. Da mesma forma,  _p1_ divide _q1q2...qs_, então _p1 = qj_, para algum **j**.  
Desse modo, _ p1 ≥ q1_. Cancelando os dois valores na igualdade, teremos que _p2...pk+1 = qs...qs _.  
Temos, assim, um número de comprimento  _k(p2...pk+1)_, que, pela hipótese de indução, possui uma única decomposição.  
Visualmente, temos:  
  ![enter image description here](http://clubes.obmep.org.br/blog/wp-content/uploads/2015/11/primos2.png)![enter image description here](http://clubes.obmep.org.br/blog/wp-content/uploads/2015/11/primos1.png)  
## Os números primos  
Um número é classificado como primo se ele é maior do que um e é divisível apenas por um e por ele mesmo. Apenas números naturais são classificados como primos.  
Eliminando alguns números com as seguintes regras de divisibilidade:  
**Divisibilidade por 2:** todo número par é divisível por 2. Os números pares são aqueles terminados em 0, 2, 4, 6 e 8.  
**Divisibilidade por 3**: um número é divisível por 3 se a soma dos seus algarismos der um número divisível por 3.  
**Divisibilidade por 4**: um número é divisível por 4 se ele for divisível duas vezes por 2 ou, então, se seus dois últimos algarismos forem divisíveis por 4.  
**Divisibilidade por 5**: todo número terminado em 0 ou 5 é divisível por cinco.  
**Divisibilidade por 6:**  se um número for par e também divisível por 3, será divisível por 6.  
**Divisibilidade por 7:**  um número é divisível por 7 se a diferença entre o dobro do último algarismo e o restante do número resultar em um número múltiplo de 7.  
teremos, para N numeros primos onde N<100:  
![crivo](https://s1.static.brasilescola.uol.com.br/img/2015/11/numeros-primos-.jpg)  
onde os números destacados em amarelo não se enquadram nas regras de divisibilidade supracitadas.  
## Aritmética modular e relação de congruências  
Uma congruência é a relação entre dois números que, divididos por um terceiro - chamado módulo de congruência - deixam o mesmo resto. Por exemplo, _32_ é congruente com _8_ módulo _12 (32 = 2 x 12 + 8 e 32 = 0 x 12 + 8)._   
Sejam **a** e **b** dois inteiros, e **m** um inteiro positivo. Então **a ≡ b (mod m)** se e somente se **a mod m = b mod m**.  
Ex: _379 ≡ 3 (mod 8)_    
## O Algoritmo de criptografia RSA  
O Algoritmo de rsa é um algoritmo de criptografia assimétrica e toma como base a matemática dos números primos. Vejamos:  
1.  Escolhe-se dois números primos **p** e **q**, distintos entre si.
2. Define-se _N = pq e φ(N) = (p - 1) (q - 1)_.
3. Deve-se escolher um número _e_, que faz parte da chave pública, de forma que _MDC( e, φ(N) ) = 1_: _( e,φ(N) ) = 1, e 1 < e < φ(N)_.
4. Resolvendo a congruência _ed ≡ 1 ( mod φ(N) )_ encontra-se _d_, que faz parte da chave privada.
5. De acordo com uma tabela rpé formulada e de domínio público( como a tabela ASCII), é feita a transformação de todos os caracteres da mensagem em números, obtendo-se a mensagem numérica em um único bloco que será dividida em blocos _b_, de forma que:  _1 ≤ b < N_. Isso garante que, ao utilizar congruência, obtenha-se um único resultado na decodificação.
6. De posse da Chave Pública (e,N) criptografa-se os blocos _b_ de acordo com a congruência (b^e) ≡ C(b) (mod N) onde C(b) é a mensagem criptografada.
7. De posse da chave privada (d, N) descriptografa-se de acordo com a congruência: _C(b)^d ≡ D(C(b)) (mod N)_, onde _D(c(b))_ é a mensagem descriptografada, _1 ≤ D(C(b)) < N._
8. Cada bloco D(C(b)) deve ser colocado em sequência e de acordo com a mesma tabela usada na **5** parte desta sequência  os números devem ser convertidos em caracteres.  
Vejamos um exemplo para φ(697):  
_φ(n) = (**p** - 1)  (**q** - 1)_  
_φ(697) = (**17** - 1) * (**41** - 1)_  
_φ(697) = **640**_  
*logo*:  
_MDC(640, 3) = 1_ e a _Chave pública = (697, 13)._  
*Para "TURING" teremos:*  
_T = 19 ^ 13 mod 697_  
_U = 20 ^ 13 mod 697_  
_R = 17 ^ 13 mod 697_  
_I = 09 ^ 13 mod 697_  
_N = 13 ^ 13 mod 697_  
_G = 07 ^ 13 mod 697_  
Que resulta em:  
`15 692 391 501 421 176`   
## Mas, por que esse método funciona?  
A Mensagem b deve ser igual à mensagem decodificada _D(C(b))_ de acordo com o processo:  
_(b^e)^ ≡ C(b) mod N_  
_C(b)^d ≡D(C(b)) mod N_  
_ed ≡ 1 mod φ(N)_  
Como _b < N D(C(b)) < N_ temos que:  
(b^e)^d^ ≡ D(C(b)) mod N. E, se p | b implica que _b≡0 mod p_, então b^e^d^≡ 0 mod p, implicando assim que _b^e^d^≡ b mod p, provando que:  
_D(C(b)) ≡b mod p_  
O que implica _(b^(q-1))^(p-1)^ ≡ 1^(p-1) mod q_, que, por transitividade, obtem-se que _b ≡D(C(b)) mod N._ Logo, a mensagem **b** é igual à mensagem decodificada _D(C(b))_.  
## Por que é seguro?  
Para quebrar o código é preciso ter a chave de decodificação _(d, N)_, acontece que a dhace pública _(e, N)_ já fornece parte do que é preciso, a saber, o número N. Assim, é apenas preciso encontrar _d_ e ter a mensagem em mãos.  
Tendo em vista o  número  _N = pq e φ(N) = (p - 1) (q - 1)_, é preciso encontrar a fatoração de N para que _p e q_ sejam encontrrados e assim encontrar φ(N), para prosseguir com _ed≡1 mod φ(N)_ e por fim **d**, o que é inviável haja vista a incapacidade computacional para fatoração de Inteiros grandes sem pequenos fatores.  
Se uma chave possui 3 bits, com **n** teremos _k = 2^n_ podemos dizer que k = 2^3^, que é a quantidade de possibilidades da representação binária elevada à quantidade de bits. Nesse caso teremos 8 possibilidades: **000, 001, 010, 011, 100, 101, 110, 111**. Assim sendo, a cada bit acrescentado ao expoente, dobra-se a quantidade de combinações possíveis e, consequentemente, esforço computacional e tempo requeridos para exaurimento das possibilidades.  
Em uma situação onde tivéssemos uma chave de 256 bits, ou, k= 2^256 para n=256, e um computador capaz de realizar 33.86 petaflops (quadrilhões de calculos por segundo), teriamos a seguinte situação:  
1 Petaflot = 2^50 operações por segundo.  Logo, nosso supercomputador pode processar _33.86 x 2^50_ operações por segundo, o que nos resulta em:  
2^256^ / (3386x2^50) x 350 x 24 x 60 x 60, que resulta em 9.63x10^52 anos.  
Deixo à sua imaginação a tarefa de imaginar o número de possibilidades/tempo para k = 2^4096^.  
## Referências  
[Criptografia RSA, por Daniele Helena Bonfim](https://upload.wikimedia.org/wikipedia/commons/1/1b/ASCII-Table-wide.svg)  
[A guide for teachers – Years 11 and 12 b y Maths delivers! RSA Encryption](https://www.amsi.org.au/teacher_modules/pdfs/Maths_delivers/Encryption5.pdf)  
[Teorema_fundamental_da_aritmética](https://pt.wikipedia.org/wiki/Teorema_fundamental_da_aritm%C3%A9tica)  
[Introdução a aritmetica Modular](https://www.cin.ufpe.br/~gdcc/matdis/aulas/aritmeticaModular.pdf)  