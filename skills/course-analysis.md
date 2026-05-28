# course-analysis.md — Skill de Análise de Cursos (Curator)

## Visão Geral
Skill obrigatória para o agente Curator. Define o fluxo completo de busca de cursos na Alura para preencher lacunas de habilidades identificadas pelo Scout.

## Pré-requisitos
- Arquivo `data/job-search-results.md` deve existir e conter habilidades faltantes. Use `find_path` ou `read_file` para validar antes de iniciar.

## Ferramentas
- `terminal`: executar comandos `firecrawl search` e `firecrawl scrape`
- `read_file`: ler `data/job-search-results.md` e `data/user-profile.md`

## Fluxo Passo a Passo

### 1. Validação de Pré-requisito
- Verificar se `data/job-search-results.md` existe usando `find_path` ou tentativa de leitura.
- Se o arquivo não existir ou estiver vazio: retornar erro no envelope de resposta, informando que o usuário deve buscar vagas primeiro (opção A).

### 2. Extração de Habilidades Faltantes
- Ler `data/job-search-results.md`
- Extrair todas as entradas únicas de `habilidades_faltantes` de cada vaga listada
- Remover duplicatas para evitar buscas repetidas

### 3. Busca de Cursos na Alura
Para cada habilidade faltante:
- Comando primário: `firecrawl search "alura [habilidade]" --json`
- Fallback (se primário falhar): `firecrawl search "site:alura.com.br [habilidade]" --json`
- Se ambos falharem: pular essa habilidade, registrar no campo `erros` e continuar com as próximas

### 4. Extração de Dados dos Cursos
Para cada resultado da busca (priorizar domínio `alura.com.br/formacoes`):
- Extrair: `nome_curso`, `duracao`, `nivel`, `link`
- `duracao`: extrair do texto do resultado (ex: "20h", "40 horas")
- `nivel`: classificar com base no título do curso:
  - `iniciante`: título contém "Introdução", "Primeiros Passos", "Fundamentos", "Básico" ou "Para Iniciantes"
  - `intermediario`: título contém "Intermediário" ou implica conhecimento prévio
  - `avancado`: título contém "Avançado", "Profundo", "Expert", "Arquitetura" ou "Especialista"
- `aborda_habilidade`: habilidade faltante que o curso atende

### 5. Classificação e Ordenação
- Filtrar até 5 cursos no total (ou até 5 por habilidade, máximo 15)
- Ordenar por nível: `iniciante` primeiro, depois `intermediario`, depois `avancado`
- Para cada curso, associar à habilidade faltante correspondente

### 6. Formato de Resposta
Retornar via Envelope de Resposta:
```
## RESPOSTA: CURATOR
### estado
[sucesso | erro]

### resumo
[Resumo legível de 2-3 frases para o usuário]

### dados
1. nome_curso: [título do curso]
   duracao: [ex: 20 horas]
   nivel: [iniciante | intermediario | avancado]
   aborda_habilidade: [nome da habilidade]
   link: [URL]

2. [próximo curso no mesmo formato]

Ordem sugerida:
1. [nome do curso]
2. [nome do curso]
3. [nome do curso]

### erros
[Apenas se estado for erro: o que deu errado, ou habilidades que falharam na busca]
```

## Tratamento de Erros
- Se a busca falhar para uma habilidade específica: tentar fallback, se falhar também, pular a habilidade e continuar com as restantes
- Não interromper o processamento inteiro por uma única falha de busca
- Registrar habilidades que falharam no campo `erros` do envelope de resposta
