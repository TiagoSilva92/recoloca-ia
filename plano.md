
# Plano para Orquestrador de Vagas de Emprego Tech

## 1. Introdução

O objetivo deste plano é detalhar a criação de um orquestrador para automatizar a busca por vagas de emprego na área de tecnologia. O sistema será composto por um agente central, o "Maestro", que gerenciará a comunicação com o usuário e delegará tarefas a agentes especializados.

## 2. Agente Maestro

O Maestro será o ponto de contato principal com o usuário. Suas responsabilidades incluem:

-   **Comunicação:** Interagir diretamente com o usuário, recebendo suas demandas e fornecendo informações.
-   **Delegação Inteligente:** Utilizar sua habilidade nativa de despacho de agentes para encaminhar tarefas específicas para os agentes apropriados.
-   **Gerenciamento de Fluxo:** Orquestrar a sequência de interações e a coleta de informações necessárias do usuário.

### Habilidades do Maestro:

-   Capacidade de enviar e receber mensagens do usuário.
-   Acesso e uso da ferramenta de despacho de agentes para delegar tarefas.
-   Lógica para gerenciar o fluxo de conversação e a coleta de dados do usuário.

## 3. Agente de Busca de Vagas (a ser criado futuramente)

Este agente será responsável por buscar ativamente por vagas de emprego na área de tecnologia em diversas fontes. Sua implementação detalhada ocorrerá em uma fase posterior do projeto.

## 4. Playbook do Maestro

O Maestro seguirá uma sequência de passos bem definida para interagir com o usuário:

1.  **Saudação ao Usuário:** Iniciar a conversa com uma mensagem de boas-vindas.
2.  **Verificação de Quiz:** Conferir se o usuário já respondeu a um quiz sobre suas habilidades e preferências.
3.  **Coleta de Preferências (se quiz não existir):** Caso não haja um quiz prévio, o Maestro fará as seguintes perguntas para coletar informações essenciais sobre o usuário:
    *   Quais são suas principais habilidades técnicas? (Ex: Python, JavaScript, SQL, Cloud, etc.)
    *   Em quais tecnologias ou linguagens de programação você tem mais interesse em trabalhar?
    *   Qual o nível de experiência que você procura? (Ex: Júnior, Pleno, Sênior, Especialista)
    *   Você tem preferência por algum tipo de empresa? (Ex: Startup, Grande Corporação, Consultoria)
    *   Qual a sua localização preferencial ou se busca trabalho remoto?
    *   Quais são suas expectativas salariais (faixa)?
    *   Há alguma área específica dentro de tecnologia que te interessa mais? (Ex: Backend, Frontend, Mobile, Dados, DevOps, Segurança, IA, etc.)
4.  **Menu de Opções:** Apresentar ao usuário um menu com as seguintes opções:
    *   a) Responder o quiz / Fornecer minhas preferências.
    *   b) Buscar vagas de emprego (esta funcionalidade será implementada futuramente).

## 5. Fluxo de Delegação

Quando o usuário solicitar a busca por vagas de emprego, o Maestro utilizará sua ferramenta de despacho para delegar essa tarefa ao Agente de Busca de Vagas.
As informações coletadas do usuário (habilidades, preferências, etc.) serão repassadas como contexto para o agente executor.
