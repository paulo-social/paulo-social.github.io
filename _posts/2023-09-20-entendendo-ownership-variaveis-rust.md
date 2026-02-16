---
layout: post
title: "Entendendo o Ownership de Variáveis em Rust"
subtitle: "Estou estudando Rust e estou gostando bastante, a robustez, segurança e o controle de memória da linguagem são incríveis."
date: 2023-09-20 12:00:00 -0300
background: "/img/bg-post-3.jpg"
---

Estou estudando Rust e estou gostando bastante, a robustez, segurança e o controle de memória da linguagem são incríveis. E dentro dessa aventura, de um iniciante nesse mundo (risos), achei bastante interessante como é tratado a propriedade dos valores de variáveis, chamado de "ownership" de variáveis.

## Mas o que é Ownership?

Em Rust, cada valor tem um único proprietário, que é a variável que possui esse valor desde a sua declaração, ou seja, quando você cria uma variável e atribui um valor a ela, essa variável se torna responsável por esse valor.

E nesse cenário, existem 3 pilares que fundamentam isso:

### 1. Único proprietário

Quando você cria uma variável em Rust, essa variável se torna a única proprietária do valor atribuído a ela, em resumo, você não pode simplesmente copiar ou atribuir esse valor a outra variável sem fazer algo especial, ou você receberá uma mensagem no momento da construção parecida com essa: `value borrowed here after move`.

### 2. Quando o proprietário sai de escopo, o valor é liberado

Rust é muito rigoroso com o gerenciamento de memória. Quando a variável que é a proprietária de um valor sai de escopo, Rust automaticamente libera a memória associada a esse valor. Isso significa que não há vazamentos de memória em Rust, pois o próprio compilador garante que a memória seja liberada adequadamente.

### 3. Transferência de Propriedade (Ownership Transfer)

Para que outros trechos de código tenham acesso a um valor que já possui um proprietário, você deve transferir a propriedade desse valor. Isso pode ser feito por meio de uma atribuição ou retornando o valor de uma função. Quando a propriedade é transferida, o antigo proprietário não pode mais acessar o valor.

## Entendendo o que é "ownership" de variáveis em Rust, temos algum exemplo prático?

Sim, agora que entendemos como funciona esse mecanismo em Rust, vamos analisar um exemplo e como corrigi-lo.

Abaixo temos um código que iremos salvar num arquivo chamado `ownership_test.rs` muito simples em que declaramos uma "String" e queremos colocar esse mesmo valor em outra variável:

```rust
fn main() {
    let s1 = String::from("Olá mundo!");
    let s2 = s1;

    println!("s1: {}", s1);
    println!("s2: {}", s2);
}
```

Quando formos compilar esse código, usando o linha de comando `rustc ownership_test.rs`, iremos receber a seguinte mensagem no momento da construção do binário.

```shell
~/projects/github
(darwin22.0) ❯ rustc ownership_test.rs
error[E0382]: borrow of moved value: `s1`
 --> ownership_test.rs:5:24
  |
2 |     let s1 = String::from("Olá mundo!");
  |         -- move occurs because `s1` has type `String`, which does not implement the `Copy` trait
3 |     let s2 = s1;
  |              -- value moved here
4 |
5 |     println!("s1: {}", s1);
  |                        ^^ value borrowed here after move
  |
  = note: this error originates in the macro `$crate::format_args_nl` which comes from the expansion of the macro `println` (in Nightly builds, run with -Z macro-backtrace for more info)
help: consider cloning the value if the performance cost is acceptable
  |
3 |     let s2 = s1.clone();
  |                ++++++++

error: aborting due to previous error

For more information about this error, try `rustc --explain E0382`.
```

Outra característica bem interessante de Rust, é que ele dá muitos detalhes sobre o erro que ocorreu no momento da construção da aplicação, nesse caso podemos ver dois blocos, um falando do erro, e outro falando de como corrigir esse erro, mais especificamente dizendo pra usar o método `.clone()`.

## Corrigindo o código para atender o fluxo desejado

Então como a mensagem no momento da construção diz, para corrigir vamos fazer a seguinte alteração no código, em que no momento da atribuição do valor em `s2`, clonarmos `s1`, dessa forma não haverá a transferência de propriedade do valor.

```rust
fn main() {
    let s1 = String::from("Olá mundo!");
    let s2 = s1.clone();

    println!("s1: {}", s1);
    println!("s2: {}", s2);
}
```

E novamente vamos compilar esse código usando o comando `rustc ownership_test.rs` e agora será compilado com sucesso. :)

```shell
~/projects/github 8s
(darwin22.0) ❯ rustc ownership_test.rs

~/projects/github
(darwin22.0) ❯ ./ownership_test
s1: Olá mundo!
s2: Olá mundo!
```

Caso queiram mais detalhes sobre "ownership" de variáveis em Rust, basta acessar a documentação [https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html).
