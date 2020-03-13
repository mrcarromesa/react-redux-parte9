<h1>API JSON</h1>

- Utilizar api JSON fake para testes

- [JSON Server](https://github.com/typicode/json-server)

- Instalar de forma global com yarn:

```bash
yarn global add json-server
```
- Criar um arquivo json na aplicação por exemplo `server.json` contendo uma estrutura json

- Como deixar a api rodando:
  - Acessar a pasta do arquivo
  - Executar o seguinte comando:

```bash
json-server NOME_DO_ARQUIVO.json -p NUMERO_DA_PORTA -w
```

- Basicamente esse comando consite de inserir o nome do arquivo `json` o número da porta
e se deseja que fique assistindo se houver alterações no arquivo.

- Exemplo na prática:

```bash
json-server server.json -p 3333 -w
```

- Depois só acessar no browser:

- Ex. nesse caso utilizando o arquivo que temos `server.js`:

  - http://localhost:3333/products ** Irá listar todos os produtos **
  - http://localhost:3333/products/4 ** Irá listar apenas um produto, nesse caso o produto id:4 **
  - http://localhost:3333/stock ** Irá listar nosso stock **

