# Alurafood

link para o certificado: [link](https://cursos.alura.com.br/certificate/beportoss/microsservicos-implementando-java-spring)

---

## Sobre: 

Este é um repositório que agrupa os microsserviços criados no curso 'Microsserviços na prática: implementando com Java e Spring'.
Para roda-los é preciso ter a versão 17 do java, assim como o docker para rodar o banco de dados a partir do docker-compose que está na raiz deste projeto, só terá que alterar o path indicando onde fica seus volumes, pois no docker-compose está o meu. Para ficar mais fácil, os exemplos das chamadas estaram do arquivo do postman, também na raiz deste repositório.

Algumas coisas interessantes que tentei fazer durante o curso, me desafiei a criar um microsserviço com Java, como o curso seguia, e outro com Kotlin. Também, outro desafio foi rodar o banco com o docker, cada vez busco mais usa-lo.

Há os seguintes microsserviços:

- alurafood-gateway : que tem a função de centralizar as chamadas
- alurafood-pedidos : que centraliza os pedidos
- alurafood-pagamentos : que tem a função de realizar a atualização (criação, cancelamento etc) de pagamento interagindo com o microsserviço de pedidos
- alurafood-eureka-server : um server que usa o spring-cloud-netflix para ser um server (como o nome já sugere hehe)

Caso tenha problemas em conseguir baixar os projetos, ou só quer dar uma olhada sem precisar baixar, vou colocar o link deles a seguir:

- [alurafood-gateway](https://github.com/BEp0/alurafood-gateway)
- [alurafood-pedidos](https://github.com/BEp0/alurafood-pedidos-service)
- [alurafood-pagamentos](https://github.com/BEp0/alurafood-pagamentos-service)
- [alurafood-eureka-server](https://github.com/BEp0/alurafood-eureka-server)

---

## Chamadas implementadas:

### alurafood-pagamentos

`GET localhost:8083/pagamentos-ms/pagamentos`
- Retorna os pagamentos com status CRIADO e paginado

`GET localhost:8083/pagamentos-ms/pagamentos/1`
- Retorna o pagamento com o id passado e com qualquer status

`POST localhost:8083/pagamentos-ms/pagamentos`
- Criação de um pagamento
- Exemplo de body:

```JSON
{
    "valor": 20.50,
    "nome": "Pagamento X",
    "numero": "4111.1111.1111.1111",
    "expiracao": "20/2023",
    "codigo": "123",
    "pedidoId": 1,
    "formaDePagamento": 1
}
```

`PUT localhost:8083/pagamentos-ms/pagamentos/1`
- Alteração de algum pagamento
- Necessário passar o id no PathParam
- Exemplo de body:

```JSON
{
    "valor": 22.50,
    "nome": "Pagamento X",
    "numero": "4111.1111.1111.1111",
    "expiracao": "20/2023",
    "codigo": "123",
    "status": "CONFIRMADO",
    "pedidoId": 1,
    "formaDePagamento": 1
}
```

`PATCH localhost:8083/pagamentos-ms/pagamentos/1/confirmar`
- Altera o status do pedido no microsserviço `alurafood-pedidos`

`DELETE localhost:8083/pagamentos-ms/pagamentos/1`
- Deleta um pagamento :V    

### alurafood-pedidos

`GET localhost:8083/pedidos-ms/pedidos`

`GET localhost:8083/pedidos-ms/pedidos/1`

`POST localhost:8083/pedidos-ms/pedidos`
- Cria um pedido
- Exemplo de body:

```JSON
{
    "id": 3,
    "dataHora": "2023-02-14T18:06:43.714161",
    "status": "REALIZADO",
    "itens": [
        {
            "id": 1,
            "quantidade": 12,
            "descricao": "Teste"
        }
    ]
}
```

`PUT localhost:8083/pedidos-ms/pedidos/1/status`
- Altera o status do pedido para qualquer outro cadastrado
- Exemplo de body:

```JSON
{
    "status": "REALIZADO"
}
```

`PUT localhost:8083/pedidos-ms/pedidos/1/pago`
- Altera o status do pedido para pago
- É o endpoint chamado no pagamento para alterar o status

---

![isso é tudo, pessoal](https://i.pinimg.com/originals/2a/82/1e/2a821ee45ca3cbc384c0b70f730248ae.gif)
