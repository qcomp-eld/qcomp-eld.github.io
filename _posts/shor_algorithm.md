---
layout: post
title:  "Algoritmo de Shor"
date:   2022-11-21 07:30:00
author: luccasmarim
categories: teoria computacao-quântica educativo
usemathjax: true
---


# O que é?
Algoritmo de decomposição. Dado um número N, encontra os fatores primos que o compõem no produto. Útil para achar decomposições não triviais
# Por que estudar?
Utilizando este algoritmo em um computador quântico podemos quebrar, por exemplo, a criptografia RSA em tempo polinomial (mais rápido). Há de se pontuar que fenomenos de quantum noise (apêndice II) ou quantum-decoherence (apêndice III) interferem neste algoritmo.
# Como funciona?
Dado um número N, nosso problema é equivalente ao problema de encontrar um número divisor não trivial de N entre 1 e N. De fato, achando um número poderiamos executar novamente o algoritmo para encontrar outro número até que N esteja completamente decomposto.
Passos:
1. Descobrir se N é primo ou composto: usar primality-testing (apêndice I)
2. Se N for primo então a única decomposição possível é a trivial: 1 e N. Acabou.
3. Se não, escolha A &isin; &alefsym; qualquer tal que 1<A<N
4. Encontre o MDC (MAIOR divisor comum) de A e N. Seja K este número.
5. Se K&ne;1 então encontramos um fator não trivial. Acabou.
6. Se K=1, então execute a rotina X para encontrar o período r da função $f(x)=A^{x}(mod N)$. Note que isso significa que r é o menor inteiro positivo que satisfaz $A^{r}=1(mod N)$
7. Se r é impar ou $a^{r/2}=-1(mod N)$ então volte para o passo 1.
8. Se não, $MDC(A^{r/2}+1,N)$ ou $MDC(A^{r/2}-1,N)$ são fatores não triviais de N. Acabou.
                                                             
### Rotina X (parte que usa de fato computação quântica para encontrar r):
Seja $Q$ tal que $Q=2^{q}$ e $N^{2}$ &le; $Q$ < $2N^{2}$. Daí, $Q/r>N$.
<br>
Os qbits de input e output precisam ser de superposições entre 0 e Q-1 (cada um tem q qbits).
<br>
Algoritmo:
1. Começamos usando a decomposição de Schmidt(confirmar com terra):<br>
$1/\sqrt[]{Q} \sum_{x=0}^{Q-1} |x> = (1/\sqrt[]{2} \sum_{x_1=0}^{1} |x_1>)\otimes...\otimes((1/\sqrt[]{2} \sum_{x_q=0}^{1} |x_q>))$
<br>
TERMINAR????

                                                             
                                                             
                                                             
## Apêndice I (primality-testing algorithm):
Algoritmo simples que usa o fato de todos os divisores de um número n serem menores ou iguais a n/2. Em Python:
<br>                                                             
def is_prime(n: int) -> bool:
<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  if n<=3:
  <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
          return n>1
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  if n%2==0 or n%3==0:
    <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
          return False
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  limit = int(n**0.5)
    <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  for i in range(5, limit+1, 6):
    <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
          if n%i==0 or n%(i+2)==0:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                return False
                  <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  return True
    <br>
    
## Apêndice II (quantum noise)
## Apêndice III (quantum-decoherence)

## Referências
* wikipedia/eng
* Chapter 5 of QFT, Period Finding & Shor's Algorithm


# O que falta?
* Terminar Rotina X? tema de dissertação de mestrado da aluna do meu professor
* Implementação?
* Apêndices II e III
