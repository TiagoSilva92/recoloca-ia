Protocolo de despacho e handoff de agentes com:
- Tabela de roteamento (A=Scout, B=Curator, C=Coach, D=Maestro lida com o quiz)
- Formato de envelope de despacho
- Formato de envelope de resposta
- Especificações de handoff por agente
- Despacho sequencial do Coach (6 despachos)
- Regras de tratamento de erros

#### Envelope de Despacho (Maestro constrói este prompt para `spawn_agent`)

```
## DESPACHO: [NOME_DO_AGENTE]
### referencia_persona
[Conteúdo completo de personas/<nome_do_agente_minusculo>.md]

### tarefa
[Uma frase descrevendo o que o agente deve fazer]

### perfil_usuario
[Conteúdo de data/user-profile.md]

### contexto
[Contexto específico: ex: qual vaga para entrevistar, quais habilidades buscar cursos]

### saida_esperada
[Exatamente em que formato o agente deve retornar]
```

#### Envelope de Resposta (agente despachado retorna isto)

```
## RESPOSTA: [NOME_DO_AGENTE]
### estado
[sucesso | erro]

### resumo
[Resumo legível de 2-3 frases para o usuário]

### dados
[Resultados como listas numeradas com pares chave-valor. Sem tabelas markdown.]

### erros
[Apenas se estado for erro: o que deu errado]
```