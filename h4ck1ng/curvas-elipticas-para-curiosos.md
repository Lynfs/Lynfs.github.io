# Criptografia de curva eliptica para curiosos


Como bem sabemos, os algoritmos criptograficos atuais sao, em essencia, problemas matematicos dificeis de resolver, e  a unica forma de quebrar esse tipo de sistema e por intermedio de alguns outros algoritmos que tentem resolver, da forma mais eficiente possivel, o problema proposto.

A criptografia de curva eliptica, utilizada em servicos como *TLS*, *SSH* e *PGP*, aparece como uma abordagem proposta na criacao de algoritmos criptograficos assimetricos, utilizando de um conceito matematico chamado **Curva Eliptica**(daa).

a titulo de exemplo, no RSA, a chave publica e um grande numero, que e um produto de dois numeros primos tambem grandes, alem de um outro numero menor e importante, e a chave privada e um outro numero relacionado com os primeiros citados.[(clique aqui para uma melhor compreensao)](https://lynfs.github.io/Rsa-para-Curiosos). Em ECC (elliptc-curve cryptography), a chave publica e uma **equacao para uma curva eliptica** e um ponto que reside naquela curva, e a chave privada e um numero tambem relacionado.

Porem, antes de qualquer aprofundamento, comecemos do principio... O que sao curvas elipticas, qual sua aplicacao na criptografia e por que funciona? e o que pretendo explicar, de maneira breve e nao tao tecnica nos escritos a seguir. Como meu conhecimento nao e aprofundado, me limito a explicar o que sei, podendo gerar uma nova postagem se necessario aprofundar mais a teoria que envolve o assunto. 

Dito isso, vejamos como essas curvas funcionam...

## Cuvas elipticas e relacoes algebricas

![enter image description here](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c1/ECClines.svg/1920px-ECClines.svg.png)

O estudo das curvas elipticas e uma area da *Geometria Algebrica* com aplicacoes em *Teoria dos Numeros*, e se definem mediante equacoes cubicas (de terceiro grau). Essas curvas sao conhecidas como curvas **nao-singulares**, que em outras palavras significa que nao possuem auto-interseccoes, e se pode definir uma operacao binaria para o conjunto de seus pontos de uma maneira geometrica natural.

Ou, de maneira simplificada, e um conjunto de pontos que satisfaz a equacao *y² = x³+ ax + b.

Essa equacao representa uma curva especial matematica, sobre qual podemos definir algumas operacoes especiais..


Adentremos na matematica um pouco para compreender esse assunto em termos matematicos.

Definiremos a soma de dois pontos *P* e *Q* de uma curva eliptica *E* da seguinte forma:

* Trace uma reta que conecta os dois pontos *P e Q*
* Essa reta vai tocar na curva em um ponto definido *-R*
* Espelhando o ponto *-R* em torno do eixo *X*, encontramos um ponto *R* pertencente a curva, o qual e o resultado da soma de *P+Q = R*

![](https://www.researchgate.net/profile/Tabassum-Ara-2/publication/326009351/figure/fig1/AS:642029855977473@1530083254406/Point-Addition-on-the-Elliptic-Curve-18.png)

mostra-se ainda que uma operacao de soma assim definida sobre pontos de uma curva eliptica forma um **Grupo Abeliano**, que significa:

* Existe um elemento neutro
* Todo elemento admite um inverso
* A soma e aditiva e comutativa

## Campos finitos Fp

Um campo finito e um *conjunto* composto por um numero finito de elementos. Simples assim!

Ja campos finitos *Fp* e um *conjunto* de numeros inteiros modulos *p*, onde *p* e um numero primo. O conjunto de inteiros modulo *p* consiste em todos os numeros inteiros de 0 ate *p-1* e suas operacoes sao feitas em aritmetica modular(checar post sobre RSA para maior entendimento).

Recaptulemos...

ex. *a ≡ b mod n* e o mesmo que dizer que "O resto da divisao de *b* por *n* = a"

## Curvas elipticas sobre Fp

Sao o conjunto de *(x,y)* que satisfazem a equacao:

*y² mod p = x³ + ax + b mod p*, com  4a³+27b ≠ 0, tal que *a, b, x e y* pertencem a Fp mais um ponto especial chamado de ponto infinito *O*.

No nosso caso criptografico, e escolhido um ponto base *G*, que possui uma ordem *n* e um cofator *h*.

Abaixo, exemplos de curvas elipticas sobre Fp para  p = 19, 97, 127 e 148.

![enter image description here](https://miro.medium.com/max/1216/0*7cfE3-WGOzLAI8EP.png)

E podemos ver que, quanto maior o primo, maior o numeros de pontos na curva eliptica, o que e muito bacana, porque quando vamos criptografar uma mensagem, precisamos mapear a mensagem em algum ponto de uma curva eliptica, entao quanto mais pontos, melhor.


## Chaves assimetricas sobre curvas elipticas

* Privada
	* define-se *d*, um inteiro randomico qualquer entre 1 e n-1, onde n e a ordem do ponto base G.
* Publica
	* um ponto D = dG
	
## sistema de ElGamal

O fluxo de cifragem C segue o processo onde, usamos a chave privada *d* do emissor, a chave publica *D* do receptor,  aplica-se o algoritmo de curva eliptica e obtemos a saida criptografada.

**C = M+d[e]D[r]**, onde definimos M como a mensagem mapeada em um ponto sobre a curva.

Ja no fluxo de descriptografia, o receptor recebe a mensagem criptografada C, e a descriptografa utilizando a sua chave privada e a publica do emissor.

**M = C - d[r]D[e]**

## Protocolo Diffie-hellman sobre curvas elipticas

No caso do sistema diffie-hellman, teremos a seguinte linha construtiva:

Nesse sistema, uma pessoa de confianca torna publico um numero primo *P* e um inteiro *G*. Agora, imaginemos que Ana e Bob querem trocar informacoes utilizando desse sistema para criptografar as mensagens.

Ana e Bob escolhem, cada um, um inteiro **X** qualquer conhecido apenas por cada um deles. Ana e Bob calculam valores congruentes a *G* elevado aos seus numeros secretos, e enviam um ao outro.

Ana calcula A ≡ G^x (mod p) e envia para Bob

Bob calcula B ≡ G^x (mod p) e envia para Ana

por fim, com os novos numeros, eles calculam o resultado da congruencia com *G* elevado aos expoentes secretos

Ana calcula M ≡ B^x (mod p)

Bob calcula M ≡ A^x (mod p)

Mostra-se que os valores de M encontrados por Bob e Ana, na verdade, sao iguais. Isto e, eles possuem uma mensagem secreta compartilhada entre eles, mas ainda vemos que para alguem que nao tem acesso aos inteiros secretos de Bob e Ana, e matematicamente dificil descobrir quais sao, e, mais ainda, qual e a mensagem criptografada.

---

No caso deste sistema aplicado as curvas elipticas, teremos um seguinte fluxo:

Seja conhecida uma curva eliptica *E*, um primo *p* e um ponto P ∈ E, ana e bob escolhem inteiros randomicos secretos e calculam o produto desse inteiro pelo ponto *P* e em seguida trocam entre si o resultado.

Ana escolhe n[a], calcula Q[a] = n[a]P, e envia para Bob

Bob escolhe n[b], calcula Q[b] = n[b]P, e envia para Ana

Com os novos numeros, eles repetem o primeiro passo:

Ana calcula Q1 = n[a]Q[b]

Bob calcula Q2 = n[b]Q[a]

Podemos provar que estes numeros resultantes sao iguais (como era de se esperar).

## Escolha das curvas elipticas

Algumas curvas elipticas tem um nivel de seguranca menor que outras. e preciso calcular os diferentes parametros dela e analizar suas propriedades para garantir sua seguranca. E, para que isso nao seja sempre necessario, ja existem curvas definidas com propriedades interessantes e sao consideradas seguras, e sao definidas por institutos como NIST FIPS PUB 186-3-DSS e SECG em SEC2, como a curva ecp256k1.

Existem tambem algoritmos como ECDSA que e o algoritmo de assinaturas digitais, que permite verificacao de autenticidade do emissor atraves do conhecimento de sua chave publica, ou ECDH que e o protocolo de acordo de chaves Diffie-hellman com curvas elipticas.

### Problematica do logaritmo discreto para curvas elipticas

Nesse momento, surge a duvida: quem garante que isso funciona?
Existe um problema que garante que a seguranca das curvas elipticas nao sera facilmente violado, e e o problema dos logaritmos discretos. 
Esse problema consiste na seguinte premissa:

"Dados dois numeros *p e q*, encontrar K tal que q = KP

Ou, no caso do campo finito Fp, conhecidos *a e b*, encontrar k tal que b = a^k mod p."

Para os problemas citados acima, e crivel que nao ha um algoritmo de tempo polinomial que rode em um computador classico para resolve-lo e quebrar todos os algoritmos de criptografia de curvas elipticas, apesar de nao existir uma prova matematica para tal afirmacao.

---

### Referencias

* [An Introduction to Elliptic Curves with Application for Secondary Education](https://periodicos.ufsm.br/cienciaenatura/article/download/14815/pdf)
* [Curvas Elipticas: Aplicacao em Criptografia Assimetrica](https://www.lncc.br/~borges/doc/Curvas%20Elipticas%20-%20Aplicacao%20em%20Criptografia%20Assimetrica.pdf)
