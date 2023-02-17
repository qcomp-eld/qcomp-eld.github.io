---
layout: post
title:  "QML 01: Data Encoding"
date:   2023-02-06 10:00:00
author: luccas_marim
categories: teoria computacao-quântica educativo
usemathjax: true
excerpt_separator: <!--more-->
---

Para utilizarmos modelos de Machine Learning, precisamos primeiro representar os dados em inputs numéricos para o nosso sistema interpretá-los. Em Quantum Machine Learning (QML) também passamos por esse processo e aqui teremos um problema mais fundamental visto que estamos transformando, a príncipio, representações de objetos clássicos em quânticos. O nome mais popular na literatura deste processo é Data Encoding ou Data Embedding. 

<!--more-->

Conforme [ref.IV], este processo é uma parte crítica dos algoritmos de QML e afeta diretamente seu poder computacional.
Neste artigo iremos estudar como esse processo é feito atualmente com base em nossas referências. Estudaremos os três métodos mais famosos de Data Encoding: Basis Encoding, Amplitude Encoding e Angle Encoding bem como exemplos ou direcionamentos de implementação. Para tanto, utilizaremos os frameworks fornecidos pela IBM Qiskit, Baidu Paddle Quantum e Xanadu PennyLane.
O processo de Data Encoding geralmente é intuitivo e simples mas quando implementamos é um pouco diferente. Nossos dispositivos quânticos atuais têm quantidade limitada de qubits e estes são estáveis por curtos períodos de tempo. Além disso, atualmente não há consenso em dizer qual dos métodos estudados nesse artigo é o melhor. A escolha entre métodos depende do problema em questão conforme [ref.I].

### Introdução
Antes de tudo, vamos definir nossa base inicial de dados. Representaremos este pelo vetor de vetores $$ H $$ que tem $$ M $$ amostras de tamanho $$ N $$:

$$
H = \left\{x^1,...,x^m,...,x^M\right\}
$$

Onde $$x^m$$ é um vetor N dimensional que representa um input com $$m=1,...,M$$.
De forma mais visual:

$$
x^1 = (x^1_1 , x^1_2, ..., x^1_N)
$$

$$
x^2 = (x^2_1 , x^2_2, ..., x^2_N)
$$

$$
...
$$

$$
x^M = (x^M_1 , x^M_2, ..., x^M_N)
$$


---
***Observação***
Para fins didáticos (wlog), fixaremos $$H_{exemplo}$$ com valores quaisquer e aplicaremos os métodos em partes dessa matriz. 
Seja $$H_{exemplo}$$ a matriz de informação de entrada com $$N=2$$ e $$M=4$$ amostras abaixo:

$$
H_{exemplo} = 
\begin{bmatrix}
5&4&3&2 \\
6&1&1&3
\end{bmatrix}
$$

isto é, 

$$
x^1 = (x^1_1=5 , x^1_2=6)
$$

$$
x^2 = (x^2_1=4 , x^2_2= 1)
$$

$$
x^3 = (x^3_1=3 , x^3_2=1)
$$

$$
x^4 = (x^4_1=2 , x^4_2=3)
$$

Nessa convenção, o elemento $$x^i_j$$ pertence a coluna $$i$$ e linha $$j$$ da matriz $$H_{exemplo}$$. Cada coluna representa uma entrada.

---

### 1. Importando módulos

Aqui importamos nossas bibliotecas. Para instalar o Quantum Paddle executamos 'pip install paddle-quantum' no terminal.

```
# Baidu Quantum Paddle
import paddle
from paddle_quantum.ansatz import Circuit
from paddle_quantum.gate import BasisEncoding, AmplitudeEncoding, AngleEncoding
import paddle_quantum as pq

# Xanadu PennyLane
import pennylane as qml
from pennylane import numpy as np
from pennylane.templates.embeddings import BasisEmbedding, AmplitudeEmbedding  # Importa o template

# IBM Qiskit
from qiskit import Aer, QuantumCircuit, execute


# Auxiliares
import math
```

### 2. Basis Encoding

O método de Basis Encoding é o mais famoso entre os métodos atualmente estudados. Este, por ser talvez o mais intuitivo e de fácil explicação, geralmente é ensinado primeiro conforme corrobora nossas referências. Seu nome se dá ao fato de transformarmos uma cadeia de string binária de entrada em um vetor da base computacional preservando os dígitos no processo. Repetimos o processo para todos as strings de entrada de forma que para cada entrada no formato string clássico, teremos um vetor da base de espaço correspondente. De fato, seria possível a utilização de outros vetores (valores) de base, porém os estados fundamentais foram utilizados por conveniência.
Primeiramente coletamos nossos dados númericos clássicos e os transformamos em cadeias de bits clássicos. Daí, aproveitamos a string de bits de entrada e o relacionamos com o estado quântico através de uma transformação bem intuitiva:

$$
010011101 \rightarrow \ket{010011101}
$$

Isto é, para cada input, usaremos um estado da base computacional. Nosso espaço de estados quânticos, agora denotado por $$ \ket{H} $$ e será uma combinação linear de todos os estados transformados:

$$
\ket{H} = \frac{1}{\sqrt{M}} \sum_{m=1}^{M} \ket{x^m}
$$

---
**Exemplo**:
Seja $$ H = \left\{x^1=101, x^2=111 \right\} $$. Daí:

$$
\ket{H} = \frac{1}{\sqrt{2}} ( \ket{101} + \ket{111})
$$

---

Agora iremos aplicar os frameworks para construir exemplos de basis encoding na prática. Usaremos como exemplo $$H_{exemplo}$$ com os elementos 

$$
x^1 = (5 , 6)
$$

$$
x^2 = (4 , 1)
$$

Primeiro, transformamos as componentes desses vetores da representação decimal para a binária:

$$
x^1_{[0,1]}=(101,110)
$$

$$
x^2_{[0,1]}=(100,001)
$$

Importante: Note que nesse caso, $$N=6$$ e não mais 2 porque tivemos que transformar os números decimais em binários.

Seguindo a metodologia de [ref.V], concatenamos então a informação contida nesses pontos como (101110) e (100001). 
Seguiremos agora transformando a string (101110) em $$\ket{101110}$$ utilizando 3 frameworks:


#### 2.1 Qiskit


O Qiskit não nos fornece uma função que faça basis encoding. Criaremos um circuito que execute a ideia:

```
n=6       #Nesse caso, número de qubits = tamanho do nosso string binário
qc = QuantumCircuit(n)
x = '101110'   #Bit clássico que queremos transformar em quântico
x=x[::-1]    #Inverte a string (só é necessário porque o padrão é inverter os qubits)

# Adicionamos um gate X (not) em cada componente do circuito para criar o qubit desejado
for i in range(len(x)):
    if x[i] == '1':
        qc.x(i)
  
qc.draw()

backend = Aer.get_backend('statevector_simulator')
result = execute(qc,backend).result().get_statevector()
result.draw('latex')
```

Podemos também utilizar o método decompose() do Qiskit para sabermos exatamente quais gates são necessários para chegarmos nesse estado quântico.

#### 2.2 Paddle

```

# Seguindo o método apresentado na seção anterior:
n=6
basis_enc = Circuit(n)    # Inicializa nosso circuito com o Paddle Quantum
x = '101110'

for i in range(len(x)):
    if x[i] == '1':
        basis_enc.x(i)
  
print(basis_enc)


# Para execução do circuito usamos:
init_state = pq.state.zero_state(n)
basis_quantum_state = basis_enc(init_state)
print(basis_quantum_state)


# Entretanto o Paddle Quantum já tem uma função pronta para Basis Encoding:
built_in_basis_enc = BasisEncoding(num_qubits=n)
x = paddle.to_tensor([1,0,1,1,1,0])   # Mudamos a informação clássica x para o tipo Tensor
built_in_basis_enc_state = built_in_basis_enc(feature=x)

print(built_in_basis_enc_state)
```


#### 2.3 PennyLane

Em [ref.V] é usado um código parecido com o abaixo para implementação.

```
dev = qml.device('default.qubit', wires=6)

@qml.qnode(dev)
def circuit(data):
    for i in range(len(data[0])):
        qml.Hadamard(i)
    for i in range(len(data)):
        BasisEmbedding(features=data[i], wires=range(6),do_queue=True)
    return  qml.state()

data=[1,0,1,1,1,0]


# Data poderia ser também um conjunto de dados como:
# data=[[1,1,1,1,1,0],
#      [1,0,0,0,0,1]]


circuit(data)

# Mostramos nosso circuito:
fig, ax = qml.draw_mpl(circuit)(data)
fig.show()
```

Entretanto, o autor não deixa claro o porquê de ter usado portas de Hadamard.
Em [ref.VI] temos uma versão mais intuitiva e mais simples sem a utilização do gate de Hadamard:

```
dev = qml.device('default.qubit', wires=6)

@qml.qnode(dev)
def circuit(feature_vector):
    qml.BasisEmbedding(features=feature_vector, wires=range(6))
    return qml.state()

X = [1,0,1,1,1,0]

print(qml.draw(circuit, expansion_strategy="device")(X))   #Neste output não é mostrado linhas onde nenhum gate foi adicionado
```

##### Conclusão
Embora o método de Basis Encoding seja bem intuitivo e simples, nossas referências nos contam que este método geralmente não é muito eficiente. Para $$N$$ bits existem $$2^N$$ possíveis estados da base. Conforme [ref.VI], se $$2^N >> M$$ então este método será esparso, ou seja, se a quantidade de amostras for muito menor do que a quantidade total de bases do sistema então nosso método se tornará confuso.
Por exemplo, assim que transformamos os valores (101110) e (100001) em kets, podemos perceber que teremos $$2^6=64$$ estados da base computacional mas apenas 2 deles estão sendo utilizados nesse caso: $$\ket{101110}$$ e $$\ket{100001}$$. Nesse caso, $$2^6=64 >> 2$$ pois consideramos apenas duas amostras de $$H_{exemplo}$$. 

### 3. Amplitude Encoding

Aqui estudaremos outro método também bem intuitivo, o amplitude encoding. A ideia de amplitude encoding vem da inserção da informação de entrada (input) nos fatores de amplitude dos estados dos qubits, ou seja, para cada entrada, construiremos um vetor clássico de tamanho $$N$$ e transformaremos as componentes do vetor clássico nas componentes $$n$$, do estado quântico com respeito a base computacional. Note que precisaremos primeiro normalizar as coordenadas dos dados para que este nos forneça estados quânticos que satisfaçam a condição de normalização.
Nosso espaço fica então da forma:

$$
\ket{H} = \sum_{i=1}^{N} \alpha_i \ket{i}
$$

Onde $$N=2^n$$

---
**Exemplo 1:** Um vetor quadrimensional
Seja

$$
x=\left[\begin{matrix}
\frac{1}{2} \\
\frac{1}{2} \\
-\frac{1}{2} \\
-\frac{1}{2}
\end{matrix}\right]
$$

nosso dado de entrada. Daí, nosso estado quântico associado será 

$$
\ket{x} = \frac{1}{2}\ket{00}+\frac{1}{2}\ket{01}-\frac{1}{2}\ket{10}-\frac{1}{2}\ket{11}
$$

que, neste caso, já está normalizado.

---

**Exemplo 2:** Dois vetores bidimensionais
Seja 

$$
H = \left[x^{(1)}=(1.5,0), x^{(2)}=(-2,3) \right]
$$

Concatenamos o vetor para achar o fator de normalização:

$$
\alpha = \frac{1}{\sqrt{15.25}} (1.5,0,-2,3)
$$

Logo, 
$$
\ket{H} = \frac{1}{\sqrt{15.25}} \left[1.5 \ket{00} -2 \ket{10}+ 3 \ket{11} \right]
$$

---

Note que agora teremos uma forma mais natural de trabalharmos com $$H_{exemplo}$$ quando cada amostra tiver mais componentes N. Iremos agora aplicar este método usando o Qiskit, o Quantum Paddle e o PennyLane.

Veremos agora como aplicar este método ao vetor

$$
x^1 = (5 , 6)
$$




#### 3.1 Qiskit

Como o Qiskit não tem uma função de Amplitude Encoding, aqui escolheremos um outro vetor já normalizado e mostraremos como chegar nesse estado através de gates com a função decompose().

```
desired_state = [
    1 / math.sqrt(15.25) * 1.5,
    0,
    1 / math.sqrt(15.25) * -2,
    1 / math.sqrt(15.25) * 3]

qc = QuantumCircuit(2)
qc.initialize(desired_state, [0,1])

qc.decompose().decompose().decompose().decompose().decompose().draw()
```

#### 3.2 Paddle

```
#Aqui usamos um dado clássico em forma de vetor. Precisamos normalizar este.
x = [5,6]
x = x / (np.linalg.norm(x))

# Quantidade de coordenadas: 2 = N, logo n = 1

n = 1
built_in_amplitude_enc = AmplitudeEncoding(num_qubits=n)
x = paddle.to_tensor(x)
state = built_in_amplitude_enc(x)

print(state)
```
#### 3.3 PennyLane

Para esse método, usando o PennyLane, as referências [ref.V] e [ref.VI] são mais parecidas e teremos, resumidamente:

```
n=1
dev = qml.device('default.qubit', wires=n)
@qml.qnode(dev)
def circuit(data):
    AmplitudeEmbedding(features=data, wires=range(n),normalize=True)
    return qml.state()

data = [5,6]
circuit(data)

#fig, aux = qml.draw_mpl(circuit)(data)
#fig.show()
```


##### Conclusão
Como um sistema de $$n$$ qubits tem $$N=2^n$$ amplitudes, para este método precisaremos de $$log_2(NM)$$ qubits para codificar.
Segundo [ref.IV], a vantagem de usar Amplitude Encoding é uma quantidade menor de qubits para codificar. Entretanto, como os algoritmos terão que trabalhar com as amplitudes dos estados, este método tende a ser ineficiente segundo [ref.IV].

### 4. Angle Encoding

Neste método codificaremos as $$N$$ componentes do vetor clássico em rotações de ângulos de $$n$$ qubits com $$N \leq n$$.
Por exemplo, seja $$x=(x_1,...,x_N)$$. Nosso estado quântico poderá ser:

$$
\ket{x} = \bigotimes cos(x_i)\ket{0} + sin(x_i)\ket{1}
$$

onde o produtório tensorial é de $$i=1$$ até $$i=N$$. Podemos fazer rotações em torno de qualquer eixo. Usaremos o eixo y como exemplo:

![angle-encoding](/assets/images/data-encoding/figura1.1_angle_encoding.png)
Fonte: [ref.V]


Usaremos agora novamente nosso vetor $$x^1=(5,6)$$ para exemplificar as implementações:


#### 4.1 Qiskit


```
qc = QuantumCircuit(2)
qc.ry(5, 0)
qc.ry(6, 1)

backend = Aer.get_backend('statevector_simulator')
result = execute(qc,backend).result().get_statevector()
result.draw('latex')
```

#### 4.2 Paddle

```
n = 2
angle_enc = Circuit(n)
x = [5,6]
x = paddle.to_tensor(x)

# Adiciona um layer para os gates RY
for i in range(len(x)):
    angle_enc.ry(qubits_idx=i, param=x[i])
        
# print(angle_enc)  # mostra o circuito

init_state = pq.state.zero_state(n)
angle_quan_state = angle_enc(init_state)

print([np.round(i, 2) for i in angle_quan_state.data.numpy()])


# Outra forma:
n = 2
built_in_angle_enc = AngleEncoding(num_qubits=n, encoding_gate="ry", feature=x)
x = [5,6]
x = paddle.to_tensor(x)
init_state = pq.state.zero_state(n)
state = built_in_angle_enc(state=init_state)

print([np.round(i, 2) for i in state.data.numpy()])
```

#### 4.3 PennyLane

Na documentação oficial [ref.VI] da Xanadu apresentam uma versão um pouco diferente do aqui mostrado. Em [ref.VI], adicionam um gate de Hadamard na primeira linha do circuito e o exemplo é com 3 qubits. Não é explicado o uso do gate de Hadamard e de fato, testando o código temos um resultado diferente do esperado usando o Qiskit e o Quantum Paddle. A diferença que pode ser impactante é que no exemplo do PennyLane usam 3 qubits. A implementação exemplo de [ref.V] também tem gates de Hadamard.
Retirando o gate de Hadamard temos:

```
dev = qml.device('default.qubit', wires=2)
n=2
@qml.qnode(dev)
def circuit(feature_vector):
    qml.AngleEmbedding(features=feature_vector, wires=range(n), rotation='Y')
    #qml.Hadamard(0)  #trecho retirado
    return qml.state()

X = [5,6]
circuit(X)
```
que nos retorna números coerentes com as respostas dos outros frameworks.

##### Conclusão
Para este método encontramos poucas informações nas referências. A [ref.IV] nos conta que este método é diferente dos dois métodos de codificação anteriores, pois codifica apenas um ponto de dados por vez, em vez de um conjunto de dados inteiro. No entanto, ele usa apenas qubits e um circuito quântico de profundidade constante, tornando-o passível de hardware quântico atual.
Segundo [ref.V], Angle Encoding é um método muito eficiente em termos de operações pois apenas um número constante de operações paralelas é necessário, independentemente de quantos valores de dados precisam ser codificados. Isso não é ideal do ponto de vista do qubit, pois cada componente do vetor de entrada requer um qubit.


### Conclusão Final
O problema de Data Encoding é relevante no cenário de QML atual e existem outros métodos criados mais recentemente como IQP Style Encoding e Hamiltonian Evolution Ansatz Encoding que não tratamos nesse artigo.
Nesse post comparamos as implementações do Qiskit, do Quantum Paddle e do Pennylane. Podemos afirmar que o Xanadu PennyLane e o Baidu Quantum Paddle nos fornecem, atualmente, frameworks mais adequados para Basis Encoding uma vez que estes já tenham funções que executem os métodos. O Pennylane ainda o faz de forma mais intuitiva e amigável com o programador.
Por fim, para discutirmos de forma sólida qual método utilizar dado um problema em QML poderiamos tentar realizar testes de performance variando $$H_{exemplo}$$. Todavia, este post tem como objetivo secundário fornecer uma intuição inicial sobre o assunto através das seções Conclusão e deixaremos esta abordagem como pesquisa futura no momento.


### Referências
I. Qiskit - Lecture 5.1 Building a Quantum Classifier [YouTube] https://www.youtube.com/watch?v=-sxlXNz7ZxU&list=PLOFEBzvs-VvqJwybFxkTiDzhf5E11p8BI&index=10 .Acessado em 06/02/2023.

II. Quantum Zeitgeist - Quantum Encoding: An Overview, https://quantumzeitgeist.com/quantum-encoding-an-overview/#:~:text=Basis%20encoding%20is%20primarily%20used%20when%20real%20numbers,into%20a%20quantum%20state%20in%20the%20computational%20basis. Acessado em 06/02/2023.

III. Baidu - Quantum Paddle: https://qml.baidu.com/tutorials/machine-learning/encoding-classical-data-into-quantum-states.html Acessado em 06/02/2023.

IV. Qiskit: Quantum Machine Learning - Data Encoding, https://learn.qiskit.org/course/machine-learning/data-encoding Acessado em 06/02/2023.

V. Baijayanta Roy - "All about Data Encoding for Quantum Machine Learning" artigo publicado em https://medium.datadriveninvestor.com/all-about-data-encoding-for-quantum-machine-learning-2a7344b1dfef. Acessado em 13/02/2023

VI. PennyLane Documentation - https://docs.pennylane.ai/en/stable/ . Acessado em 13/02/2023

### Futuro
1. Buscar por comparações mais concretas entre os métodos.
2. Testar os métodos de Data Encoding variando H.



