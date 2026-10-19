# Testes de API REST com Postman e Newman

Projeto de testes automatizados da API [ServeRest](https://serverest.dev), mantido em uma coleção do Postman e executável pela interface do Postman ou pela linha de comando com Newman.

A coleção contém mais de 100 verificações e cobre os recursos de **login, usuários, produtos e carrinhos**, incluindo operações de consulta, cadastro, alteração e exclusão. Os testes verificam respostas da API, como códigos HTTP, conteúdo e estrutura dos dados.

## Arquivos do projeto

- `API_REST_Servrest.postman_collection.json`: coleção com as requisições e os testes automatizados.
- `API_REST_Servrest.postman_environment.json`: ambiente com a URL base e variáveis usadas durante a execução.

## Pré-requisitos

- [Node.js](https://nodejs.org/) instalado (o npm é instalado junto com o Node.js).
- Acesso à internet para alcançar a API `https://serverest.dev`.

Confira se Node.js e npm estão disponíveis:

```bash
node --version
npm --version
```

## Executar pelo Postman

1. Abra o Postman e selecione **Import**.
2. Importe os arquivos `API_REST_Servrest.postman_collection.json` e `API_REST_Servrest.postman_environment.json`.
3. Selecione o ambiente **API REST Servrest** no seletor de ambientes.
4. Abra a coleção **API REST Servrest** e execute as requisições ou a coleção inteira pelo **Collection Runner**.
5. Consulte os resultados dos testes no Runner e na aba **Test Results** de cada requisição.

O ambiente já define `baseUrl` como `https://serverest.dev`. As demais variáveis, como identificadores de usuário, produto, carrinho e chave de autorização, são preenchidas ou utilizadas pelos testes durante a execução. Se alterar a URL, atualize `baseUrl` no ambiente.

## Executar pelo Newman

Newman é a ferramenta de linha de comando para executar coleções do Postman. Instale o Newman e o reporter HTML `htmlextra` globalmente:

```bash
npm install --global newman newman-reporter-htmlextra
```

Confirme a instalação:

```bash
newman --version
```

No terminal, acesse a pasta do projeto e execute a coleção com o ambiente:

```bash
newman run .\API_REST_Servrest.postman_collection.json --environment .\API_REST_Servrest.postman_environment.json
```

Os resultados dos testes serão exibidos no terminal. Para gerar também um relatório HTML detalhado, crie a pasta de saída e execute:

```powershell
New-Item -ItemType Directory -Force .\reports
newman run .\API_REST_Servrest.postman_collection.json --environment .\API_REST_Servrest.postman_environment.json --reporters cli,htmlextra --reporter-htmlextra-export .\reports\relatorio.html
```

Após a execução, abra `reports/relatorio.html` no navegador para consultar o resumo, as requisições e o resultado de cada teste. A pasta `reports` é criada se ainda não existir.

## Publicar o relatório no GitHub Pages

O workflow do GitHub Actions executa a coleção a cada push nas branches `main` e `master`. Também pode ser iniciado manualmente pela aba **Actions**, usando **Newman Postman Tests** e **Run workflow**. O relatório é publicado no GitHub Pages mesmo quando algum teste falha; nesse caso, a execução do workflow continua marcada como falha para que o resultado não passe despercebido. Em pull requests, o workflow executa os testes e salva o HTML como artefato, mas não publica no Pages.

Para habilitar a publicação:

1. No GitHub, abra **Settings > Pages** do repositório e selecione **GitHub Actions** como fonte de publicação.
2. Envie este workflow para a branch `main` ou `master`.
3. Acompanhe a execução em **Actions**. Depois de concluída, o relatório estará disponível em [`https://jorgeadammo.github.io/servrest-test-postman-newman/`](https://jorgeadammo.github.io/servrest-test-postman-newman/).

O relatório publicado omite cabeçalhos e corpos de requisições e respostas para não expor tokens de autenticação ou dados gerados pelos testes. O Pages é público por padrão; não publique nele relatórios que precisem permanecer privados.

### Executar no PowerShell

Os comandos acima usam caminhos relativos à pasta do projeto. No PowerShell, navegue até ela antes de executar Newman. Por exemplo:

```powershell
Set-Location "C:\caminho\para\servrest-test-newman"
newman run .\API_REST_Servrest.postman_collection.json --environment .\API_REST_Servrest.postman_environment.json --reporters cli,htmlextra --reporter-htmlextra-export .\reports\relatorio.html
```

Se o processo retornar código diferente de zero, verifique os testes com falha no terminal e no relatório HTML. O Newman usa esse código para indicar que a execução encontrou falhas; o relatório pode ser consultado para investigar os resultados.
