# o poeta mecanico

Poemas sao expressoes textuais. As vezes felizes, as vezes tristes; As vezes dificeis de compreender, as vezes tao simples quanto a mais simples das somas matematicas. Poemas sao, em geral, expressoes sentimentais de alguem que prefere escrever à aderir qualquer outro meio de expressao artistica.  
O mundo esta repleto de poemas, e poemas estao repletos de um unico fator comum:  **Vida**. Por algum motivo, alguma mente humana os deu um inicio e um fim, com alguma finalidade, para que o possamos ler.  

> “A vida e a respiracao hipocondriaca do mundo no coracao. Ela me apaga, nunca e so. Entao o mundo… as estrelas… que tanto tem ruido às carnificinas que que julgamos tao pequenas: Linhas que nos levam à um caminho. tinha uma pedra no meio do caminho tinha uma pedra no meio do ser.”  

**Ao ler esta poesia, qual sentimento te abraca?**  


----------  
Caso tenha reparado tracos de algum(a) escritor nacional na poesia acima, voce possui uma razao parcial: O texto foi escrito por uma Rede Neural(Manualmente Adaptada e corrigida gramaticalmente) criada em  **modelo sequencial**, utilizando  **entropia cruzada**  e  **memoria de longo prazo**, tomando base em textos de escritores Brasileiros popularmente conhecidos.  
“_Teria um poema gerado por um computador algum proposito? Um proposito alem de o de ser gerado por um computador? Seriam as emocoes transmitidas por ele reais? A randomicidade inerente à maquina tornaria a arte gerada por ela, a arte generativa, inferior ou menos sagrada_?”  

# **Rede Neural Artificial? O que e isso?**  


Redes neurais artificiais(**RNA**) sao um conjunto de  **algoritmos matematicos**, modelados vagamente por um  **cerebro humano**, projetados para  **identificar padroes**. Elas  **interpretam**  os dados sensoriais atraves de um tipo de  **percepcao da maquina**, rotulando ou agrupando dados brutos. Os padroes que eles reconhecem sao  **numericos**, e podem ser perceptiveis à qualquer dado do mundo real, sejam imagens, sons ou textos. Redes neurais Artificiais sao uma abstracao da rede neural biologica. Seu objetivo nao e replicar, mas sim servir de modelo para o aprendizado e resolucoes de problemas complexos.  


![](https://miro.medium.com/max/393/1*K3kwICoLdrxczie-tYz7jQ.jpeg)  


RNA model with math format  


# Neuronios Artificiais? O que e isso?  


Assim como no cerebro humano, os neuronios artificiais possuem a missao de transmitir informacoes.  


![](https://miro.medium.com/max/828/1*lh5hiL8XKjMaWuWzauhnrQ.png)  


**Imaginemos a seguinte situacao**: Queremos realizar uma previsao de preco de uma casa que esta sendo construida em uma regiao  **R**  e que temos intencao de comprar. Para isso, recolhemos dados de diversas casas que ja existem na regiao, e estes dados sao — Para fins de agilidade —  **Tamanho**  e  **preco**.  
Portanto,  **y = f(x)**, o que significa que o preco da casa **(y)**  e a funcao do tamanho da casa  **(x).**  Vejamos em um grafico:  


![](https://miro.medium.com/max/1120/1*EpawrKOBRXp-cxbHDkah6A.png)  


Agora, vamos desenhar uma linha reta usando esses dados no grafico para visualizar a tendencia dos dados.  

![](https://miro.medium.com/max/1151/1*rNMcykuFM15g_12WR3IzAQ.png)


Esta linha reta representa a tendencia do preco das casas em relacao aos seus respectivos tamanhos. Agora, Poodemos encontrar,  **aproximadamente**, o preco de qualquer casa desta regiao colocando-a nesse grafico linear.  
E onde entram os neuronios? Bem, como acabamos de ver, o tamanho da casa e a  **entrada**  **da**  **funcao**  **(x)**  e o preco e a  **saida (y)**, e e ai que os  **neuronios artificiais**  aparecem.  
![](https://miro.medium.com/max/478/1*HRXdKWUeMitbi5bludDxGg.jpeg)


Esse e o exemplo mais simples de uma rede neural, com uma unica entrada, processamento e saida. Esse processamento, caso haja alguma duvida, e feito por funcoes matematicas.  
Caso queiramos deixar as coisas mais interessantes, podemos adicionar outras variaveis de entrada, como  **tamanho da familia**,  **cep**,  **numero de quartos**,  **qualidade das escolas da redondeza**  e a **classe economica da familia**. O resultado da interligacao destas novas variaveis, seria algo mais ou menos assim: 


![](https://miro.medium.com/max/572/1*LOzkv1jwMh2JrKexkzJ7yg.png)


# **Entropia cruzada? O que e isso?**


**Entropia**  e a medida do  **grau de desordem**  de um sistema, sendo uma medida da  **indisponibilidade da energia**.  
e uma grandeza fisica que esta relacionada com a  **Mecanica Estatistica**  e com a  **Segunda Lei da Termodinamica**, e que tende a aumentar naturalmente no Universo. A “_desordem_” nao deve ser compreendida como “_bagunca_” e sim como a forma de organizacao de sistema.  
**_Exemplo_**:  
**Cubos de gelo derretendo,**  antes de um cubo de gelo derreter, tem sua entropia menor, assim como seu  **grau de desordem**. Quando no estado solido, as distancias intermoleculares eram  **bem**-**definidas**, bem como os angulos entre essas moleculas. Durante a  **fusao**  do gelo, ocorre um grande aumento na **multiplicidade de distancias e angulos**  entre as moleculas, o que caracteriza o estado liquido.  
----------  
Na computacao nao e diferente! A Entropia e uma medida da aleatoriedade de uma variavel, e define a medida de “_falta de informacao_”, mais precisamente o numero de  **bits**  necessarios, em media, para representar a informacao  **em falta**.  
Dado um conjunto  **S**, com instancias pertencentes à classe  **I**, com probabilidade  **P(i)**  , temos:  
![](https://miro.medium.com/max/442/1*WHf71OjjWUL8fkGG7rBXBg.png)  
![](https://miro.medium.com/max/585/1*WUu58Q-zBTAJO5gSdHx2uA.png)  
**S**  e o conjunto de exemplo de treino,  **p+**  e a porcao de exemplos positivos, **p-**  e a porcao de exemplos negativos. Temos  **Entropia(S) = 0**  se existe um  **i**  tal que  **P(i)**  = 1 e assumido que  **0 * log2 0 = 0.**  
Ela especifica o numero minimo de  **bits**  de  **informacao**  necessario para codificar uma classificacao de um  **membro arbitrario**  de  **S**.  
----------  
Podemos classificar a  **Entropia-Cruzada**  como:  
![](https://miro.medium.com/max/301/1*ImtWFEVxsg78A5vK40D89A.png)  
Onde  **N**  e o numero de amostras,  **M**  e o numero de camadas,  **Sm**  e a dimensao de cada camada e  **Y**  sao as saidas da rede.  
# Poesia Artificial  
Para nosso caso, utilizaremos uma base de dados(**arquivo de texto**) contendo diversas poesias. Caso voce queira, pode utilizar seus escritos, ou os escritos de seus escritores prediletos. Quanto maior a quantidade e diversidade, melhor!  
# Keras  
Às palavras do proprio google, a Keras e uma biblioteca de rede neural de codigo aberto escrita em  **Python**(Linguagem de programacao). e capaz de rodar sobre  **TensorFlow**,  **Microsoft Cognitive Toolkit**,  **R**,  **Theano**  ou  **PlaidML**. Projetado para permitir experimentacao rapida com redes neurais profundas, ele se concentra em ser facil de usar, modular e extensivel, e ela sera utilizada para criacao do algoritmo.  
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
Entao, o  **tokenizer**  basicamente permite a gente vetorizar todas as palavras dos poemas em nosso enorme  **corpus**  (arquivo de texto) em uma sequencia de numeros inteiros ou vetores, o que pode ser util para inserir na rede neural.  
```
tokenizer = Tokenizer()  
data = open(‘poesias.txt’,encoding=”utf8").read()  
corpus = data.lower().split(“\n”)  
tokenizer.fit_on_texts(corpus)  
total_words = len(tokenizer.word_index) + 1**
```  
Para um entendimento aprofundado do procedimento de tokenizing, eu recomendo este  [video](https://www.youtube.com/watch?v=AcLKZg-GvxA).  
Apos vetorizadas, essas serao as entradas dos nossos  **Neuronios Artificiais,** No qual esperamos, na saida final, **Uma Poesia.**  
# Modelo de Construcao  
O metodo sequencial no  **Keras**  e o mais simples para criar um empilhamento de arquitetura de camadas, e e ele que nos vamos usar. O modelo sequencial nos permite inserir camadas de uma  **Rede Neural Artificial**  em serie, onde a  **saida**  da primeira camada serve como  **entrada**  da segunda, e assim por diante, como visto acima no exemplo das casas com multiplas variaveis.  
Tambem usaremos  **entropia cruzada categorica**  como funcao de perda e  **Adam**  ([Adaptive Moment Estimation](http://sebastianruder.com/optimizing-gradient-descent/index.html#adam)  )como otimizador.  
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
E entao, configuramos uma parte mais “visual”:  
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
seed_text = “A vida e uma”  
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


“**seed_text**” e um “_gatilho_” para nossa Rede Neural. e com base nela que tudo vai comecar à ser escrito. E “**next_words**” serve para especificar quantas palavras deseja que ele escreva. E, apos tudo isso, basta rodar o codigo e ver nosso  **Poeta Mecanico**  agir. E entao, cabe a voce julgar a saida e interpretar, sentimentalmente ou nao, a expressao de uma maquina.


Alem da saida feita com escritores nacionais, esse e um exemplo de saida de testes com poemas feitos por escritores internacionais:


> What is this life converge in a late to depart blue years day a moon in little years putting putting i was blue without my victories day a ragtag bell without day a moon to our cache years the letters snaps blue day a moon day day they were never best friends into the


Lembrando que

**o objetivo nao e a perfeicao**, e pode nao ter tanto sentido nas primeiras tentativas. Mas, nada que tempo e ajustes manuais de gramatica e pontuacao nao resolvam. Claro, tambem e possivel corrigir os proprios erros utilizando Redes Neurais Artificiais, mas essa eu vou deixar para um proximo dia.


O codigo completo pode ser encontrado [aqui](https://github.com/agrawalparth08/poetry-generation-lstm/blob/master/Poetry_Generation.ipynb), e uma explicacao aprofundada no primeiro link nas referencias.  
Para fins de estudo e diversao, recomendo a publicacao de  [caioluders](https://medium.com/caio-noobs-around/p3ss04-utilizando-cadeias-de-markov-e-algoritmo-gen%C3%A9tico-para-a-gera%C3%A7%C3%A3o-de-poemas-generativos-caeed388dd0a)  onde ele utiliza um metodo alternativo e super interessante para escrever poesias. Saca la!


## Referencias: 


TOWARDSDC —  [CREATING POEMS FROM ONE OWN POEMS](https://towardsdatascience.com/creating-poems-from-ones-own-poems-neural-networks-and-life-paradoxes-a9cffd2b07e3)  
TOWARDSDC — [WHAT’S A NEURAL NETWORK?](https://towardsdatascience.com/creating-poems-from-ones-own-poems-neural-networks-and-life-paradoxes-a9cffd2b07e3)  
REDES NEURAIS ARTIFICIAIS — CAPITULO2.PDF  