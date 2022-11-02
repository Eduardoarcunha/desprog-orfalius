# Método de Karatsuba

## Definições elementares

A **multiplicação** é uma operação matemática que fazemos desde o ensino fundamental, essa já é tão automatizada em nossas cabeças que até seu conceito pode ser esquecido as vezes! Na prática, multiplicar é somar uma quantidade finita de números iguais.

Provavelmente, quando mais novo, você decorou a tabela de multiplicação envolvendo números com apenas **um** dígito, por exemplo, $7 \times 7 = 49$.

Entretanto, para realizar multiplicações envolvendo números com mais de **dois** dígitos, nos foi ensinado um processo simples para resolver o problema. Ja parou para pensar no algorítimo que você usa para realizar multiplicações com mais de um dígito? Vamos relembrar!

??? Checkpoint 1
Repetindo os mesmos passos que você faz para resolver uma multiplicação a mão tente resolva esta operação:

$$
\begin{equation}
\frac{
    \begin{array}[b]{r}
      \left( 1 2 3\right)\\
      \times \left( 4 5 6 \right)
    \end{array}
  }{
    \left( produto \right)
  }
\end{equation}
$$

::: Gabarito

$$
\begin{equation}
\frac{
    \begin{array}[b]{r}
      \left( 1 2 \right)\\
      \times \left( 3 4 \right)
    \end{array}
  }{
    \frac{
    \begin{array}[b]{r}
      \left( 4 8 \right)\\
      + \left( 3 6 0 \right)
    \end{array}
  }{
    \left( 408 \right)
  }
  }
\end{equation}
$$

:::

???

Sim, nós sabemos que isso pode ter sido chato, mas você já se perguntou qual a complexidade desse algoritmo para dois número de $n$ dígitos? Vamos descobrir!

Para isso, é preciso calcular quantas operações estão sendo realizadas, vamos considerar que a multiplicação de dois números de um dígito equivale a uma **única operação**. Isto é, vamos considerar as somas internas que ocorrem em uma multiplicação: $4 \times 3 = 4 + 4 + 4$ como uma {red}(**única**) operação.

??? Checkpoint 2

Calcule quantas operações foram feitas na última multiplicação. (Não se esqueça de contar as somas que acontecem depois da multiplicação!)
::: Gabarito

|                   | Execuções |
| ----------------- | :-------: |
| **Multiplicação** |     4     |
| **Soma**          |     3     |

Portanto, para uma multiplacação de dois números de dois dígitos, foram feitas **4** multiplicações e **3** somas.
???

??? Checkpoint 3

Realize o mesmo processo dos checkpoints anteriores, mas dessa vez, para uma multiplicação de dois números de 3 dígitos. Lembre-se que não estamos interessados no resultado final, mas na quantidade de operações feitas.

$$
\begin{equation}
\frac{
    \begin{array}[b]{r}
      \left( 1 2 3\right)\\
      \times \left( 4 5 6 \right)
    \end{array}
  }{
    \left( produto \right)
  }
\end{equation}
$$

::: Gabarito

|                   | Execuções |
| ----------------- | :-------: |
| **Multiplicação** |     9     |
| **Soma**          |     6     |

:::

???

Talvez você já consiga enxergar um padrão, será que conseguimos relacionar a quantidade de multiplicações e de somas com o número de dígitos $n$?

??? Checkpoint 4
Relacione matematicamente o número de multiplicações $m$ com a quantidade $n$ de dígitos:

::: Gabarito
$m = n^{2}$
:::

???

E quanto as somas? É possível provar que a quantidade de somas $s$, é $O(n)$, porém por questão de tempo não iremos demonstrar.

Dessa forma, a quantidade de operações para $n$ dígitos é $n^{2} + n$, portanto, utilizando as regras de simplificação, chegamos que a complexidade do algoritmo é $O(n^{2})$.

## O problema

Agora que entendemos melhor como funciona o algoritmo clássico de multiplicação, vamos esclarecer melhor qual o nosso problema:

**Dado dois números inteiros de $n$ dígitos, queremos encontrar o produto desses de forma mais rápida, isto é, com o menor número de operações possíveis.**

## Uma maneira mais eficiente

Certo, já sabemos que o processo clássico de multiplicação tem complexidade $O(n^2)$, mas será que existe algum algorítimo mais eficiente?

Já adiantamos que sim! E ainda bem que sim! Pois, para números muito grandes, com centenas ou milhares de dígitos, a quantidade de operações realizadas exigiria muito tempo de processamento de um computador, e em áreas como a criptografia, na qual operações com esses números acontecem, os processos demorariam muito, mas muito mais tempo para serem concluidos.

Para compreender um desses novos métodos vamos utilizar uma estratégia ja vista na diciplina de Desafios de Programação.

??? Checkpoint 5
Você consegue pensar em alguma estratégia ja vista antes em **Desafios da Programação** que envolva transformar duas entradas em quatro até atingir um caso base?

::: Gabarito
Iremos utilizar a estratégia de divisão e conquista associada à ideia de recursão.
:::

???

E se fosse possível calcular o produto de dois números de n dígitos por meio de um método com um número menor de operações? Foi exatamente isso que Karatsuba conseguiu fazer.

Primeiramente vamos imaginar que queremos dividir a nossa entrada em duas.
Seja um inteiro: $$ x=314159 $$ vamos separá-lo nossa de modo a obter os primeiros digitos em um número $a$ e os últimos digitos em um número $b$, ou seja, teremos $$ a=314 \; \; \; \;b=159$$

??? Checkpoint 6

Como poderíamos escrever a nossa entrada $x$ em função de $a$ e $b$?

_Dica: lembre-se que é possível escrever números em potência de 10_
::: Gabarito
$$x=314 \times 10^3 + 159 \times 10^0$$
:::
???

??? Checkpoint 7
Vamos dar mais um passo e generalizar o processo, como escreveríamos $x$ em funcao de $a$, $b$ e $n$. Sendo $a$ a primeira metade dos dígitos de $x$, $b$ a segunda metade, e $n$ a quantidade de dígitos.
::: Gabarito
$$x = a \times 10^{n/2} + b$$
:::
???

Agora vamos introduzir outro inteiro $y$ qualquer de modo que a primeira metade de $y$ seja $c$ e a segunda metade $d$ e vamos escrever a multiplicação $x \times y$ em funcão de $a$, $b$, $c$, $d$ e $n$.

_Por simplicidade iremos supor que $x$ e $y$ possuem a mesma quantidade de dígitos $n$, mas lembre-se que esse não precisa ser necessariamente o caso_

$$x \times y = (a \times 10^{n/2} + b)(c \times 10^{n/2} + d)$$

$$= (a\times10^{n/2}) (a\times10^{n/2}) + ad \times 10^{n/2} + bc \times 10^{n/2} + bd$$

$$= \underline{ac} \times 10^{2(n/2)} + (\underline{ad}+\underline{bc}) \times 10^{n/2} + \underline{bd}$$

Até agora chegamos em quatro multiplicações de números com $n/2$ dígitos, se fossemos analisar a complexidade aqui, ainda teriamos $O(n^{2})$.

??? Checkpoint 8

É possível aumentar o número de multiplicações de 4 para 5, tente manipular essa expressão para atingir esse resultado. (Tenha fé, isso fará sentido!)

_Dica: tente fatorar algum termo da equação_

::: Gabarito

$(ad + bc) = (a + c) \times (d + c) - ac - bd$

Você pode estar pensando que pioramos nossa situação, uma vez que agora temos:

|                        | Aparições |
| ---------------------- | :-------: |
| $ac$                   |     2     |
| $bd$                   |     2     |
| $(a + c)\times(d + c)$ |     1     |

Mas na verdade melhoramos, apenas 3 multiplicações vão acontecer!

:::

???

??? Checkpoint 9

Consegue justificar o porquê na realidade temos 3 multiplicações e não 5?

::: Gabarito

As multiplicações $ac$ e $bd$ só precisam ser calculadas uma vez, então podemos guardar o valor delas em alguma variável!

:::
???

Você também pode criar

1. listas;

2. ordenadas,

assim como

- listas;

- não-ordenadas

e imagens. Lembre que todas as imagens devem estar em uma subpasta _img_.

![](logo.png)

Para tabelas, usa-se a [notação do
MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html),
que é muito flexível. Vale a pena abrir esse link para saber todas as
possibilidades.

| coluna a | coluna b |
| -------- | -------- |
| 1        | 2        |

Ao longo de um texto, você pode usar _itálico_, **negrito**, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo.

$$\lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} \leq 1$$

Para inserir uma animação, use `md :` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em _img_.

:bubble

Você também pode inserir código, inclusive especificando a linguagem.

```py
def f():
    print('hello world')
```

```c
void f() {
    printf("hello world\n");
}
```

Se não especificar nenhuma, o código fica com colorização de terminal.

```
hello world
```

!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!

??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???
