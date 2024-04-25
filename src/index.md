Transformada de Hough
======

O que é
---------
(Badin)


Ideia inicial
---------
Teoria matemática (Lucas)

Para compreender como detectar linhas em uma imagem, é útil revisitar o conceito matemático de uma linha. Uma linha pode ser representada pela equação $y=mx+c$, onde $y$ e $x$ representam as coordenadas de um ponto na linha, $m$ é o coeficiente angular (indicando o quão inclinada a linha é) e $c$ é o coeficiente linear (indicando onde a linha intercepta o eixo $y$).

![](equacao-grau1.webp)

Essa equação nos capacita a descrever qualquer linha em um plano cartesiano, o que se revela especialmente útil em imagens, já que uma imagem digital geralmente é representada por uma matriz, ou seja, um plano cartesiano, no qual cada pixel (ponto) possui coordenadas $x$ e $y$. 

Tá mas, por que isso é últil na Transformada de Hough?  E antes disso, no que consiste uma transformada? 

Uma transformada basicamente consiste em passar uma função matemática de um domínio para outro. De maneira mais simplória, **uma transformada é um jeito de olhar para as coisas de uma nova perspectiva**. Imagine que você tem uma função (como uma equação matemática) que descreve algo em um espaço, como pontos em uma imagem. Agora, aplicar uma transformada significa pegar essa função e mudar a forma como a representamos, levando-a para um novo espaço. Isso nos permite ver coisas que talvez não fossem óbvias na forma original.

A transformada de Hough faz exatamente isso! Mas com um objetivo específico: **encontrar padrões lineares em uma imagem**, como formas geométricas (no nosso caso, linhas retas). Em vez de olhar diretamente para os pixels da imagem, a transformada de Hough mapeia esses pixels para um espaço de parâmetros, passando de $y=mx+c$ para $c=-mx+y$, onde podemos identificar padrões lineares mais facilmente.

??? Checkpoint
O gráfico abaixo representa os pixels do contorno de uma imagem, e agora que você sabe que a transformada passa de $y=mx+c$ para $c=-mx+y$, tente imaginar como seria a representação de um ponto no novo domínio, usando suas cooredeanadas $x$ e $y$ como constantes. Se possível pegue um papel e uma caneta e desenhe um gráfico.

![](graficos/1.png)

::: Gabarito
![](graficos/2.png)

Um ponto do domínio das coordendas é representado por uma reta no domínio dos parâmetros!
:::
???

??? Checkpoint
Agora que vocẽ sabe que um ponto do domínio das coordendas é representado por uma reta no domínio dos parâmetros, tente entender o que essa reta significa no domínio das coordeandas. De novo, se possível desenhe.

::: Gabarito
![](graficos/3.png)

Um reta do domínio dos parâmetros é representada por infinitas retas que passam pelo respectivo ponto ($x$, $y$) no domínio das coordenadas!
:::
???

Okay, espero que tenha ficado claro o que um representa no outro e vice-versa, mas onde identificamos os padrões que a transformada promete? 

Se você é bastante atento, pode ter percebido que, a medida com que os pontos da imagem são transferidos para o domínio dos parâmetros, essas retas podem se cruzar, e é exatamente esse o pulo do gato! Pois **cruzamentos na transformada significam uma correspondência entre parâmetros que descrevem uma potêncial linha na imagem original.** Calma, respira, vou explicar...

Quando duas linhas no domínio dos parâmetros se cruzam, isso significa que os dois pontos correspondentes na imagem original estão conectados por uma linha com os parâmetros $m$ e $c$ os quais são a coordenada do cruzamento no gráfico da transformada.

{red}(Se está difícil entender apenas lendo, dê uma olhada no carrossel seguinte. Tente reler e rever até ficar claro pra você.)

:animacao_t_retas_perfeita

??? Checkpoint
O caso de exemplo é um caso perfeito, onde todos os pontos estão alinhados, em uma imagem real há muitos pontos, muitos deles (na verdade a maioria) desalinhados. Tente fazer a transformada no exemplo abaixo, onde há um ponto deslocado da reta que liga os demais. De novo, desenhe, tudo fica mais fácil visualmente.

![](graficos/deslocado.png)

::: Gabarito

:animacao_t_retas_deslocado

:::
???

??? Checkpoint

Em uma imagem real, cada ponto pode estar potencialmente conectado a todos os outros por uma reta. Ás vezes a reta liga apenas dois pontos ou  pode ligar inúmeros deles. Tente pensar em como determinar ás retas mais relevantes de uma imagem analisando apenas o gráfico da transformada.

**Dica:** Analise o gabarito do checkpoint anterior e busque uma maneira de justificar por que a reta amarela é a mais relevante para o conjunto de pontos da imagem. 

**OBS:** Justifique com base no gráfico da transformada.

::: Gabarito

A maneira de determinar as retas mais relevantes é **analisar qual coordenada ($m$, $c$) obteve mais intersecções no domínio dos parâmetros**, pois, quanto maior a quantidade de cruzamentos em um mesmo ponto ($m$, $c$), maior a quantidade de pontos na imagem original que estão na respectiva reta.

No caso do gabarito do checkpoint anterior, a reta amarela era a mais relevante para aquele conjunto de pontos pois, no gráfico da transformada, a intersecção no ponto amarelo foi feita por 4 retas, enquanto nos outros pontos apenas por duas.
:::
???


Esta é uma das grandes vantagens da Transformada de Hough, poder detectar todas as retas que ligam pontos na imagem original e filtrá-las pelo seu grau de relevância. 



<div style="display: flex; justify-content: space-between;">
    <div style="width: 48%;">
        <img src="road.jpeg" alt="Imagem 1">
        <p style="display: flex; justify-content: center;">Imagem original</p>
    </div>
    <div style="width: 48%;">
        <img src="road-borders.png" alt="Imagem 2">
        <p style="display: flex; justify-content: center;">Imagem com bordas detectadas</p>
    </div>
</div>



Teoria do algorítmo (Pini)

Problemas (Badin)


Uma possível solução
---------
Teoria matemática (Lucas)

Teoria do algorítmo (Pini)

![](logo.png) 

Implementando o algorítmo (Todos - quinta)
---------


Complexidade  (Todos - quinta)
---------


Exercícios  (Todos - quinta)
---------
Lucas Lima