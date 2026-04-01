# diquinhas
Diquinhas da dev do passado para a dev do futuro não esquecer

### Links e referencias

- API mockada: https://mockoon.com/download/

### gitgnore

- adicionar .vscode/ para manter configurações da IDE fora do commit.
- .env

### Configs de ambiente e Debug:

- usar envs no lauch json para realizar o debug como em:

"configurations": [{
"env": {
"VAR": "valor"
}
}]

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

### Arquitetura

- Rotas de edição sempre com campos opcionais
- Rotas de delete hard sempre que possível.
- Acesso a rotas com permissão.
- Modelos dto para entrada de dados e saída
- Arquivo para mensagens fixas isolado em constantes
- multitenancy em aplicações multi client
- usar alembic para realizar migrações
- usar logging para inserir logs
- usar um gerador de erro geral com códigos HTTP padrão de mercado
- usar documentação de API com swegger
- montar urls com envs dentro do código no arquivo de configuração
- no arquivo de inicialização, validar se todas as variáveis de ambiente foram definidas e encerrar aplicação se não tiver sido
- 🔹 constants: São valores imutáveis em tempo de execução e normalmente fazem parte da regra do sistema. Ex: MAX_LOGIN_ATTEMPTS = 5, DEFAULT_PAGE_SIZE = 20, JWT_ALGORITHM = 'HS256'
- 🔹 env: Valores configuráveis, dependentes do ambiente. São dependentes do ambiente (dev, staging, prod), mutáveis sem recompilar, normalmente sensíveis (segredos). Ex: DATABASE_URL, REDIS_HOST, JWT_SECRET, PORT
- 🔹 config: São uma camada intermediária que organiza e valida valores vindos de variáveis de ambiente, de arquivos (.env, config.json), de defaults
- Exemplo de arquitetura limpa:
src/
 ├─ config/
 │   ├─ env.ts        // leitura e validação
 │   └─ index.ts      // objeto config exportado
 ├─ constants/
 │   └─ app.constants.ts
- No banco, sempre armazenar para o usuário, a data de criação, de atualização e último login
- Fazer um Regras.md com regras de negócio e requisitos funcionais na pasta docs
- usar um mock.sql para alimentar o banco com dados iniciais da aplicação como user, permissão
- usar uma variável global de ambiente

### Comandos

- Limpar cache do python: find. -name "__pycache__" -exec rm -rf {} +
- Embelezar formatação: Ctrl + Shift + P -> Format Document With... -> Configure Default Formatter

### Documentação

Relação entre RF e UC

Um RF é uma regra ou capacidade obrigatória.

Um UC é uma descrição estruturada do comportamento do sistema para atender um ou mais RFs.

Exemplo prático

- **RF01**: O sistema deve permitir login com e-mail e senha.


  #### UC01 – Realizar Login

**Ator**: Usuário

**Pré-condição**: Usuário cadastrado

**Fluxo principal**:
1. Usuário informa e-mail
2. Usuário informa senha
3. Sistema valida credenciais
4. Sistema concede acesso

**Fluxo alternativo**:

3a. Credenciais inválidas → Sistema exibe mensagem de erro
