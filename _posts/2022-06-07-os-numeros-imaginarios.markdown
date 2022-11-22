---
layout: post
title:  "Os Números Imaginários, uma base para Computação Quântica"
date:   2022-06-07 12:13:00
author: carla_dias
categories: matemática computacao-quântica educativo números-imaginários
usemathjax: true
excerpt_separator: <!--more-->
---

Os números complexos são a base para a construção do estado quântico.  Os números foram criados e desenvolvidos a partir de problemas humanos e da mesma forma não foi diferente com os números complexos. 

<!--more-->

### Um pouco de história

Os números foram criados e desenvolvidos a partir de problemas humanos. Os números naturais resolviam o problema de ter que gerenciar os recursos. Saber se os recursos foram alterados, aumentando ou diminuindo de alguma maneira. Conseguir determinar se haveriam recursos para todas as pessoas do grupo. E perceber se há necessidade de ir buscar mais recursos ou se dá para descansar. Os números inteiros foram desenvolvidos na idade média junto ao surgimento dos bancos. A ideia que você poderia ter continuar removendo um valor mesmo quando não há mais itens a remover continuam dando outros números com sentido oposto. Os números decimais e fracionários são mais antigos, e surgem da necessidade de se dividir os recursos de forma igualitária.
Um número real pode ser considerado uma "(...) distância, medida em termos de uma dada unidade, com um sentido que lhe é conferido pelo sinal" (Conway 1999, p.230). 

Até o século XVI, não havia soluções para equações do segundo grau tais como $$x^2 + 1 = 0$$. Sabendo-se da impossibilidade de multiplicar dois números e encontrar um valor negativo, muitos matemáticos paravam por aí. Gerônimo Cardano (1501-1576), em sua obra *Ars Magna*, falava do seguinte problema: "Determinar dois números cuja soma seja 10 e o produto seja 40". Para tal considerou as expressões $$ 5 + \sqrt{15}$$ e $$ 5 - \sqrt{-15}$$, sem dar mais significado a elas. Conta-se que ele aprendeu com Tartalli.

Raffaelle Bombelli (1526-1572), numa obra de nome *Algebra*, procurava resolver equações de terceiro grau. Bombelli estava a trabalhar com o seguinte problema: "Seja $$ x^3 $$ o volume de um cubo de aresta $$x$$ e $$15x$$ o volume de um paralelepípedo retângulo cuja área da base é 15 e cuja altura é igual à aresta do cubo. Determine $$x$$ de modo que $$x^3 = 15x + 4$$". A resposta por tentativa é $$ x = 4$$. Porém, aplicando-se a fórmula de cardano nesta equação de terceiro grau, apareceu na solução uma raiz quadrada de número negativo: $$ x = ( 2 - \sqrt{-121})^{1/3} + ( 2 + \sqrt{-121})^{1/3}$$.

Teve a estranha ideia de procurar $$a$$ e $$b$$ positivos tais que $$ a + \sqrt{-b} $$ e $$ a - \sqrt{-b}$$ quando somados fossem iguais as suas raízes. 
Ou seja:

$$ x = ( 2 - \sqrt{-121})^{1/3} + ( 2 + \sqrt{-121})^{1/3} = (2 + \sqrt{-1}) + (2 - \sqrt{-1}) = 4 $$

Bombelli também apresentou as leis algébricas que regiam os cálculos entre números da forma $$ a + b \sqrt{-1}$$. Em partícular, mostrou que as 4 operções aritméticas sobre números complexos produzem números ainda da mesma forma. Ou seja, o conjuntos dos complexos é fechado para estas operações.

Um grande passo no estudo dos números complexos foi a sua representação visual. O primeiro a representar geometricamente os números complexos foi Caspar Wessel em 1797, mas foi Jean Argand a quem ficou ligada o nome. 

O símbolo $$i$$, para a representação de $$\sqrt{-1}$$ foi usado pela primeira vez por Euler, mas só após o seu uso por Gauss em 1801 é que foi aceite. 
A expressão número complexo foi introduzida em 1832 por Gauss.

Desta forma, podemos definir os números complexos "(...) como distâncias ao logo de direções arbitrárias num plano fixado." (Conway, 1999, p230).

![Experimento com projéteis](/assets/images/numeros-imagninarios/complexos_Figura4.gif)
<sub><sup>*Ref: https://www.somatematica.com.br/emedio/complexos/complexos.php* </sup></sub>

### Propriedades dos números complexos


#### Par ordenado e Vetor

Seja um número complexo $$z$$ da forma:

$$ z = a + bi; a, b \in \mathbb{R} $$

Podemos representar este número como um vetor em um plano complexo bidimensional.

![Experimento com projéteis](/assets/images/numeros-imagninarios/numeros-complexos2.jpg)
<sub><sup>*Ref: https://www.infoescola.com/matematica/numeros-complexos/* </sup></sub>


Desta maneira, um par ordenado pode ser identificado como um ponto, como um vetor e ainda como um número complexo. O número complexo, nada mais é do que uma grandeza comporta de duas dimensões.

#### Módulo

O módulo do número complexo é o comprimento do vetor que se formaria da origem ao ponto $$(a,b)$$. Pelo teorema de Pitágoras:

$$ (\|z\|)^2 = a^2 + b^2 $$


$$ \|z\| = \sqrt{a^2 + b^2} $$

#### Representação trigonométrica e polar

Existe uma maneira chamada trigonométrica de representar os números complexos:

$$ z = a + bi $$

$$ z = \|z\|  cos(\theta) + \|z\| sen(\theta) i $$

$$ z = \|z\| (cos(\theta) + i sen(\theta)) $$

Aqui, $$\theta$$ é chamado argumento ou fase de $$z$$.

Se aplicada, ainda a Fórmula de Euler:

$$ e^{i\theta} = cos(\theta) + i sen(\theta) $$

Teremos a forma polar, fazendo $$\|z\| = r$$

$$ z = re^{i\theta} $$

#### Complexo Conjugado

Considerando um número complexo $$ z = a + bi $$, o seu complexo conjugado $$ \bar{z} = a - bi$$.

![Experimento com projéteis](/assets/images/numeros-imagninarios/C_6_v4_sabsegunda.png)
<sub><sup> *Ref: https://wikiciencias.casadasciencias.org/wiki/index.php/Conjugado_de_um_n%C3%BAmero_complexo* </sup></sub>

O complexo conjungado pode ser vista como a inversão de sinal da parte imaginária do número, ou na forma trigonométrica como a inversão da fase.

![Experimento com projéteis](/assets/images/numeros-imagninarios/C_6.1FF_novo_2_sab.png)
<sub><sup> *Ref: https://wikiciencias.casadasciencias.org/wiki/index.php/Conjugado_de_um_n%C3%BAmero_complexo* </sup></sub>


Uma relação interessante existe nos números complexos em relação ao seu módulo:

$$ z . \bar{z} = (a + bi) (a - bi) $$

$$ z . \bar{z} =  a^2 -abi + abi -(bi)^2  $$

$$ z . \bar{z} =  a^2 -(bi)^2  $$

$$ z . \bar{z} =  a^2 -(b^2 . -1)  $$

$$ z . \bar{z} =  a^2  + b^2  $$

$$ z . \bar{z} =  \|z\|^2  $$

### As aplicações

Um número imaginário não serve para medir a quantidade de água num copo, nem para contar o número de dedos que temos! No entanto, existem algumas medidas no nosso mundo onde os números complexos são medidores perfeitos. Um campo eletromagnético é um exemplo: ele tem uma componente elétrica e outra magnética e por isso, é preciso um par de números reais para o descrever! Este par pode ser visto como um número complexo e encontramos, assim, uma aplicação direta na Física, para a estranha regra da multiplicação de números complexos.

Mas como o conjunto dos números complexos tem a propriedade em que se pode resolver qualquer equação polinomial de qualquer grau. Então uma aplicação interessante dos números complexos é realizar o calculo das raízes usando números complexos e depois consideramos apenas aquelas que, afinal, são reais. 

#### Computação Quântica

O primeiro ponto de aplicação dos números complexos é que a Equação de Shrodinger traz em si a constate $i$. A equação de Shoringer é uma equação que tenta determinar a evolução de um sistema quântico qualquer. 

![Experimento com projéteis](/assets/images/numeros-imagninarios/img308.png)

Quando aplicada por exemplo, ao movimento livre de uma partícula, ao longo do eixo $$x$$, em uma única direção é descrito pela seguinte função de onda: 

$$ \psi(x) = A e^{ikx} $$

Opa, se nos lembrarmos bem... É a descrição de um número complexo se trocarmos $$A$$ por $$r$$ e $$kx$$ por $$\theta$$.

Para se calcular a probabilidade de a partícula se encontrar em determinada região é preciso resolver uma integral do tipo:

$$ \int_{a}^{b} \| \psi(x) \|^2 dx $$

Conhecendo-se as propriedades de números complexos:

$$ \int_{a}^{b} A e^{ikx} A e^{-ikx} dx  = A^2 \int_{a}^{b} dx = A^2 (b-a) $$


### Referências

- <https://pt.wikipedia.org/wiki/Ficheiro:Conjugado_de_um_N%C3%BAmero_Complexo.png>
- <https://pt.wikipedia.org/wiki/N%C3%BAmero_complexo>
- <https://noic.com.br/materiais-fisica/ideias/fisica-ideia-37/>
- <https://www.infoescola.com/matematica/numeros-complexos/>
- <https://www.ime.unicamp.br/~tcunha/Teach/bit_quantico_VIIIBienal.pdf>
- <https://wikiciencias.casadasciencias.org/wiki/index.php/Conjugado_de_um_n%C3%BAmero_complexo>
- <https://www.somatematica.com.br/emedio/complexos/complexos.php>
