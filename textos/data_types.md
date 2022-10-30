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
