# 🧠 Codex CLI — Guia Rápido de Comandos

> **Objetivo:** facilitar o uso do Codex CLI no dia a dia de desenvolvimento, com exemplos e flags úteis.  
> **Fonte oficial:** documentação do Codex CLI (2025).

---

## 🔐 Autenticação e Sessões

| Comando | Descrição | Exemplo |
|----------|------------|---------|
| `codex login` | Faz login via OAuth, device auth ou API key. | `codex login` |
| `codex logout` | Encerra sessão e remove credenciais. | `codex logout` |
| `codex resume` | Retoma a última sessão interativa. | `codex resume` |
| `codex resume <id>` | Retoma uma sessão específica. | `codex resume 9f83b2` |

---

## ⚙️ Execução e Interação com o Codex

| Comando | Descrição | Exemplo |
|----------|------------|---------|
| `codex` | Inicia a interface interativa (modo TUI). | `codex` |
| `codex exec` ou `codex e` | Executa prompt direto via CLI (modo não-interativo). | `codex exec "otimize este código"` |
| `echo "..." \| codex exec -` | Lê prompt do `stdin`. | `echo "melhore o README" \| codex exec -` |
| `codex apply` ou `codex a` | Aplica *diffs* gerados por uma tarefa Codex Cloud. | `codex apply` |

### 🔧 Flags úteis para `codex exec`

| Flag | Descrição | Exemplo |
|------|------------|---------|
| `--model, -m` | Define o modelo a ser usado. | `--model gpt-5-codex` |
| `--image, -i` | Anexa uma ou mais imagens à primeira mensagem. | `-i ./print.png` |
| `--sandbox, -s` | Define o nível de acesso do sandbox. | `-s workspace-write` |
| `--full-auto` | Executa sem prompts, com sandbox seguro. | `--full-auto` |
| `--yolo` ou `--dangerously-bypass-approvals-and-sandbox` | Remove aprovações e sandbox (⚠️ perigoso). | `--yolo` |
| `--cd, -C` | Muda o diretório de trabalho antes de executar. | `-C ./frontend` |
| `--profile, -p` | Usa um perfil de configuração diferente. | `-p personal` |
| `--config key=value` | Sobrescreve configs inline. | `--config sandbox.policy=workspace-write` |
| `--json` | Retorna a saída em formato JSONL (útil em CI). | `--json` |
| `--output-last-message, -o` | Salva a última resposta do assistente em arquivo. | `-o output.txt` |

---

## ☁️ Integração com o Codex Cloud

| Comando | Descrição | Exemplo |
|----------|------------|---------|
| `codex cloud` | Abre interface para gerenciar tarefas no Codex Cloud. | `codex cloud` |
| `codex cloud exec "<prompt>"` | Executa tarefa diretamente no Cloud. | `codex cloud exec "melhore performance do backend"` |

### ⚙️ Subcomandos úteis

| Subcomando | Descrição |
|-------------|------------|
| `list` | Lista tarefas recentes. |
| `get <id>` | Mostra detalhes de uma tarefa. |
| `cancel <id>` | Cancela uma tarefa em andamento. |

---

## 🧱 Sandbox e Segurança

| Comando | Descrição | Exemplo |
|----------|------------|---------|
| `codex sandbox` | Executa comandos dentro de um ambiente isolado (Landlock/macOS seatbelt). | `codex sandbox -- /bin/sh -c "ls -la"` |

### 🔒 Modos de Sandbox

| Modo | Descrição |
|------|------------|
| `read-only` | Só leitura (sem alterações locais). |
| `workspace-write` | Permite escrita apenas no diretório de trabalho. |
| `danger-full-access` | Acesso completo ao sistema (⚠️ use com cuidado). |

---

## 🧩 MCP (Model Context Protocol)

| Comando | Descrição | Exemplo |
|----------|------------|---------|
| `codex mcp list` | Lista servidores MCP configurados. | `codex mcp list` |
| `codex mcp add` | Adiciona um servidor MCP. | `codex mcp add myserver --url https://api.example.com` |
| `codex mcp remove <name>` | Remove um servidor MCP. | `codex mcp remove myserver` |
| `codex mcp login <name>` | Faz login em um servidor MCP. | `codex mcp login myserver` |
| `codex mcp logout <name>` | Faz logout de um servidor MCP. | `codex mcp logout myserver` |

---

## 🧰 Utilitários e Desenvolvimento

| Comando | Descrição | Exemplo |
|----------|------------|---------|
| `codex completion <shell>` | Gera script de autocompletar para o shell. | `codex completion zsh` |
| `codex mcp-server` | Roda o Codex como servidor MCP via stdio (modo experimental). | `codex mcp-server` |
| `codex app-server` | Inicia servidor local do app (para debug/desenvolvimento). | `codex app-server` |

---

## ⚡ Dicas Práticas

- 💡 Use `--full-auto` em automações seguras (ex: scripts de CI/CD).
- ⚠️ Evite `--yolo` fora de ambientes isolados (sem sandbox = risco total).
- 📄 Combine `--json` + `--output-last-message` para registrar logs e saídas.
- 🧠 Configure perfis diferentes em `~/.config/codex/config.toml`.
- 📦 Use `codex apply` para aplicar sugestões diretamente ao repositório local.

---

## 💻 Exemplos de Uso Comum

```bash
# 1. Abrir interface interativa
codex

# 2. Gerar código automaticamente
codex exec "crie um serviço para validar CPF" --model gpt-5-codex --full-auto

# 3. Corrigir testes quebrados lendo do stdin
echo "corrija o teste user-service" | codex exec - --json -o fix.txt

# 4. Aplicar diff de tarefa cloud
codex apply

# 5. Rodar em ambiente isolado
codex sandbox -s read-only -- /bin/sh -c "ls -la"
