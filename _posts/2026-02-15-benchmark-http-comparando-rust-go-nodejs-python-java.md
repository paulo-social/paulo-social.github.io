---
layout: post
title: "Benchmark HTTP: Comparando Rust, Go, Nodejs, Python e Java"
subtitle: "Seguindo o fluxo de estudo que estou fazendo de Rust, tive a curiosidade de compara-la com outras linguagens que utilizo, pra entender sua performance em relação as outras."
date: 2026-02-15 23:35:00 -0300
background: "/img/bg-post-3.jpg"
---

Para integrações de aplicações é muito comum utilizarmos APIs utilizando protocolo HTTP, permitindo a comunicação entre sistemas, aplicativos e serviços.

A escolha da linguagem que será utilizada pra desenvolver determinada API irá impactar diretamente e significativamente o desempenho e a eficiência do serviço criado, então é preciso conhecer muito bem o produto que está sendo desenvolvido, seu potencial e o quanto precisará escalar, pra que a escolha da tecnologia não seja um problema pra sua aplicação.

Neste artigo vou comparar Rust (Actix), Golang (Std), Java (Quarkus), Nodejs (Express) e Python (Sanic e FastAPI), para avaliar como cada uma delas reagem com uma tarefa simples, retornar uma mensagem "Hello from <tecnologia>".

## Configuração da máquina utilizada no Benchmark

Para realizar uma comparação justa e precisa entre essas linguagens e frameworks, utilizaremos a mesma máquina de teste e as seguintes configurações:

- **Sistema Operacional:** Ubuntu 23.04 x86_64
- **CPU:** Intel i7–8565U (8) @ 4.600GHz
- **RAM:** 16GB

Todos os servidores serão configurados para responder na porta 8080.

## Linguagens utilizadas com seus respectivos códigos

Os seguintes trechos de código foram utilizados para o benchmark, com seus respectivos comandos para iniciar a aplicação, e todos esses exemplos foram testados com o seguinte comando.

### Rust com Actix

Como vocês já devem ter percebido no último artigo que fiz "ownership de variáveis" perceberam que Rust é conhecido por sua segurança e desempenho. O framework Actix é altamente concorrente e promete alto desempenho, e também é o framework que estou estudando.

```rust
use actix_web::{App, HttpServer, HttpResponse, Responder, get};

#[get("/")]
async fn hello() -> impl Responder {
    HttpResponse::Ok().body("Hello world!")
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new()
            .service(hello)
    })
    .bind("127.0.0.1:8080")?
    .run()
    .await
}
```

### Golang com a Standard Lib

Go é uma linguagem de programação projetada para eficiência e desempenho. Utilizei a biblioteca padrão, pois fiz o mesmo teste utilizando Gin e a performance com a biblioteca padrão foi melhor.

```go
package main

import (
  "io"
  "net/http"
)

func main() {
  http.HandleFunc("/", helloWorld)
  http.ListenAndServe(":8080", nil)
}

func helloWorld(w http.ResponseWriter, r *http.Request) {
  io.WriteString(w, "Hello world from Golang!")
}
```

### Python com Sanic e FastAPI

Python é conhecido por sua legibilidade e facilidade de uso. FastAPI é um framework web moderno e de alto desempenho para Python, resolvi colocar o Sanic nos testes por ter excelente desempenho também, embora nunca tenha trabalhado com ele.

**Sanic:**

```python
from sanic import Sanic
from sanic.response import text

app = Sanic("BenchmarkHelloWorldApp")

@app.get("/")
async def hello_world(_):
    return text("Hello, world from Sanic!")
```

**FastAPI:**

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return "Hello World from Fastapi!"
```

### Java com Quarkus no formato nativo

Java é uma linguagem de programação amplamente utilizada, e Quarkus é um framework Java nativo de nuvem conhecido por sua inicialização rápida.

```java
package org.acme;

import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.Produces;
import jakarta.ws.rs.core.MediaType;

@Path("/hello")
public class GreetingResource {

    @GET
    @Produces(MediaType.TEXT_PLAIN)
    public String hello() {
        return "Hello from Java Quarkus!";
    }

}
```

### Node.js com Express

Node.js é uma plataforma de execução JavaScript amplamente utilizada, e Express é um framework Node.js conhecido por sua simplicidade e flexibilidade no desenvolvimento de aplicativos da web.

```javascript
const express = require("express");
const app = express();
const port = 8080; // Escolha a porta desejada, por exemplo, 3000

app.get("/", (req, res) => {
  res.send("Hello, World from Node.js!");
});

app.listen(port, () => {
  console.log(`Running: http://localhost:${port}`);
});
```

## Executando o Benchmark

Para realizar o benchmark, utilizaremos a ferramenta Apache Benchmark (ab) para fazer 100.000 requisições a cada API e mediremos o tempo médio de resposta. O comando de benchmark será o mesmo para todas as linguagens:

```bash
ab -n 100000 -c 10 http://127.0.0.1:8080/
```

## Resultados do Benchmark

Após a execução do benchmark, obtivemos os seguintes resultados:

**Rust com Actix:**

- Tempo médio de resposta: 3.9 ms
- Requisições por segundo: 25123.34

**Go com Std:**

- Tempo médio de resposta: 4.3ms
- Requisições por segundo: 22812.47

**Python com FastAPI:**

- Tempo médio de resposta: 19.4 ms
- Requisições por segundo: 5148.51

**Python com Sanic:**

- Tempo médio de resposta: 6.1 ms
- Requisições por segundo: 16214.37

**Java com Quarkus:**

- Tempo médio de resposta: 5 ms
- Requisições por segundo: 19987.55

**Node.js com Express:**

- Tempo médio de resposta: 14 ms
- Requisições por segundo: 6973.13

Nesse teste simples realizado, com esses parâmetros e códigos, Rust é muito mais rápido que as outras. No repositório estão os códigos utilizados e os resultados com mais detalhes do teste no arquivo result.txt que está nos projetos.
