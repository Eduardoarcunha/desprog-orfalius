# Método de Karatsuba

## Definições elementares

A **multiplicação** é uma operação matemática que fazemos desde o ensino fundamental, essa já é tão automatizada em nossas cabeças que até seu conceito pode ser esquecido as vezes! Na prática, multiplicar é somar uma quantidade finita de números iguais.

Provavelmente, quando mais novo, você decorou a tabela de multiplicação envolvendo números com apenas **um** dígito, por exemplo, $7 \times 7 = 49$.

Entretanto, para realizar multiplicações envolvendo números com mais de **dois** dígitos, nos foi ensinado um processo simples para resolver o problema. Já parou para pensar no algoritmo que você usa para realizar multiplicações com mais de um dígito? Vamos relembrar!

??? Checkpoint 1
Repetindo os mesmos passos que você faz para resolver uma multiplicação a mão resolva esta operação:

$$
\begin{equation}
\frac{
    \begin{array}[b]{r}
      \left( 1 2\right)\\
      \times \left( 3 4 \right)
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

Sim, nós sabemos que isso pode ter sido chato, mas você já se perguntou qual a complexidade desse processo? Vamos descobrir!

Antes de mais nada, precisamos dar um passo para trás no estudo de complexidade, uma vez que queremos entender a complexidade da multiplicação clássica, precisamos estudar a camada mais baixa de operações.

Primeiramente, precisamos definir qual a operação básica de nossos processos, isso é qual operação é $O(1)$, nós estabeleceremos que **qualquer operação atômica entre dois números de {red}(um dígito) tem complexidade $O(1)$**.

??? Checkpoint 2

Até então na discisplina, definimos a complexidade de um algoritmo em função de uma entrada, por exemplo, para algoritmos de ordenação, utilizamos o tamanho _n_ do vetor que será ordenado.

A complexidade de uma operação de dois números de vários dígitos pode ser definida em função do que?

::: Gabarito

    A quantidade *n* de dígitos de dois números.

???

Agora que definimos o que consideramos como operação básica, e em função do que escrevemos a complexidade de outras operações de mais dígitos, vamos ver qual a complexidade da operação mais simples de todas, a **soma**.

??? Checkpoint 3

Quantas operações são realizadas em $34 + 12$?

::: Gabarito

2 operações $(2+4)$ e $(3+1)$.

:::
???

??? Checkpoint 4

Quantas operações são realizadas em $456 + 123$?

::: Gabarito

3 operações $(4+1)$, $(5+2)$ e $(6+3)$!

:::
???

??? Checkpoint 5
Relacione matematicamente o número de operações $m$ com a quantidade $n$ de dígitos:

::: Gabarito
$m = n$
:::

???

Dessa forma, a operação de soma tem complexidade $O(n)$.

Agora vamos partir para a multiplicação clássica, para definirmos sua complexidade, **precisamos saber quantas multiplicações de um dígito ocorrem em uma operação de $n$ dígitos!**

??? Checkpoint 6

Calcule quantas multiplicações de um dígito ocorrem na multiplicação que você fez no **checkpoint 1**.
::: Gabarito

| Multiplicação | Ocorrências |
| :-----------: | :---------: |
|     4 X 2     |      1      |
|     4 X 1     |      1      |
|     3 X 2     |      1      |
|     3 X 1     |      1      |

Quatro multiplicações.
???

??? Checkpoint 7

Realize o mesmo processo, mais uma vez. Lembre-se que não estamos interessados no resultado do produto, mas na quantidade de multiplicações de um dígitos que são feitas.

$$
\begin{equation}
\frac{
    \begin{array}[b]{r}
      \left( 2 3\right)\\
      \times \left( 5 6 \right)
    \end{array}
  }{
    \left( produto \right)
  }
\end{equation}
$$

::: Gabarito

| Multiplicação | Ocorrências |
| :-----------: | :---------: |
|     6 X 3     |      1      |
|     6 X 2     |      1      |
|     5 X 3     |      1      |
|     5 X 2     |      1      |

4 multiplicações.

???

Talvez você já consiga enxergar um padrão, será que conseguimos definir a complexidade da multiplicação?

??? Checkpoint 8
Relacione matematicamente o número de multiplicações $m$ com a quantidade $n$ de dígitos:

_Dica: Se não tiver enxergado um padrão, realize o mesmo processo para números de três dígitos_

::: Gabarito
$m = n^{2}$
:::

???

Dessa forma, a complexidade do algoritmo clássico de multiplicação é $O(n^{2})$.

## O problema

Agora que entendemos melhor como funciona o algoritmo clássico de multiplicação, vamos esclarecer melhor qual o nosso problema:

**Dado dois números inteiros de $n$ dígitos, queremos encontrar o produto desses de forma mais rápida, isto é, com o menor número de operações possíveis.**

## Uma maneira mais eficiente

Certo, já sabemos que o processo clássico de multiplicação tem complexidade $O(n^2)$, mas será que existe algum algoritmo mais eficiente?

Já adiantamos que sim! E ainda bem que sim! Pois, para números muito grandes, com dezenas ou centenas de dígitos, a quantidade de operações realizadas exigiria muito tempo de processamento de um computador, e em áreas como a criptografia, na qual operações com esses números acontecem, os processos demorariam muito mais tempo para serem concluídos.

Para compreender um desses novos métodos vamos utilizar uma estratégia já vista.

??? Checkpoint 9
Você consegue pensar em alguma estratégia ja vista antes em **Desafios de Programação** que consiga resolver um problema quebrando ele em versões mais simples, e combinando as soluções dessas para resolver o problema original?

::: Gabarito
Iremos utilizar a estratégia de divisão e conquista.
:::

???

{red}(**Antes de avançar-mos, por questão de simplicidade, consideraremos todas entradas $n$, como potências de 2. (Isso não compromete o resultado, mas simplifica as demonstrações).**)

E se fosse possível calcular o produto de dois números de $n$ dígitos por meio de um método com um número menor de multiplicações? Foi exatamente isso que Karatsuba conseguiu fazer.

Primeiramente vamos imaginar que queremos dividir a nossa entrada em duas.
Seja um inteiro: $$ x=314159 $$ vamos separá-lo de modo a obter os primeiros dígitos em um número $a$ e os últimos dígitos em um número $b$, ou seja, teremos $$ a=314 \; \; \; \;b=159$$

??? Checkpoint 10

Como poderíamos escrever a nossa entrada $x$ em função de $a$ e $b$?

_Dica: lembre-se que é possível escrever números com potência de 10_
::: Gabarito
$$x=314 \times 10^3 + 159 \times 10^0$$
:::
???

Você pode estar se perguntando se escrever um número dessa forma não seria pior, uma vez que estamos escrevendo $x$ em função de duas multiplicações e uma soma. Na prática, essas multiplicações por potências de dez são muito simples, estamos apenas adicionando zeros a direita do número.

**Assim, ao contar a quantidade de multiplicações do algoritmo iremos desconsiderar o produto com potências de dez.**

??? Checkpoint 11
Vamos dar mais um passo e generalizar o processo, como escreveríamos $x$ em função de $a$, $b$ e $n$. Sendo $a$ a primeira metade dos dígitos de $x$, $b$ a segunda metade, e $n$ a quantidade de dígitos.
::: Gabarito
$$x = a \times 10^{n/2} + b$$
:::
???

Agora vamos introduzir outro inteiro $y$ qualquer, de modo que a primeira metade de $y$ seja $c$ e a segunda metade $d$ e vamos escrever a multiplicação $x \times y$ em funcão de $a$, $b$, $c$, $d$ e $n$.

_Por simplicidade iremos supor que $x$ e $y$ possuem a mesma quantidade de dígitos $n$, mas lembre-se que esse não precisa ser necessariamente o caso_

$$x \times y = (a \times 10^{n/2} + b)(c \times 10^{n/2} + d)$$

$$= (a\times10^{n/2}) (c\times10^{n/2}) + ad \times 10^{n/2} + bc \times 10^{n/2} + bd$$

$$= \underline{ac} \times 10^{2(n/2)} + (\underline{ad}+\underline{bc}) \times 10^{n/2} + \underline{bd}$$

Até agora chegamos em quatro multiplicações de números com $n/2$ dígitos, se fossemos calcular a complexidade aqui, seria possível provar que ainda teriamos $O(n^{2})$. Ou seja, ainda não conseguimos melhorar a complexidade da operação.

É possível aumentar o número de multiplicações de 4 para 5, tentaremos manipular essa expressão para atingir esse resultado. (Tenha fé, isso fará sentido!)

??? Checkpoint 12

Fatore o termo $(ad + bc)$ de forma a resultar em três multiplicações ao invés de duas.

_Dica: Pelo produto de duas somas, podemos chegar em $(ad + bc)$ $+$ $alguma$ $coisa$, e depois isolarmos $(ad + bc)$_

::: Gabarito

$(ad + bc) = (a + c) \times (d + c) - ac - bd$

Você pode estar pensando que pioramos nossa situação, uma vez que agora temos:

$$\underline{ac} \times 10^{2(n/2)} +  \underline{(a + c) \times (d + c)} - \underline{ac} - \underline{bd} \times 10^{n/2} + \underline{bd}$$

|                        | Aparições |
| ---------------------- | :-------: |
| $ac$                   |     2     |
| $bd$                   |     2     |
| $(a + c)\times(d + c)$ |     1     |

Mas na verdade melhoramos, apenas 3 multiplicações vão acontecer!

:::

???

??? Checkpoint 13

Consegue entender o porquê na realidade temos 3 multiplicações e não 5?

::: Gabarito

As multiplicações $ac$ e $bd$ só precisam ser calculadas uma vez, então podemos guardar o valor delas em alguma variável!

:::
???

## Complexidade

Finalmente, temos a ideia do algoritmo pronto, mas é interessante saber qual a complexidade desse algoritmo, para vermos o quão rápido ele é em relação à multiplicação clássica.

{red}(**Como a eficiência dos algoritmos são muito próximas, não simplifique as bases logaritmas, por exemplo, considere $log_{2}$ como $log_{2}$ e $log_{3}$ como $log_{3}$.**)

??? Exercicio 1

Lembre-se das árvores de complexidade para algoritmos recurssivos e tente escrever a recorrência desse algoritmo.

_Dica: Considere que a parte não-recursiva de cada chamada tem complexidade $O(n)$ (isso é consequência das operações de soma que acontecem entre os produtos)_
::: Gabarito

```txt
         /
        | 1             se n <= 1;
f(n) = <
        | 3f(n/2) + n   se n > 1.
         \
```

:::

???

??? Exercicio 2

Desenhe a árvore do algoritmo _(não é necessário desenhar a árvore inteira)_.

_Dica: A árvore é semelhante a prova (sim, ela é grande), e lembre-se que o valor de n muda para cada andar $(n, n/2, ...)$!_
::: Gabarito

![](karatsuba.png|20)

:::

???

??? Exercicio 3

Qual a altura $h$ dessa árvore?

::: Gabarito

Dígitos $n$ divide por 2 a cada andar enquanto for maior que 1.
No andar $(h-2)$, o valor de $n$ é maior que 1.

$\frac{n}{2^{h-2}} > 1$

$2^{h} < 9n$

$h = O(log_{2}(n))$, ou seja $h <= c * log_{2}(n)$

:::

???

Sim, chegou o momento.

??? Exercicio 4

Encontre a soma dos {red}(**vermelhos**) da árvore, e deixe o resultado em função de $h$.

::: Gabarito

Ao longo dos andares, temos

$(n + 3(n/2) + 9(n/4) + ... + 3^{h-2}*n/2^{h-2} + 3^{h-1}*n/2^{h-1})$

$= n*(1 + 3/2 + 9/4 + ... + 3^{h-2}/2^{h-2} + 3^{h-1}/2^{h-1})$

Soma de PG

- primeiro elemento 1
- razão 3/2
- número de elementos h

Dessa forma:

$= n * \frac{((\frac{3}{2})^{h} - 1)}{3/2 - 1}$

$= n * \frac{(\frac{3^{h}}{2^{h}} - 1)}{0.5}$
:::
???

??? Exercicio 5

Substitua $h$ e manipule a expressão para que o $2^{h}$ desapareça.

_Dica: Teremos que usar propriedades logarítmicas e exponenciais_

::: Super Dica

Pense na definição de logaritmo, isto é, o que significa o número $log_{2}(n)$, e depois pense o que significa $2^{log_{2}(n)}$

:::

::: Gabarito

Substituindo h:

$\large = \frac{n}{0.5}(\frac{3^{log_{2}(n)}}{2^{log_{2}(n)}} - 1)$

Um número qualquer $A$ elevado por um logaritmo de base $A$ é igual ao logaritmando:

$\large = \frac{n}{0.5} *(\frac{3^{log_{2}(n)}}{n} - 1)$

:::

???

??? Exercício 6

Por fim, calcule a complexidade da árvore.

_Dica: Pode ser interessante mudar a base do logaritmo restante_

::: Gabarito

Realizando a mudança de base:

$\large = \frac{n}{0.5} *(\frac{3^{\frac{log_{3}(n)}{log_{3}(2)}}}{n} - 1)$

Propriedade da multiplicação de expoentes:

$\large = \frac{n}{0.5} *(\frac{(3^{log_{3}(n)})^{\frac{1}{log_{3}(2)}}}{n} - 1)$

Um número qualquer $A$ elevado por um logaritmo de base $A$ é igual ao logaritmando:

$\large = \frac{n}{0.5} *(\frac{n^{\frac{1}{log_{3}(2)}}}{n} - 1)$

Propriedade da inversão de base:

$\large = \frac{n}{0.5} *(\frac{n^{log_{2}(3)}}{n} - 1)$

Realizando simplificações:

$\large = (\frac{n^{log_{2}(3)}}{0.5} - \frac{1}{0.5})$

Finalmente, conforme as regras de simplificação, a complexidade é:

$O(n^{log_{2}3}) \approx O(n^{1.58})$

:::

???

Assim, comprova-se que o algoritmo realmente tem complexidade menor que a multiplicação clássica! E esse algoritmo que acabamos de estudar é o **método de Karatsuba**!

Porém ele não é o melhor dos algoritmos! Se ficou curioso em ver o quanto a quantidade de operações _cresce_ para diferentes algoritmos de multiplicação, temos aqui gráficos de comparação!

::: Graficos de comparação

Aqui comparamos o karatsuba com o algoritmo clássico e outros três, repare que do penúltimo gráfico para o último, nós apenas demos um zoom em dois algoritmos!

:graficos
:::

## Desafio (Implementação)

Por fim, caso tenha acabado o handout antes do fechamento, vamos pensar em como implementar esse algorítimo.

Primeiramente, vamos estabelecer algumas regras, por motivos obvios não é permitido simplesmente escrever:

```c
int karatsuba(x, y):
    return x*y;
```

A essa altura você já deve ter percebido que é de extrema importância separarmos os dígitos dos nossos fatores da multiplicação, portanto, precisamos definir qual a quantidade de dígitos de nosso número e então separá-lo em outros dois.

??? Exercício 1
Implemente em **C** uma forma de encontrar a quantidade `c n` de dígitos de um inteiro `c x`

_Dica: Já fizemos esse exercício de forma recurssiva na Aula 2_

::: Gabarito

Implementação da aula 2:

```c
int num_digitos(int n) {
    if (n < 10) {
        return 1;
    }
    return num_digitos(n / 10) + 1;
}
```

Implementação com logaritmo:

```c
int n = (x == 0) ? 1 : log10(x) + 1;
```

:::
???

Em seguida, precisamos determinar uma maneira de extrair de um inteiro seus primeiros e últimos dígitos.

??? Exercício 2
Implemente em **C** uma forma de armazenar a primeira e a segunda metade dos `c n` dígitos de um inteiro `c x`.

_Dica: Explore os tipos de divisão em c_
::: Gabarito

```c
int half = n / 2;
int a = x / pow(10, half);
int b = x % pow(10, half);
```

:::
???

Finalmente, lembrando da expressão para calcular $x \times y$ em funcão de $a$, $b$, $c$, $d$ e $n$:

$$x \times y= ac \times 10^{2(n/2)} + (ad+bc) \times 10^{n/2} + bd$$

??? Exercício 3
Implemente em **C** uma função recursiva que recebe `c int x` e `c int y` e retorna o produto entre esses paramentros:

```c
int karatsuba(int x, int y);
```

Se quiser testar seu código antes de olhar o gabarito, basta comparar o resultado chamando a função karatsuba com a operação padrão de multiplicação:

```c
if (karatsuba(239022, 982311) == (239022 * 982311)){
  printf("Acertei!\n");
}
```

_Dica: Relembre-se do passo a passo para implementar um algoritmo recurssivo (Aula 2)_
::: Gabarito

```c
int karatsuba(int x, int y){
  int n, a, b, c, d, ac, bd, ad_plus_bc;
  double half;
  if ((x < 10) || (y < 10)){
    return x * y;
  }

  n = (x == 0) ? 1 : log10(x) + 1;  // Número de dígitos
  half = n / 2;

  a = x / (int) pow(10.0, half);
  b = x % (int) pow(10.0, half);
  c = y / (int) pow(10.0, half);
  d = y % (int) pow(10.0, half);

  ac = karatsuba(a, c);
  bd = karatsuba(b, d);
  ad_plus_bc = karatsuba(a+b,c+d) - ac - bd;

  return ac * pow(10, 2*half) + ad_plus_bc * pow(10, half) + bd;
}
```

:::
???
