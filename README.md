# diquinhas
Diquinhas da dev do passado para a dev do futuro não esquecer

### gitgnore

- adicionar .vscode/ para manter configurações da IDE fora do commit.
- .env

### Debug

- usar envs no lauch json para realizar o debug como em:

"configurations": [{
"env": {
"VAR": "valor"
}
}]

### Configs de ambiente:

- usar um .env.example e transformar ele em .env assim que a aplicação for executada.
- usar uma script de inicialização com configuração .sh com inicialização para ambiente de produção

### O que é o Redis?

Redis é um servidor de banco de dados em memória, open-source, classificado como:
- Key–Value Store
- NoSQL
- In-memory data structure store
- Message broker (pub/sub)
- Cache distribuído

Ele roda como um processo separado, normalmente na porta 6379, e as aplicações se conectam via TCP.

Quando usar NoSQL?
- Cache (Redis)
- Alta escala distribuída
- Dados semi-estruturados
- Big Data
- Alta taxa de escrita/leitura
- Microserviços


