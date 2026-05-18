
# Exemplo 13: Embeddings + Neo4j + RAG

> Integração de embeddings, banco de dados Neo4j e técnicas de Retrieval-Augmented Generation (RAG) para enriquecer respostas de IA com dados estruturados.

---

## Funcionalidades
- Processamento automático de documentos PDF e geração de embeddings
- Armazenamento e busca vetorial de textos no Neo4j
- Recuperação de contexto relevante para perguntas
- Geração de respostas com IA generativa (OpenAI/OpenRouter)

---

## Estrutura do Projeto

- `src/` — Código-fonte principal
  - `index.ts` — Orquestra o fluxo principal (processamento, embeddings, busca e resposta)
  - `documentProcessor.ts` — Carrega e divide PDFs em trechos
  - `ai.ts` — Lógica de busca vetorial e geração de resposta
  - `config.ts` — Configurações centralizadas
  - `util.ts` — Funções auxiliares
- `neo4j/` — Dados e configuração do banco Neo4j (persistência local)
- `prompts/` — Templates e regras para geração de respostas
- `respostas/` — Respostas geradas automaticamente (ignorado pelo git)
- `docker-compose.yml` — Facilita o deploy do Neo4j via Docker
- `.gitignore` — Mantém o repositório limpo

---

## Pré-requisitos

- Node.js 22+
- Docker (para rodar o Neo4j)
- Conta e chave de API OpenAI/OpenRouter (para geração de respostas)

---

## Configuração Inicial

1. **Clone o repositório:**
   ```sh
   git clone https://github.com/SEU_USUARIO/NOME_DO_REPOSITORIO.git
   cd NOME_DO_REPOSITORIO
   ```

2. **Instale as dependências:**
   ```sh
   npm install
   ```

3. **Configure as variáveis de ambiente:**
   Crie um arquivo `.env` na raiz com o seguinte conteúdo (ajuste conforme necessário):
   ```env
   NEO4J_URI=bolt://localhost:7687
   NEO4J_USER=neo4j
   NEO4J_PASSWORD=password
   NLP_MODEL=openai/gpt-3.5-turbo
   OPENROUTER_API_KEY=sua-chave-aqui
   OPENROUTER_SITE_URL=https://seusite.com
   OPENROUTER_SITE_NAME=Seu Projeto
   EMBEDDING_MODEL=Xenova/all-MiniLM-L6-v2
   ```

4. **Inicie o Neo4j com Docker:**
   ```sh
   npm run infra:up
   # ou diretamente
   docker-compose up -d
   ```
   O Neo4j estará disponível em http://localhost:7474 (usuário: neo4j, senha: password).

---

## Executando o Projeto

1. **Processar o PDF e popular o banco:**
   - Coloque o arquivo PDF desejado na raiz (ex: `tensores.pdf`).
   - Ajuste o caminho do PDF em `src/config.ts` se necessário.
   - Execute:
     ```sh
     npm start
     ```
   - O sistema irá:
     - Carregar e dividir o PDF em trechos
     - Gerar embeddings para cada trecho
     - Armazenar os vetores no Neo4j
     - Permitir buscas e geração de respostas via IA

2. **Ambiente de desenvolvimento:**
   ```sh
   npm run dev
   ```
   (Recarrega automaticamente ao salvar arquivos)

3. **Parar e remover o Neo4j:**
   ```sh
   npm run infra:down
   # ou
   docker-compose down --volumes
   ```

---

## Exemplos de Uso

- O sistema responde perguntas sobre o conteúdo do PDF usando contexto recuperado do Neo4j e IA generativa.
- As respostas são salvas em arquivos dentro da pasta `respostas/`.
- Exemplo de resposta gerada:

```md
O hot encoding é uma técnica comum em machine learning, especialmente em problemas de classificação. Basicamente, ele é usado para representar variáveis categóricas de forma numérica. ...
```

---

## Dicas e Troubleshooting

- Se o Neo4j não iniciar, verifique se a porta 7687 está livre.
- Se variáveis de ambiente não forem reconhecidas, confira o arquivo `.env`.
- Para limpar todos os dados do Neo4j, apague a pasta `neo4j/data` (com o Neo4j parado).
- Logs do Neo4j ficam em `neo4j/logs`.
- Para customizar prompts e regras, edite os arquivos em `prompts/`.

---

## Licença
Projeto para fins educacionais. Sinta-se livre para adaptar e experimentar!
