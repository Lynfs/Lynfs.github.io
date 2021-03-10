Quando falamos de exploração de algoritmos de criptografia, é comum imaginar-mos um certo nível de integridade, normalmente carregado de confiabilidade na segurança de determinados dados, entretanto, alguns desses métodos utilizados para criptografia de dados possuem fragilidade no algoritmo, e ao decorrer desta publicação, pretendo abordar um desses exemplos, passando por breves noções introdutórias à exploração que este sucederá, pretendendo, assim, demonstrar a importância da preocupação e compreensão (ainda que superficial) da proteção de dados.


## Compreendendo Algoritmos de Cripografia simétrica

Algoritmos de Criptografia simétrica são algoritmos que utlizam de uma mesma *Chave criptográfica* para encriptação e decriptação de um dado qualquer. Essa chave é um segredo compartilhado de maneira privada entre o emissor e o receptor de tal dado, sendo ela utilizada no processo inicial e no final.

E aí mora um dos principais problemas da criptografia de chave simétrica: ter como requisito que, tanto o emissor quanto o receptor da informação tenham acesso à uma mesma chave, na premissa de que essa será de conhecimento apenas desses supostos envolvidos, o que faz com que qualquer pessoa que intercepte essa chave criptográfica tenha acesso ao dado sensível original.

![ass](https://www.gta.ufrj.br/ensino/eel879/trabalhos_vf_2008_2/hugo/NotesImages/Topic12NotesImage1.jpg)


A criptografia simétrica possui dois tipos de cifras comuns para utilização.

As cifras de fluxo: Criptografam conteúdo gerando uma corrente de bits pseudoaleatória (chamados de fluxo de chave) que combinados com o conteúdo (seja ele um texto, uma imagem, um vídeo, etc.) gera o texto cifrado. Um exemplo de cifra de fluxo seria o RC4.


As cifras de bloco: Criptografam um conjunto de bytes de tamanho fixo, chamados de bloco. Mensagem que não são do tamanho do bloco ou de um múltiplo dele precisam ser ajustadas para isso preenchendo com mais dados (desnecessários) para que o tamanho da mensagem seja apropriado para o uso com uma cifra desse tipo. Um exemplo de cifra de bloco seria o AES.

## Compreendendo a Cifragem de Bloco e o Modo ECB

Em criptografia, uma cifra de bloco (block cypher) é um algoritmo determinístico que opera sobre agrupamentos de bits (blocos de dados) de tamanho fixo, normalmente 64 ou 128 bits, chamados de **blocos**, com uma transformação invariável que é especificada por uma chave simétrica. Um dado qualquer é dividido em blocos de tamanho fixo pelo algoritmo, o qual opera sobre cada bloco de maneira independente gerando um resultado cifrado **de mesmo tamanho B** cifrado por uma chave *K*.

No caso do Modo ECB (Eletronic codebook), nossa mensagem é dividida em blocos e cada bloco é criptografado **separadamente**.

 ![1](/assets/images/ecb/1.png)

A cifra opera sobre um bloco de texto claro de *n* bits para produzir um bloco de texto cifrado com o mesmo tamanho *n*, possuindo 2^n diferentes blocos de texto possíveis.

Para n = 4, teríamos:

[0000
0001
0010
...
1111]

Vejamos um exemplo de cifragem com determinada configuração para uma entrada n = 4 em [0,0,0,1].

 ![2](/assets/images/ecb/2.png)


E onde mora o problema? Na análise estatística! 

Suponha que essa nossa cifra com n = 4 foi utilizada para cifragem de uma cadeia de dados quaisquer em um sistema real;
Como o Tamanho do bloco é pequeno (4 bits) essa cifra estaria passiva à ataques de análise estatística do texto cifrado, uma vez que, caso o atacante capture o texto cifrado "0100", ele sabe que o texto claro utilizado nessa sequência de blocos é uma cadeia de 2^4, ou, de 16 possibilidades, sendo facilmente mapeável (claro, em tamanhos pequenos de n).

Outro problema que encontramos é a repetição quase inevitável. Se o mesmo bloco de texto claro se repetir, o texto cifrado resultante será sempre igual, o que pode gerar um padrão de repetição mapeável por um atacante.


## Compreendendo o Algoritmo AES

O algoritmo AES (Advanced Encryption Standard), também conhecida como Rijndael, é um algoritmo simétrico, subconjunto de cifra de bloco (vista acima) com tamanhos de bloco de 128 bits, mas comprimentos de chave: 128, 192 e 256 bits que opera sobre um arranjo bidimensional de bytes com 4x4 posições, denominado de **estado**. Para criptografar, cada turno do AES (exceto o último) consiste em quatro estágios:

### AddRoundKey

Na etapa de AddRoundKey, a sub-chave é combinada com o estado. Para cada turno, uma sub-chave é derivada da chave principal usando o escalonamento de chaves do Rijndael; cada sub-chave é do mesmo tamanho que o estado. A sub-chave é somada combinando cada byte do estado com o byte correspondente do sub-chave, utilizando a operação XOR bit a bit.

 ![3](/assets/images/ecb/3.png)

### Etapa de SubBytes

Na etapa de SubBytes, cada byte no arranjo é atualizado usando uma S-box de 8 bits. Para evitar os ataques baseados em propriedades algébricas simples, a S-box é construída combinando-se uma função inversora com uma transformação afim invertível. A S-box é escolhida também de forma a evitar qualquer ponto fixo. 

 ![4](/assets/images/ecb/4.png)

### ShiftRows

A etapa de ShiftRows opera sobre as linhas do estado, deslocando os bytes em cada linha de um determinado número de posições. No AES, a primeira linha fica inalterada. Cada byte da segunda linha é deslocado à esquerda de uma posição. Similarmente, a terceira e quarta fileiras são deslocadas de duas e de três posições respectivamente. Para o bloco de bits de tamanho 128 e 192 bits, o padrão de deslocamento é mesmo. Desta forma, cada coluna do estado ao fim da etapa de ShiftRows fica composta por bytes de todas as coluna do estado da entrada.

No caso de blocos de 256 bits, a primeira fileira fica inalterada, deslocando-se a segunda, terceira e quarta fileiras. O deslocamento é de 1 , 2 e 4 bytes respectivamente - embora esta mudança se aplique somente ao Rijndael quando usado com um bloco de 256 bits, o que não ocorre no AES.

 ![5](/assets/images/ecb/5.png)

### MixColumns

É uma operação de mescla que opera nas colunas do estado e combina os quatro bytes de cada coluna usando uma transformação linear. Os 4 bytes de cada coluna do estado são combinados usando uma transformação linear invertível, como uma multiplicação matricial, proporcionando uma difusão à cifra.


 ![6](/assets/images/ecb/6.png)


# Exemplo prático de ataque Bit-Flipping

Analizemos o código à seguir:

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

Percebe-se que o código utiliza do algoritmo **AES** visto acima para cifrar nossa "Flag", qual pode ser compreendida como qualquer outro dado sensível que não deveríamos ter acesso. O código realiza a "fusão" de uma entrada do usuário com a nossa Flag.

O Bit-flipping consiste em controlar o valor inicial de um dado (Texto claro) à partir do controle de determinados valores de entrada. Como assim?

```
    $retorno =  $data . $flag . str_repeat(chr($pad_len), $pad_len);
```

Recebe nosso valor "Data" e adiciona o valor "Pad"

```
    $pad_len = (16 - (strlen($data.$flag) % 16));
```

Qual podemos ver que é de 16 bytes por bloco. Lembra que comentamos da possibilidade do ataque de análise estatística? pois bem... Retomemos nosso exemplo inicial de 4 bits:

Suponhamos que nossa entrada seja a palavra "Stars". Sabendo que o nosso bloco imaginário é de tamanho 4, podemos arranjar nossa entrada + "flag" da seguinte maneira:

[s,t,a,r,s] + [f,l,a,g]. Perceba que nossa entrada possui um tamanho maior que 4. Nesse caso, o que aconteceria?

Teríamos um arranjo mais ou menos assim:

[s,t,a,r] [s,f,l,a] [g,x,x,x] onde X seria um caractere de deslocamento qualquer. Perceba que nossa string foi realocada para que assim pudesse se encaixar nos blocos de tamanho 4. Partindo desse princípio, o que aconteceria se, ao invés de passarmos um caractere, colocássemos um a menos?

[s,t,a] + [f,l,a,g] se converteria em [s,t,a,f] [l,a,g,x], e o que nós receberíamos seria um valor específico para o "valor que puxamos" (Ex. 123). Daí, você já deve imaginar o que precisa ser feito, mas caso ainda não tenha compreendido, permita-me explicar.


[s,t,a] + [f,l,a,g] se converteria em [s,t,a,f] [l,a,g,x], e esse retorno nos traria um valor cifrado '123321' referente à esse bloco. Sabendo desse resultado final, nossa entrada agora seria:

[s,t,a,f] [l,a,g,x] se converteria em [s,t,a,X] [f,l,a,g], onde, dessa vez, X seria um valor aleatório gerado para nosso ataque de força bruta que, levando em consideração o tamanho da chave e do bloco, não nos consumirá muito tempo. Para cada resultado de [s,t,a,X] + [f,l,a,g], seria teríamos um valor retornado, e assim faríamos uma comparação para saber se o valor cifrado retornado com nosso caractere randômico X é igual ao retorno cifrado de  [s,t,a,f] + [l,a,g,x]. Quando a comparação for verdadeira, descobrimos um caractere do texto original, e assim se fará sucessivamente até que tenhamos o "texto completo", qual, nesse caso, é o valor de "FLAG".

 ![7](/assets/images/ecb/7.png)

Ao enviarmos uma requisição vazia (text_to_encrypt sem parâmetros de entrada) e analizarmos o retorno com o xxd, podemos perceber que a saída traz um retorno de 32 caracteres (128 bits por bloco) começando em "4434" e terminando em 0a. Agora, realizamos entradas somando valores, nesse caso de 16 em 16. O nosso objetivo é visualizar esse mesmo retorno 4434...0a, pois assim, sabemos que nossa entrada "ocupou de maneira correta os blocos", vide que o retorno "completo/vazio" é o 4434...0a.

 ![8](/assets/images/ecb/8.png)

Perceba que, de 16 em 16 caracteres(nesse caso, zeros) somados à entrada, conseguimos retornar nosso retorno inicial 4434...0a. Ou seja, tudo acima desse offsset nos pertence, e tudo abaixo pertence à "FLAG" que queremos, e assim sabemos o tamanho exato do nosso bloco de entrada.

Agora, se removermos um "Zero" dos caracteres de entrada que enviamos, poderemos ver que, assim como no exemplo de star + flag, os endereços foram modificados dado que o bloco pertencente à flag teve um byte "puxado" para preencher o nosso "Zero" faltante. 

 ![9](/assets/images/ecb/9.png)

Agora, sabemos que nossa flag começa com a string "FLAG" (como visto no código php). Desse modo, o que devemos fazer é, sabendo que a primeira letra é F, mandamos a requisição com um F adicional. Se o retorno dos endereços com o F for igual ao anterior (Que, em tese, puxou o F para ocupar o "Zero" que removemos), podemos confirmar que o primeiro caracrere é a letra F. Claro, nesse caso, temos a informação prévia de que a string começa com a letra F, mas, caso não soubéssemos, bastaria realizar um ataque de força bruta de caracteres até que conseguíssimos o mesmo resultado.

 ![10](/assets/images/ecb/10.png)


Como podemos ver na imagem, o retorno foi exatamente o mesmo (coisa que não aconteceria se colocássemos um A, B, N, Y...), confirmando assim o primeiro caractere do nosso suposto "dado secreto qual não deveríamos ter acesso". 

E, assim, temos nosso ataque realizado, demonstrando a fragilidade que pode existir nas cifras de bloco em geral. Para ambientes reais, esse processo tende à ser automatizado com alguma linguagem de programação qualquer, tornando o processo incomparavelmente mais rápido e eficaz.

# Conclusões

Como vimos ao decorrer da publicação, o perigo das cifras de criptografia é, não só presente, como real, e compreender a existência desse tipo de ataque permite, não só o acréscimo do conhecimento técnico, como a compreensão de que os avanços da Ciência e da Tecnologia como um todo vêm como um benefício, demonstrando-nos que evoluir é sempre preciso, e que o investimento nos setores de segurança da informação permite que pesquisadores, matemáticos e qualquer outro colaborador científico some sempre mais e mais ao mundo cibernético, proporcionando experiências cada vez mais seguras e íntegras.


# Referências

- [TenableCTF - Netrunner Encryption, by Sql3t0](https://www.youtube.com/watch?v=JTgy1gYyaSY)

- [Block cipher mode of operation at WIKIPEDIA](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation)

- [Advanced Encryption Standard at WIKIPEDIA](https://pt.wikipedia.org/wiki/Advanced_Encryption_Standard)

- [Algoritmos de Chave simétrica at WIKIPEDIA](https://pt.wikipedia.org/wiki/Algoritmo_de_chave_sim%C3%A9trica)