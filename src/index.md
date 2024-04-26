# Transformada de Hough

## O que é e como funciona?

Imagine que você tem uma imagem e quer detectar linhas nela. Uma das maneiras de fazer isso é usando a Transformada de Hough. A Transformada de Hough é uma técnica usada principalmente em processamento de imagens para detectar formas geométricas, desde retas, circulos, elipses e outras diversas. Mas como ela funciona?

<div style="display: flex; justify-content: space-between;">
    <div style="width: 32.5%;">
        <img src="hough-input.png" alt="Imagem 1">
        <p style="display: flex; justify-content: center;">1. Input</p>
    </div>
    <div style="width: 32.5%;">
        <img src="hough-output.png" alt="Imagem 2">
        <p style="display: flex; justify-content: center;">2. Output</p>
    </div>
    <div style="width: 32.5%;">
        <img src="hough-use.png" alt="Imagem 3">
        <p style="display: flex; justify-content: center;">3. Uso</p>
    </div>
</div>

1. A transformada recebe uma imagem com os contornos da imagem original (outro algorítimo é responsável por isso).

2. Após o processamento, ela retorna os parâmetros das retas presentes na imagem original. Não se preocupe em entender o que esse gráfico de senóides signfica por agora, apenas entenda que os circulos vermelhos representam pontos, os quais as coordenadas são os parâmetros das retas na imagem original.

3. Com os parâmetros fornecidos pela transformada, é possível visualizar as respectivas retas na imagem original.

## Como pode ser utilizada na prática?

Anteriormente, na matéria de Robótica Computacional, vocês aprenderam a controlar robôs de forma autônoma, e a Transformada de Hough é uma técnica bastante utilizada nesse contexto para ajudar na navegação de robôs.

??? Checkpoint
Vamos supor que queremos desenvolver um carro autônomo que deve percorrer uma estrada. Que processo poderia ser utilizado, utilizando a transformada de Hough, para encontrar a direção que o carro deve seguir?

**Dica**: Imagine as retas que compõem a imagem.

![](input-output/road.jpeg)

::: Gabarito
Uma possível solução seria segmentar as bordas da pista e, a partir disso, detectar as linhas presentes nessas bordas. A partir das linhas detectadas, poderíamos encontrar o ponto futuro em seus cruzamentos, ponto o qual dita a direção que o carro deve seguir para continuar na pista.

<div style="display: flex; justify-content: space-between;">
    <div style="width: 32.5%;">
        <img src="input-output/road.jpeg" alt="Imagem 1">
        <p style="display: flex; justify-content: center;">1. Imagem original</p>
    </div>
    <div style="width: 32.5%;">
        <img src="input-output/road-borders.png" alt="Imagem 2">
        <p style="display: flex; justify-content: center;">2. Bordas segmentadas</p>
    </div>
    <div style="width: 32.5%;">
        <img src="input-output/road-lines.png" alt="Imagem 3">
        <p style="display: flex; justify-content: center;">3. Linhas detectadas</p>
    </div>
</div>

:::
???

## Ideia inicial

Teoria matemática (Lucas)

Para compreender como detectar retas em uma imagem, é útil revisitar o seu conceito matemático. Uma reta pode ser representada pela equação $y=mx+c$, onde $y$ e $x$ representam as coordenadas de um ponto na reta, $m$ é o coeficiente angular (indicando o quão inclinada ela é) e $c$ é o coeficiente linear (indicando onde ela intercepta o eixo $y$).

![](equacao-grau1.webp)

Essa equação nos capacita a descrever qualquer reta em um plano cartesiano, o que se revela especialmente útil em imagens, já que uma imagem digital geralmente é representada por uma matriz, ou seja, um plano cartesiano, no qual cada pixel (ponto) possui coordenadas $x$ e $y$. 

Tá mas, por que isso é últil na Transformada de Hough? E antes disso, no que consiste uma transformada?

Uma transformada basicamente consiste em passar uma função matemática de um domínio para outro. De maneira mais simplória, **uma transformada é um jeito de olhar para as coisas de uma nova perspectiva**. Imagine que você tem uma função (como uma equação matemática) que descreve algo em um espaço, como pontos em uma imagem. Agora, aplicar uma transformada significa pegar essa função e mudar a forma como a representamos, levando-a para um novo espaço. Isso nos permite ver coisas que talvez não fossem óbvias na forma original.

A transformada de Hough faz exatamente isso! Mas com um objetivo específico: **encontrar padrões lineares em uma imagem**, como formas geométricas (no nosso caso, retas). Em vez de olhar diretamente para os pixels da imagem, a transformada de Hough mapeia esses pixels para um espaço de parâmetros, passando de $y=mx+c$ para $c=-mx+y$, onde podemos identificar padrões lineares mais facilmente.

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

Se você é bastante atento, pode ter percebido que, a medida com que os pontos da imagem são transferidos para o domínio dos parâmetros, essas retas podem se cruzar, e é exatamente esse o pulo do gato! Pois **cruzamentos na transformada significam uma correspondência entre parâmetros que descrevem uma potêncial reta na imagem original.** Calma, respira, vou explicar...

Quando duas linhas no domínio dos parâmetros se cruzam, isso significa que os dois pontos correspondentes na imagem original estão conectados por uma reta com os parâmetros $m$ e $c$, os quais são a coordenada da interesecção no gráfico da transformada.

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

Em uma imagem real, cada ponto pode estar potencialmente conectado a todos os outros por uma reta. Ás vezes a reta liga apenas dois pontos ou pode ligar inúmeros deles. Tente pensar em como determinar ás retas mais relevantes de uma imagem analisando apenas o gráfico da transformada.

**Dica:** Analise o gabarito do checkpoint anterior e busque uma maneira de justificar por que a reta amarela é a mais relevante para o conjunto de pontos da imagem.

**OBS:** Justifique com base no gráfico da transformada.

::: Gabarito

A maneira de determinar as retas mais relevantes é **analisar qual coordenada ($m$, $c$) obteve mais intersecções no domínio dos parâmetros**, pois, quanto maior a quantidade de intersecções em um mesmo ponto ($m$, $c$), maior a quantidade de pontos na imagem original que estão na respectiva reta.

No caso do gabarito do checkpoint anterior, a reta amarela era a mais relevante para aquele conjunto de pontos pois, no gráfico da transformada, a intersecção no ponto amarelo foi feita por 4 retas, enquanto nos outros pontos apenas por duas.
:::
???

Esta é uma das grandes vantagens da Transformada de Hough, poder detectar todas as retas que ligam pontos na imagem original e filtrá-las pelo seu grau de relevância.

Teoria do algorítmo (Pini)


Para encontrar a reta mais relevante, que possui o maior número de interseções com os pontos da imagem, adotamos uma abordagem baseada em uma matriz $c$ por $m$, inicializada com zeros. Essa matriz representa todos os possíveis parâmetros $m$ e $c$ que definem uma reta no espaço.

Ao parametrizar os pontos da imagem em outro domínio, geramos uma linha parametrizada para cada ponto da linha original. Cada ponto dessa linha parametrizada é associado a um par de coordenadas ($m$, $c$), e incrementamos o valor correspondente na matriz em uma unidade. Isso nos permite analisar qual par de parâmetros ($m$, $c$) resulta na maior quantidade de interseções, indicando assim a reta mais adequada para representar os dados.

Esta abordagem nos permite encontrar de forma eficiente a reta que melhor se ajusta aos pontos da imagem, considerando sua frequência de interseção com outras linhas parametrizadas.

:animacao_matriz1

Outro exemplo: 

:animacao_matriz2

Percebemos neste caso que um dos pontos está um pouco fora da reta, o que causa uma certa irregularidade na matriz, com mais de um ponto com um valor maior que 1 (mais de uma intesecção), porém, mesmo neste caso o algorítimo ainda escolhe a coordenada central da matriz, que possui o maior número de votos, o que representa a reta que inclui mais pontos. 

## Problema prático da parametrização

Apesar de a parametrização acima estar correta, ela não é a mais adequada para a implementação da Transformada de Hough. Isso porque, A busca por linhas no espaço de parâmetros torna-se mais complexa devido ao grande número de combinações possíveis de $m$ e $c$, o tempo de processamento aumenta consideravelmente, especialmente em imagens com grande quantidade de pixels e a eficiência da Transformada de Hough é afetada negativamente.

Tudo isso, pois $-\infty < m < \infty$ e $-\infty < c < \infty$, o que torna a busca por linhas no espaço de parâmetros muito custosa.

Para resolver esse problema, podemos utilizar a parametrização polar, que é dada por:

$$x\sin(\theta) - y\cos(\theta) + \rho = 0$$

O conceito é o mesmo e, na pratica, ambas as parametrizações são equivalentes, mas a parametrização polar é mais eficiente computacionalmente.

??? Checkpoint
Agora que você sabe que a parametrização polar é mais eficiente computacionalmente, tente entender o porquê disso.

**Dica:** Tente pensar em como a parametrização polar pode reduzir o número de combinações possíveis de $m$ e $c$.

::: Gabarito
$\theta$ é um ângulo que varia de 0 a 180 graus, e $\rho$ é a distância da origem ao ponto mais próximo da reta, ou seja, seu valor máximo corresponde ao tamanho da imagem. Essa pequena alteração na parametrização reduz o número de combinações possíveis, o que torna a busca por linhas no espaço de parâmetros mais eficiente.
:::
???

## Uma possível solução

Teoria matemática (Lucas)

Teoria do algorítmo (Pini)

![](logo.png)

## Implementando o algorítmo (Todos - quinta)

## Complexidade (Todos - quinta)

??? Checkpoint

Levando o código montado anteriormente, tente analisar a complexidade do algorítmo. Pense em alto nível nesse primeiro momento, e depois tente analisar a complexidade de cada operação.

::: Gabarito

A complexidade do algorítmo é $O(n*m)$, onde $n$ é o número de pixels da imagem e $m$ é o número de $\theta$ iterados no segundo loop. Como temos 2 loops, um dentro do outro, a complexidade seria a multiplicação dos dois.

:::

???

## Exercícios (Todos - quinta)

Lucas Lima
