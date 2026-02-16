---
layout: post
title: "Diferenças na propagação e tratamentos de erros em Rust, Golang e Java"
subtitle: "Mais um texto sobre Rust, e novamente comparando com outras linguagens pra ter uma melhor didática."
date: 2026-02-15 22:45:00 -0300
background: "/img/bg-post-3.jpg"
---

Mais um texto sobre Rust, e novamente comparando com outras linguagens pra ter uma melhor didática. Achei interessante trazer Golang pela semelhança com Rust e Java por propagar e tratar da mesma forma que várias linguagens populares.

Escrevi esse texto, porque tratamentos de erros é uma parte muito importante no comportamento de um software.

## Propagação de erros em Golang

Golang é conhecida por sua simplicidade e eficiência. No que diz respeito à propagação de erros, Golang utiliza um modelo de retorno de valores múltiplos para indicar erros. Isso significa que uma função pode retornar tanto o resultado desejado quanto um valor de erro, que é um valor especial que representa um erro. Um exemplo simples de código em Golang que ilustra esse conceito:

```go
package main

import (
    "errors"
    "fmt"
)

// Função que realiza uma operação e retorna um erro, se ocorrer algum.
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("Divisão por zero não é permitida")
    }
    return a / b, nil
}

func main() {
    // Chamada da função divide com tratamento de erro.
    resultado, err := divide(10, 0)
    if err != nil {
        fmt.Println("Erro:", err)
    } else {
        fmt.Println("Resultado:", resultado)
    }

    // Chamada bem-sucedida.
    resultado, err = divide(10, 2)
    if err != nil {
        fmt.Println("Erro:", err)
    } else {
        fmt.Println("Resultado:", resultado)
    }
}
```

Neste exemplo, a função `divide` retorna dois valores: o resultado da divisão e um erro, que é `nil` se a operação for bem-sucedida ou contém uma mensagem de erro se a divisão por zero for detectada.

A propagação de erros em Golang é normalmente realizada por meio de verificações de erros locais, onde a função que chama verifica explicitamente o erro retornado e decide como lidar com ele. Isso significa que o controle sobre como os erros são tratados está nas mãos do desenvolvedor, o que pode levar a um código mais claro e fácil de entender.

## Propagação de erros em Rust

Rust é conhecida por seu foco em segurança e prevenção de erros em tempo de execução. Em Rust, a propagação de erros é tratada por meio do sistema de tipos do Rust e do uso extensivo de enums (tipos enumerados). O exemplo a seguir demonstra como a propagação de erros é feita em Rust:

```rust
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        return Err("Divisão por zero não é permitida".to_string());
    }
    Ok(a / b)
}

fn main() {
    // Chamada da função divide com tratamento de erro.
    match divide(10.0, 0.0) {
        Ok(resultado) => println!("Resultado: {}", resultado),
        Err(erro) => println!("Erro: {}", erro),
    }

    // Chamada bem-sucedida.
    match divide(10.0, 2.0) {
        Ok(resultado) => println!("Resultado: {}", resultado),
        Err(erro) => println!("Erro: {}", erro),
    }
}
```

Em Rust o código fica menor e aqui, a função `divide` retorna um `Result`, que pode ser `Ok` com o valor resultante ou `Err` com uma mensagem de erro. A verificação de erros em Rust é fortemente incentivada por meio do uso do operador `match`, que permite ao desenvolvedor lidar de maneira abrangente com os resultados, como mostra nesse trecho:

```rust
match divide(10.0, 2.0) {
    Ok(resultado) => println!("Resultado: {}", resultado),
    Err(erro) => println!("Erro: {}", erro),
}
```

Dessa forma, Rust impõe uma forte verificação de erros em tempo de compilação, garantindo que os desenvolvedores considerem todas as possíveis situações de erro.

## Propagação de erros em Java

E por fim, a propagação e tratamentos de erros em Java são feitos por meio do uso de exceções. Resolvi trazer um exemplo em Java, porque é bem próximo como é utilizado em outras linguagens mais comuns, como Javascript, Python, Ruby e etc.

É uma forma de mostrar como o tratamento em Rust e Golang se diferente das formas tradicionais por uso de exceção:

```java
public class ExemploDivisao {

    // Método que realiza uma operação de divisão e lança uma exceção em caso de erro.
    public static double divide(double a, double b) throws ArithmeticException {
        if (b == 0) {
            throw new ArithmeticException("Divisão por zero não é permitida");
        }
        return a / b;
    }

    public static void main(String[] args) {
        try {
            // Chamada do método divide com tratamento de exceção.
            double resultado = divide(10.0, 0.0);
            System.out.println("Resultado: " + resultado);
        } catch (ArithmeticException e) {
            System.out.println("Erro: " + e.getMessage());
        }

        try {
            // Chamada bem-sucedida.
            double resultado = divide(10.0, 2.0);
            System.out.println("Resultado: " + resultado);
        } catch (ArithmeticException e) {
            System.out.println("Erro: " + e.getMessage());
        }
    }

}
```

Neste exemplo em Java, a função `divide` recebe dois números como entrada e lança uma exceção `ArithmeticException` caso a divisão por zero seja detectada.

No método `main()`, a função `divide` é chamada duas vezes: uma vez com uma divisão por zero intencional e outra vez com uma divisão válida.

O tratamento de exceções é feito por meio do uso de blocos `try-catch`, onde a exceção `ArithmeticException` é capturada e a mensagem de erro é impressa caso ocorra um erro. Caso contrário, o resultado da operação é impresso.

## Visão geral das diferenças entre Golang, Rust e Java?

### Go (Golang)

- **Valores de Erro Explícitos:** Em Go, a propagação de erros é feita usando valores de erro explícitos que são retornados como múltiplos valores de uma função. Isso significa que as funções frequentemente retornam um resultado junto com um valor de erro.
- **Verificação de Erros Locais:** A verificação de erros em Go é frequentemente feita localmente, o que significa que o código que chama uma função deve explicitamente verificar o erro retornado e tomar decisões com base nesse erro.
- **Convenção de nomenclatura:** A convenção de nomenclatura comum em Go é nomear o valor de erro como `err`. As verificações de erro geralmente envolvem a verificação se `err` é igual a `nil`, o que indica que não ocorreu erro.

### Rust

- **Resultado (Result) como Tipo de Retorno:** Em Rust, a propagação de erros é feita usando o tipo de retorno `Result`, que pode conter um valor bem-sucedido (`Ok`) ou um valor de erro (`Err`). As funções que podem gerar erros retornam `Result`.
- **Padrão de Tratamento de Erros:** Em Rust, o tratamento de erros é realizado usando o operador `match`, que permite aos desenvolvedores fazer correspondência de padrões nos valores `Result` para lidar com os casos de sucesso e erro de forma explícita.

### Java

- **Exceções:** Em Java, a propagação de erros é geralmente feita por meio do uso de exceções. Quando ocorre um erro, uma exceção é lançada explicitamente usando a palavra-chave `throw`.
- **Tratamento de Exceções:** O tratamento de exceções é realizado por meio de blocos `try-catch`. O código que pode gerar uma exceção é colocado dentro de um bloco `try`, e as exceções são capturadas e tratadas em blocos `catch`.

Em resumo, Go utiliza valores de erro explícitos e a verificação de erros locais, Rust usa o tipo de retorno `Result` e requer o tratamento explícito usando `match`. Java, por outro lado, emprega exceções e blocos `try-catch` para lidar com erros.

[Link para o repositório](https://github.com/xxpauloxx/propagation-errors-in-rust-and-golang/)
