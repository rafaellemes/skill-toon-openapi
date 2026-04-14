[📖 Read in English](#english) | [📖 Ler em Português](#portugues)

---

<a name="english"></a>
# TooN_OpenApi — Claude Code Skill

**TooN_OpenApi** is a hyper-optimized Agentic Toolkit designed to bridge the gap between large, complex REST APIs and Large Language Models (LLMs) with strict context windows.

It parses massive OpenAPI/Swagger `.json` or `.yaml` specs locally and compiles them into **TooN** (Token-Optimized Notation) — a custom, highly compressed syntactic grammar. By moving the heavy lifting of JSON parsing, circular `$ref` dereferencing, and nested schema flattening away from the AI agent and into local Python processes, **TooN_OpenApi drastically reduces token consumption by up to ~90%**, all while keeping your AI perfectly aware of the entire API structure.

## How It Works

TooN_OpenApi is installed as a **Claude Code skill**. Once installed, your AI agent gains all capabilities below — activated by natural language, no need to know the internals:

| What you say | What toon-openapi does |
|---|---|
| Provide an OpenAPI/Swagger URL or file | Ingests and compiles the API into TooN notation |
| "How do I call POST /users?" / "Generate a Go client" | Consults the contract and generates integration code |
| "Generate a complete TypeScript client" / "Generate only the POST /orders method" | Generates HTTP client for the full API or a single endpoint |
| "Generate Pytest tests for all endpoints" | Generates integration test suite |
| "Validate this JSON against the contract" | Validates the payload against the ingested schema |
| "Compare this API with the v2 I just ingested" | Diffs two versions and flags breaking changes |
| "Export the TooN context to use in another thread" | Generates a compact, self-contained context block |

---

## Installation

Clone this repository into your project's `.claude/skills/` directory:

```bash
cd your-project/
git clone https://github.com/rafaellemes/skill-toon-openapi .claude/skills/toon-openapi
```

Install Python dependencies (from your project root or a shared venv):

```bash
pip install -r .claude/skills/skill-toon-openapi/requirements.txt
```

That's it. Claude Code will automatically load `SKILL.md` on the next session.

---

## Usage Examples

### 1. Ingesting an API (URL or Local File)

Instead of copying a 7,000-token JSON into the chat, just prompt your Agent:

> *"Ingest the API at https://fakerestapi.azurewebsites.net/swagger/v1/swagger.json"*
> *(You can also pass a local file: `./downloads/api.yaml`)*

The `toon-openapi` skill processes everything locally and stores the compressed output under `.toon_apis/apis/fakerestapiweb-v1/`. It replies with the ultra-light TooN overview (~500 tokens):

```text
[API: fakerestapiweb-v1 | BASE: ]
---
GET   /api/v1/Authors -> getApiV1Authors |  [Authors]
POST  /api/v1/Authors -> postApiV1Authors |  [Authors]
  Req: body.id:i? body.idBook:i? body.firstName:s? body.lastName:s?
  Res: 200 (body.id:i? body.idBook:i? body.firstName:s? body.lastName:s?)
---
20 operações | Namespace: fakerestapiweb-v1

[toon-openapi] API mapeada. Você pode pedir:
  → detalhes de endpoint ou código de integração  (ex: "como chamo esse endpoint?" / "gera cliente Go")
  → cliente HTTP ou SDK — API inteira ou endpoint  (ex: "gera classe TypeScript completa" / "gera só o método POST /orders")
  → testes de integração                          (ex: "gera testes pytest para esse endpoint")
  → validação de payload                          (ex: "valida esse JSON contra o contrato")
  → diff com outra versão                         (ex: "compara com a v2 que acabei de ingerir")
  → exportar contexto para outra thread           (ex: "exporta o bloco TooN desta API")
```

### 2. Generating Code and Test Suites

Once ingested, ask your Agent in natural language — `toon-openapi` handles the rest:

> *"Generate a complete Python client for this API."*
> *"Generate only the method for POST /authors."*
> *"Generate a full Pytest suite for all endpoints."*

The skill reads from `.toon_apis/apis/` automatically and outputs hallucination-free code — for the whole API or a single endpoint.

### 3. Comparing API Versions (Diff)

> *"Compare this API with the v2 I just ingested."*

`toon-openapi` compares the two metadata folders and cleanly reports which endpoints were added, removed, or had breaking parameter changes.

---

## Storage

All processed data is stored in `.toon_apis/` at your project root (created automatically on first ingest):

```
.toon_apis/
├── apis/
│   └── <namespace>/
│       ├── toon.txt       ← semantic overview (read by the LLM)
│       ├── mapping.json   ← technical contract (queried via jq)
│       └── metrics.json   ← token usage history
├── diffs/
├── exports/
├── tests/
├── clients/
└── validations/
```

`.toon_apis/` is excluded from git by default via `.toon_apis/.gitignore`.

---

## Running Tests (for skill developers)

TooN_OpenApi has an aggressive TDD base covering edge cases (deep recursion limits, root primitives, priority form-data parsers):

```bash
cd .claude/skills/toon-openapi/
pip install -r requirements.txt
pytest tests/test_unit.py tests/test_diff.py tests/test_validate.py -v
```

---

## License

This project is licensed under the [MIT License](LICENSE).

<br><br>

---

<a name="portugues"></a>
# TooN_OpenApi — Skill para Claude Code

O **TooN_OpenApi** é um Toolkit Agêntico hiper-otimizado projetado para preencher a lacuna entre APIs REST complexas e Modelos de Linguagem (LLMs) com janelas de contexto estritas.

Ele analisa documentos `.json` ou `.yaml` massivos do OpenAPI/Swagger localmente e os compila para **TooN** (Token-Optimized Notation) — uma gramática tática e altamente comprimida. Ao transferir o peso do processamento de JSON, desserialização cíclica de `$ref` e resoluções encadeadas para scripts Python locais e gratuitos, o **TooN_OpenApi reduz o consumo de tokens na IA em até ~90%**, mantendo o agente perfeitamente ciente de toda a estrutura da API.

## Como Funciona

O TooN_OpenApi é instalado como uma **skill do Claude Code**. Uma vez instalado, o seu agente ganha todas as capacidades abaixo — ativadas por linguagem natural, sem precisar conhecer os internos:

| O que você diz | O que o toon-openapi faz |
|---|---|
| Fornecer URL ou arquivo OpenAPI/Swagger | Ingere e compila a API em notação TooN |
| "Como chamo o POST /users?" / "Gera um cliente Go" | Consulta o contrato e gera código de integração |
| "Gera o cliente TypeScript completo" / "Gera só o método POST /orders" | Gera cliente HTTP para toda a API ou um único endpoint |
| "Gera testes pytest para todos os endpoints" | Gera suíte de testes de integração |
| "Valida esse JSON contra o contrato" | Valida o payload contra o schema ingerido |
| "Compara essa API com a v2 que acabei de ingerir" | Faz diff entre duas versões e aponta breaking changes |
| "Exporta o contexto TooN para usar em outra thread" | Gera bloco compacto e auto-explicativo |

---

## Instalação

Clone este repositório dentro do diretório `.claude/skills/` do seu projeto:

```bash
cd seu-projeto/
git clone https://github.com/rafaellemes/skill-toon-openapi .claude/skills/toon-openapi
```

Instale as dependências Python (da raiz do seu projeto ou de um venv compartilhado):

```bash
pip install -r .claude/skills/skill-toon-openapi/requirements.txt
```

Pronto. O Claude Code carregará o `SKILL.md` automaticamente na próxima sessão.

---

## Exemplos de Uso

### 1. Ingerindo uma API (Via URL ou Arquivo Local)

Em vez de colar o JSON gigante no chat, basta dizer para o agente:

> *"Faça o ingest da API disponível em https://fakerestapi.azurewebsites.net/swagger/v1/swagger.json"*
> *(Você também pode mapear um arquivo local: `./downloads/minha_api.yaml`)*

A skill `toon-openapi` processa tudo localmente e armazena o mapeamento em `.toon_apis/apis/fakerestapiweb-v1/`. O agente responderá no chat apenas com o extrato ultraleve (~500 tokens):

```text
[API: fakerestapiweb-v1 | BASE: ]
---
GET   /api/v1/Authors -> getApiV1Authors |  [Authors]
POST  /api/v1/Authors -> postApiV1Authors |  [Authors]
  Req: body.id:i? body.idBook:i? body.firstName:s? body.lastName:s?
  Res: 200 (body.id:i? body.idBook:i? body.firstName:s? body.lastName:s?)
---
20 operações | Namespace: fakerestapiweb-v1

[toon-openapi] API mapeada. Você pode pedir:
  → detalhes de endpoint ou código de integração  (ex: "como chamo esse endpoint?" / "gera cliente Go")
  → cliente HTTP ou SDK — API inteira ou endpoint  (ex: "gera classe TypeScript completa" / "gera só o método POST /orders")
  → testes de integração                          (ex: "gera testes pytest para esse endpoint")
  → validação de payload                          (ex: "valida esse JSON contra o contrato")
  → diff com outra versão                         (ex: "compara com a v2 que acabei de ingerir")
  → exportar contexto para outra thread           (ex: "exporta o bloco TooN desta API")
```

### 2. Gerando Códigos e Testes

Com a API ingerida, basta pedir em linguagem natural — o `toon-openapi` cuida do resto:

> *"Gera o cliente Python completo para essa API."*
> *"Gera apenas o método para o POST /authors."*
> *"Gera a suíte de testes pytest para todos os endpoints."*

A skill lê de `.toon_apis/apis/` automaticamente e entrega código livre de alucinações — para toda a API ou um único endpoint.

### 3. Comparador Histórico (Diff)

> *"Compara essa API com a v2 que acabei de ingerir."*

O `toon-openapi` compara as duas versões e reporta quais rotas foram adicionadas, removidas ou tiveram breaking changes nos parâmetros.

---

## Storage

Todos os dados processados ficam em `.toon_apis/` na raiz do seu projeto (criado automaticamente no primeiro ingest):

```
.toon_apis/
├── apis/
│   └── <namespace>/
│       ├── toon.txt       ← visão semântica (lida pelo LLM)
│       ├── mapping.json   ← contrato técnico (consultado via jq)
│       └── metrics.json   ← histórico de uso de tokens
├── diffs/
├── exports/
├── tests/
├── clients/
└── validations/
```

`.toon_apis/` é excluído do git por padrão via `.toon_apis/.gitignore`.

---

## Rodando os Testes (para desenvolvedores da skill)

```bash
cd .claude/skills/toon-openapi/
pip install -r requirements.txt
pytest tests/test_unit.py tests/test_diff.py tests/test_validate.py -v
```

---

## Licença

Este projeto está sob a licença [MIT](LICENSE).