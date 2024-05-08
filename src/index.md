# Transformada de Hough

## O que é e como funciona?

Imagine que você tem uma imagem e quer detectar linhas nela. Uma das maneiras de fazer isso é usando a Transformada de Hough. A Transformada de Hough é uma técnica usada principalmente em processamento de imagens para detectar formas geométricas, desde retas, circulos, elipses e outras diversas. Mas como ela funciona?

<div style="display: flex; justify-content: space-between;">
    <div style="width: 40%;">
        <img src="hough-input.png" alt="Imagem 1">
        <p style="display: flex; justify-content: center;">1. Input</p>
    </div>
    <div style="width: 40%;">
        <img src="hough-use.png" alt="Imagem 2">
        <p style="display: flex; justify-content: center;">2. Output</p>
    </div>
</div>

1. A transformada recebe uma imagem com os contornos da imagem original (outro algorítimo é responsável por isso).

2. Após o processamento, ela retorna os parâmetros das retas presentes na imagem original.

3. Com os parâmetros fornecidos pela transformada, é possível visualizar as respectivas retas na imagem original.

E como isso seria útil em uma aplicação real?

Anteriormente, na matéria de Robótica Computacional, vocês aprenderam a controlar robôs de forma autônoma, e a Transformada de Hough é uma técnica bastante utilizada nesse contexto para ajudar na navegação de robôs.

??? Checkpoint
Vamos supor que queremos desenvolver um carro autônomo que deve percorrer uma estrada. Levando em consideração que já temos as bordas da estrada segmentadas, como você poderia, utilizando a transformada de Hough, encontrar o caminho que o carro deve seguir?

**Dica**: Como poderiamos encontrar a intersecção das bordas da estrada?

![](input-output/road-borders.png)

::: Gabarito
Uma possível solução seria detectar as linhas presentes nessas bordas. A partir das linhas detectadas, poderíamos encontrar o ponto futuro em seus cruzamentos, ponto o qual dita a direção que o carro deve seguir para continuar na pista.

![](input-output/road-lines.png)

:::
???

## Compreendendo a ideia: o pulo do gato!

Para compreender como detectar retas em uma imagem, é útil revisitar o seu conceito matemático. Uma reta pode ser representada pela equação $y=mx+c$, onde $y$ e $x$ representam as coordenadas de um ponto na reta, $m$ é o coeficiente angular (tangente do o angulo theta entre a reta e a horizontal) indicando o quão inclinada ela é, e $c$ é o coeficiente linear (indicando onde ela intercepta o eixo $y$).

![](equacao1grau.png)

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

Se você é bastante atento, pode ter percebido que, a medida com que os pontos da imagem são transferidos para o domínio dos parâmetros, essas retas podem se cruzar, e é exatamente esse o **pulo do gato**! Pois **cruzamentos na transformada significam uma correspondência entre parâmetros que descrevem uma potêncial reta na imagem original.** Calma, respira, vou explicar...

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

## Compreendendo a ideia: o algorítmo

Após entender a ideia matemática, você deve estar um pouco curioso em como essa lógica é implementada em um algorítimo.

Ora, começemos do início, temos uma imagem com o contorno da imagem original, ou seja, uma matriz dos pixels, cada pixel possui uma coordenada ($x$, $y$), basta passa-lo para o domínio ($m$, $c$). Esse domínio pode ser representado por outra matriz, onde as linhas são representadas por $c$ e as colunas por $m$.

<!-- ![](matriz.png) -->

??? Checkpoint

Tendo em mente os conceitos da transformada ditos anteriormente, imagine como um ponto da imagem do contorno poderia ser representado na matriz dos parâmetros. Assuma que a matriz inicia com todos valores zerados e os pontos por onde passa uma reta se soma 1.

![](graficos/1.png)

::: Gabarito

![](animacao_matriz1/2.png)

Lembre-se, um ponto do domínio das coordendas é representado por uma reta no domínio dos parâmetros.

:::

???

??? Checkpoint

O que você imagina que deve acontecer quando pontos alinhados na imagem original são transferidos para a matriz dos parâmetros? Simule como foi feito no gabarito do checkpoit anterior.

**OBS**: Não se esqueça de incrementar em 1 os pontos por onde passa uma reta.

::: Gabarito

:animacao_matriz1

De acordo com que os pontos da imagem são transferidos para a matriz dos parâmetros, os pontos onde há intersecção são incrementados.

:::

???

??? Checkpoint

Como vocẽ deduziria que a matriz dos parâmetros fosse ficar após a transformada dos pontos abaixo?

![](pontos-tortos.png)

::: Gabarito

:animacao_matriz2

Um ou mais indíces da matriz teriam valores maiores do que um. E como dito anteriormente, a coordenada onde há mais intersecções na transformada corresponde aos parâmetros da reta mais relevante no conjunto de pontos da imagem original.

:::

???

Após aplicar a transformada para cada ponto na imagem, basta varrer a matriz ($m$, $c$) e selecionar as coordenadas que amarzenam os maiores valores. Essas coordenadas correspondem aos parâmetros das retas mais relevantes na imagem original.

## Problema prático dos parâmetros ($m$, $c$)

A implementação acima é correta teoricamente, mas na prática não funciona bem. Isso ocorre porque, ao utilizar os parâmetros $m$ e $c$, pode-se encontrar alguns problemas ao lidar com linhas verticais, onde a inclinação $m$ se torna infinita ($tan (90°)$).

Dessa forma, essa abordagem não é a mais adequada quando pensamos numa aplicação prática.

## Uma possível solução: a mágica da parametrização polar

Uma parametrização nada mais é do que uma forma de escrever uma função, por exemplo, uma reta usando os parâmetros $m$ e $c$. Mas como dito anteriormente, a parametrização $y=mx+c$ não é conveniente para implementar o algorítimo.

Visto isso, uma parametrização polar, na qual descreve uma reta em função de $θ$ e $ρ$, onde $θ$ significa o ângulo em radianos entre a reta e a horizontal, e $ρ$ signifca a distância da origem até a reta, superaria tais dificuldades.

$$y=\frac{x\sin(\theta) + \rho}{cos(\theta)}$$

![](reta-polar.png)

- $θ$: ângulo em radianos entre a reta e a horizontal

- $ρ$: distância da origem até a reta

Antes de prosseguir, entre [neste link](https://www.geogebra.org/m/xzkd8ffg) do GeoGebra, altere os parâmetros e se convença de que a parametrização acima faz sentido.

??? Checkpoint

Sabendo da parametrização polar, tente explicar como ela arrumaria o problema acima na aplicação do algorítimo da Transformada de Hough.

**Dica:** lembre-se de que na parametrização anterior, a questão que trazia problemas ao código era: $$m\rightarrow\infty$$

::: Gabarito

Como agora a parametrização não está em função da tangente de um ângulo, não existe a possibilidade de termos um número infinito nos cálculos. Isso nos permite corrigir o erro de não ser possível encontrar uma reta vertical a qual o ponto pertence.

:::
???

Certo, mas como funciona matemáticamente? Ora, da mesma maneira que na primeira parametrização, mas ao invés de passarmos os pontos de uma imagem para o domínio ($m$, $c$), passaremos para o domínio ($θ$, $ρ$). Ou seja, de $y=mx+c$ para $\rho =-x\sin(\theta) + y\cos(\theta)$.

??? Checkpoint

Como foi feito na primeira transformada, tente imaginar como seria a representação de um ponto no novo domínio ($θ$, $ρ$), usando suas cooredeanadas $x$ e $y$ como constantes. Se possível pegue um papel e uma caneta e desenhe um gráfico.

![](graficos/1.png)

::: Gabarito

![](animacao_senoides/1.png)

Um ponto do domínio das coordendas é representado por uma senóide no domínio dos novos parâmetros!

:::
???

De maneira análoga ao domínio ($m$, $c$), quando duas senóides no domínio dos parâmetros se cruzam, isso significa que os dois pontos correspondentes na imagem original estão conectados por uma reta com os parâmetros $θ$ e $ρ$, os quais são a coordenada da interesecção no gráfico da transformada.

:animacao_senoides

O exemplo acima mostra dois pontos de cruzamento, $\theta$ e $\theta + \pi$, mas isto é só para mostrar que de acordo com que a senóide se repete, ela passa a cruzar em coordenadas de parãmetros equivalentes. Na prática isso não acontece, pois $0 < θ < π$.

Assim, o algorítimo também é feito de maneira análoga a quando os parâmetros eram $m$ e $c$, ou seja, cria-se uma matriz onde as linhas são representadas por $ρ$ e as colunas representadas por $θ$. E aqui que está a grande vantagem, pois o tamanho desta matriz é limitado, ou seja, o máximo de valores em que $ρ$ pode assumir (linhas), é de $0$ até a quantidade de pixels na diagonal da imagem $\sqrt{x^2 + y^2}$, e a máxima quantidade de valores que $\theta$ pode assumir (colunas) são valores de $0$ a $\pi$, com um $\Delta \theta$ a escolha do usuário, sendo que, quanto menor, mais preciso será o algorítimo.

:animacao_matriz3

No exemplo acima foi trasnferido apenas um ponto da imagem original para a matriz dos parâmetros, isto por que a quantidade de linhas e colunas são pequenas, o que seria incoveninete desenhar várias senóides.

Mas a lógica é a mesma das retas, num contexto prático, haverá inúmeros mais pontos, e a medida com que as senóides vão se sobrepondo, é incrementado 1. Após aplicar a transformada para cada ponto na imagem, basta varrer a matriz ($\theta$, $\rho$) e filtrar as coordenadas que amarzenam os maiores valores. Essas coordenadas correspondem aos parâmetros das retas mais relevantes na imagem original.

## Complexidade

??? Checkpoint

Levando o código montado anteriormente, tente analisar a complexidade do algorítmo. Pense em alto nível nesse primeiro momento, e depois tente analisar a complexidade de cada operação.

::: Gabarito

A complexidade do algorítmo é $O(n*m)$, onde $n$ é o número de pixels da imagem e $m$ é o número de $\theta$ iterados no segundo loop. Como temos 2 loops, um dentro do outro, a complexidade seria a multiplicação dos dois.

:::

???

## Exercícios

PROXIMA ENTREGA
