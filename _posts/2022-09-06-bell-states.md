---
layout: post
title: "Bell States"
date: 2022-09-04 12:13:00
author: alisson_fumaco
categories: matemática computacao-quântica entrelaçamento bell-states
usemathjax: true
excerpt_separator: <!--more-->
---

<!--more-->

Os bell states foram apresentados em 1964 por John Stewart Bell e, rapidamente, ganharam a atenção da comunidade de mecânica quântica. Sendo um dos tópicos mais importantes para a computação quântica e a referência de um sistema entrelaçado.

### História

A mecanica quântica começou a ganhar rapidamente defensores durante o século 20. Tão rápido quanto os seus seguidores, surgiram os seus críticos.

Um dos críticos mais famosos da mecanica quântica foi Albert Einstein. Um de seus alunos de doutorado Podolsky publicou em 1935 - tendo Einstein e Rosen como co-autores - o artigo _Can Quantum-Mechanical Description of Physical Reality be Considered Complete?_. Nesse artigo, foi apresentado um experimento mental afirmando que a mecânica quântica era uma teoria incompleta, ou seja, existiria alguma variável desconhecida (_hiden variables_) pela física.

Esse experimento ficou conhecido como paradoxio EPR e iniciou uma longa discução entre Einsten e Niels Bohr.

O artigo começa descrevendo um gerador de pares de particulas. Cada uma das particulas com duas propriedades. Vamos dizer, 0 e 1 para a primeira e A e B para a segunda. Cada valor possui 50% de chance de acontecer.

| ![](/assets/images/bell-states/bohms-variant.png)
||:--:||
<sub><sup> \*Ref: https://upload.wikimedia.org/wikipedia/commons/thumb/5/57/EPR_illustration.svg/500px-EPR_illustration.svg.png</sup></sub>|

Uma vez medido uma das propriedades, digamos 0 e 1, as demais medições irão encontrar o mesmo valor, ou seja, teremos 100% de chance de encontrar o mesmo valor.

Agora se medimos a segunda propriedade, A e B, teremos 50% de chance de encontrar A e 50% de chance de encontrar B. Após medir novamente a primeira propriedade, esperariamos encontrar o primeiro valor medido, porém, em metade dos casos vamos encontrar o valor 0 e na outra metade o valor 1.

Assim quando mudamos a propriedade medida, de alguma forma, perdemos a medição anterior.

O mais interessante é quando trabalhamos com particulas entrelaçadas. Essas particulas possuem pares de valores fixos. Digamos 1-1. Assim quando medimos a primeira particula e encontramos 1, sabemos que a segunda vai ter o valor 1.

O que Einstein alega no artigo é que a informação da medição de uma das particulas não pode ser transmitida mais rápida do que a luz. Ou seja, quando medimos, a segunda particula não pode esperar a "luz" chegar até ela para "decidir" o valor da propriedade. Por isso, Einstein afirma que falta alguma variável que serviria como DNA para que a segunda particula "saiba" qual o valor dessa propriedade.

Esse paradoxo durou até 1964 quando John Stewart Bell publicou um estudo provando que existem sistemas que não podem ser explicados por uma teoria local e com variáveis escondidadas. Nesse mesmo artigo, ele mostra 4 sistemas que não podem ser explicados por variáveis escondidas. Esses sistemas passam a ser conhecidos como Bell States.

A partir desse artigo, físicos começaram a testar a ideia apresentada por Bell e encontrar os resultados esperados pelo seu artigo.

### Bell States

Uma vez que o John Bell conseguiu encontrar um sistema que possui um entrelaçamento quântico. Os físicos que desejavam estudar esse fenomeno começaram a trabalhar com esse sistema. Tornando esses sistemas um hello world da mecânica quântica.

Os 4 bell states são mostrados abaixo:

1. $$\ket{\Psi^+} = \frac{ \ket{01} + \ket{10} }{\sqrt{2}}$$
2. $$\ket{\Psi^-} = \frac{ \ket{01} - \ket{10} }{\sqrt{2}}$$
3. $$\ket{\Phi^+} = \frac{ \ket{00} + \ket{11} }{\sqrt{2}}$$
4. $$\ket{\Phi^-} = \frac{ \ket{00} - \ket{11} }{\sqrt{2}}$$

Vamos analisar o sistema 1. Nele temos duas possibilidades: primero, Alice mede seu qubit e verifica que ele colapsa para o qubit $$\ket{0}$$. Assim, ela sabe que Bob vai ter o qubit $$\ket{1}$$. Agora se o resultado da medição de Alice é $$\ket{1}$$, então ela sabe que Bob vai ter o valor $$\ket{0}$$. E podemos ver que ambos os estados possuem o coeficiente $$\frac{1}{\sqrt{2}}$$, o que significa que o sistema possui 50% de chance de colapsar para o estado $$\ket{01}$$ e 50% para o estado $$\ket{10}$$.

Outro ponto a ser observado é que temos dois grupos de sistemas os: $$\Phi$$ e os $$\Psi$$. Nos $$\Phi$$, ambos os observadores terão o mesmo resultado após a medição. Enquanto que no $$\Psi$$, os observadores terão valores diferentes.

O circuito para gerar os bells states são listados abaixo.

1. circuito para gerar o $$\Phi^+$$ state <br>

| ![circuito para gerar o $\Phi$ state](/assets/images/bell-states/bell-state-1.png)
||:--:||
<sub><sup>\*Ref: imagem construída pelo autor através do QisKit.</sup></sub>|

2. circuito para gerar o $$\Phi^-$$ state

| ![circuito para gerar o $$\Phi$$ state](/assets/images/bell-states/bell-state-2.png)
||:--:||
<sub><sup>\*Ref: imagem construída pelo autor através do QisKit.</sup></sub>|

3. circuito para gerar o $$\Psi^+$$ state

| ![circuito para gerar o $$\Phi$$ state](/assets/images/bell-states/bell-state-3.png)
||:--:||
<sub><sup>\*Ref: imagem construída pelo autor através do QisKit.</sup></sub>|

4. circuito para gerar o $$\Psi^-$$ state

| ![circuito para gerar o $$\Phi$$ state](/assets/images/bell-states/bell-state-4.png)
||:--:||
<sub><sup>\*Ref: imagem construída pelo autor através do QisKit.</sup></sub>|

Além de gerar um entrelaçamento através dos bell states, também podemos "remover" esse entreleçamento através de um circuito de decoder. A parte legal, é que podemos usar o mesmo decoder para todos os bell states.

| ![Bell state decoder](/assets/images/bell-states/bell-state-decoder.png)
||:--:||
<sub><sup>\*Ref: imagem construída pelo autor através do QisKit.</sup></sub>|

No exemplo acima, primeiro construimos um bell state e depois aplicamos o decoder.

Os bell states também são conhecidos como par EPR.

### Algoritmo de Teletransporte

Você certamente já ouviu falara no algoritmo de teletransporte, mas, ao contrário, do que você imagina, no algoritmo de teletransporte não iremos mover um qubit de posição. Isto é, não iremos pegar um qubit localizado em Porto Alegre e mover ele para Campinas, por exemplo.

O que vamos fazer é mover a informação do qubit localizado em Porto Alegre para o qubit localizado em Campinas.

O Algoritmo de teletransporte é fundamental para a computação quântica porque diferente da computação clássica onde podemos copiar uma informação e transferir para outro lugar, na computação quântica não podemos fazer isso por causa do no clone theorem. Assim, esse algoritmo permite a computação quântica enviar informações a longa distância.

Esse circuito é um pouco maior do que os circuitos que estamos acostumados. O exemplo a seguir, mostra como teletransportar o valor em $$q_0$$ para o $$q_2$$.

| ![Bell state decoder](/assets/images/bell-states/teleportation.png)
||:--:||
<sub><sup>\*Ref: imagem construída pelo autor através do QisKit.</sup></sub>|

O circuito é dividido em 4 partes. A primeira parte é usada para configurar a informação contina no $$q_0$$, ou seja, é usada para definir qual informação será teletransportada.

A segunda parte, é usada para criar um entrelaçamento entre os qubits $$q_1$$ e $$q_2$$ e $$q_0$$ e $$q_1$$ e assim ser possível modificar esses qubits mesmo quando eles não estão fisicamente no mesmo lugar. Nesse exemplo, foi utilizado o bell state $$\Phi^+$$, mas poderiamos ter utilizado qualquer outro.

A parte três faz uma medição para colapsar o sistema e a quarta parte serve como decoder da informação.

As três primeiras partes são executadas por quem vai enviar a informação enquanto que a última parte é executada por quem está recebendo a informação. Apenas o $$q_2$$ é enviado, os outros dois qubits permanecem em posse do emissor.

### Dicas de Leitura

A Wikipedia possui um excelente resumo da história paradoxo EPR, porém a sua explicação não é muito clara. Felizmente, no YouTube temos dois vídeos sobre esse paradoxo ótimos. O primeiro foi publicado pelo canal TED-Ed sob o título [Einstein's brilliant mistake: Entangled states - Chad Orzel](https://www.youtube.com/watch?v=DbbWx2COU0E) e é um vídeo, puramente, de divulgação. Para os mais corajosos temos o vídeo do DrPhysicsA — [The Einstein Podolsky Rosen (EPR) Paradox - A simple explanation](https://www.youtube.com/watch?v=0x9AgZASQ4k) — e também temos o artigo em si no [CERN Document Server](https://cds.cern.ch/record/405662/files/PhysRev.47.777.pdf).

A Wikipedia também possui um excelente post sobre os bell states. Contando toda a sua história e uma explicação breve sobre o que são. O canal DrPhysicsA também possui um excelente vídeo, tentando, explicar de forma mais técnica a demonstração que John Bell fez no seu artigo. Eu não cheguei a ler o artigo original, mas ele pode ser encontrado no site da [American Physical Society](https://journals.aps.org/ppf/abstract/10.1103/PhysicsPhysiqueFizika.1.195).

Já o quantum teleportation Algorithm pode ser encontrado em diversos lugares. A minha primeira recomendação é a própria Wikipedia e também existe um vídeo do [Qiskit](https://www.youtube.com/watch?v=mMwovHK2NrE) no YouTube.

### Referências

- Eleanor Rieffel and Wolfgang Polak. 2011. Quantum Computing: A Gentle Introduction (1st. ed.). The MIT Press.
- [Bell state](https://en.wikipedia.org/wiki/Bell_state)
- [EPR paradox](https://en.wikipedia.org/wiki/EPR_paradox)
- [The Einstein Podolsky Rosen (EPR) Paradox - A simple explanation](https://www.youtube.com/watch?v=0x9AgZASQ4k)
- [Things we know about spin in quantum mechanics](https://www.youtube.com/watch?v=gh7xITmvgyU)
- [Introduction to Bell states in Qiskit with Code](https://quantumcomputinguk.org/tutorials/introduction-to-bell-states)
- [Quantum Teleportation Algorithm — Programming on Quantum Computers Season 1Ep 5](https://www.youtube.com/watch?v=mMwovHK2NrE)
- [Einstein's brilliant mistake: Entangled states - Chad Orzel](https://www.youtube.com/watch?v=DbbWx2COU0E)
- [Can Quantum-Mechanical Description of Physical Reality be Considered Complete?](https://cds.cern.ch/record/405662/files/PhysRev.47.777.pdf)
