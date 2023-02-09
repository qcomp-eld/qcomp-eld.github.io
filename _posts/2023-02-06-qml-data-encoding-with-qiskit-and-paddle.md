---
layout: post
title:  "QML 01: Data Encoding com Qiskit e Quantum Paddle"
date:   2023-02-06 10:00:00
author: luccas_marim
categories: teoria computacao-quântica educativo
usemathjax: true
---

Para utilizarmos modelos de Machine Learning, precisamos primeiro representar os dados em inputs numéricos para o nosso sistema interpretá-los. Em Quantum Machine Learning (QML) também passamos por esse processo e aqui teremos um problema mais fundamental visto que estamos transformando, a príncipio, representações de objetos clássicos em quânticos. O nome mais popular na literatura deste processo é Data Encoding. 
Conforme [ref.IV], este processo é uma parte crítica dos algoritmos de QML e afeta diretamente seu poder computacional.
Neste artigo iremos estudar como esse processo é feito atualmente com base em nossas referências. Estudaremos os três métodos mais famosos de Data Encoding: Basis Encoding, Amplitude Encoding e Angle Encoding bem como exemplos ou direcionamentos de implementação. Para tanto, utilizaremos os frameworks fornecidos pela IBM Qiskit e Baidu Paddle Quantum. Iremos também, superficialmente, comparar os módulos.
O processo de Data Encoding geralmente é intuitivo e simples mas quando implementamos é diferente. Nossos dispositivos quânticos atuais tem quantidade limitada de qubits e estes são estáveis por curto período de tempo. Além disso, não há consenso em dizer qual dos métodos estudados nesse artigo é o melhor. Atualmente a escolha entre métodos depende do problema em questão.

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

Primeiramente coletamos nossos dados númericos clássicos e o transformamos em cadeias de bits clássicos. Daí, aproveitamos a string de bits de entrada e o relacionamos com o estado quântico através de uma transformação bem intuitiva:

$$
010011101 \rightarrow \ket{010011101}
$$

Isto é, para cada input, usaremos um estado da base computacional. Nosso espaço de estados quânticos, agora denotado por $$ \ket{H} $$ e será uma combinação linear de todos os estados transformados:

$$
\ket{H} = \frac{1}{\sqrt{M}} \sum_{m=1}^{M} \ket{x^m}
$$

Exemplo:
Seja $$ H = \left\{x^1=101, x^2=111 \right\} $$. Daí:

$$
\ket{H} = \frac{1}{\sqrt{2}} ( \ket{101} + \ket{111})
$$

#### 2.1 Qiskit

```
n=16       #Nesse caso, número de qubits = tamanho do nosso string binário
qc = QuantumCircuit(n)
x = '1011100010101110'   #Bit clássico que queremos transformar em quântico
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
n = 4
basis_enc = Circuit(n)    # Inicializa nosso circuito com o Paddle Quantum
x = '1011'

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
x = paddle.to_tensor([1, 0, 1, 1])   # Mudamos a informação clássica x para o tipo Tensor
built_in_basis_enc_state = built_in_basis_enc(feature=x)

print(built_in_basis_enc_state)
```


#### 2.3 PennyLane

Em [ref.V] é usado um código parecido com o abaixo para implementação.

```
dev = qml.device('default.qubit', wires=6)

@qml.qnode(dev)
def circuit(data):
    for i in range(6):
        qml.Hadamard(i)
    for i in range(len(data)):
        BasisEmbedding(features=data[i], wires=range(6),do_queue=True)
    return  qml.state()

data=[[1,0,1,1,1,0],
      [1,0,0,0,0,1]]

circuit(data)

# Mostramos nosso circuito:
fig, ax = qml.draw_mpl(circuit)(data)
fig.show()
```

Entretanto, o autor não deixa claro o porquê de ter usado portas de Hadamard.
Em [ref.VI] temos uma versão mais intuitiva e mais simples:

```
dev = qml.device('default.qubit', wires=3)

@qml.qnode(dev)
def circuit(feature_vector):
    qml.BasisEmbedding(features=feature_vector, wires=range(3))
    return qml.state()

X = [1,1,1]

print(qml.draw(circuit, expansion_strategy="device")(X))
```

##### Conclusão
Embora o método de Basis Encoding seja bem intuitivo e simples, nossas referências nos contam que este método geralmente não é muito eficiente. 

### 3. Amplitude Encoding

Aqui estudaremos outro método também bem intuitivo. Para cada entrada, construiremos um vetor clássico de tamanho $$N$$ e transformaremos as componentes do vetor clássico nas componentes, $$n$$, do estado quântico com respeito a base computacional. Note que precisaremos primeiro normalizar as coordenadas dos dados para que este nos forneça estados quânticos que satisfaçam a condição de normalização.
Nosso espaço fica então da forma:

$$
\ket{H} = \sum_{i=1}^{N} \alpha_i \ket{i}
$$

Onde $$N=2^n$$

Exemplo 1:
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

Exemplo 2:
Seja 

$$
H = \left[x^{(1)}=(1.5,0), x^{(2)}=(-2,3) \right]
$$

Concatemos o vetor para achar o fator de normalização:

$$
\alpha = \frac{1}{\sqrt{15.25}} (1.5,0,-2,3)
$$

Logo, 
$$
\ket{H} = \frac{1}{\sqrt{15.25}} \left[1.5 \ket{00} -2 \ket{10}+ 3 \ket{11} \right]
$$

#### 3.1 Qiskit

Aqui escolhemos um vetor já normalizado e mostramos como chegar nesse estado através de gates com a função decompose().

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
x = [-1,1,-2,6]
x = x / (np.linalg.norm(x))

# Numero de qubits
# N=3 n=log_2(N)
n = 2
built_in_amplitude_enc = AmplitudeEncoding(num_qubits=n)
# Altera a natureza da informação clásica em quântica (nesse caso, um tensor)
x = paddle.to_tensor([0.5, 0.5, 0.5])
state = built_in_amplitude_enc(x)

print(state)
```
#### 3.3 PennyLane

Para esse método, usando o PennyLane, as referências [ref.V] e [ref.VI] são mais parecidas e teremos, resumidamente:

```
dev = qml.device('default.qubit', wires=2)
@qml.qnode(dev)
def circuit(data):
    AmplitudeEmbedding(features=data, wires=range(2),normalize=True)
    return qml.state()

data = [6,-12.5,11.15,7]
circuit(data)

fig, aux = qml.draw_mpl(circuit)(data)
fig.show()
```


##### Conclusão
Como um sistema de $$n$$ qubits tem $$N=2^n$$ amplitudes, para este método precisaremos de $$log_2(NM)$$ qubits para codificar.
Segundo [ref.IV], a vantagem de usar Amplitude Encoding é uma quantidade menor de qubits para codificar. Entretanto, como os algoritmos terão que trabalhar com as amplitudes dos estados, este método tende a ser ineficiente.

### 4. Angle Encoding

Neste método codificaremos as $$N$$ componentes do vetor clássico em rotações de ângulos de $$n$$ qubits.
Por exemplo, seja $$x=(x_1,...,x_N)$$. Nosso estado quântico será:

$$
\ket{x} = \bigotimes R(x_i) \ket{0^N}
$$

onde o produtório é de $$i=1$$ até $$i=N$$.
Faremos essa tradução através da ação de gates unitários bem conhecidos como $$RY$$:

$$
RY(\theta) = e^{-i \frac{\theta}{2} Y} = 
\begin{pmatrix}
cos(\frac{\theta}{2})&-sin(\frac{\theta}{2})\\
sin(\frac{\theta}{2})&cos(\frac{\theta}{2})
\end{pmatrix}
$$

Exemplo:

$$
x = 
\begin{bmatrix}
\pi \\
\pi \\
\pi
\end{bmatrix}
$$

Se escolhermos $$RY$$ teremos que o estado correspondente será $$\ket{111}$$ conforme será mostrado em 4.2.


#### 4.1 Qiskit

```
qc = QuantumCircuit(3)
qc.ry(0, 0)
qc.ry(2*math.pi/4, 1)
qc.ry(2*math.pi/2, 2)
qc.draw()
```

#### 4.2 Paddle

```
# Números de qubits = Tamanho da string de informação classica
n = 3
# Inicializa o circuito
angle_enc = Circuit(n)
# x é a string clássica
x = paddle.to_tensor([np.pi, np.pi, np.pi], 'float64')
# Adiciona um layer para os gates RY
for i in range(len(x)):
    angle_enc.ry(qubits_idx=i, param=x[i])
        
print(angle_enc)

init_state = pq.state.zero_state(n)
angle_quan_state = angle_enc(init_state)

print([np.round(i, 2) for i in angle_quan_state.data.numpy()])


# Outra forma:
n = 3
built_in_angle_enc = AngleEncoding(num_qubits=n, encoding_gate="ry", feature=x)
x = paddle.to_tensor([np.pi, np.pi, np.pi], 'float64')
init_state = pq.state.zero_state(n)
state = built_in_angle_enc(state=init_state)

print([np.round(i, 2) for i in state.data.numpy()])
```

#### 4.3 PennyLane
```
dev = qml.device('default.qubit', wires=3)

@qml.qnode(dev)
def circuit(feature_vector):
    qml.AngleEmbedding(features=feature_vector, wires=range(3), rotation='Z')
    qml.Hadamard(0)
    return qml.probs(wires=range(3))

X = [1,2,3]

print(qml.draw(circuit, expansion_strategy="device")(X))
```

##### Conclusão
Esse método é diferente dos outros porque codifica apenas um ponto dos dados por vez. Segundo [ref.IV], este método é um método que pode ser utilizado com o hardware quântico atual.


O problema de Data Encoding ainda é relevante no cenário de QML atual e existem outros métodos mais recentes como IQP Style Encoding e Hamiltonian Evolution Ansatz Encoding que não tratamos nesse artigo.

### Referências
I. Qiskit - Lecture 5.1 Building a Quantum Classifier [YouTube] https://www.youtube.com/watch?v=-sxlXNz7ZxU&list=PLOFEBzvs-VvqJwybFxkTiDzhf5E11p8BI&index=10 .Acessado em 06/02/2023.

II. Quantum Zeitgeist - Quantum Encoding: An Overview, https://quantumzeitgeist.com/quantum-encoding-an-overview/#:~:text=Basis%20encoding%20is%20primarily%20used%20when%20real%20numbers,into%20a%20quantum%20state%20in%20the%20computational%20basis. Acessado em 06/02/2023.

III. Baidu - Quantum Paddle: https://qml.baidu.com/tutorials/machine-learning/encoding-classical-data-into-quantum-states.html Acessado em 06/02/2023.

IV. Qiskit: Quantum Machine Learning - Data Encoding, https://learn.qiskit.org/course/machine-learning/data-encoding Acessado em 06/02/2023.

V. https://medium.datadriveninvestor.com/all-about-data-encoding-for-quantum-machine-learning-2a7344b1dfef

VI. PennyLane Documentation - https://docs.pennylane.ai/en/stable/

### Futuro
1. Buscar por comparações mais concretas entre os métodos.



