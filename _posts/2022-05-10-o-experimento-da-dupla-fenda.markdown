---
layout: post
title:  "O Experimento da Dupla Fenda"
date:   2022-05-10 14:24:00
categories: teoria computacao-quântica educativo
usemathjax: true
---

Este experimento é interessante por que ilustra as diferenças entre o mundo intuitivo que temos construído e o mundo que é construído a partir dos experimentos em escala atômica. 

### O Experimento de Young

A primeira vez que se tem conhecimento que este experimento foi realizado foi por [Thomas Young](https://en.wikipedia.org/wiki/Thomas_Young_(scientist)) em 1801. Nesta época tinha-se o pensamento de que a luz consistia ou de ondas ou de partículas. Newton era defensor de que a luz fosse composta de partículas enquanto que Huygens defendia que a luz fosse uma onda. Thomas Young foi importante para a construção da ideia de que a luz tinha uma natureza ondular. Ja que ele demonstrou que com a luz acontecia o efeito chamado de [Difração](https://pt.wikipedia.org/wiki/Difra%C3%A7%C3%A3o), um fenômeno físico que ocorre quando uma onda se deforma após encontrar um obstáculo.

![Difração nas ondas do Mar](/assets/images/dupla-fenda/difracao_ondas_mar.jpg)

Acontece então que pelo princípio de Huygens cada ponto de uma frente de onda possui a funcionalidade de uma nova fonte pontual. Neste processo, cada nova onda interage uma com as outras de forma a compor a nova frente de onda resultante. Este processo de interação é chamado *Interferência*.

![Difração segundo os princípios de Huygens](/assets/images/dupla-fenda/500px-Refraction_on_an_aperture_-_Huygens-Fresnel_principle.svg.png)


O padrão de interferência varia conforme o tamanho da abertura em relação ao tamanho do cumprimento de onda.

![Aberturas diferentes](/assets/images/dupla-fenda/tamanhos_diferentes_abertura.jpg)
![Aberturas diferentes](/assets/images/dupla-fenda/tamanho_cumprimento_onda.jpg)


### O Experimento

O experimento consiste em projetar um feixe de luz através de duas fendas e perceber a figura projetada em um anteparo colocado depois da fenda.

![O Experimento](/assets/images/dupla-fenda/young.jpg)

Ao invés de mostrar apenas o formato das duas fendas no anteparo, é possivel ver mais que duas figuras projetadas no fundo da caixa.

Se a luz fosse de pequenas unidades de matéria, estas iriam se comportar passando por uma pequena fresta ou por outra, seguindo em linha reta e apresentando o seguinte padrão no fundo da caixa.

![Padrão de Interferência com uma única fenda](/assets/images/dupla-fenda/padrao_particula.jpg)

Porém, o visto é um padrão chamado de interferência, em que há posições em que acontece interferência construtiva e há posições em que acontece interferência destrutiva.



### Experimentando a Difração


Aqui, usei este [simulador](https://www.openstaxcollege.org/l/28interference) na opção Diffraction para observamos as possíveis figuras projetadas. No experimento, um lazer com comprimento de onda variável é apontado para um anteparo com algumas opções de aberturas. 

![Padrão de Interferência com uma única fenda](/assets/images/dupla-fenda/unica_fenda.PNG)

Podemos perceber então, um padrão em que a figura formada é muito maior que a fenda, quanto menor inclusive o tamanho da fenda maior o espalhamento da figura.
A figura apresenta uma maior intensidade no meio e vai diminuindo sua intensidade conforme se afasta do centro, porém, existem alguns pontos em que nenhuma luz chega
em que presumimos uma interferência negativa, ou destrutiva, aconteceu.

E qual seria o padrão visto no caso de haver duas fendas?

![Padrão de Interferência com duas fendas](/assets/images/dupla-fenda/duas_fendas_experimento.PNG)

É um padrão de interferência.

### O Experimento Mental de Feyman

Devido a dificuldade de realizar o experimento da dupla fenda com um disparador de elétrons, Feyman, construiu um arranjo experimental didático para explicar aos seus alunos alguns conceitos da mecânica quântica. Este arranjo experimental era dito "mental" por que ainda não havia sido demonstrado em laboratório em sua versão com um único elétron. 


#### O caso dos projéteis

A ideia era repetir a construção de Young, uma vez utilizando um atirador de projéteis que atiraria através das fendas. As balas só ultrapassam através das fendas. Varios detectores são espalhados pelo anteparo depois das fendas. Ao final de um certo tempo em que o disparador atira projéteis, conta-se as balas em cada detector e se pode fazer uma distribuição diferentes posições é obtida.

![Experimento com projéteis](/assets/images/dupla-fenda/bala_experimento.gif)

Em um primeiro cenário, apenas a primeira fenda está aberta.

![Experimento com projéteis](/assets/images/dupla-fenda/bala_cenario1.gif)

No segundo cenário, apenas a segunda fenda está aberta.

![Experimento com projéteis](/assets/images/dupla-fenda/bala_cenario2.gif)

E no terceiro cenário, as duas fendas estão abertas.

![Experimento com projéteis](/assets/images/dupla-fenda/bala_cenario3.gif)


Perceba que não existem regiões vagas de projéteis, em que projéteis possam não ter chegado de alguma forma. A distribuição das posições que os projéteis chegam ao anteparo final com as duas fendas abertas é percebido como a soma das distribuições de quando cada um das fendas está fechada. Entendemos que as distribuições do cenário 1 e 2 são independentes e por isso, a propabilidade de um projétil chegar em um ponto x é sempre a soma da probabilidade do projétil chegar na posição vindo pela fenda 1 com a probabilidade do projetil chegar na mesma posição vinda pela fenda 2.


#### O caso do disparador de Elétrons

Da mesma maneira, considera-se que seja possível criar um disparador de elétrons. Este disparador envia um elétron por vez. O disparador atira os elétrons através de duas fendas e sobre o aparato final existem detectores que registram em que posição o elétron o atingiu. Repetimos os mesmos três cenários do caso de projéteis. A distribuição P<sub>1</sub> foi construída enquanto a fenda 2 estava fechada. A distribuição P<sub>2</sub> foi construída quando a fenda 1 estava fechada. E, a distribuição P<sub>12</sub> foi construída com as duas fendas abertas.

![Experimento com projéteis](/assets/images/dupla-fenda/electron_gun.png)

Acontece agora que, quando as duas fendas estão abertas, os eventos de passar por uma fenda ou outra não são mais independentes. Eventos independentes podem ser somados, o que nao acontece na distribuição P<sub>12</sub>, ela não é a simples soma dos eventos independentes de passar por uma fenda ou passar por outra. Um ajuste matemático é necessário fazer para determinar a distribuição P<sub>12</sub>. Aparentemente, P<sub>1</sub> ou P<sub>2</sub> tem componentes em si que nem sempre são positivas $$\phi$$<sub>1</sub> e $$\phi$$<sub>2</sub>. Algumas vezes,  estas componentes tem sinais invertidos e quando somados podem inclusive se cancelar. Desta forma P<sub>12</sub> deve ser calculado usando o quadrado do módulo da soma entre estas componentes de maneira a fazerem os dados se ajustarem. 

Acontece então que embora o elétron seja encontrado em uma posição pontual no anteparo, existem posições que ele nunca é atingido e também existe um alargamento da área em que o elétron alcança.

Lembramos que apenas um elétron por vez é disparado, e passar por uma fenda ou passar por outra fenda não são mais eventos independentes. Aparenta que de forma similar a uma onda  o elétron passou pelas duas fendas, se propagou de forma ondulatória até que chegou no anteparo. E é este fenômeno que chamamos *Superposição*. Reparamos que o elétron esteve na sua forma ondulatória, em superposição, até chegar no anteparo, quando interagiu com o anteparo em sua forma corpuscular.

![Simulação baseada na Equação de Shrodinger](/assets/images/dupla-fenda/Double_slit_experiment.gif)

#### O caso do disparador de elétros em que se tenta determinar a fenda pelo qual o elétron passou

Neste caso final, uma fonte de luz é colocada depois das fendas e alguns detectores são acionados. Eles são reponsáveis por observar cada um sua fenda e determinar se o elétron passou por lá ou não. Quando um elétron passa pela fenda 1 então o detector $$D_1$$ é acionado, quando o elétron passa pela fenda 2 o detector $$D_2$$ é acionado.


![Experimento com detectores](/assets/images/dupla-fenda/determinar_a_fenda.png)

Quando repetimos os três cenários de observação, primeiro tampando a fenda 2, depois tampando a fenda 1 e por fim deixando ambas abertas o resultado é:

![Experimento com detectores](/assets/images/dupla-fenda/eletron_observado.png)

![Experimento com projéteis](/assets/images/dupla-fenda/particula_comportamento.jpg)

O que acontece é que quando passamos a observar por qual fenda o elétron passou  o resultado é o que o padrão de interferência desaparece! Quando observamos a posição do elétron antes de chegar no anteparo, o elétron para de se comportar como onda e passa a se comportar com partícula. *Percebemos que a partícula permanece em superposição até interagir com outros objetos*. Quando esta interação acontece chamamos de *observação*. 

Em mais uma disposição da experiência, a intensidade da fonte de luz é reduzida e por isso nem sempre é possível aos detectores perceber por qual fenda o elétron passou. Este caso considera quando o detector não é muito aguçado, consegue observar alguns elétrons mas não todos. O resultado é um misto entre as figuras de interfências e a de partícula.

![Experimento com projéteis](/assets/images/dupla-fenda/detector_pouco_agu%C3%A7ado.jpg)


### Por que não observamos a superposição no cotidiano?

O que acontece no nosso mundo cotidiano é que os corpos não estão isolados. O isolamento é parte fundamental para que a superposição aconteça. A interação faz com que conhecimento sobre as características dos corpos sejam conhecidos e a superposição se desfaz.



### Curiosidades


Em 1927, Clinton Davisson e Lester Germer observaram a difração de feixes de eletrons através de cristal de niquel. Demostrando propriedades ondulatórias das partículas pela primeira vez. E Geroge Thomson fez o mesmo com filmes finos de celulóide e outros materiais pouco depois. Davisson e Thomson receberam o premio Nobel de 1937 "por sua descoberta experimental da difração de [feixe de] elétrons através de cristais". Porém, nunca executaram o experimento da dupla fenda com elétrons. O que foi feito em 1961 por Claus Jönsson of Tübingen e em 1970 por Pier Giorgio Merli, Giulio Pozzi and GianFranco Missiroli em Bologna. 

![Experimento realizado por Pier Giorgio Merli, Giulio Pozzi and GianFranco Missiroli em Bologna](/assets/images/dupla-fenda/bologna-image.jpg)






















