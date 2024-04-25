Transformada de Hough
======

O que é
---------
(Badin)


Ideia inicial
---------
Teoria matemática (Lucas)

Para compreender como detectar linhas em uma imagem, é útil revisitar o conceito matemático de uma linha. Uma linha pode ser representada pela equação $y=ax+b$, onde $y$ e $x$ representam as coordenadas de um ponto na linha, $a$ é o coeficiente angular (indicando o quão inclinada a linha é) e $b$ é o coeficiente linear (indicando onde a linha intercepta o eixo $y$).

![](equacao-grau1.webp)

Essa equação nos capacita a descrever qualquer linha em um plano cartesiano, o que se revela especialmente útil em imagens, já que uma imagem digital geralmente é representada por uma matriz, ou seja, um plano cartesiano, no qual cada pixel (ponto) possui coordenadas $x$ e $y$. Tá mas, por que isso é últil na Transformada de Hough?  E antes disso, no que consiste uma transformada? 

Uma transformada basicamente consiste em passar uma função matemática de um domínio para outro. De maneira mais simplória, **uma transformada é um jeito de olhar para as coisas de uma nova perspectiva**. Imagine que você tem uma função (como uma equação matemática) que descreve algo em um espaço, como pontos em uma imagem. Agora, aplicar uma transformada significa pegar essa função e mudar a forma como a representamos, levando-a para um novo espaço. Isso nos permite ver coisas que talvez não fossem tão óbvias na forma original.

A transformada de Hough faz exatamente isso! Mas com um objetivo específico: **encontrar padrões lineares em uma imagem**, como formas geométricas (no nosso caso, linhas retas). Em vez de olhar diretamente para os pixels da imagem, a transformada de Hough mapeia esses pixels para um espaço de parâmetros, passando de $y=ax+b$ para $c=-mx+y$, onde podemos identificar padrões lineares mais facilmente.

??? Checkpoint

O gráfico abaixo representa os pixels do contorno de uma imagem, e agora que você sabe que a transformada passa de $y=ax+b$ para $c=-mx+y$, tente imaginar como seria a representação de um ponto no novo domínio, usando suas cooredeanadas $x$ e $y$ como constantes. Se possível pegue um papel e uma caneta e desenhe um gráfico.

![](graficos/1.png)

::: Gabarito

![](graficos/2.png)

Um ponto do domínio das coordendas é representado por uma reta no domínio dos parâmetros!

:::

???


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