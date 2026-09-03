# Linguagem de Expressões com Suporte a Records

Universidade Federal de Pernambuco - Centro de Informática
IN1007 - Paradigmas de Linguagens de Programação

## Equipe

* Dayane Lira
* Lucas Mascena

## Escopo

### Descrição

Este projeto busca estender a linguagem **Expressões 2** com suporte nativo a **Records**, permitindo a criação de estruturas compostas por campos nomeados e heterogêneos.

Records são valores imutáveis formados por um conjunto de pares no formato `campo = expressão`, em que cada campo pode armazenar valores dos tipos já suportados pela linguagem ou outros Records.

A extensão também adiciona acesso a campos por meio do operador `.`, possibilitando expressões como `pessoa.nome` e acessos encadeados como `pessoa.endereco.cidade`.

O sistema de tipos da linguagem é estendido com **tipagem estrutural de Records**. Dessa forma, o tipo de um Record é determinado pelos nomes e tipos de seus campos, independentemente dos valores armazenados.

Por exemplo:

```text
record {
    nome = "Dayane",
    idade = 26
}
```

possui o tipo:

```text
Record {
    nome: STRING,
    idade: INTEIRO
}
```

A extensão mantém os Records imutáveis e não introduz operações de alteração, inclusão ou remoção de campos após sua criação.

### Elementos Adicionados

* Estrutura de dados:

  * `record`: representa uma coleção imutável de campos nomeados.

* Expressões:

  * criação de Record;
  * acesso a campo;
  * acesso encadeado a campos.

* Sistema de tipos:

  * `TipoRecord`;
  * inferência do tipo de cada campo a partir de sua expressão;
  * igualdade estrutural entre tipos de Records;
  * validação estática do acesso a campos.

* Validações:

  * acesso a campo inexistente;
  * acesso a campo em valor que não seja Record;
  * declaração duplicada de campos.

### Características dos Records

Os Records possuem campos heterogêneos, permitindo combinar diferentes tipos em uma única estrutura:

```text
record {
    nome = "Dayane",
    idade = 26,
    ativo = true
}
```

Os valores dos campos podem ser definidos por expressões:

```text
record {
    idade = 20 + 6,
    descricao = "Paradigmas " ++ "de Linguagens"
}
```

Records também podem ser aninhados:

```text
record {
    nome = "Dayane",
    endereco = record {
        cidade = "Recife",
        estado = "PE"
    }
}
```

### Tipagem Estrutural

Dois Records possuem o mesmo tipo quando possuem os mesmos campos associados aos mesmos tipos.

Assim:

```text
record {
    nome = "Dayane",
    idade = 26
}
```

e:

```text
record {
    idade = 30,
    nome = "Maria"
}
```

possuem o mesmo tipo estrutural:

```text
Record {
    nome: STRING,
    idade: INTEIRO
}
```

A ordem de declaração dos campos não interfere na equivalência entre tipos.

A extensão não contempla subtipagem estrutural. Portanto, Records com conjuntos diferentes de campos são considerados tipos diferentes.

## Exemplos de Código

### Criação de um Record

```text
record {
    nome = "Dayane",
    idade = 26
}
```

### Record associado a uma variável

```text
let
    var pessoa = record {
        nome = "Dayane",
        idade = 26
    }
in
    pessoa.nome
```

Resultado:

```text
Dayane
```

### Campos definidos por expressões

```text
let
    var ano = 2026
in
    record {
        anoAtual = ano,
        proximoAno = ano + 1
    }.proximoAno
```

Resultado:

```text
2027
```

### Records aninhados

```text
let
    var pessoa = record {
        nome = "Dayane",
        endereco = record {
            cidade = "Recife",
            estado = "PE"
        }
    }
in
    pessoa.endereco.cidade
```

Resultado:

```text
Recife
```

### Acesso inválido

```text
let
    var pessoa = record {
        nome = "Dayane"
    }
in
    pessoa.idade
```

O programa é considerado semanticamente inválido, pois o campo `idade` não pertence ao tipo do Record associado a `pessoa`.

## Gramática

```text
Programa ::= Expressao

Expressao ::= Valor
           | ExpUnaria
           | ExpBinaria
           | ExpDeclaracao
           | Id
           | ExpRecord
           | ExpAcessoCampo

Valor ::= ValorConcreto
        | ValorRecord

ValorConcreto ::= ValorInteiro
                | ValorBooleano
                | ValorString

ExpRecord ::= "record" "{" "}"
            | "record" "{" ListaCampos "}"

ListaCampos ::= Campo
              | Campo "," ListaCampos

Campo ::= Id "=" Expressao

ExpAcessoCampo ::= Expressao "." Id

ExpUnaria ::= "-" Expressao
            | "not" Expressao
            | "length" Expressao

ExpBinaria ::= Expressao "+" Expressao
             | Expressao "-" Expressao
             | Expressao "and" Expressao
             | Expressao "or" Expressao
             | Expressao "==" Expressao
             | Expressao "++" Expressao

ExpDeclaracao ::= "let" Declaracao "in" Expressao

Declaracao ::= DecVariavel
             | DecComposta

DecVariavel ::= "var" Id "=" Expressao

DecComposta ::= Declaracao "," Declaracao
```

