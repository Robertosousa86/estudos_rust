# Data Types

Todo valor em Rust é de um certo de tipo, que diz ao Rust que tipo de dado foi especificado, então ele irá saber como trabalhar com esse tipo de dado especifico.

Em Rust existem dois tipos de subsets de dados: `Scalar` e `Composto`.

Lembrando que Rust é uma linguagem estaticamente tipada, o que significa que os tipos das variáveis devem ser conhecidos em tempo de compilação, mas o copilador também pode inferir o tipo de dado que esta sendo usado baseado no valor e como nós estamos utilizando esse valor, por exemplo:

```rust
let guess: u32 = "42".parse().expect("Not a number!!!");
```

Se no trecho de código acima nós não adicionarmos um tipo, a seguinte mensagem de erro irá aparecer:

```bash
error[E0282]: type annotations needed
--> src/main.rs:2:9
|
2 |
let guess = "42".parse().expect("Not a number!");
|
^^^^^
|
|
|
cannot infer type for `_`
|
consider giving `guess` a type

```

## Scalar Types

Um tipo `Scalar`representa um tipo único. Rust tem 4 tipos primários de `scalar types`:

- Integers
- Floating-point numbers
- Booleans
- Characters

### O Tipo Integer

Um `Integer` é um número sem ponto flutuante. Existem duas representações para `Integer` em Rust, `u` e `i`, mas qual a diferença??
As variáveis declaradas como `u32`, por exemplo, não recebem valores negativos, já as declaradas como `i32` podem ser (ou não) negativas.
Exemplos:

| Tamanho | Signed | Unsigned |
| :-----: | :----: | :------: |
|  8-bit  |   i8   |    u8    |
| 16-bit  |  i16   |   u16    |
| 32-bit  |  i32   |   u32    |
| 64-bit  |  i64   |   u64    |
| 128-bit |  i128  |   u128   |
|  arch   | isize  |  usize   |

Cada variável pode ser `unsigned` ou `signed` e tem seu seu tamanho definido.

Cada variável do tipo `signed` pode armazenar números de -(2 <sup>n-1</sup>) até 2<sup>n -1</sup> inclusivo, onde `n` é o número de bits que a variável utiliza. Então uma variável declarada como `i8` pode receber números de -(2<sup>7</sup>) até 2<sup>7</sup> - 1, que é o equivalente a `-128` até `127`. Já variáveis declaradas como `unsigned` recebem números de 0 2<sup>n</sup> - 1, então um número declarado como `u8` vai de 2<sup>8</sup> -1, ou seja `0 até 255`.

Já o tipo `isize` e `usize` vão depender do tipo de arquitetura que o programa está sendo executado: `64-bit` para arquiteturas 64 bits e `32-bit` para arquiteturas 32 bits.

Podemos escrever inteiros literais de várias formas, mas podemos observar que todos os literais numéricos, exceto o literal de `byte`, permitem um sufixo de tipo, como `57u8`, e `_` como separador visual, como `1_000`.

| Number Literals |   Example   |
| :-------------: | :---------: |
|     Decimal     |   98_222    |
|       Hex       |    0xff     |
|      Octal      |    0o77     |
|     Binary      | 0b1111_0000 |
| Byte (u8 only)  |    b'A'     |

O tipo padrão que o Rust oferece é o `i32`, essa é uma ótima escolha e normalmente é mais rápida, inclusive em sistemas `64-bits`. A situação primaria para o uso de `isize`ou `isize` é a indexação de organização de alguma coleção.

---

**O erro Integer Overflow (Estouro de Inteiro)**

Digamos que nós temos uma variável do tipo `u8` que pode receber valores entre **0 e 255**, se quisermos associar uma valor fora desse range, como por exemplo **256** o erro `Integer Overflow` irá ocorrer. Ruts possui alguns comportamentos interessantes para esse comportamento. Quando estamos compilando em modo de debug, Rust inclui checagem para o erro `integer overflow` que causa no nosso programa um comportamento chamado `panic` no runtime. Quando nós compilamos em modo de release com o a flag `-- release` o Rust não incluirá o `panic`. Em vez disso, se o erro ocorre, Rust realiza o `envolvimento de complemento de dois`. Em suma, valores maiores que o valor máximo que o tipo pode conter “envolvendo” ao mínimo do valor que o tipo pode conter. No caso de um `u8`, **256 se torna 0**, **257 se torna 1**, e assim por diante. O programa não entrará em pânico, mas a variável terá um valor que provavelmente não é o que você esperava que tivesse. Baseando-se em estouros de inteiros o comportamento de empacotamento é considerado um erro. Se você quiser envolver explicitamente, você pode usar o tipo de biblioteca padrão `Wrapping`.

---

## O tipo numérico com pontos flutuante (Floating-Point Types)

Rust também possui dois tipos primários para `floating-point numbers`, que são números com pontos decimais. São eles; `f32` e `f64` que são `32-bits` e `64-bits`, respectivamente. O tipo padrão é `f64` porque em CPUs modernas é aproximadamente a mesma velocidade que `f32`, mas é capaz
de ser mais preciso.

Exemplo:

```rust
fn main() {
  let x = 2.0; // f64
  let y: f32 = 3.0 // f32 com tipagem explicita.
}
```

Os números de ponto flutuante são representados de acordo com a norma IEEE-754. O tipo `f32` é um float de precisão simples e `f64` tem precisão **dupla**.

## Operadores numéricos

O Rust possui os tipos clássicos de operadores numéricos encontrados em outras linguagem de programação:

```rust
fn main() {
// addition
let sum = 5 + 10;
// subtraction
let difference = 95.5 - 4.3;
// multiplication
let product = 4 * 30;
// division
let quotient = 56.7 / 32.2;
// remainder
let remainder = 43 % 5;
}
```

## O tipo Boolean

Como em outras linguagens Rust possui dois possíveis valores para `booleans`: `true` ou `false`:

```rust
fn main() {
  let t = true;
  let f: bool = false; // com tipagem explicita
}
```

## O tipo Char

Até agora trabalhamos apenas com números, mas o Rust também suporta letras. O `char` é o tipo alfabético mais primitivo da linguagem, e o seguinte código mostra uma maneira de usá-lo. (Observe que os literais char são especificados com aspas simples, ao contrário de literais de string, que usam aspas duplas.)

```rust
fn main () {
  let t = "Teste";
  let letra: char = 'A';
  let gato_com_olhos_de_coracao = '😻';
}
```

O tipo `char` do Rust tem quatro bytes de tamanho e representa um Unicode de valor scalar, o que significa que pode representar muito mais do que apenas ASCII, podendo receber: Acentos; letras; caracteres chineses, japoneses e coreanos; emoji; e espaços vazios são todos valores de caracteres válidos em Rust. Os valores escalares Unicode variam de `U+0000 a U+D7FF` e `U+E000 a U+10FFFF` inclusive. No entanto, um “carácter” não é realmente um conceito em Unicode, então sua intuição humana para o que um “carácter” pode não corresponder com o que é um `char` em Rust.

## Tipos compostos

Tipos compostos podem agrupar múltiplos valores em um tipo em um tipo. Rust possui dois tipos primitivos de `compound types`:

- Tuples (tuplas)
- Arrays

## O tipo tuple

Uma tupla é a maneira mais comum de agrupar valores de tipos diferentes dentro de um tipo composto. Uma característica marcante das tuplas é que elas possuem tamanho fixo, quando declaradas não podem ser aumentadas nem diminuídas. Cada posição da tupla recebe um valor diferente e esses valores não precisão ser do mesmo tipo, como no exemplo a seguir:

```rust
fn main () {
  let tup: (i32, f32, u8) = (444, 3.14, 2);
}
```

Uma tupla é declarada através da palavra reservada `let` e os tipos são passados dentro de parenteses, após isso os valores são atribuídos aos tipos.

**OBS: A variável tup se liga à tupla inteira, porque uma tupla é considerada um único elemento composto, porém se um valor for referenciado ao tipo que não corresponde um erro irá ocorrer, como por exemplo:**

```rust
let tup: (i32, u32) = (-10, 2.0) // ocorrerá o erro mismatched types
```

Podemos utilizar a desestruturação com a `tuple` e assim acessar os valores:

```rust
fn this_tuple () {
  let tup = (500, 3.14, -1);

  let (x, y, z) = tup;

  println!("O valor de y é: {}", y);
}
```

Dessa maneira "partimos" a tupla em três partes. Mas também podemos acessar os valores através do `.`, acessando os valores pelos seus respectivos indexadores. Assim como em outras linguagens de programação a contagem por indexador começa por `0`:

```rust
fn main() {
  let my_tuple: (i32, f64, u8) = (-1, 3.14, 100);

  let negativo = my_tuple.0;
  let valor_de_pi = my_tuple.1;
  let centena = my_tuple.2;
}
```

## O tipo Array

Outra maneira de criarmos uma coleção de múltiplos valores em Rust é através de `Arrays`, mas diferente das `Tuples` os `Arrays` só podem conter valores do mesmo tipo. Porém, assim como as tuplas, os arrays em Rust possuem tamanho fixo, o que destoa de outras linguagens, como Javascript, por exemplo.

```rust
let my_array = [1, 2, 3, 4, 5];
```

`Arrays` são úteis quando desejamos alocar dados na `pilha` ao invés da `heap`, ou quando você deseja garantir que sempre tenha um número fixo de elementos. Um `Array` não é um tipo tão flexível quanto um `vetorial`. Um `vetor` é similar a coleção padrão que pode crescer ou diminuir de tamanho. Um exemplo de quando você pode querer usar um `array` em vez de um vetor está em um programa que precisa saber os nomes dos meses do ano. É muito improvável que tal programa precise adicionar ou remover meses, então você pode usar um `array` porque sabe que ela sempre conterá
12 itens:

```rust
let months = ["January", "February", "March", "April", "May", "June", "July",
"August", "September", "October", "November", "December"];
```

Podemos declarar um `array` com tipo especifico e fixar o seu tamanho:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

Declaramos o tipo e o tamanho do `array` separados por ponto e virgula dentro de colchetes e depois os seus valores. Podemos também criar um `array` com valores repetidos e tamanho:

```rust
let my_array = [3; 5];
```

Essa declaração seria mais concisa que; `let a = [3, 3, 3, 3, 3]`.

Se tentarmos acessar um elemento fora do tamanho do array o que acontecerá?

```rust
fn main() {
  let a = [1, 2, 3, 4, 5];
  let index = 10;
  let element = a[index];
  println!("The value of element is: {}", element);
}
```

O código acima não produz nenhum erro em tempo de compilação porém se tentarmos executar o código receberemos um belo erro em tempo de execução, o famoso `runtime error`:

```bash
$ cargo run
Compiling arrays v0.1.0 (file:///projects/arrays)
Finished dev [unoptimized + debuginfo] target(s) in 1.50 secs
Running `target/debug/arrays`
thread '<main>' panicked at 'index out of bounds: the len is 5 but the index
is 10', src/main.rs:6
note: Run with `RUST_BACKTRACE=1` for a backtrace.
```

Isso irá gerar um erro em tempo de execução e o Rust vai entrar em **Panico**, em outras palavras o código irá quebrar.

## Funções

Um dos pontos principais da linguagem `Rust` são as funções, já vimos a mais importante função em `Rust`: A função `main`, que é o ponto de entrada (o equivalente a index em JS). Também vimos a palavras reservada `fn` que é usada para declarar novas funções.

Outro ponto interessante em `Rust` é que utilizamos `snake_case` para declarar funções e nomes de variáveis, o `snake_case` utiliza letras minusculas separando cada palavra por um underscore: `snake_case` `let my_snake_case = "isso é um snake case =)"`.

Podemos criar uma função após declararmos a `main` e invoca-la dentro da função `main`, algo parecido como o `hoisting` em javascript:

```rust
fn main() {
  println!("Hello, world!");
  another_function(); // hoisting
}
fn another_function() {
  println!("Another function.");
}
```

### Funções com parâmetros

Funções também podem receber parâmetros, que são variáveis especiais que fazem parte da declaração de uma função:

```rust
 fn main() {
  println!("Hello, world!");
  another_function(10); // hoisting
}
fn another_function(number: i32) {
  println!("Another function.");
}
```

Devemos declarar o tipo de cada parâmetro na assinatura da função. Essa é uma escolha deliberada no design do Rust: Exigir que o tipo do parâmetro seja declarado na definição das funções. Isso significa que o compilador quase nunca precisa que você fique procurando no código qual o tipo de parâmetro você quis passar. Podemos passar mais de um parâmetro para a função e os tipos não precisam ser os mesmos:

```rust
fn two_params(x: u32, y: i32) {
    println!("The value of x is: {}", x);
    println!("The value of y is: {}", y);
}
```

### Declarações e Expressões em bloco de funções (Statements and Expressions in Function Bodies)

Blocos de funções são compostos de uma série de instruções que terminam opcionalmente em uma expressão. Rust é uma linguagem baseada em expressões, esta é uma distinção importante entre ela e outas linguagens de programação, vamos observar como expressões afetam o corpo das funções. Na verdade, já usamos declarações e expressões. As declarações são instruções que executam alguma ação e não retornam um valor, já expressões avaliam para um valor resultante.

Criar uma variável e atribuir um valor a ela utilizando a palavra reservada `let` é uma declaração (`statement`);

```rust
let y = 6; // declaração (statement)
```

Definições de funções também são `statements`, `statements` não retornam valores, portanto, você não pode atribuir uma declaração (`statement`) `let` para outra variável, como o código a seguir tenta fazer; o que lançara um erro:

```rust
fn  error_statement() {
  let x = (let y = 6);
}
```

Quando tentamos rodar o código acima o seguinte erro acontecera:

```bash
$ cargo run

Compiling functions v0.1.0 (file:///projects/functions)
error: expected expression, found statement (`let`)
--> src/main.rs:2:14
|
 |
let x = (let y = 6);
|
^^^
|
= note: variable declaration using `let` is a statement
```

A instrução `let y = 6` não retorna um valor, então não há nada para ser atribuído a `x`. Isso é diferente do que acontece em outras linguagens, como `C` ou `Ruby`, onde a atribuição retorna o valor de outra atribuição. Nessas linguagens, você pode escrever `x = y = 6` e fazer com que `x` e `y` contenham o valor `6`, o que não acontece em Rust. Expressões avaliam algo e compõem a maior parte do código que você escreverá em Rust. Considere uma operação matemática simples, como `5 + 6`, que é uma expressão que resulta no valor `11`. Expressões podem fazer parte de declarações: No proximo exemplo, o `6` na instrução `let y = 6` é uma expressão que resulta no valor `6`. Chamar uma função é uma expressão, assim como chamar uma macro é uma expressão. O bloco que usamos para criar novos escopos, `{}` também é uma expressão, por exemplo:

```rust
fn statement() {
  let x = 5;

  let y = {
    let x = 3;
    x + 1
  };

  println!("O valor de y é: {}", y);
}
```

- O segundo bloco de código é uma expressão que resulta no valor `4`: `{ let x = 3 }`;
- Este valor é vinculado a `y` como parte da instrução `let`;
- Podemos observar que no final do trecho de código `x + 1` não há ponto e virgula (`;`) o que destoa da maioria das linhas de código que vimos até aqui, isso significa que expressões não incluem ponto e virgula (`;`) no final;
- Se adicionarmos um ponto e virgula ao final de uma `expressão`, acabamos de transforma-la em uma `declaração`, que não irá retornar nenhum valor.

Esses pontos acima mencionados merecem bastante atenção...

### Funções com retorno de valores

Funções podem retornar valores aos trechos de códigos que à chamam. Em Rust não nomeamos os valores de retorno, ao invés disso utilizamos o "simbolo" `->`. O valor de retorno é sinônimo do valor final da expressão do bloco do corpo da função. Podemos retornar um valor mais cedo de uma função usando a palavra-chave `return` e especificando um valor, mas a maioria das funções retorna a última expressão implicitamente. Aqui está um exemplo de uma função que retorna um valor:

```rust
fn this_return_five() -> i32 {
  5
}

fn returning_five() {
  let x = five();

  println!("O valor de x é: {}", x)
}
```

O número 5 é o retorno da função `returning_five`, e o tipo do returno é um inteiro assinado de 32-bits, ou seja, `i32`. Existem duas partes importantes:

- A linha `let x = this_return_five()`; mostra que estamos usando o valor de retorno de uma função para inicializar uma variável. Como a função `this_return_five` retorna um 5, essa linha é o equivalente a; `let x = 5;`
- A função `this_return_five` não tem parâmetros e define o tipo de valor de retorno, mas o corpo da função só contem um número 5 sem ponto e virgula, porque isso é uma expressão que retornara um valor.

Outro exemplo:

```rust
fn main {
  let x plus_one(5);

  println!("O valor de x é: {}", x);
}

fn plus_one(x: i32) -> i32 {
  x + 1
}
```

Se executarmos esse código o resultado será; `O valor de x é 6`, mas se colocarmos um ponto e virgula no final da linha `x + 1;`, mudamos uma `expressão (expression)` para uma `declaração (statement)` o que resultara em um erro quando compilamos o código:

```bash
error[E0308]: mismatched types
--> src/main.rs:7:28
|
 |
fn plus_one(x: i32) -> i32 {
| ____________________________^
 | |
x + 1;
| |
= help: consider removing this semicolon
 | | }
| |_^ expected i32, found ()
|
= note: expected type `i32`
found type `()
```

A principal mensagem de erro é: "mismatched types", o que revela o problema principal com esse código. A definição da função `plus_one` diz que ela retornara um `i32`, mas a **declaração (statement)** não resulta em um valor, o que é representado por `expect i32, found ()` o que quer dizer; `Uma tupla vazia`. Portanto, nada é retornado, o que contradiz a definição da função, o que resulta no erro.

## Fluxo de controle de dados (Data flow)

Decidir se devemos ou não executar algum código dependendo se uma condição é verdadeira ou falsa ou decidir executar algum código repetidamente enquanto uma condição está sendo atendida são blocos de construção básicos na maioria das linguagens de programação. A maioria dessas construções que permitem controlar o fluxo de execução do código Rust são expressões `if` e `loops`.

### Expressões if

Uma expressão `if` permite ramificar o nosso código dependendo de condições. Provemos uma condição e um estado, "se a condição for atendida, execute esse bloco de código. Se esse trecho de código não atender a condição não execute esse trecho de código."

um exemplo simples:

```rust
fn what_the_result() {
  let number: i32 = 3;

  if number < 5 {
    println!("A condição é verdadeira.");
  } else {
    println!("A condição é falsa.");
  }
}
```

Também vale a pena notar que a condição neste código deve ser um `bool`. Se a condição não for um `bool`, receberemos um erro. Por exemplo, tente executar o seguinte código:

```rust
fn main () {
  let number = 3;

  if number {
    println!("O número é três.");
  }
}
```

Se tentarmos executar, o código retornara um erro:

```bash
error[E0308]: mismatched types
--> src/main.rs:4:8


if number {

^^^^^^ expected bool, found integral variable

= note: expected type `bool`
found type `{integer}`
```

O erro indica que a condição esperava um boolean mas recebeu integer. Diferente de outras linguagens como `Ruby` ou `Javascript`, `Rust` não tenta converter automaticamente um valor não-boolean em boolean. Devemos ser explícitos e sempre fornecer ao `if` um boolean como sua condição. Se quisermos que o bloco de código `if` seja executado somente quando um número não é igual a 0, por exemplo, podemos alterar o `if` para a seguinte expressão:

```rust
fn main() {
  let number = 3;

  if number != 0 {
    println!("O número é diferente de zero.");
  }
}
```

### Múltiplas condições com `else if`

Podemos lidar com múltiplas condições utilizando a combinação de `if`e `else` na expressão `else if`:

```rust
fn main() {
 let number = 6;
 if number % 4 == 0 {
   println!("number is divisible by 4");
} else if number % 3 == 0 {
    println!("number is divisible by 3");
} else if number % 2 == 0 {
    println!("number is divisible by 2");
} else {
    println!("number is not divisible by 4, 3, or 2");
}
}
```

Quando o programa é executado, ele verifica cada expressão `if` por sua vez e executa o primeiro bloco para o qual a condição é verdadeira. Observe que mesmo `6` sendo divisível por `2`, não vemos que o número de saída é divisível por `2`, nem vemos que o número não é divisível por `4`, `3` ou `2` do bloco de `else`. Isso porque Rust só executa o bloco para a primeira condição verdadeira, e uma vez que encontra uma condição satisfatória, ele nem verifica o resto. Usar muitas expressões `else if` pode desordenar nosso código, então se você tiver mais de um, você pode querer refatorar seu código. Existe uma condição chamada `match` que é ideal para essas situações, veremos ela em breve.

### Utilizando `if` em uma declaração `let`

Como `if` é uma expressão, nós podemos utiliza-la do lado direito da declaração `let`, como no exemplo a seguir:

```rust
fn main() {
  let condition = true;
  let number = if condition {
    5
  } else {
    6
  };

  println!("O valor de number é: {}", number);
}
```

A variável `number` será vinculada a um valor com base no resultado da expressão `if`:

```bash
$ O valor do número é: 5
```

Lembre-se de que os blocos de código são avaliados para a última expressão neles, e os números por si só também são expressões. Neste caso, o valor de toda a expressão `if` depende de qual bloco de código é executado. Isso significa que os valores têm o potencial de serem resultados de cada "ramificação" do `if` e deve ser do mesmo tipo; na Listagem 3-2, os resultados do braço if e o outro braço eram i32 inteiros. Se os tipos forem incompatíveis, como no exemplo a seguir, receberemos um erro:

```rust
fn main() {
  let condition = true;
  let number = if condition {
    5
} else {
    "six"
};
  println!("The value of number is: {}", number);
}
```

O que acontece aqui é a incompatibilidade de tipos:

```bash
error[E0308]: if and else have incompatible types
--> src/main.rs:4:18
|
 |
let number = if condition {
| __________________^
 | |

 | |
} else {
 | |
"six"
 | |
};
| |_____^ expected integral variable, found &str
|
= note: expected type `{integer}`
found type `&str`
```

A expressão no bloco `if` é avaliada como um `inteiro`, e a expressão no bloco `else` é avaliada como uma `string`. Isso não funcionará porque as variáveis devem ter um único tipo. Rust precisa saber em **tempo de compilação** qual o tipo da variável `number`, para poder verificar em tempo de compilação que seu tipo é válido em todos os lugares em que usamos a variável `number`. Rust não seria capaz de fazer isso se o tipo de `number` fosse determinado apenas em **tempo de execução**; o compilador seria mais complexo e daria menos garantias sobre o código se tivesse que acompanhar vários tipos hipotéticos para qualquer variável.

### Repetição com Loops

É comum executar um bloco de códigos mais de uma vez, para essa tarefa `Rust` prove vários `loops`. Um `loop` é executado dentro de um bloco de código de repetição até o final e depois volta ao começo imediatamente:

```rust
fn main () {
  loop {
    println!("De novo e de novo... e de nono...");
  }
}
```

Quando executamos esse código ele repete o texto até usarmos o comando `CTRL-C`, que irá interromper a execução. Outra maneira de parar o código é usar a palavra reservada `break`, como no programa de adivinhação de números.

### Retornando valores a partir de `loops`

Um dos usos de `loops` é tentar novamente uma operação que você sabe que pode falhar, como verificar se um `thread` concluiu seu trabalho. No entanto, você pode precisar passar o resultado dessa operação para o resto do seu código, para fazer isso, você pode adicionar o valor que deseja retornar após a expressão `break` que você usa para parar o `loop`; esse valor será retornado fora do `loop` para que você possa usar isso, como mostrado aqui:

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

    if counter == 10 {
        break counter * 2;
    }
};

  println!("The result is {}", result);
}
```

Antes do `loop`, nós declaramos uma variável chamada `counter` e a inicializamos com `0`, depois disso declaramos outra variável chamada `result` para guardar o valor retornado do `loop`. Para cada iteração do `loop`, nós adicionamos `1` a variável `counter` e utilizamos o `if` para checar se o contador tem seu valor é igual a `10`, quando isso acontece o trecho de código `break counter * 2;` Após o loop, usamos um ponto e vírgula para encerrar o estado (statement) que atribui o valor a `result`. Por fim, imprimimos o valor em `result`, que neste caso é `20`.

### Loops condicionais com `while`

Muitas vezes é útil para m programa avaliar uma condição entro e um `loop`. Enquanto uma condição for verdadeira ela será executada, quando ela deixar de ser verdadeira, o programa executara o `break`. Uma maneira de criarmos loops com condições é utilizarmos o loop `while`:

```rust
fn main() {
  let mut number: i32 = 3;

  while number != 0 {
    println!("{}!", number);

    number = number - 1
  }

  println("LIFTOFF! (WOW isso significa decolar!)");
}
```

Essa implementação elimina muito a necessidade de usarmos `if's, loop's, else's e break's` e acaba sendo mais fácil de entendermos.

### Percorrendo uma coleção utilizando o laço `for`

Podemos utilizar o laço `while` para percorrer os elementos de uma coleção, como de um `array` por exemplo:

```rust
fn main() {
  let a [10, 20, 30, 40, 50];
  let mut index = 0;

  while index < 5 {
    println!("O valor é: {}", a[index]);

    index = index + 1;
  }
}
```

Todos os cinco valores da matriz aparecem no terminal, conforme esperado. Apesar de índice atingirá um valor de `5` em algum ponto, o loop para de executar antes, tentando buscar um sexto valor da matriz. Mas essa abordagem é propensa a erros; podemos fazer com que o programa entre em `pânico` se o comprimento do índice estiver incorreto. Também é lento, **porque o compilador adiciona código de tempo de execução para realizar a verificação condicional em cada elemento em cada iteração através do loop**. Como uma alternativa mais concisa, você pode usar um `loop for` e executar algum código para cada item em uma coleção:

```rust
fn main() {
  let a = [10, 20, 30, 40, 50, 60];

  for element in a.iter() {

     println!("O valor é: {}", element);
  }
}
```

Quando executarmos esse código, veremos a mesma saída do acima. Mais importante, agora aumentamos a segurança do código e eliminamos a chance de bugs que podem resultar de ir além do final do a matriz ou não indo longe o suficiente e faltando alguns itens. Por exemplo, no código de exemplos anteriores, se você removeu um item do `array`, mas esqueceu de atualizar a condição para `while index < 4`, o código entraria em pânico. Usando o `loop for`, você não precisa se lembrar de alterar qualquer outro código se você alterou o número de valores na matriz (array).
A segurança e a concisão dos `loops for` os tornam os mais comuns em Rust. Mesmo em situações em que você deseja executar algum código um certo número de vezes, como no exemplo de contagem regressiva que usado um `loop while`, a maioria dos `Rustaceans` usaria um `loop for`.
A maneira de fazer isso seria usar um Range, que é um tipo fornecido pela biblioteca de dados que gera todos os números em sequência a partir de um número e terminando antes de outro número. Podemos fazer uma contagem regressiva utilizando o método `rev` e o laço `loop for`:

```rust
fn main() {
  // (1..4) esse Range vai de 1 até 3.
  for number in (1..4).rev() {
    println!("{}!", number);
  }

  println!("LIFTOFF!!!!");
}
```

Assim o código fica muito mais elegante =)
