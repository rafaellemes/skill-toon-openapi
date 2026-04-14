---
name: consult-api
description: >
  Responde perguntas sobre APIs ingeridas e gera código de integração
  em qualquer linguagem. Ativar ao perguntar sobre endpoints (linguagem
  natural ou método+path), pedir código, declarar guidelines de geração,
  mencionar API carregada com ingest-api, ou pedir código que envolva
  múltiplas APIs simultaneamente.
---

# Consulta e Geração de Código

## Fluxo (namespace único)

0. Resolver contexto (ver SKILL.md — Estratégia de Contexto).
1. `ls .toon_apis/apis/` — verificar namespace.
2. `cat .toon_apis/apis/<ns>/toon.txt` — match semântico do operationId.
3. `jq '.<operationId>' .toon_apis/apis/<ns>/mapping.json` — dados técnicos.
4. Gerar código idiomático com contexto resolvido.
5. Validar payload gerado (ver SKILL.md — Validação Automática).
6. `python .claude/skills/toon-openapi/scripts/consult/log_metrics.py <ns> <tokens> consult`

## Fluxo Multi-namespace

Ativar quando o pedido menciona duas ou mais APIs distintas.
Ex: "cadastra o user na auth-api e cria o perfil na profiles-api"

1. Detectar os namespaces envolvidos (por nome ou por contexto).
2. Gerar visão combinada:
   `python .claude/skills/toon-openapi/scripts/consult/resolve_multi.py <ns1> <ns2> [<ns3>...]`
3. Usar a visão combinada para identificar qual operationId de qual namespace.
4. Para cada operação: `jq '.<opId>' .toon_apis/apis/<ns>/mapping.json`
5. Gerar código orquestrado que encadeia as chamadas na sequência correta.
6. Validar cada payload individualmente.
7. Registrar métricas por namespace.

## Match de operationId
- Linguagem natural: "cadastrar usuário" → summary → operationId
- Método + path: "POST /users" → linha TooN
- Nome técnico: match direto

## Clientes HTTP padrão (quando não declarado)
python=requests, javascript=fetch, typescript=axios, kotlin=ktor,
go=net/http, rust=reqwest, ruby=faraday, php=guzzle, swift=urlsession,
csharp=httpclient, dart=dio, elixir=httpoison

## Anti-alucinação
PROIBIDO inventar campos. Nomes dos params vêm SEMPRE do mapping.json.
Em multi-namespace: NUNCA misturar params de namespaces diferentes.


## Linguagem Cognitiva (Cognitive Language)
Você deve pensar e responder ao usuário exatamente no mesmo idioma em que ele fez o prompt. Exemplo: Se o prompt for em Inglês, pense e responda em Inglês. Se for em Português do Brasil, pense e responda em Português do Brasil (PT-BR). Se for em Espanhol, etc.
