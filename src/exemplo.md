Método de Karatsuba
======

Definições elementares
---------

A **multiplicação** é uma forma simples de se adicionar uma quantidade finita de números iguais.

Muito provavelmente você está acostumado com o conceito de multiplicação. Principalmente se tratando de números com apenas **um** dígito. Entretanto, para realizar multiplicações envolvendo números com mais de **dois** dígitos, nos foi ensinado um algorítimo simples para resolver o problema. Ja parou para pensar no algorítimo que **você** usa para realizar multiplicações do tipo $$n\geq2$$ dígitos? Vamos relembrar!

??? Checkpoint 1
Repetindo os mesmos passos que você faz para resolver uma multiplicação a mão tente resolver esta operação:

$$\begin{equation}
\frac{
    \begin{array}[b]{r}
      \left( 1 2 \right)\\
      \times \left( 3 4 \right)
    \end{array}
  }{
    \left( produto \right)
  }
\end{equation}$$

::: Gabarito
$$\begin{equation}
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
\end{equation}$$
:::

???

??? Checkpoint 2

Agora que relembrou o algorítimo, tente descobrir quantas multiplicações entre números de apenas **um** dígito foram feitas.
::: Gabarito
$$4\times2 = 8$$
$$4\times1 = 4$$
$$3\times2 = 6$$
$$3\times1 = 3$$

Portanto, **4** multiplicações. Você consegue pensar em alguma estratégia ja vista antes em **Desafios da Programação** que envolva transformar duas entradas em quatro? Iremos retomar esse tópico mais a frente no handout.
:::
???


Você também pode criar

1. listas;

2. ordenadas,

assim como

* listas;

* não-ordenadas

e imagens. Lembre que todas as imagens devem estar em uma subpasta *img*.

![](logo.png)

Para tabelas, usa-se a [notação do
MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html),
que é muito flexível. Vale a pena abrir esse link para saber todas as
possibilidades.

| coluna a | coluna b |
|----------|----------|
| 1        | 2        |

Ao longo de um texto, você pode usar *itálico*, **negrito**, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo.

$$\lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} \leq 1$$

Para inserir uma animação, use `md :` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em *img*.

:bubble

Você também pode inserir código, inclusive especificando a linguagem.

``` py
def f():
    print('hello world')
```

``` c
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
