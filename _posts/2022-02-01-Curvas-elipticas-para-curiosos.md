# Criptografia de curva elíptica para curiosos

Como bem sabemos, os algoritmos criptográficos atuais são, em essência, problemas matemáticos difíceis de resolver, e  a única forma de quebrar esse tipo de sistema é por intermédio de alguns outros algoritmos que tentem resolver, da forma mais eficiente possível, o problema proposto.

A criptografia de curva elíptica, utilizada em serviços como *TLS*, *SSH* e *PGP*, aparece como uma abordagem proposta na criação de algoritmos criptográficos assimétricos, utilizando de um conceito matemático chamado **Curva Elíptica**(dãã).

À título de exemplo, no RSA, a chave pública é um grande número, que é um produto de dois números primos também grandes, além de um outro número menor e importante, e a chave privada é um outro número relacionado com os primeiros citados.[(clique aqui para uma melhor compreensão)](https://lynfs.github.io/rsa-para-curiosos.html). Em ECC (elliptc-curve cryptography), a chave pública é uma **equação para uma curva elíptica** e um ponto que reside naquela curva, e a chave privada é um número também relacionado.

Porém, antes de qualquer aprofundamento, comecemos do princípio... O que são curvas elípticas, qual sua aplicação na criptografia e por quê funciona? É o que pretendo explicar, de maneira breve e não tão técnica nos escritos à seguir. Como meu conhecimento não é aprofundado, me limito à explicar o que sei, podendo gerar uma nova postagem se necessário aprofundar mais à teoria que envolve o assunto. 

Dito isso, vejamos como essas curvas funcionam...

## Cuvas elípticas e relações algébricas

![enter image description here](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c1/ECClines.svg/1920px-ECClines.svg.png)

O estudo das curvas elípticas é uma área da *Geometria Algébrica* com aplicações em *Teoria dos Números*, e se definem mediante equações cúbicas (de terceiro grau). Essas curvas são conhecidas como curvas **não-singulares**, que em outras palavras significa que não possuem auto-intersecções, e se pode definir uma operação binária para o conjunto de seus pontos de uma maneira geométrica natural.

Ou, de maneira simplificada, é um conjunto de pontos que satisfaz a equação *y² = x³+ ax + b.

Essa equação representa uma curva especial matemática, sobre qual podemos definir algumas operações especiais..


Adentremos na matemática um pouco para compreender esse assunto em termos matemáticos.

Definiremos a soma de dois pontos *P* e *Q* de uma curva elíptica *E* da seguinte forma:

* Trace uma reta que conecta os dois pontos *P e Q*
* Essa reta vai tocar na curva em um ponto definido *-R*
* Espelhando o ponto *-R* em torno do eixo *X*, encontramos um ponto *R* pertencente à curva, o qual é o resultado da soma de *P+Q = R*

![](https://www.researchgate.net/profile/Tabassum-Ara-2/publication/326009351/figure/fig1/AS:642029855977473@1530083254406/Point-Addition-on-the-Elliptic-Curve-18.png)

mostra-se ainda que uma operação de soma assim definida sobre pontos de uma curva elíptica forma um **Grupo Abeliano**, que significa:

* Existe um elemento neutro
* Todo elemento admite um inverso
* A soma é aditiva e comutativa

## Campos finitos Fp

Um campo finito é um *conjunto* composto por um número finito de elementos. Simples assim!

Já campos finitos *Fp* é um *conjunto* de números inteiros módulos *p*, onde *p* é um número primo. O conjunto de inteiros módulo *p* consiste em todos os números inteiros de 0 até *p-1* e suas operações são feitas em aritmética modular(checar post sobre RSA para maior entendimento).

Recaptulemos...

ex. *a ≡ b mod n* é o mesmo que dizer que "O resto da divisão de *b* por *n* = a"

## Curvas elípticas sobre Fp

São o conjunto de *(x,y)* que satisfazem a equação:

*y² mod p = x³ + ax + b mod p*, com  4a³+27b ≠ 0, tal que *a, b, x e y* pertencem a Fp mais um ponto especial chamado de ponto infinito *O*.

No nosso caso criptográfico, é escolhido um ponto base *G*, que possui uma ordem *n* e um cofator *h*.

Abaixo, exemplos de curvas elípticas sobre Fp para  p = 19, 97, 127 e 148.

![enter image description here](https://miro.medium.com/max/1216/0*7cfE3-WGOzLAI8EP.png)

E podemos ver que, quanto maior o primo, maior o números de pontos na curva elíptica, o que é muito bacana, porque quando vamos criptografar uma mensagem, precisamos mapear a mensagem em algum ponto de uma curva elíptica, então quanto mais pontos, melhor.


## Chaves assimétricas sobre curvas elípticas

* Privada
	* define-se *d*, um inteiro randômico qualquer entre 1 e n-1, onde n é a ordem do ponto base G.
* Pública
	* um ponto D = dG
	
## sistema de ElGamal

O fluxo de cifragem C segue o processo onde, usamos a chave privada *d* do emissor, a chave pública *D* do receptor,  aplica-se o algoritmo de curva elíptica e obtemos a saída criptografada.

**C = M+d[e]D[r]**, onde definimos M como a mensagem mapeada em um ponto sobre a curva.

Já no fluxo de descriptografia, o receptor recebe a mensagem criptografada C, e a descriptografa utilizando a sua chave privada e a pública do emissor.

**M = C - d[r]D[e]**

## Protocolo Diffie-hellman sobre curvas elípticas

No caso do sistema diffie-hellman, teremos a seguinte linha construtiva:

Nesse sistema, uma pessoa de confiança torna público um número primo *P* e um inteiro *G*. Agora, imaginemos que Ana e Bob querem trocar informações utilizando desse sistema para criptografar as mensagens.

Ana e Bob escolhem, cada um, um inteiro **X** qualquer conhecido apenas por cada um deles. Ana e Bob calculam valores congruentes à *G* elevado aos seus números secretos, e enviam um ao outro.

Ana calcula A ≡ G^x (mod p) e envia para Bob
Bob calcula A ≡ G^x (mod p) e envia para Ana


## Escolha das curvas elípticas

Algumas curvas elípticas tem um nível de segurança menor que outras. É preciso calcular os diferentes parâmetros dela e analizar suas propriedades para garantir sua segurança. E, para que isso não seja sempre necessário, já existem curvas definidas com propriedades interessantes e são consideradas seguras, e são definidas por institutos como NIST FIPS PUB 186-3-DSS e SECG em SEC2, como a curva ecp256k1.

Existem também algoritmos como ECDSA que é o algoritmo de assinaturas digitais, que permite verificação de autenticidade do emissor através do conhecimento de sua chave pública, ou ECDH que é o protocolo de acordo de chaves Diffie-hellman com curvas elípticas.

### Problemática do logaritmo discreto para curvas elípticas

Nesse momento, surge a dúvida: quem garante que isso funciona?
Existe um problema que garante que a segurança das curvas elípticas não será facilmente violado, e é o problema dos logaritmos discretos. 
Esse problema consiste na seguinte premissa:

"Dados dois números *p e q*, encontrar K tal que q = KP

Ou, no caso do campo finito Fp, conhecidos *a e b*, encontrar k tal que b = a^k mod p."

Para os problemas citados acima, é crível que não há um algoritmo de tempo polinomial que rode em um computador clássico para resolvê-lo e quebrar todos os algoritmos de criptografia de curvas elípticas, apesar de não existir uma prova matemática para tal afirmação.

---
### Referências

* [An Introduction to Elliptic Curves with Application for Secondary Education](https://periodicos.ufsm.br/cienciaenatura/article/download/14815/pdf)
* [Curvas Elípticas: Aplicação em Criptografia Assimétrica](https://www.lncc.br/~borges/doc/Curvas%20Elipticas%20-%20Aplicacao%20em%20Criptografia%20Assimetrica.pdf)
* [Criptus | Aula 5: Criptografia de Curvas Elípticas (ECC)](https://youtu.be/AEVrpvaObdI)
