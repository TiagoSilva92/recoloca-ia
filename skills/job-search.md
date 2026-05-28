# Skills de Busca de Vagas

## 1. `job-search.md`

Este arquivo contém a lógica para o agente Scout buscar vagas de emprego.

### Função Principal: `buscar_vagas(perfil_usuario)`

**Parâmetros**:
- `perfil_usuario`: Dicionário contendo os dados do perfil do usuário (ex: `area_interesse`, `nivel_experiencia`, `localizacao`, `habilidades_atuais`).

**Lógica**:
1.  **Construir URL de Busca**: Formatar a string de busca para o `firecrawl search` com base em `area_interesse`, `localizacao` e `nivel_experiencia` do `perfil_usuario`.
    Exemplo: `firecrawl search "Engenheiro Backend Pleno em São Paulo" --json`
2.  **Executar Busca com Firecrawl**:
    ```bash
    terminal --command "firecrawl search 'Engenheiro Backend Pleno em São Paulo' --json" --cd "recoloca-ia"
    ```
    *   Se o comando falhar, registrar erro e retornar lista vazia.
3.  **Processar Resultados da Busca**:
    -   Analisar a resposta JSON do `firecrawl search`.
    -   Extrair URLs das vagas mais relevantes (até 5).
4.  **Extrair Detalhes das Vagas**:
    -   Para cada URL extraída, executar `firecrawl scrape <url> --format markdown`.
    -   Se `firecrawl scrape` falhar para uma URL específica, usar o título e descrição do resultado da busca inicial e anotar a falha.
    -   Extrair: `titulo`, `empresa` (se possível inferir da URL ou descrição), `localizacao`, `link`, `descricao_completa`.
5.  **Corresponder Habilidades**:
    -   Comparar as `habilidades_atuais` do usuário com as habilidades mencionadas na `descricao_completa` de cada vaga.
    -   Utilizar correspondência de strings sem distinção de maiúsculas/minúsculas.
    -   Contar as habilidades correspondentes e identificar as habilidades faltantes.
6.  **Filtrar por Nível de Experiência**:
    -   Priorizar vagas que correspondam ao `nivel_experiencia` do usuário.
    -   Se houver poucas vagas do nível exato, expandir a busca para níveis adjacentes (ex: Júnior/Pleno se o usuário for Pleno) e anotar a discrepância.
7.  **Formatar Saída**:
    -   Criar uma lista numerada de vagas, cada uma com:
        -   `titulo`
        -   `empresa`
        -   `localizacao`
        -   `link`
        -   `habilidades_correspondentes`
        -   `habilidades_faltantes`
        -   `contagem_correspondencia` (ex: "3 de 5 habilidades correspondem")
    -   Retornar no máximo 5 vagas.
8.  **Tratamento de Erros**:
    -   Se `firecrawl search` falhar, reportar erro e retornar lista vazia.
    -   Se nenhuma vaga for encontrada, informar ao Maestro.

### 2. `firecrawl.md` (Referência)

Este arquivo (assumido como existente) contém as regras e comandos do CLI `firecrawl`.

### 3. `dispatch.md` (Atualização necessária)

Adicionar a entrada para o agente Scout na tabela de roteamento.

### 4. `maestro.md` (Atualização necessária)

Adicionar a opção "A — Buscar vagas" ao menu e implementar a lógica de despacho para o Scout.

### 5. `data/job-search-results.md` (Novo arquivo)

Esquema para armazenar os resultados da busca:
```
Data da Busca: [AAAA-MM-DD HH:MM]
Parâmetros de Busca:
  Área: [valor]
  Localização: [valor]
  Nível: [valor]

Resultados:
1. titulo: [título da vaga]
   empresa: [nome da empresa]
   localizacao: [cidade ou Remoto]
   link: [URL]
   habilidades_correspondentes: [habilidade1, habilidade2]
   habilidades_faltantes: [habilidade3, habilidade4]
   contagem_correspondencia: [X de Y habilidades correspondem]

2. titulo: [próximo título de vaga]
   ...
```
