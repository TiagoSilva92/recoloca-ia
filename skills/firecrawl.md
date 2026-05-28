# firecrawl.md — Skill de Comandos Firecrawl CLI

## Visão Geral
Skill de referência para comandos do Firecrawl CLI. Usado por Scout, Curator e outros agentes que precisam acessar a web.

## Configuração
- Requer `FIRECRAWL_API_KEY` definida no ambiente
- Instalar via: `npm install -g firecrawl`

## Comandos Principais

### 1. firecrawl search
Busca na web e retorna resultados estruturados em JSON.
```
firecrawl search "termo de busca" --json
```
- Retorna: `url`, `titulo`, `descricao` para cada resultado
- Use para descobrir URLs relevantes antes de fazer scrape

### 2. firecrawl scrape
Extrai conteúdo limpo de uma URL específica.
```
firecrawl scrape <url> --format markdown
```
- Retorna markdown limpo da página (renderiza JavaScript)
- Use para extrair descrições completas de vagas ou cursos

### 3. firecrawl map
Descobre todas as URLs de um site.
```
firecrawl map <url_do_site> --json
```
- Retorna lista de URLs do site
- Use para mapear estrutura de sites antes de scrape

### 4. firecrawl crawl
Extrai conteúdo de múltiplas páginas de um site.
```
firecrawl crawl <url_inicial> --max-pages 10 --format markdown
```
- Extrai conteúdo de até N páginas seguindo links
- Use para extrair múltiplas vagas ou cursos de uma formação

## Regras de Uso
1. **Sempre use `search` primeiro** para descobrir URLs, depois `scrape` para extrair detalhes
2. **Prefira `--json`** na busca para facilitar parsing de resultados estruturados
3. **Use `--format markdown`** no scrape para obter texto limpo
4. **Respeite limites de rate**: não faça muitas requisições em sequência muito rápida
5. **Trate erros**: se um comando falhar, reporte o erro exato no campo `erros` do envelope

## Fallback
Se o firecrawl falhar consistentemente:
- Use `curl` ou `wget` como fallback para obter HTML bruto
- Note as limitações: sem renderização JS, possíveis bloqueios anti-bot, HTML bruto em vez de markdown limpo
- Evite `Fetch`, `webfetch` ou outras ferramentas HTTP — prefira firecrawl primeiro

## Exemplos de Uso

### Busca de vagas (Scout):
```bash
firecrawl search "vagas desenvolvedor python remoto" --json
firecrawl scrape <url_da_vaga> --format markdown
```

### Busca de cursos (Curator):
```bash
firecrawl search "alura python curso" --json
firecrawl search "site:alura.com.br python" --json
firecrawl scrape <url_do_curso> --format markdown
```
