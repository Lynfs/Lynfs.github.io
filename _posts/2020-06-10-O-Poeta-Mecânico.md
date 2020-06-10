
Poemas são expressões textuais. As vezes felizes, as vezes tristes; As vezes difíceis de compreender, as vezes tão simples quanto a mais simples das somas matemáticas. Poemas são, em geral, expressões sentimentais de alguém que prefere escrever à aderir qualquer outro meio de expressão artística.

O mundo está repleto de poemas, e poemas estão repletos de um único fator comum:  **Vida**. Por algum motivo, alguma mente humana os deu um início e um fim, com alguma finalidade, para que o possamos ler.

> “A vida é a respiração hipocondríaca do mundo no coração. Ela me apaga, nunca é só. Então o mundo… as estrelas… que tanto tem ruído às carnificinas que que julgamos tão pequenas: Linhas que nos levam à um caminho. tinha uma pedra no meio do caminho tinha uma pedra no meio do ser.”

**Ao ler esta poesia, qual sentimento te abraça?**

----------

Caso tenha reparado traços de algum(a) escritor nacional na poesia acima, você possui uma razão parcial: O texto foi escrito por uma Rede Neural(Manualmente Adaptada e corrigida gramaticalmente) criada em  **modelo sequencial**, utilizando  **entropia cruzada**  e  **memória de longo prazo**, tomando base em textos de escritores Brasileiros popularmente conhecidos.

“_Teria um poema gerado por um computador algum propósito? Um propósito além de o de ser gerado por um computador? Seriam as emoções transmitidas por ele reais? A randomicidade inerente à maquina tornaria a arte gerada por ela, a arte generativa, inferior ou menos sagrada_?”

# **Rede Neural Artificial? O que é isso?**

Redes neurais artificiais(**RNA**) são um conjunto de  **algoritmos matemáticos**, modelados vagamente por um  **cérebro humano**, projetados para  **identificar padrões**. Elas  **interpretam**  os dados sensoriais através de um tipo de  **percepção da máquina**, rotulando ou agrupando dados brutos. Os padrões que eles reconhecem são  **numéricos**, e podem ser perceptíveis à qualquer dado do mundo real, sejam imagens, sons ou textos. Redes neurais Artificiais são uma abstração da rede neural biológica. Seu objetivo não é replicar, mas sim servir de modelo para o aprendizado e resoluções de problemas complexos.

![](https://miro.medium.com/max/30/1*K3kwICoLdrxczie-tYz7jQ.jpeg?q=20)

![](https://miro.medium.com/max/393/1*K3kwICoLdrxczie-tYz7jQ.jpeg)

RNA model with math format

# Neurônios Artificiais? O que é isso?

Assim como no cérebro humano, os neurônios artificiais possuem a missão de transmitir informações.


![](https://miro.medium.com/max/828/1*lh5hiL8XKjMaWuWzauhnrQ.png)

**Imaginemos a seguinte situação**: Queremos realizar uma previsão de preço de uma casa que está sendo construida em uma região  **R**  e que temos intenção de comprar. Para isso, recolhemos dados de diversas casas que já existem na região, e estes dados são — Para fins de agilidade —  **Tamanho**  e  **preço**.

Portanto,  **y = f(x)**, o que significa que o preço da casa **(y)**  é a função do tamanho da casa  **(x).**  Vejamos em um gráfico:


![](https://miro.medium.com/max/1120/1*EpawrKOBRXp-cxbHDkah6A.png)

Agora, vamos desenhar uma linha reta usando esses dados no gráfico para visualizar a tendência dos dados.


![](https://miro.medium.com/max/1151/1*rNMcykuFM15g_12WR3IzAQ.png)

Esta linha reta representa a tendência do preço das casas em relação aos seus respectivos tamanhos. Agora, Poodemos encontrar,  **aproximadamente**, o preço de qualquer casa desta região colocando-a nesse gráfico linear.

E onde entram os neurônios? Bem, como acabamos de ver, o tamanho da casa é a  **entrada**  **da**  **função**  **(x)**  e o preço é a  **saída (y)**, e é aí que os  **neurônios artificiais**  aparecem.

![](https://miro.medium.com/max/478/1*HRXdKWUeMitbi5bludDxGg.jpeg)

Esse é o exemplo mais simples de uma rede neural, com uma única entrada, processamento e saída. Esse processamento, caso haja alguma dúvida, é feito por funções matemáticas.

Caso queiramos deixar as coisas mais interessantes, podemos adicionar outras variáveis de entrada, como  **tamanho da família**,  **cep**,  **número de quartos**,  **qualidade das escolas da redondeza**  e a c**lasse econômica da família**. O resultado da interligação destas novas variáveis, seria algo mais ou menos assim:

![](https://miro.medium.com/max/572/1*LOzkv1jwMh2JrKexkzJ7yg.png)

# **Entropia cruzada? O que é isso?**

**Entropia**  é a medida do  **grau de desordem**  de um sistema, sendo uma medida da  **indisponibilidade da energia**.

É uma grandeza física que está relacionada com a  **Mecânica Estatística**  e com a  **Segunda Lei da Termodinâmica**, e que tende a aumentar naturalmente no Universo. A “_desordem_” não deve ser compreendida como “_bagunça_” e sim como a forma de organização de sistema.

**_Exemplo_**:

**Cubos de gelo derretendo,**  antes de um cubo de gelo derreter, tem sua entropia menor, assim como seu  **grau de desordem**. Quando no estado sólido, as distâncias intermoleculares eram  **bem**-**definidas**, bem como os ângulos entre essas moléculas. Durante a  **fusão**  do gelo, ocorre um grande aumento na **multiplicidade de distâncias e ângulos**  entre as moléculas, o que caracteriza o estado líquido.

----------

Na computação não é diferente! A Entropia é uma medida da aleatoriedade de uma variável, e define a medida de “_falta de informação_”, mais precisamente o número de  **bits**  necessários, em média, para representar a informação  **em falta**.

Dado um conjunto  **S**, com instâncias pertencentes à classe  **I**, com probabilidade  **P(i)**  , temos:

![](https://miro.medium.com/max/442/1*WHf71OjjWUL8fkGG7rBXBg.png)

![](https://miro.medium.com/max/585/1*WUu58Q-zBTAJO5gSdHx2uA.png)

**S**  é o conjunto de exemplo de treino,  **p+**  é a porção de exemplos positivos, **p-**  é a porção de exemplos negativos. Temos  **Entropia(S) = 0**  se existe um  **i**  tal que  **P(i)**  = 1 e assumido que  **0 * log2 0 = 0.**

Ela especifica o número mínimo de  **bits**  de  **informação**  necessário para codificar uma classificação de um  **membro arbitrário**  de  **S**.

----------

Podemos classificar a  **Entropia-Cruzada**  como:

![](https://miro.medium.com/max/301/1*ImtWFEVxsg78A5vK40D89A.png)

Onde  **N**  é o número de amostras,  **M**  é o número de camadas,  **Sm**  é a dimensão de cada camada e  **Y**  são as saídas da rede.

# Poesia Artificial

Para nosso caso, utilizaremos uma base de dados(**arquivo de texto**) contendo diversas poesias. Caso você queira, pode utilizar seus escritos, ou os escritos de seus escritores prediletos. Quanto maior a quantidade e diversidade, melhor!

# Keras

Às palavras do próprio google, a Keras é uma biblioteca de rede neural de código aberto escrita em  **Python**(Linguagem de programação). É capaz de rodar sobre  **TensorFlow**,  **Microsoft Cognitive Toolkit**,  **R**,  **Theano**  ou  **PlaidML**. Projetado para permitir experimentação rápida com redes neurais profundas, ele se concentra em ser fácil de usar, modular e extensível, e ela será utilizada para criação do algoritmo.

```
from tensorflow.keras.preprocessing.sequence import pad_sequences  
from tensorflow.keras.layers import Embedding, LSTM, Dense, Dropout, Bidirectional  
from tensorflow.keras.preprocessing.text import Tokenizer  
from tensorflow.keras.models import Sequential  
from tensorflow.keras.optimizers import Adam  
from tensorflow.keras import regularizers  
import tensorflow.keras.utils as ku
```

# Tokenizing

Então, o  **tokenizer**  basicamente permite a gente vetorizar todas as palavras dos poemas em nosso enorme  **corpus**  (arquivo de texto) em uma sequência de números inteiros ou vetores, o que pode ser útil para inserir na rede neural.

```
tokenizer = Tokenizer()  
data = open(‘poesias.txt’,encoding=”utf8").read()  
corpus = data.lower().split(“\n”)  
tokenizer.fit_on_texts(corpus)  
total_words = len(tokenizer.word_index) + 1**
```

Para um entendimento aprofundado do procedimento de tokenizing, eu recomendo este  [vídeo](https://www.youtube.com/watch?v=AcLKZg-GvxA).

Após vetorizadas, essas serão as entradas dos nossos  **Neurônios Artificiais,** No qual esperamos, na saída final, **Uma Poesia.**

# Modelo de Construção

O método sequencial no  **Keras**  é o mais simples para criar um empilhamento de arquitetura de camadas, e é ele que nós vamos usar. O modelo sequencial nos permite inserir camadas de uma  **Rede Neural Artificial**  em série, onde a  **saída**  da primeira camada serve como  **entrada**  da segunda, e assim por diante, como visto acima no exemplo das casas com múltiplas variáveis.

Também usaremos  **entropia cruzada categórica**  como função de perda e  **Adam**  ([Adaptive Moment Estimation](http://sebastianruder.com/optimizing-gradient-descent/index.html#adam)  )como otimizador.

  ```
model = Sequential()  
model.add(Embedding(total_words, 100, input_length=max_sequence_len-1))  
model.add(Bidirectional(LSTM(150, return_sequences = True)))  
model.add(Dropout(0.2))  
model.add(LSTM(100))  
model.add(Dense(total_words/2, activation=’relu’, kernel_regularizer=regularizers.l2(0.01)))  
model.add(Dense(total_words, activation=’softmax’))  
model.compile(loss=’categorical_crossentropy’, optimizer=’adam’, metrics=[‘accuracy’])  
print(model.summary())
```


Depois disso, adicionamos a **camada bidirecional** [LSTM](https://colah.github.io/posts/2015-08-Understanding-LSTMs/)(_Long short-term memory)_ que deixa a arquitetura mais ou menos assim:

![](https://miro.medium.com/max/395/1*hDwEVtQqqiPtzyJrACbGTQ.jpeg)

E então, configuramos uma parte mais “visual”:

```
plt.plot(epochs, acc, ‘b’, label=’Training accuracy’)  
plt.title(‘Training accuracy’)  
plt.figure()  
plt.plot(epochs, loss, ‘b’, label=’Training Loss’)  
plt.title(‘Training loss’)  
plt.legend()  
plt.show()
```


Essa parte nos ajuda à visualizar, graficamente, o desempenho da nossa  **Rede Neural Artificial**, para saber se esta flui com bom desempenho no aprendizado.

# Hora de Redigir!

```
seed_text = “A vida é uma”  
next_words = 50  
   
for _ in range(next_words):  
 token_list = tokenizer.texts_to_sequences([seed_text])[0]  
 token_list = pad_sequences([token_list], maxlen=max_sequence_len-1, padding=’pre’)  
 predicted = model.predict_classes(token_list, verbose=0)  
 output_word = “”  
 for word, index in tokenizer.word_index.items():  
 if index == predicted:  
 output_word = word  
 break  
 seed_text += “ “ + output_word  
print(seed_text)
```


“**seed_text**” é um “_gatilho_” para nossa Rede Neural. É com base nela que tudo vai começar à ser escrito. E “**next_words**” serve para especificar quantas palavras deseja que ele escreva. E, após tudo isso, basta rodar o código e ver nosso  **Poeta Mecânico**  agir. E então, cabe a você julgar a saída e interpretar, sentimentalmente ou não, a expressão de uma máquina.

Além da saída feita com escritores nacionais, esse é um exemplo de saída de testes com poemas feitos por escritores internacionais:

> What is this life converge in a late to depart blue years day a moon in little years putting putting i was blue without my victories day a ragtag bell without day a moon to our cache years the letters snaps blue day a moon day day they were never best friends into the

Lembrando que  **o objetivo não é a perfeição**, e pode não ter tanto sentido nas primeiras tentativas. Mas, nada que tempo e ajustes manuais de gramática e pontuação não resolvam. Claro, também é possível corrigir os próprios erros utilizando Redes Neurais Artificiais, mas essa eu vou deixar para um próximo dia.

O código completo pode ser encontrado  [aqui](https://github.com/agrawalparth08/poetry-generation-lstm/blob/master/Poetry_Generation.ipynb), e uma explicação aprofundada no primeiro link nas referências.

Para fins de estudo e diversão, recomendo a publicação de  [caioluders](https://medium.com/caio-noobs-around/p3ss04-utilizando-cadeias-de-markov-e-algoritmo-gen%C3%A9tico-para-a-gera%C3%A7%C3%A3o-de-poemas-generativos-caeed388dd0a)  onde ele utiliza um método alternativo e super interessante para escrever poesias. Saca lá!

## Referências:

TOWARDSDC —  [CREATING POEMS FROM ONE OWN POEMS](https://towardsdatascience.com/creating-poems-from-ones-own-poems-neural-networks-and-life-paradoxes-a9cffd2b07e3)

TOWARDSDC — [WHAT’S A NEURAL NETWORK?](https://towardsdatascience.com/creating-poems-from-ones-own-poems-neural-networks-and-life-paradoxes-a9cffd2b07e3)

REDES NEURAIS ARTIFICIAIS — CAPITULO2.PDF
