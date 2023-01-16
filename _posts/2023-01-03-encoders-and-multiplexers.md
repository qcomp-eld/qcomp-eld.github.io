---
layout: post
title:  "Exemplo de Codificador e Multiplexador Quânticos"
date:   2023-01-03 10:00:00
author: luccas_marim
categories: teoria computacao-quântica educativo
usemathjax: true
---

Nesse artigo iremos introduzir dois dispositivos quânticos que simulam a ação de um codificador binário 8:3 e um multiplexador 4:1 utilizando como orientação o artigo [ref.I]. A generalização n:m desses dispositivos pode ser facilmente construída e caso o leitor se interesse a referência I mostra alguns detalhes. 
É importante pontuar que embora usemos a palavra "quântica" estes dispositivos não dependem de qualquer fenômeno quântico. Os gates que realizam a função dos dispositivos clássicos são substituidos por composições simples de gates quânticos e portanto podem ser simulados em ferramentas como o IBM Quantum Composer.
Antes da leitura desse post é recomendado um conhecimento prévio em gates quânticos e clássicos.

## Codificador e Multiplexador
Um codificador é um dispositivo que é utilizado para transformar dados de entrada X em dados de saída Y de acordo com certas regras. O decodificador, por outro lado, faz o caminho inverso levando os dados Y em X. Como o nome sugere, um conjunto de entrada é codificado de forma que apenas decodificadores podem recuperar os dados iniciais. O leitor pode perceber o quão importante é o estudo desses dispositivos não apenas em criptografia ou compiladores mas para a computação em geral, como processamento de informação. Um exemplo de codificador é o binário que transforma números em cadeias de bits.

Um multiplexador é um dispositivo que recebe como entrada dados de diversos canais e retorna, através de controladores (bits), estes dados em apenas um canal otimizando, geralmente, o tratamento dos dados posteriormente. O demultiplexador realiza a operação inversa, recebe como entrada os dados através de um canal e distribui a saida em vários canais.
Conforme esperado, em computação clássica essas operações são realizadas através de circuitos, gates e bits. Em computação quântica desejamos que ocorra o mesmo. Neste artigo introduziremos esses dispositivos clássicos e construiremos os análogos quânticos usando como referência os artigos listados na referência através do Qiskit.

### Codificador binário 8:3

Utilizaremos como exemplo o codificador binário com inputs entre 0 e $$ N $$. Logo, teremos $$ log_2 (N+1) = K $$ linhas de saída. Se $$ N = 7 $$, por exemplo, $$ K = 3 $$ e precisaremos de 3 bits para descrever o número a ser codificado. Seguindo esse exemplo, escreveremos essa cadeia como XYZ com X,Y e Z sendo 0 ou 1.
Notemos imediatamente que XYZ = 000 quando a entrada for 0. Por outro lado, Z será 1 para as entradas 1, 3, 5 ou 7. Y será 1 quando as entradas forem 2, 3, 6 ou 7 e por fim, X será 1 quando as entradas forem 4, 5, 6 ou 7. Deste sistema com 4 equações, notamos que precisaremos de pelo menos 4 gates OR.
Utilizando gates quânticos em dimensão 2, o gate OR pode ser escrito como uma composição de gates X com um gate de Toffoli:

![Gate-OR](/assets/images/encoders-multiplexers/gate_or.png)
Fonte: Autor. Utilizado o IBM Quantum Composer.

De fato, podemos ver que este gate representa o gate clássico OR através das respectivas tabelas verdade:

![00](/assets/images/encoders-multiplexers/00.png)
Fonte: Autor. Utilizado o IBM Quantum Composer.

![01](/assets/images/encoders-multiplexers/01.png)
Fonte: Autor. Utilizado o IBM Quantum Composer.

![10](/assets/images/encoders-multiplexers/10.png)
Fonte: Autor. Utilizado o IBM Quantum Composer.

![11](/assets/images/encoders-multiplexers/11.png)
Fonte: Autor. Utilizado o IBM Quantum Composer.

Onde o primeiro bit da tabela de probabilidades, ou o último qubit no Composer (q[2]), é a resposta da operação. Uma implementação em Python usando o Qiskit está presente em [ref.I].

O IBM Quantum Composer não nos fornece uma ferramenta direta para trabalharmos com mais de 7 qubtis. Para implementar o codificador binário, teremos que programar no IBM Quantum Lab. Uma implementação em Python utilizando o Qiskit e o IBMQ podem ser encontradas em [ref.I]. A construção do circuito codificador binário 8:3 fica, conforme [ref.I]:

![encoder](/assets/images/encoders-multiplexers/encoderfinal.png)
Fonte: Narula, Hridey & Behera, Bikash & Panigrahi, Prasanta. (2020). Implementing Quantum Data Processing Circuits Using IBM Quantum Experience Platform. 10.13140/RG.2.2.36062.38729

Onde as linhas q16 correspondem aos inputs, q18 os outputs, q17 e q19 à qubits auxiliares usados como buffer e validação. Aparentemente, foram utilizados 16 qubits (e não 8) porque queriam preservar os valores de entrada.
Para realizar testes nesse sistema, basta acessar o IBM Quantum Lab com sua conta, criar um projeto .ipynb e executar o código do apêndice I. O input n que você quiser testar deve ser escrito na parte "circuit.x(qi[2])" onde, nesse caso, n=2.

### Multiplexador 4:1

Para implementar um multiplexer precisaremos das portas quânticas que reproduzem o efeito das portas clássicas OR e AND. Entretanto, a porta OR já foi estudada na seção anterior e a porta AND é equivalente, em questão de tabela verdade, a porta Toffoli desde que o target qubit seja 0. De fato, fixando o terceiro qubit de entrada como zero no Qiskit:

![000](/assets/images/encoders-multiplexers/000.png)
Fonte: Autor. Utilizado o IBM Quantum Composer.

![010](/assets/images/encoders-multiplexers/010.png)
Fonte: Autor. Utilizado o IBM Quantum Composer.

![100](/assets/images/encoders-multiplexers/100.png)
Fonte: Autor. Utilizado o IBM Quantum Composer.

![110](/assets/images/encoders-multiplexers/110.png)
Fonte: Autor. Utilizado o IBM Quantum Composer.

Para fixar o terceiro qubit como 0 podemos usar uma estrutura condicional que flipe o qubit caso este seja 1. Dessa forma nosso "AND quântico" fica:

![Gate-AND](/assets/images/encoders-multiplexers/quantum_and.png)
Fonte: Autor. Utilizado o IBM Quantum Composer.

Da mesma forma que o codificador, não podemos implementar um multiplexador no IBM Quantum Composer. Uma implementação deste está presente no apêndice II e segue o mesmo passo-a-passo (i.e. utilizando o IBMQ Lab) do codificador. O circuito, caso tivessemos 16 qubits, ficaria:
![multiplexer](/assets/images/encoders-multiplexers/mux.png)
Fonte: Narula, Hridey & Behera, Bikash & Panigrahi, Prasanta. (2020). Implementing Quantum Data Processing Circuits Using IBM Quantum Experience Platform. 10.13140/RG.2.2.36062.38729

Resta construir o decodificador e o demultiplexador. Entretanto, estes só dependem das portas já construídas e portanto, em teoria, basta apenas substitui-las. O decodificador por exemplo só usa portas OR e NOT. A implementação, bem como os circuitos destes, estão detalhados em [ref.I] e o código nos apêndices III e IV.

##### Apêndice I: Uma implementação para o codificador binário 8:3

```
%matplotlib inline

from qiskit import QuantumCircuit, execute, Aer, IBMQ, QuantumRegister, ClassicalRegister
from qiskit.compiler import transpile, assemble
from qiskit.tools.jupyter import *
from qiskit.visualization import *


provider = IBMQ.load_account()
simulator=Aer.get_backend('qasm_simulator')

qi=QuantumRegister(8)
qb=QuantumRegister(2)
qo=QuantumRegister(3)
v=QuantumRegister(1)
c=ClassicalRegister(4)

circuit=QuantumCircuit(qi,qb,qo,v,c)

def fun_or(qc,q0,q1,q2):
    qc.x(q0)
    qc.x(q1)
    qc.ccx(q0,q1,q2)
    qc.x(q2)
    qc.x(q1)
    qc.x(q0)

def or3(qc,q0,q1,q2,b,q3):
    fun_or(qc,q0,q1,b)
    fun_or(qc,b,q2,q3)
    qc.reset(b)

def or4(qc,q0,q1,q2,q3,b1,b2,q4):
    or3(qc,q0,q1,q2,b1,b2)
    fun_or(qc,b2,q3,q4)
    qc.reset(b1)
    qc.reset(b2)

circuit.x(qi[2])  #aqui estamos colocando 2 como input

or4(circuit,qi[4],qi[5],qi[6],qi[7],qb[0],qb[1],qo[2])
or4(circuit,qi[2],qi[3],qi[6],qi[7],qb[0],qb[1],qo[1])
or4(circuit,qi[1],qi[5],qi[3],qi[7],qb[0],qb[1],qo[0])
or4(circuit,qi[0],qo[0],qo[1],qo[2],qb[0],qb[1],v[0])

circuit.measure(v[0],c[3])
circuit.measure(qo[2],c[2])
circuit.measure(qo[1],c[1])
circuit.measure(qo[0],c[0])

job=execute(circuit,simulator,shots=100)
result=job.result()
counts=result.get_counts(circuit)

print(counts)
#circuit.draw()
#plot_histogram(counts)
```

##### Apêndice II: Uma implementação para o multiplexador 4:1

```
%matplotlib inline

from qiskit import QuantumCircuit, execute, Aer, IBMQ, QuantumRegister, ClassicalRegister
from qiskit.compiler import transpile, assemble
from qiskit.tools.jupyter import *
from qiskit.visualization import *


provider = IBMQ.load_account()
simulator=Aer.get_backend('qasm_simulator')

qi=QuantumRegister(4)
qs=QuantumRegister(2)
qb=QuantumRegister(2)
qe=QuantumRegister(4)
qxs=QuantumRegister(2)
qo=QuantumRegister(1)
c=ClassicalRegister(1)

circuit=QuantumCircuit(qi,qs,qb,qe,qxs,qo,c)

def fun_or(qc,q0,q1,q2):
    qc.x(q0)
    qc.x(q1)
    qc.ccx(q0,q1,q2)
    qc.x(q2)
    qc.x(q1)
    qc.x(q0)

def or3(qc,q0,q1,q2,b,q3):
    fun_or(qc,q0,q1,b)
    fun_or(qc,b,q2,q3)
    qc.reset(b)

def fun_and(qc,q0,q1,q2):
    qc.ccx(q0,q1,q2)

def and3(qc,q0,q1,q2,b,q3):
    fun_and(qc,q0,q1,b)
    fun_and(qc,b,q2,q3)
    qc.reset(b)

def or4(qc,q0,q1,q2,q3,b1,b2,q4):
    or3(qc,q0,q1,q2,b1,b2)
    fun_or(qc,b2,q3,q4)
    qc.reset(b1)
    qc.reset(b2)

#Input
circuit.x(qi[0])
circuit.x(qi[2])
circuit.x(qi[3])

#Control signal
circuit.x(qs[1])

for i in range(0,2):
    circuit.cx(qs[i],qxs[i])
    circuit.x(qxs[i])

and3(circuit,qxs[0],qxs[1],qi[0],qb[0],qe[0])
and3(circuit,qs[0],qxs[1],qi[1],qb[0],qe[1])
and3(circuit,qxs[0],qs[1],qi[2],qb[0],qe[2])
and3(circuit,qs[0],qs[1],qi[3],qb[0],qe[3])
or4(circuit,qe[0],qe[1],qe[2],qe[3],qb[0],qb[1],qo[0])

circuit.measure(qo[0],c[0])

job=execute(circuit,simulator,shots=100)
result=job.result()
counts=result.get_counts(circuit)

print(counts)
#circuit.draw()
#plot_histogram(counts)
```

##### Apêndice III: Uma implementação para o decodificador 3:8

```
%matplotlib inline

from qiskit import QuantumCircuit, execute, Aer, IBMQ, QuantumRegister, ClassicalRegister
from qiskit.compiler import transpile, assemble
from qiskit.tools.jupyter import *
from qiskit.visualization import *


provider = IBMQ.load_account()
simulator=Aer.get_backend('qasm_simulator')

qi=QuantumRegister(3)
qb=QuantumRegister(1)
qx=QuantumRegister(3)
qo=QuantumRegister(8)
c=ClassicalRegister(8)

circuit=QuantumCircuit(qi,qx,qb,qo,c)

def fun_or(qc,q0,q1,q2):
    qc.x(q0)
    qc.x(q1)
    qc.ccx(q0,q1,q2)
    qc.x(q2)
    qc.x(q1)
    qc.x(q0)

def or3(qc,q0,q1,q2,b,q3):
    fun_or(qc,q0,q1,b)
    fun_or(qc,b,q2,q3)
    qc.reset(b)

#Input:
circuit.x(qi[2])
circuit.x(qi[1])
circuit.x(qi[0])

for i in range(0,3):
    circuit.cx(qi[i]),qx[i])
    circuit.x(qx[i])

or3(circuit,qx[0],qx[1],qx[2],qb[0],qo[7])
or3(circuit,qi[0],qx[1],qx[2],qb[0],qo[6])
or3(circuit,qx[0],qi[1],qx[2],qb[0],qo[5])
or3(circuit,qi[0],qi[1],qx[2],qb[0],qo[4])
or3(circuit,qx[0],qx[1],qi[2],qb[0],qo[3])
or3(circuit,qi[0],qx[1],qi[2],qb[0],qo[2])
or3(circuit,qx[0],qi[1],qi[2],qb[0],qo[1])
or3(circuit,qi[0],qi[1],qi[2],qb[0],qo[0])

for i in range(0,8):
    circuit.x(qo[i])

circuit.measure(qo[7],c[0])
circuit.measure(qo[6],c[1])
circuit.measure(qo[5],c[2])
circuit.measure(qo[4],c[3])
circuit.measure(qo[3],c[4])
circuit.measure(qo[2],c[5])
circuit.measure(qo[1],c[6])
circuit.measure(qo[0],c[7])

job=execute(circuit,simulator,shots=100)
result=job.result()
counts=result.get_counts(circuit)

print(counts)
#circuit.draw()
#plot_histogram(counts)
```

##### Apêndice IV: Uma implementação para o demultiplexador 1:4

```
%matplotlib inline

from qiskit import QuantumCircuit, execute, Aer, IBMQ, QuantumRegister, ClassicalRegister
from qiskit.compiler import transpile, assemble
from qiskit.tools.jupyter import *
from qiskit.visualization import *


provider = IBMQ.load_account()
simulator=Aer.get_backend('qasm_simulator')

qi=QuantumRegister(1)
qs=QuantumRegister(2)
qxs=QuantumRegister(2)
qb=QuantumRegister(1)
qo=QuantumRegister(4)
c=ClassicalRegister(4)

circuit=QuantumCircuit(qi,qb,qs,qxs,qo,c)

def fun_or(qc,q0,q1,q2):
    qc.x(q0)
    qc.x(q1)
    qc.ccx(q0,q1,q2)
    qc.x(q2)
    qc.x(q1)
    qc.x(q0)

def or3(qc,q0,q1,q2,b,q3):
    fun_or(qc,q0,q1,b)
    fun_or(qc,b,q2,q3)
    qc.reset(b)

def fun_and(qc,q0,q1,q2):
    qc.ccx(q0,q1,q2)

def and3(qc,q0,q1,q2,b,q3):
    fun_and(qc,q0,q1,b)
    fun_and(qc,b,q2,q3)
    qc.reset(b)

def or4(qc,q0,q1,q2,q3,b1,b2,q4):
    or3(qc,q0,q1,q2,b1,b2)
    fun_or(qc,b2,q3,q4)
    qc.reset(b1)
    qc.reset(b2)

#Input
circuit.x(qi[0])

#Control signal
circuit.x(qs[1])

for i in range(0,2):
    circuit.cx(qs[i],qxs[i])
    circuit.x(qxs[i])

and3(circuit,qxs[0],qxs[1],qi[0],qb[0],qo[0])
and3(circuit,qs[0],qxs[1],qi[0],qb[0],qo[1])
and3(circuit,qxs[0],qs[1],qi[0],qb[0],qo[2])
and3(circuit,qs[0],qs[1],qi[0],qb[0],qo[3])

circuit.measure(qo[0],c[3])
circuit.measure(qo[1],c[2])
circuit.measure(qo[2],c[1])
circuit.measure(qo[3],c[0])

job=execute(circuit,simulator,shots=100)
result=job.result()
counts=result.get_counts(circuit)

print(counts)
#circuit.draw()
#plot_histogram(counts)
```

## Referências
I. Narula, Hridey & Behera, Bikash & Panigrahi, Prasanta. (2020). Implementing Quantum Data Processing Circuits Using IBM Quantum Experience Platform. 10.13140/RG.2.2.36062.38729 [https://www.researchgate.net/publication/338411513_Implementing_Quantum_Data_Processing_Circuits_Using_IBM_Quantum_Experience_Platform]


II. Arijit Roy, Dibyendu Chatterjee and Subhasis Pal. (2012). Synthesis of Quantum Multiplexer Circuits [http://ijcsi.org/papers/IJCSI-9-1-3-67-74.pdf]



### Futuro/Questionamentos
* Adicionar mais conteúdo teórico usando [ref.II]
* É possível criar um gate que flipe 1 para 0 sempre? 
* Irreversibilidade https://ieeexplore.ieee.org/document/5392446
* É possível criar um multiplexador com um qubit de controle?
* Invertendo as portas como fazemos no algoritmo de Shor, produzirá os efeitos do decoder e do demux?


