# compreendendo e explorando agoritmos de criptografia simetrica e algoritmos de bloco

Quando falamos de exploracão de algoritmos de criptografia, e comum imaginarmos um certo nivel de integridade, normalmente carregado de confiabilidade na seguranca de determinados dados, entretanto, alguns desses metodos utilizados para criptografia de dados possuem fragilidade no algoritmo, e ao decorrer desta publicacão, pretendo abordar um desses exemplos, passando por breves nocoes introdutorias a exploracão que este sucedera, pretendendo, assim, demonstrar a importancia da preocupacão e compreensão (ainda que superficial) da protecão de dados.


## Compreendendo Algoritmos de Cripografia simetrica

Algoritmos de Criptografia simetrica são algoritmos que utlizam de uma mesma *Chave criptografica* para encriptacão e decriptacão de um dado qualquer. Essa chave e um segredo compartilhado de maneira privada entre o emissor e o receptor de tal dado, sendo ela utilizada no processo inicial e no final.

E ai mora um dos principais problemas da criptografia de chave simetrica: ter como requisito que, tanto o emissor quanto o receptor da informacão tenham acesso a uma mesma chave, na premissa de que essa sera de conhecimento apenas desses supostos envolvidos, o que faz com que qualquer pessoa que intercepte essa chave criptografica tenha acesso ao dado sensivel original.

![ass](https://www.gta.ufrj.br/ensino/eel879/trabalhos_vf_2008_2/hugo/NotesImages/Topic12NotesImage1.jpg)


A criptografia simetrica possui dois tipos de cifras comuns para utilizacão.

As cifras de fluxo: Criptografam conteudo gerando uma corrente de bits pseudoaleatoria (chamados de fluxo de chave) que combinados com o conteudo (seja ele um texto, uma imagem, um video, etc.) gera o texto cifrado. Um exemplo de cifra de fluxo seria o RC4.


As cifras de bloco: Criptografam um conjunto de bytes de tamanho fixo, chamados de bloco. Mensagem que não são do tamanho do bloco ou de um multiplo dele precisam ser ajustadas para isso preenchendo com mais dados (desnecessarios) para que o tamanho da mensagem seja apropriado para o uso com uma cifra desse tipo. Um exemplo de cifra de bloco seria o AES.

## Compreendendo a Cifragem de Bloco e o Modo ECB

Em criptografia, uma cifra de bloco (block cypher) e um algoritmo deterministico que opera sobre agrupamentos de bits (blocos de dados) de tamanho fixo, normalmente 64 ou 128 bits, chamados de **blocos**, com uma transformacão invariavel que e especificada por uma chave simetrica. Um dado qualquer e dividido em blocos de tamanho fixo pelo algoritmo, o qual opera sobre cada bloco de maneira independente gerando um resultado cifrado **de mesmo tamanho B** cifrado por uma chave *K*.

No caso do Modo ECB (Eletronic codebook), nossa mensagem e dividida em blocos e cada bloco e criptografado **separadamente**.

 ![1](/assets/images/ecb/1.png)

A cifra opera sobre um bloco de texto claro de *n* bits para produzir um bloco de texto cifrado com o mesmo tamanho *n*, possuindo 2^n diferentes blocos de texto possiveis.

Para n = 4, teriamos:

[0000
0001
0010
...
1111]

Vejamos um exemplo de cifragem com determinada configuracão para uma entrada n = 4 em [0,0,0,1].

 ![2](/assets/images/ecb/2.png)


E onde mora o problema? Na analise estatistica! 

Suponha que essa nossa cifra com n = 4 foi utilizada para cifragem de uma cadeia de dados quaisquer em um sistema real;
Como o Tamanho do bloco e pequeno (4 bits) essa cifra estaria passiva a ataques de analise estatistica do texto cifrado, uma vez que, caso o atacante capture o texto cifrado "0100", ele sabe que o texto claro utilizado nessa sequencia de blocos e uma cadeia de 2^4, ou, de 16 possibilidades, sendo facilmente mapeavel (claro, em tamanhos pequenos de n).

Outro problema que encontramos e a repeticão quase inevitavel. Se o mesmo bloco de texto claro se repetir, o texto cifrado resultante sera sempre igual, o que pode gerar um padrão de repeticão mapeavel por um atacante.


## Compreendendo o Algoritmo AES

O algoritmo AES (Advanced Encryption Standard), tambem conhecida como Rijndael, e um algoritmo simetrico, subconjunto de cifra de bloco (vista acima) com tamanhos de bloco de 128 bits, mas comprimentos de chave: 128, 192 e 256 bits que opera sobre um arranjo bidimensional de bytes com 4x4 posicoes, denominado de **estado**. Para criptografar, cada turno do AES (exceto o ultimo) consiste em quatro estagios:

### AddRoundKey

Na etapa de AddRoundKey, a sub-chave e combinada com o estado. Para cada turno, uma sub-chave e derivada da chave principal usando o escalonamento de chaves do Rijndael; cada sub-chave e do mesmo tamanho que o estado. A sub-chave e somada combinando cada byte do estado com o byte correspondente do sub-chave, utilizando a operacão XOR bit a bit.

 ![3](/assets/images/ecb/3.png)

### Etapa de SubBytes

Na etapa de SubBytes, cada byte no arranjo e atualizado usando uma S-box de 8 bits. Para evitar os ataques baseados em propriedades algebricas simples, a S-box e construida combinando-se uma funcão inversora com uma transformacão afim invertivel. A S-box e escolhida tambem de forma a evitar qualquer ponto fixo. 

 ![4](/assets/images/ecb/4.png)

### ShiftRows

A etapa de ShiftRows opera sobre as linhas do estado, deslocando os bytes em cada linha de um determinado numero de posicoes. No AES, a primeira linha fica inalterada. Cada byte da segunda linha e deslocado a esquerda de uma posicão. Similarmente, a terceira e quarta fileiras são deslocadas de duas e de tres posicoes respectivamente. Para o bloco de bits de tamanho 128 e 192 bits, o padrão de deslocamento e mesmo. Desta forma, cada coluna do estado ao fim da etapa de ShiftRows fica composta por bytes de todas as coluna do estado da entrada.

No caso de blocos de 256 bits, a primeira fileira fica inalterada, deslocando-se a segunda, terceira e quarta fileiras. O deslocamento e de 1 , 2 e 4 bytes respectivamente - embora esta mudanca se aplique somente ao Rijndael quando usado com um bloco de 256 bits, o que não ocorre no AES.

 ![5](/assets/images/ecb/5.png)

### MixColumns

e uma operacão de mescla que opera nas colunas do estado e combina os quatro bytes de cada coluna usando uma transformacão linear. Os 4 bytes de cada coluna do estado são combinados usando uma transformacão linear invertivel, como uma multiplicacão matricial, proporcionando uma difusão a cifra.


 ![6](/assets/images/ecb/6.png)


# Exemplo pratico de ataque Bit-Flipping

Analizemos o codigo a seguir:

```
<html>
<body>
  <h1>Netrunner Encryption Tool</h1>
  <a href="netrun.txt">Source Code</a>
  <form method=post action="crypto.php">
  <input type=text name="text_to_encrypt">
  <input type="submit" name="do_encrypt" value="Encrypt">
  </form>

<?php

function pad_data($data)
{
    $flag = "flag{wouldnt_y0u_lik3_to_know__}"; 
    
    $pad_len = (16 - (strlen($data.$flag) % 16));
    $retorno =  $data . $flag . str_repeat(chr($pad_len), $pad_len);
    echo '$pad_len='.$pad_len;
    echo '$data='.$data."\n";
    echo '$retorno='.$retorno."\n";
    return $retorno;
}

if(isset($_POST["do_encrypt"]))
{
  $cipher = "aes-128-ecb";
  $iv  = hex2bin('00000000000000000000000000000000');
  $key = hex2bin('74657374696E676B6579313233343536');
  echo "</br><br><h2>Encrypted Data:</h2>";
  $ciphertext = pad_data($_POST['text_to_encrypt']);
  $ciphertext = openssl_encrypt(pad_data($_POST['text_to_encrypt']), $cipher, $key, 0, $iv); 

  echo "<br/>";
  echo "<b>$ciphertext</b>";
}
?>
</body>
```

Percebe-se que o codigo utiliza do algoritmo **AES** visto acima para cifrar nossa "Flag", qual pode ser compreendida como qualquer outro dado sensivel que não deveriamos ter acesso. O codigo realiza a "fusão" de uma entrada do usuario com a nossa Flag.

O Bit-flipping consiste em controlar o valor inicial de um dado (Texto claro) a partir do controle de determinados valores de entrada. Como assim?

```
    $retorno =  $data . $flag . str_repeat(chr($pad_len), $pad_len);
```

Recebe nosso valor "Data" e adiciona o valor "Pad"

```
    $pad_len = (16 - (strlen($data.$flag) % 16));
```

Qual podemos ver que e de 16 bytes por bloco. Lembra que comentamos da possibilidade do ataque de analise estatistica? pois bem... Retomemos nosso exemplo inicial de 4 bits:

Suponhamos que nossa entrada seja a palavra "Stars". Sabendo que o nosso bloco imaginario e de tamanho 4, podemos arranjar nossa entrada + "flag" da seguinte maneira:

[s,t,a,r,s] + [f,l,a,g]. Perceba que nossa entrada possui um tamanho maior que 4. Nesse caso, o que aconteceria?

Teriamos um arranjo mais ou menos assim:

[s,t,a,r] [s,f,l,a] [g,x,x,x] onde X seria um caractere de deslocamento qualquer. Perceba que nossa string foi realocada para que assim pudesse se encaixar nos blocos de tamanho 4. Partindo desse principio, o que aconteceria se, ao inves de passarmos um caractere, colocassemos um a menos?

[s,t,a] + [f,l,a,g] se converteria em [s,t,a,f] [l,a,g,x], e o que nos receberiamos seria um valor especifico para o "valor que puxamos" (Ex. 123). Dai, voce ja deve imaginar o que precisa ser feito, mas caso ainda não tenha compreendido, permita-me explicar.


[s,t,a] + [f,l,a,g] se converteria em [s,t,a,f] [l,a,g,x], e esse retorno nos traria um valor cifrado '123321' referente a esse bloco. Sabendo desse resultado final, nossa entrada agora seria:

[s,t,a,f] [l,a,g,x] se converteria em [s,t,a,X] [f,l,a,g], onde, dessa vez, X seria um valor aleatorio gerado para nosso ataque de forca bruta que, levando em consideracão o tamanho da chave e do bloco, não nos consumira muito tempo. Para cada resultado de [s,t,a,X] + [f,l,a,g], seria teriamos um valor retornado, e assim fariamos uma comparacão para saber se o valor cifrado retornado com nosso caractere randomico X e igual ao retorno cifrado de  [s,t,a,f] + [l,a,g,x]. Quando a comparacão for verdadeira, descobrimos um caractere do texto original, e assim se fara sucessivamente ate que tenhamos o "texto completo", qual, nesse caso, e o valor de "FLAG".

 ![7](/assets/images/ecb/7.png)

Ao enviarmos uma requisicão vazia (text_to_encrypt sem parametros de entrada) e analizarmos o retorno com o xxd, podemos perceber que a saida traz um retorno de 32 caracteres (128 bits por bloco) comecando em "4434" e terminando em 0a. Agora, realizamos entradas somando valores, nesse caso de 16 em 16. O nosso objetivo e visualizar esse mesmo retorno 4434...0a, pois assim, sabemos que nossa entrada "ocupou de maneira correta os blocos", vide que o retorno "completo/vazio" e o 4434...0a.

 ![8](/assets/images/ecb/8.png)

Perceba que, de 16 em 16 caracteres(nesse caso, zeros) somados a entrada, conseguimos retornar nosso retorno inicial 4434...0a. Ou seja, tudo acima desse offsset nos pertence, e tudo abaixo pertence a "FLAG" que queremos, e assim sabemos o tamanho exato do nosso bloco de entrada.

Agora, se removermos um "Zero" dos caracteres de entrada que enviamos, poderemos ver que, assim como no exemplo de star + flag, os enderecos foram modificados dado que o bloco pertencente a flag teve um byte "puxado" para preencher o nosso "Zero" faltante. 

 ![9](/assets/images/ecb/9.png)

Agora, sabemos que nossa flag comeca com a string "FLAG" (como visto no codigo php). Desse modo, o que devemos fazer e, sabendo que a primeira letra e F, mandamos a requisicão com um F adicional. Se o retorno dos enderecos com o F for igual ao anterior (Que, em tese, puxou o F para ocupar o "Zero" que removemos), podemos confirmar que o primeiro caractere e a letra F. Claro, nesse caso, temos a informacão previa de que a string comeca com a letra F, mas, caso não soubessemos, bastaria realizar um ataque de forca bruta de caracteres ate que conseguissimos o mesmo resultado.

 ![10](/assets/images/ecb/10.png)


Como podemos ver na imagem, o retorno foi exatamente o mesmo (coisa que não aconteceria se colocassemos um A, B, N, Y...), confirmando assim o primeiro caractere do nosso suposto "dado secreto qual não deveriamos ter acesso". 

E, assim, temos nosso ataque realizado, demonstrando a fragilidade que pode existir nas cifras de bloco em geral. Para ambientes reais, esse processo tende a ser automatizado com alguma linguagem de programacão qualquer, tornando o processo incomparavelmente mais rapido e eficaz.

# Conclusoes

Como vimos ao decorrer da publicacão, o perigo das cifras de criptografia e, não so presente, como real, e compreender a existencia desse tipo de ataque permite, não so o acrescimo do conhecimento tecnico, como a compreensão de que os avancos da Ciencia e da Tecnologia como um todo vem como um beneficio, demonstrando-nos que evoluir e sempre preciso, e que o investimento nos setores de seguranca da informacão permite que pesquisadores, matematicos e qualquer outro colaborador cientifico some sempre mais e mais ao mundo cibernetico, proporcionando experiencias cada vez mais seguras e integras.


# Referencias

- [TenableCTF - Netrunner Encryption, by Sql3t0](https://www.youtube.com/watch?v=JTgy1gYyaSY)

- [Block cipher mode of operation at WIKIPEDIA](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation)

- [Advanced Encryption Standard at WIKIPEDIA](https://pt.wikipedia.org/wiki/Advanced_Encryption_Standard)

- [Algoritmos de Chave simetrica at WIKIPEDIA](https://pt.wikipedia.org/wiki/Algoritmo_de_chave_sim%C3%A9trica)