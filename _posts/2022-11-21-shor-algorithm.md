---
layout: post
title:  "Algoritmo de Shor"
date:   2022-11-21 07:30:00
author: luccasmarim
categories: teoria computacao-quântica educativo
usemathjax: true
---

# Algoritmo de Shor

## O que é?
Algoritmo de decomposição de um número em produtos de números primos. Útil para achar decomposições em produto não triviais de números compostos utilizados em criptografia
## Por que estudar?
Os algoritmos utilizados atualmente para fatorar números inteiros o fazem em tempo superpolinomial.
O algoritmo de Shor implementado em um computador quântico pode quebrar criptografias baseadas em chave pública (e.g. RSA) em tempo polinomial. Há de se pontuar que fenomenos de quantum noise ou quantum-decoherence interferem, naturalmente, na execução deste algoritmo. Atualmente o problema em utilizar este algoritmo é devido a limitações de hardware nas operações que usam computação quântica.
## Como funciona?
Dado um número N, queremos escrevê-lo como $$N=k_1^{e_1}k_2^{e_2}k_3^{e_3}...k_m^{e_m}$$ onde $k_i \in \N$ é um número primo e $e_j \in \N$ com $0<i,j<m+1$
Este problema é equivalente ao problema de encontrar um número divisor não trivial de N entre 1 e N. De fato, achando um número-produto poderiamos executar novamente o algoritmo para encontrar outro número até que N esteja completamente decomposto:
Seja $N$ nosso número. Encontrando um divisor primo A teriamos: $N_0=N=A*k_1*...*k_n$. Daí, basta achar $N_1=k_1*...*k_n$ e assim sucessivamente.
O algoritmo é composto por duas etapas. A etapa clássica - mais simples e de fácil implementação - e a etapa onde precisaremos de computação quântica - mais sofisticada.
Um algoritmo que resolve nosso problema pode ser resumido assim:

Dado $N$, um número natural a ser fatorado em produtos, 
1. Descobrir se $N$ é primo ou composto: usar primality-testing (apêndice I).
2. Se $N$ for primo então a única decomposição possível é a trivial: 1 e $N$. Acabou
3. Se $N$ for par então repita o algoritmo com $N/2$
4. Use o algoritmo do apêndice IV para verificar se existem $a$ e $b$ tais que $N=a^b$. Retornar $a$ caso positivo.
5. Se não, escolha $A \in \N$ qualquer tal que $1<A<N$
6. Encontre o MDC (MAIOR divisor comum) entre A e N. Seja K este número, isto é, $K=MDC(A,N)$
7. Se K&ne;1 então encontramos um fator não trivial. Acabou.
8. Se K=1, então execute a rotina X para encontrar o período r da função $f(x)=A^{x}(mod N)$. Note que isso significa que r é o menor inteiro positivo que satisfaz $A^{r}=1(mod N)$
9. Se r é impar ou $A^{r/2}=-1(mod N)$ então volte para o passo 1.
10. Se não, $MDC(A^{r/2}+1,N)$ ou $MDC(A^{r/2}-1,N)$ devem ser fatores não triviais de N. Se não forem, então o algoritmo falhou.

Com exceção da etapa 8, a qual usa computação quântica, utilizamos apenas rotinas clássicas. Por isso, focaremos nessa etapa que alguns autores chamam de algoritmo de Shor (ao invés de todo o processo).

### Rotina X:
Antes de começar, introduziremos alguns conceitos importantes para a construção dessa rotina. Estes são: Transformada de Fourier quântica, aritmética modular, algoritmo de Euclides e de frações continuadas. Outras ferramentas importantes para simplificar o algoritmo estarão disponiveis na seção de apêndice.
#### 1. TFQ (Transformada de Fourier Quântica)
A Transformada de Fourier Discreta é um operador linear definido de $C^{N}$ para $C^{N}$ levando $(x_0, x_1, ..., x_{N-1})$ para $(y_0, y_1, ..., y_{N-1})$ tal que:
$$y_k = \frac{1}{\sqrt{N}}\sum_{j=0}^{N-1}x_je^{\frac{2\pi ijk}{N}}$$
Por ser um operador linear unitário (provado em ref. II) este tem representação matricial. Na base canônica de $C^{N}$, esta é:
$$\frac{1}{\sqrt{N}}\left(\begin{matrix}
1&1&1&...&1\\
1&\omega&\omega^2&...&\omega^{N-1}\\
1&\omega^2&\omega^4&...&\omega^{N-2}\\
...&...&...&...&...\\
1&\omega^{N-1}&\omega^{N-2}&...&\omega
\end{matrix}\right)$$
Onde $\omega=e^{\frac{2\pi i}{N}}$.
Em mecânica quântica, substituimos os vetores do tipo $(x_0, x_1, ..., x_{N-1})$ da base canônica por kets do tipo $\sum_{j=0}^{N-1}x_j\ket{j}$. Isto é, as coordenadas (ou amplitudes) continuam as mesmas (existe a bijeção) e a base canônica de $C^{N}$ é escrita como kets $\ket{0}$, $\ket{1}$,...,$\ket{N-1}$. Por exemplo, em N=3: $$(1,0,0)^{t} \rightarrow 1\ket{0}+0\ket{1}+0\ket{2} = \ket{0}$$

A justificativa teórica da construção do circuito que executa a TFQ para qualquer valor de N é um pouco complicada. Ao invés disso trabalharemos com o caso $N=2^n$ que é conveniente uma vez que o número N de entrada será representado por bits. 
Substituindo $N=2^{n}$ com a base ${\ket{0},..., \ket{2^{n}-1}}$ na expressão da transformada de fourier discreta encontramos que a TFQ age num ket $\ket{j} $ de forma:
$$\ket{j}\rightarrow 1/(2^{n/2})\sum_{k=0}^{2^{n}-1}e^{2\pi ijk/(2^{n})}\ket{k}$$ Pode-se mostrar ainda que a TFQ transforma o ket $\ket{j}$ de forma que (ref. III pg39):
$$\ket{j}\rightarrow (\ket{0}+e^{2\pi i0.j_n}\ket{1})\otimes(\ket{0}+e^{2\pi i0.j_{n-1}j_n}\ket{1})\otimes...\otimes(\ket{0}+e^{2\pi i0.j_1j_2...j_n}\ket{1})$$ Onde $0.j_1j_2...j_n = \frac{j_1}{2}+\frac{j_2}{4}+...+\frac{j_n}{2^n}$  com $j_k=0$ ou $1$
Aqui fica claro que o circuito que implementa essa transformação para um estado $\ket{j}=\ket{j_1j_2j_3...j_n}$ é: 

![Circuito-TFQ-2n](/assets/images/shor-algorithm/figura3.1_dissertacao.png)
Fonte: [Dissertação de mestrado](https://repositorio.ufmg.br/bitstream/1843/EABA-85FJXP/1/dissertacao_adrianaxavier.pdf) (2010)
Onde H representa o gate de Hadamard e $R_k$ um gate de controle definido por:
$$\left(\begin{matrix}
1&0\\
0&e^{\frac{2\pi i}{2^k}}
\end{matrix}\right)$$
A matriz inversa da matriz de representação (numa base ortonormal) também terá muita importância. Como a matriz da TFQ é unitária então a matriz hermitiana será igual a inversa. Logo esta é:
$$\left(\begin{matrix}
1&1&1&...&1\\
1&\tau&\tau^2&...&\tau^{N-1}\\
1&\tau^2&\tau^3&...&\tau^{N-2}\\
...&...&...&...&...\\
1&\tau^{N-1}&\tau^{N-2}&...&\tau\\
\end{matrix}\right)$$Onde $\tau=e^{\frac{-2\pi i}{N}}$.
O circuito que executa a operação inversa da TQF é:
![Circuito-TFQ-2n-inversa](/assets/images/shor-algorithm/tqfinversa.png)
Fonte: [Dissertação de mestrado](https://repositorio.ufmg.br/bitstream/1843/EABA-85FJXP/1/dissertacao_adrianaxavier.pdf) (2010)

##### Exemplo da TFQ em n=3
Neste caso a TFQ é um operador unitário que leva $C^8$ em $C^8$ e a matriz que o representa na base {$\ket{ijk}$} com $i,j,k=0$ ou $1$ é:
$$\frac{1}{\sqrt{8}}\left(\begin{matrix}
1&1&1&1&1&1&1&1\\
1&\omega&\omega^2&\omega^3&\omega^4&\omega^5&\omega^6&\omega^7\\
1&\omega^2&\omega^4&\omega^6&1&\omega^2&\omega^4&\omega^6\\
1&\omega^3&\omega^6&\omega&\omega^4&\omega^7&\omega^2&\omega^5\\
1&\omega^4&1&\omega^4&1&\omega^4&1&\omega^4\\
1&\omega^5&\omega^2&\omega^7&\omega^4&\omega&\omega^6&\omega^3\\
1&\omega^6&\omega^4&\omega^2&1&\omega^6&\omega^4&\omega^2\\
1&\omega^7&\omega^6&\omega^5&\omega^4&\omega^3&\omega^2&\omega
\end{matrix}\right)$$ Onde $\omega=e^{\frac{2\pi i}{8}}$.
Aplicando a TFQ as coordenadas de $\ket{000}$ obtemos o vetor: $$\frac{1}{\sqrt{8}}(1 1 1 1 1 1 1 1)^t=\frac{1}{\sqrt{8}}\sum_{i,j,k=0,1}^{}\ket{i,j,k}$$.
Ou seja, a TFQ transforma o ket $\ket{000}$:
$$\ket{000} \rightarrow^{TFQ} \frac{1}{\sqrt{8}}(\ket{000}+\ket{001}+\ket{010}+\ket{100}+\ket{110}+\ket{101}+\ket{011}+\ket{111})$$ Em Python, importando o modulo qiskit, a implementação do circuito fica:

qc = QuantumCircuit(3)
qc.h(2)
qc.cp(pi/2,1,2)
qc.cp(pi/4,0,2)
qc.h(1)
qc.cp(pi/2,0,1)
qc.h(0)
qc.swap(0,2)

![Circuito-TFQ-n3](/assets/images/shor-algorithm/n3tfq.png)
Fonte: [qiskit](https://qiskit.org/textbook/ch-algorithms/quantum-fourier-transform.html#8.-Qiskit-Implementation)
Como o qbit de entrada padrão é $\ket{0}$ o circuito nos retorna o valor esperado. 
A implementação no quantum composer da IBM fica:
![Circuito-TFQ-n3-ibm](/assets/images/shor-algorithm/quantumcomposertfq.png)
Fonte: Autor
Note que as probabilidades estão coerentes com o esperado teórico.



#### 2. Grupo $Z_N$ e aritmética modular      
Será apresentado algumas notações importantes para o entendimento do algoritmo:
$$Z_2 = {\overline{0}, \overline{1}} $$
É, por exemplo, o conjunto de todos os números os quais resultam 0 ou 1 na divisão por 2. Isto é, $\overline{0}$ representa os números pares enquanto $\overline{1}$ os impares. 
Pode-se definir (ref. I) operações de adição e multiplicação nesse conjunto de forma que $Z_n = \overline{0}, \overline{1}, ..., \overline{n-1}$ seja um grupo. A adição pode ser tal que $\overline{a}+\overline{b}=\overline{a+b}$ e a multiplicação, não obstante, pode ser $\overline{a}*\overline{b}=\overline{a*b}$. Note que é extremamente conveniente a forma que definimos essas operações. De fato, para somar ou multiplicar os elementos do grupo bastar somar e multiplicar da forma usual e os elementos neutros serão os usuais.
Definimos também a ordem de um número $a$ como o menor inteiro k tal que $a^{k}=e$ onde $e$ denota o elemento neutro do grupo com respeito a operação de multiplicação
Outra notação mais importante ainda é a de $A (mod N)$. Esta representa o resto da divisão de A por N (em Python usamos: A%N)
Nesta notação, a ordem ou período de um número inteiro A é o menor inteiro $r$ tal que $A^r(modN)=1$ 
Por fim, definimos a congruência modular no conjunto $Z_N$ como $a \equiv b\mod N$ quando $\overline{a}=\overline{b}$.
#### 3. Algoritmo de frações continuadas
A teoria geral de frações continuadas é vasta em conteúdo. Neste artigo abordaremos apenas uma aplicação para um caso particular onde é útil para o algoritmo de Shor.
Queremos expressar um certo número racional $k=\frac{a}{b}$ como combinações de frações dentro de frações conhecidas como frações continuadas ou contínuas.
Uma fração continuada é uma expressão do tipo: $$a_0 + \frac{b_1}{a_1+\frac{b_2}{a_2+\frac{b_3}{a_3+...}}}$$
Nosso caso particular será quando a fração em questão é finita e $b_1=b_2=...=1$. Nesse caso teremos:
$$a_0 + \frac{1}{a_1+\frac{1}{a_2+\frac{1}{...+\frac{1}{a_m}}}}$$
Utilizaremos o algoritmo de Euclides (apêndice V) para computar os termos $a_i$. Achemos, por exemplo, o MDC entre 69 e 15:

$69=4*15+9$
$15=1*9+6$
$9=1*6+3$
$6=2*3+0$

Note que podemos expressar, a partir da primeira linha acima dividida por 15, a fração $\frac{69}{15}$ como:
$$\frac{69}{15}=4+\frac{9}{15}=4+\frac{1}{\frac{15}{9}}$$ Utilizando a segunda linha dividido por 9:
$$\frac{69}{15}=4+\frac{1}{1+\frac{6}{9}}=4+\frac{1}{1+\frac{1}{\frac{9}{6}}}$$ Realizando a mesma operação para a terceira e quarta linha encontramos, por fim:
$$\frac{69}{15}=4+\frac{1}{1+\frac{1}{1+\frac{1}{2}}}$$ Este algoritmo terá fundamental importância para acharmos os termos convergentes de uma fração resultante do algoritmo de Shor.

### Algoritmo para a Rotina X
A rotina X é basicamente um algoritmo quântico de busca de ordem $r$ de um número $x \in Z_{N}$. Tendo este, é fácil achar um fator primo para a decomposição.
Utilizaremos operador unitário (provado em ref. III pg46) definido por $U:C^{n}\rightarrow C^{n}$ tal que:
$$U\ket{k}=\ket{xk modN}$$ Este circuito será de importância fundamental para o funcionamento do algoritmo. Sua construção está no apêndice II. 
Comecemos pelo caso em que $r$ pode ser reescrito como $2^s$ e depois iremos para o caso geral.

##### Circuito da busca de ordem no caso em que a ordem r de um elemento x de $Z_N$ pode ser escrita como $r = 2^{s}$

O primeiro registrador deve possuir $t=[log_2N^2+1]$ qbits no estado $\ket{0}$ e o segundo registrador estará no estado $\ket{00...01}=\ket{1}$. A quantidade de zeros do estado do segundo registrador será $L-1=log_2N$. Note que o valor de L é a quantidade de bits necessária para representar o número $N$ em notação binária. O circuito que implementa [ref. II] nossa busca pela ordem $r=2^s$ é:

![busca-de-ordem-caso-simples](/assets/images/shor-algorithm/figura4.1_dissertacao.png)
Fonte: [Dissertação de mestrado](https://repositorio.ufmg.br/bitstream/1843/EABA-85FJXP/1/dissertacao_adrianaxavier.pdf) (2010) 
OBS.: Aqui os gates representados por $x^{2^j}$ são exatamente iguais aos gates $U^{2^j}$ abordados no apêndice II sendo apenas uma questão de notação.

Começamos por $\ket{\psi_0} = \ket{0...00} \otimes \ket{1}$ o estado inicial conforme o circuito criado.
Aplicamos as portas de Hadamard nos qbits do primeiro registrador resultando em $$\ket{\psi_1} = \frac{1}{\sqrt{2^t}}\sum_{j=0}^{2^{t}-1}\ket{j}\otimes\ket{1}$$
Aplicando as portas $U^{2^j}$ no estado $\ket{\psi_1}$ obtemos:
$$\ket{\psi_2}= \frac{1}{\sqrt{2^t}}\sum_{j=0}^{2^{t}-1}\ket{j}\otimes\ket{x^j}$$ Agora, conforme [ref. III], usamos o fato de que estamos trabalhando no grupo $Z_N$ e uma função periódica de período $r$ para encontrarmos termos do tipo abaixo na somatória:
$$\ket{0}\otimes\ket{1}, \ket{r}\otimes\ket{1}, \ket{2r}\otimes\ket{1}, ...,\ket{2^t-r}\otimes\ket{1}$$ Dessa forma, rearranjando o estado $\ket{\psi_2}$, obtemos [ref. III]:
$$\ket{\psi_2}=\frac{1}{2^{\frac{t}{2}}}\sum_{b=0}^{r-1}(\sum_{a=0}^{\frac{2^t}{r}-1}\ket{ar+b})\otimes\ket{x^b}$$
Note que todos os estados de $\ket{\psi_2}$ são equiprováveis.
Realizando uma medição e supondo, sem perda de generalidade, que o resultado tenha sido $\ket{x^{b_1}}$ teremos como estado atual:
$$\ket{\psi_3}=\sqrt{\frac{r}{2^t}}(\sum_{a=0}^{\frac{2^t}{r}-1}\ket{ar+b_1})\otimes\ket{x^{b_1}}$$
Para descobrir o valor de $a$ aplicaremos o $TFQ^*$ no primeiro registrador (onde * denota o hermitiano da aplicação). Aplicando a matriz no estado $\ket{\psi_3}$ obtemos:
$$\ket{\psi_4}=\frac{1}{\sqrt{r}}(\sum_{k=0}^{r-1}e^{-2\pi ikb_1/r}\ket{\frac{k2^t}{r}})\otimes\ket{x^{b_1}}$$
Enfim realizamos a medição na base computacional. Obtendo $\ket{0}$ quando k=0 não teremos informação sobre o sistema mas se o resultado da medição for $\ket{\frac{k_12^t}{r}}$ por algum $k_1>0$, dividimos por $2^t$ e teremos duas possibilidades: MDC(k,r)=1 ou MDC(k,r) é diferente de 1. No primeiro caso fazemos $x^r = 1 mod N$ e temos a ordem. No segundo caso o denominador será um d que é um fator de r e nesse caso o algoritmo falha.
##### Caso Geral
Os valores de t e L não mudam. O circuito que implementa a busca de ordem no caso geral ainda é:
![busca-de-ordem-caso-geral](/assets/images/shor-algorithm/figura4.2_dissertacao.png)
Fonte: [Dissertação de mestrado](https://repositorio.ufmg.br/bitstream/1843/EABA-85FJXP/1/dissertacao_adrianaxavier.pdf) (2010)
A sequência de estados fica:
$$\ket{\psi_0}=\ket{0}\otimes\ket{1}$$
$$\ket{\psi_1}=\frac{1}{2^{\frac{t}{2}}}\sum_{j=0}^{2^t-1}\ket{j}\otimes\ket{1}$$
$$\ket{\psi_2}=\frac{1}{2^{\frac{t}{2}}}\sum_{j=0}^{2^t-1}\ket{j}\otimes\ket{x^j}$$
$$\ket{\psi_3}=\frac{1}{2^{t}}\sum_{j,k}^{2^t-1}e^{\frac{-2\pi ikj}{2^t}}\ket{k}\otimes\ket{x^j}$$
$$\ket{\psi_4}=\ket{c}\otimes\ket{\alpha_c}$$
com $\ket{c}\in \ket{0}, \ket{1},..., \ket{2^t-1}$ e $\ket{\alpha_c}$ vem da decomposição de $\ket{\psi_3}$ em relação à base computacional do primeiro registrador. 
Por fim, encontrar r dependerá de probabilidade. Nem sempre este algoritmo nos retornará a resposta correta. Para aumentar esta probabilidade fazemos $r=\frac{c}{2^t}$. O algoritmo de frações contínuas nos retornará ele. 
A probabilidade de obter um estado $\ket{c}\ket{x^k}$ na base computacional é:
$$|\frac{1}{q}\sum_{j:x^j\equiv x^k}^{}e^{\frac{-2\pi ijc}{q}}|^2$$
onde $j:x^j\equiv x^k$ significa todos os j para os quais $x^j\equiv x^k modN$ e $q=2^t$. Pode-se (ref III) mostrar que isso é equivalente a uma expressão do tipo: 
$$|\frac{1}{q}(\frac{1-e^{-2\pi i\alpha (rc)_q/q}}{1-e^{-2\pi i (rc)_q/q}})|$$ onde $\alpha = \frac{q-\beta}{r}$ com sendo o resto da divisão de q por r.
Na referência II temos uma analise mais detalhada de quando o algoritmo falha, quando da certo e quando nos retorna nenhuma informação útil.




#### Busca de ordem de um número e a fatoração de números inteiros

Sabendo a ordem $r$ de um número $x \in Z_N$, fica fácil achar o divisor do número N. Aqui daremos um exemplo prático de como isso acontece.

##### Exemplo (ref. II)

Vamos fatorar o número 21 utilizando o algoritmo segundo a refII:
$\textbf{Passo 1}$: Pode-se usar o algoritmo de primality testing para verificar que N não é primo. Trata-se de um número composto por $3*7$       
$\textbf{Passo 2}$: Como 21 não é primo ignoramos essa etapa.
$\textbf{Passo 3}$: 21 Não é par. Ignoramos essa etapa.
$\textbf{Passo 4}$: Utilizando o algoritmo descrito no apêndice IV descobrimos que não existe $a$ e $b$ inteiros positivos para os quais $21=a^b$.
$\textbf{Passo 5}$: Escolhemos $2 \in$ [$1, 20$] um número qualquer entre $1$ e $20$.
$\textbf{Passo 6}$: Daí, $K$=MDC(2,21)=1.
$\textbf{Passo 7}$: Não ocorre. Ignoramos.
$\textbf{Passo 8}$: Executamos a rotina X para encontrar o período ou ordem do número 2. Isto é, $r$ tal que $2^r=1(mod21)$.
Nesse caso, $t=[log_221^2]+1=9$ (o valor exato é $9.78463...$ mas consideramos apenas o primeiro digito) e portanto teremos 9 qbits iniciados no estado $\ket{0}$ no primeiro registrador. Como $L=[log_221+1]=5$, o estado do segundo registrador será $\ket{1}=\ket{00001}$

A porta $U: C^{2^5} \rightarrow C^{2^5} $ definida para $\ket{y}$ será: $$U\ket{y}=\ket{(2y)mod21}$$ O estado inicial será:
$$\ket{\psi_0}=\ket{00...01}=\ket{0}\otimes \ket{0}\otimes \ket{0}\otimes \ket{0}\otimes \ket{0}\otimes \ket{0}\otimes\ket{0}\otimes \ket{0}\otimes \ket{0}\otimes \ket{1}$$ Aplicando Hadamard nos 9 qbits do primeiro registrador: $$\ket{\psi_1}=\frac{1}{\sqrt{2^9}}\sum_{j=0}^{511}\ket{j}\otimes \ket{1}$$ Aplicando as portas $U^{2^j}$ onde $j$ vai de 0 até 8 obtemos: $$\ket{\psi_2}=\frac{1}{\sqrt{512}}\sum_{j=0}^{511}\ket{j}\otimes \ket{2^j mod21}=$$ $$\frac{1}{\sqrt{512}}(\ket{0} \ket{1}+\ket{1} \ket{2}+\ket{2} \ket{4}+\ket{3} \ket{8}+\ket{4} \ket{16}+\ket{5} \ket{11}+ \\\ket{6} \ket{1}+\ket{7} \ket{2}+\ket{8} \ket{4}+\ket{9} \ket{8}+\ket{10} \ket{16}+\ket{11} \ket{11}+\\ \ket{12} \ket{1}+\ket{13} \ket{2}+\ket{14} \ket{4}+\ket{15} \ket{8}+\ket{16} \ket{16}+\ket{17} \ket{11}+\\...\\ +\ket{510} \ket{1}+\ket{511} \ket{2})$$ Agrupando os termos: $$=\frac{1}{\sqrt{512}}((\ket{0}+\ket{6} +\ket{12}+\ket{18}+\ket{24}+...+\ket{510})\ket{1}+ \\ (\ket{1}+\ket{7}+\ket{13}+\ket{19}+\ket{25}+...+\ket{511})\ket{2}+\\ (\ket{2}+\ket{8}+\ket{14}+\ket{20}+\ket{26}+...+\ket{506})\ket{4}+\\  (\ket{3}+\ket{9}+\ket{15}+\ket{21}+\ket{27}+...+\ket{507})\ket{8}+\\ (\ket{4}+\ket{10}+\ket{16}+\ket{22}+\ket{28}+...+\ket{508})\ket{16}+\\  (\ket{5}+\ket{11}+\ket{17}+\ket{23}+\ket{29}+...+\ket{509})\ket{11})$$ Note que visualizando dessa forma, fica claro que podemos decompor o estado atual como uma combinação dos estados {$\ket{1}, \ket{2}, \ket{4}, \ket{8}, \ket{16}, \ket{11}$}. 
O próximo passo, seguindo a figura lá de cima é realizar uma medição no sistema do segundo registrador. Suponha que o resultado da medição seja $\ket{2}$. Daí o sistema passa a ser descrito pelo estado: $$\ket{\psi_3}=\frac{1}{\sqrt{86}}(\ket{1}+\ket{7}+\ket{13}+\ket{19}+\ket{25}+...+\ket{511})\otimes \ket{2}\\=\frac{1}{\sqrt{86}}\sum_{a=0}^{85}\ket{6a+1}\otimes \ket{2}$$ onde o termo $\sqrt{86}$ foi construido para normalização do estado.
Aplicando a Transformada de Fourier Inversa obtemos: $$\ket{\psi_4}=\frac{1}{\sqrt{512}}\frac{1}{\sqrt{86}}\sum_{j=0}^{511}([\sum_{a=0}^{85}e^{\frac{-2\pi ij6a}{512}}]e^{\frac{-2\pi ij}{512}})\ket{2}$$ Encontrando, por fim, a probabilidade de encontrar um estado $\ket{j}$ (tiramos o modulo ao quadrado): $$P(j)=\frac{1}{512*86}|\sum_{a=0}^{85}e^{\frac{-2\pi i6ja}{512}}|^2$$ $$=\frac{1}{512*86}\frac{1-cos(\frac{86\pi 3j}{128})}{1-cos(\frac{3\pi j}{128})}$$ que tem máximo global em $\frac{512k}{6}$ com $k$ entre 0 e 5. Será a partir desse $k$ que encontraremos a ordem $r$ de 2.
O próximo passo é medirmos agora na base canônica. Suponha que seja retornado o valor $j=85$. Dividindo por 512 ainda não teremos 6 no denominador.
Usemos o algoritmo de frações continuadas para achar convergentes menores que 21:
$$\frac{85}{512}=\frac{1}{\frac{512}{85}}=\frac{1}{6+\frac{2}{85}}=\frac{1}{6+\frac{1}{42+\frac{1}{2}}}$$ Disso temos que os candidatos (convergentes) são: $\frac{1}{6}$, $\frac{42}{253}$ e $\frac{85}{512}$. Testemos $\frac{1}{6}$: Selecionando o denominador e calculando $2^6=1mod21$ encontramos o valor de $r$ como $r=6$.
Entretanto, por se tratar de um algoritmo probabilistico, poderiamos ter outros valores retornados. É possível que nosso algoritmo nos retorne um valor inútil ou pior ainda, falhe quando tivermos alguns resultados especificos. Por exemplo, se $j=0$ e não 85, não teríamos nenhum resultado útil para os convergentes. Se $j=171$ encontrariamos 3 candidatos que não nos fornece a ordem de 2.
Continuando com o algoritmo, como $r=6$ é par e $2^\frac{r}{2}=2^3=8$ que é diferente de $-1mod21$. Daí, calculamos MDC($2^3-1$, $21$)=7 e MDC($2^3+1$, $21$)=3. Encontramos assim dois fatores de 21 conforme queriamos.


                
## Apêndice I (primality-testing algorithm):
Algoritmo simples que usa o fato de todos os divisores de um número n serem menores ou iguais a n/2. Em Python, retornando True para primo e False para composto:
                                                           
def is_prime(n: int) -> bool:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if n<=3:
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return n>1            
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if n%2 == 0 or n%3 == 0:
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return False
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;limit = int(n**0.5)
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for i in range(5, limit+1, 6):
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if n%i==0 or n%(i+2)==0:
                &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return False                
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return True

Existem outros algoritmos mais sofisticados para n grande.
    

## Apêndice II (gate U)
Este gate é comumente conhecido como quantum phase estimation. Trata-se de um operador unitário (pode-se provar isso) definido por:
$$U\ket{y}\equiv \ket{(ay) modN}$$
Isto é, $U$ transforma um ket $\ket{y}$ em um ket que guarda o resto da operação $\frac{ay}{N}$.
Exemplos:
1. Tomando N=2 e A=25: $$U\ket{1}=\ket{25mod2}=\ket{1}$$ Logo, neste caso, $r=1$ uma vez que $\ket{25mod2}=\ket{25^1mod2}$
2. Tomando N=35 e A=3: $$U\ket{1}=\ket{3 mod35}=\ket{3}$$ 
Uma vez que o algoritmo da divisão nos retorna $3=(35*0)+3$ onde 0 é o resultado da divisão $\frac{3}{35}$.
Neste caso teriamos também: $$U^2\ket{1}=U\circ U\ket{1}=U\ket{3}=\ket{3*3mod35}=\ket{3^2mod35}=\ket{9}$$ $$U^3\ket{1}=U\circ U^2\ket{1}=U\ket{9}=\ket{9*3mod35}=\ket{3^3mod35}=\ket{27}$$ $$\vdots$$ $$U^{r-1}\ket{1}=\ket{12}$$ $$U^r\ket{1}=U\circ U^{r-1}\ket{1}=U\ket{12}=\ket{3^{12}mod35}=\ket{1}$$ daonde temos que $r=12$

Chegar ao estado $\ket{1}$ após aplicações sucessivas de $U$ não é coincidência. Por construção, iniciando em $\ket{1}$ chegaremos novamente em $\ket{1}$ após r aplicações de $U$. Daí o nome de período da função.

Seja $\ket{x_s}$ o estado definido por: $$\ket{x_s}=\frac{1}{\sqrt{r}}\sum_{k=0}^{r-1}e^{\frac{-2\pi sik}{r}}\ket{A^kmodN}$$ Onde $0\leq s\leq r-1 \in \N$.  Note que este está dentro do espaço de estados por ser combinação linear de autovetores de U. O termo de fase $e^{\frac{-2\pi sik}{r}}$ aparece porque estamos interessados em estados cuja fase difere do estado $\ket{x}=\frac{1}{\sqrt{r}}\sum_{k=0}^{r-1}\ket{A^kmodN}$ o qual nos retorna como autovalor 1 (ref V). O fator $r$ é acrescentado porque é útil associarmos o período da função com cada coeficiente. O termo $s$ é acrescentado para generalizar essa diferença de fase e garantir a unicidade de cada autoestado para cada valor de s.
Aplicando U, pode-se mostrar (ref V) que: $$U\ket{x_s}=e^{\frac{2\pi si}{r}}\ket{x_s}$$ que tem autovalores $e^{\frac{2\pi si}{r}}$.
Não obstante, ainda teremos que (ref V) $$\frac{1}{\sqrt{r}}\sum_{s=0}^{r-1}\ket{x_s}=\ket{1}$$ Daí, temos que $\ket{1}$ é uma superposição desses estados. Realizando QPE (apêndice III) em U usando o estado $\ket{1}$ mediremos a fase $\phi = \frac{s}{r}$. Usamos o algoritmo de frações continuadas em $\phi$ para achar $r$. O circuito que implementa isso é:

![frac_cont_gate](/assets/images/shor-algorithm/shor_circuit_1.svg)
Fonte: [Qiskit-Algoritmo Shor](https://qiskit.org/textbook/ch-algorithms/shor.html)

## Apêndice III (aplicação do gate U)
Dado um operador unitário $U$, queremos estimar um ângulo para o qual $U\ket{\psi}=e^{2\pi i\theta}\ket{\psi}$.
Começamos com dois conjuntos de qbits. O primeiro conjunto terá qbits configurados para estarem em $\ket{0}$ e o segundo conjunto para o estado $\ket{\psi}$ de forma que nosso estado inicial seja representado por $$\ket{\psi_0}=\ket{0}\otimes\ket{0}\otimes...\otimes\ket{0}\otimes\ket{\psi}$$ onde temos, por exemplo, n vezes o produto tensorial entre os $\ket{0}$. Aplicamos, após isso, n portas de Hadamard (isto é, $H\otimes H\otimes... \otimes H\otimes I$). Teremos, portanto:
$$\ket{\psi_1}=\frac{1}{2^{n/2}}(\ket{0}+\ket{1})^{\otimes n}\otimes\ket{\psi}$$
Definimos o gate unitário de controle $CU$ que aplica $U$ no estado $\ket{\psi_i}$ se e só se o bit de controle for $\ket{1}$.
Como $U\ket{\psi}=e^{2\pi i\theta}\ket{\psi}$ então:
$$U^{2^j}\ket{\psi}=U^{2^j-1}U\ket{\psi}=U^{2^j-1}e^{2\pi i\theta}\ket{\psi}=...=e^{2\pi i2^j\theta}\ket{\psi}$$ com $0\leq j\leq n-1$. Aplicando os $n$ $CU^{2^j}$ no primeiro conjunto de qbits pode-se mostrar que:
$$\ket{\psi_2}=\frac{1}{2^{n/2}}\sum_{k=0}^{2^n-1}e^{2\pi i\theta k}\ket{k}\otimes \ket{\psi}$$ Aplicamos agora a transformada inversa de fourier quântica em todos os qbits do primeiro conjunto, conforme mostrado la em cima, obtemos:
$$\frac{1}{2^{n/2}}\sum_{k=0}^{2^n-1}e^{2\pi i\theta k}\ket{k}\otimes \ket{\psi}\rightarrow \ket{\psi_3}=\frac{1}{2^n}\sum_{x=0}^{2^n-1}\sum_{k=0}^{2^n-1}e^{\frac{-2\pi i k}{2^n}(x-2^n\theta)}\ket{x}\otimes\ket{\psi}$$ Por fim, a expressão acima tem pico perto de $x=2^n\theta$. Se $2^n\theta\in\N$ teremos que uma medição na base computacional nos da a fase no registrador auxiliar com alta probabilidade:$$\ket{\psi_4}=\ket{2^n\theta}\otimes\ket{\psi}$$ se não, a probabilidade de acerto é aproximadamente $40%$ [fonte1 refV ]. Estas operações podem ser representadas pela figura abaixo
![QPErepresentatio](/assets/images/shor-algorithm/qpe_tex_qz.png)
Fonte: [Qiskit-Quantum Phase Estimation](https://qiskit.org/textbook/ch-algorithms/quantum-phase-estimation.html)

## Apêndice IV (algoritmo $N=a^b$)
Suponha que N possa ser escrito como $N=a^b$ com $a$,$b$ inteiros.
$$N=a^b \implies log_2N = b*log_2a \implies L>b*log_2a$$ Onde $L=[log_2N+1]$ é a quantidade de bits para representar o número $N$. 
1. Escolha i = 2
2. Defina $x=\frac{log_2N}{i}$
3. Encontre os dois inteiros mais próximos de $2^x$: $u_1$ e $u_2$
4. Compute $u_1^i$ e $u_2^i$. Se um dos resultados for igual a $N$ então retorne o par ($i$,$u_j$) tal que $u_j^i=N$. Se não, i=i+1 e retorne a 1.

Note que o algoritmo é de simples implementação. Tua complexidade é $O(L^4)$ conforme mostrado na refII.

##### Exemplo:
Seja N=9. 

Iniciando por i=2 temos $x=\frac{log_29}{2}=1.58496...\approx1.585$. 
Daí, $2^{1.585}\approx3$ e portanto $u_1=3$ e $u_2=4$.
Calculamos $u_1^2=9$ e $u_2^2=16$. Retornamos portanto o par (2, $u_1=3$) = ($a$, $b$). 

## Apêndice V (algoritmo de Euclides)
Este algoritmo serve para encontrar o maior divisor comum (MDC) entre dois números naturais. 
Sejam $a$ e $b$ dois números inteiros positivos dados. 
1. Defina $c_1=a$, $d_1=b$ e $i=1$
2. Divida $c_i$ por $d_i$ e retorne $r_i$ o resto dessa divisão
3. Se $r_i=0$ retorne $d_i$. 
Se não, $c_{i+1}=d_i$ enquanto $d_{i+1}=r_i$ e, por fim, i=i+1.
##### Exemplo:
Seja $a=348$ e $b=156$.
Primeira iteração:
$r_1=36$ pois $348=156*2+36$ (algoritmo da divisão).
Segunda iteração:
$c_2=156$, $d_2=36$ e $r_2=12$ pois $156=36*4+12$
Terceira iteração:
$c_3=36$, $d_3=12$ e $r_3=0$ pois $36=12*3+0$

Logo, MDC(9,6)=3

## Referências
I. QFT, Period Finding & Shor's Algorithm [https://courses.edx.org/c4x/BerkeleyX/CS191x/asset/chap5.pdf]
II. Algoritmo de Shor e sua aplicação à fatoração de números inteiros, Adriana Xavier Freitas, UFGM. Dissertação de Mestrado com orientação de Marcelo Terra Cunha
III. Marcelo Terra Cunha - Curso de mecânica quântica para matemáticos em formação
IV. Shor's Algorithm do Qiskit [https://qiskit.org/textbook/ch-algorithms/shor.html] visitado em 07/12/2022.
V. Quantum Computation and Quantum Information - Michael A. Nielsen (2000)


### Futuro
* Complexidade dos algoritmos apresentados
* Implementação (usando bibliotecas prontas) e comparação entre algoritmo clássico nos casos em que N é grande.
* Explicar/entender melhor quando o algoritmo falha e quando não retorna nada de útil.


