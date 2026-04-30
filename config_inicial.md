# ⚙️ Git — Configuração Inicial do Ambiente

> Guia completo da Etapa 1 do GitMentor — configurando o Git do zero, do jeito certo.

---

## 1. Verificar a Instalação

Confirme que o Git foi instalado com sucesso:

```bash
git --version
```

Você deve ver algo como `git version 2.4x.x`.

---

## 2. Configurar sua Identidade Global

Toda vez que você fizer um commit, o Git registra **quem** fez aquela alteração. Essa identidade precisa estar configurada antes do primeiro commit.

```bash
git config --global user.name "Seu Nome Aqui"
git config --global user.email "seu@email.com"
```

> ⚠️ Use o mesmo e-mail da sua conta no GitHub/Bitbucket. Isso garante que seus commits apareçam vinculados ao seu perfil corretamente.

---

## 3. Definir um Editor Padrão

Quando o Git precisar que você escreva algo (mensagem de commit, resolver merge, etc.), ele abre um editor. Escolha o que preferir:

**VS Code** *(recomendado para a maioria)*
```bash
git config --global core.editor "code --wait"
```

**Nano** *(simples, bom para iniciantes no terminal)*
```bash
git config --global core.editor "nano"
```

**Vim** *(apenas se já souber usar)*
```bash
git config --global core.editor "vim"
```

---

## 4. Definir a Branch Padrão

O padrão moderno — e o usado pelo GitHub — é `main`. Alinhe sua configuração:

```bash
git config --global init.defaultbranch main
```

> 💡 Isso garante que todo novo repositório criado já inicie com a branch chamada `main`, sem precisar renomear depois.

---

## 5. Confirmar tudo que foi configurado

```bash
git config --list
```

Você deve ver pelo menos estas linhas:

```
user.name=Seu Nome
user.email=seu@email.com
core.editor=code --wait
init.defaultbranch=main
```

---

## 6. Chave SSH — Conexão Segura com o GitHub

**O que é?** Um par de chaves criptográficas — uma fica no seu computador (privada) e outra vai para o GitHub (pública). Permite fazer `push` e `pull` sem digitar senha toda vez, de forma segura.

### Verificar se já existe uma chave

```bash
ls ~/.ssh
```

### Gerar o par de chaves

```bash
ssh-keygen -t ed25519 -C "seu@email.com"
```

> 💡 `ed25519` é o algoritmo moderno recomendado — mais seguro e mais rápido que o RSA antigo.

**Ao executar, o terminal fará 3 perguntas:**

| Pergunta | O que fazer |
|---|---|
| Local para salvar a chave | Pressione **Enter** para aceitar o padrão |
| Senha da chave (passphrase) | **Enter** para deixar em branco (uso pessoal) |
| Confirmar senha | **Enter** novamente |

### Verificar se as chaves foram criadas

```bash
ls ~/.ssh
```

Você deve ver dois arquivos:

```
id_ed25519        ← chave PRIVADA (nunca compartilhe)
id_ed25519.pub    ← chave PÚBLICA (essa vai pro GitHub)
```

> ⚠️ **Nunca compartilhe o arquivo `id_ed25519`** (sem extensão). A chave pública é a `.pub` — ela é feita para ser compartilhada.

### Iniciar o agente SSH

O **ssh-agent** gerencia suas chaves em segundo plano. Ative-o e registre sua chave:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### Copiar a chave pública

```bash
clip < ~/.ssh/id_ed25519.pub
```

> 💡 O `clip` é o comando do Windows para copiar — equivale ao Ctrl+C, mas direto do terminal.

### Adicionar a chave no GitHub

1. Acesse **github.com** e faça login
2. Clique na sua **foto de perfil** (canto superior direito)
3. Vá em **Settings**
4. No menu lateral esquerdo, clique em **SSH and GPG keys**
5. Clique no botão verde **New SSH key**
6. Preencha:
   - **Title:** nome que identifique sua máquina (ex: `Notebook Pessoal`)
   - **Key type:** `Authentication Key` (já vem selecionado)
   - **Key:** cole com **Ctrl+V**
7. Clique em **Add SSH key**

### Testar a conexão

```bash
ssh -T git@github.com
```

Na primeira vez, confirme com `yes` quando perguntado. Se der certo, você verá:

```
Hi SeuNome! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 7. Aliases — Atalhos de Produtividade

**O que são?** Apelidos para comandos longos — economizam digitação no dia a dia.

### Configurar os aliases essenciais

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.lg "log --oneline --graph --decorate --all"
git config --global alias.undo "reset HEAD~1 --mixed"
```

### O que cada um faz

| Alias | Comando completo | Para que serve |
|---|---|---|
| `git st` | `git status` | Ver o estado atual dos arquivos |
| `git co` | `git checkout` | Trocar de branch ou restaurar arquivo |
| `git br` | `git branch` | Listar ou criar branches |
| `git lg` | `git log --oneline...` | Ver histórico visual e compacto |
| `git undo` | `git reset HEAD~1 --mixed` | Desfazer o último commit sem perder as alterações |

> ⚠️ O `git undo` desfaz o commit mas mantém seus arquivos intactos — você não perde nenhum código.

### Confirmar que foram salvos

```bash
git config --list | grep alias
```

---

## 8. `.gitignore` Global — Nunca Commitar Lixo

**O que é?** Um arquivo que diz ao Git para ignorar arquivos específicos em **todos os seus projetos** — sem precisar configurar em cada repositório separadamente.

### Criar o arquivo

```bash
touch ~/.gitignore_global
code ~/.gitignore_global
```

### Conteúdo recomendado

```gitignore
# Windows
Thumbs.db
Desktop.ini
$RECYCLE.BIN/

# VS Code
.vscode/
*.code-workspace

# Logs e temporários
*.log
*.tmp
*.temp

# Dependências (Node, Python)
node_modules/
__pycache__/
*.pyc

# Variáveis de ambiente (segurança!)
.env
.env.local
```

### Registrar o arquivo no Git

```bash
git config --global core.excludesfile ~/.gitignore_global
```

### Confirmar

```bash
git config --global core.excludesfile
# Deve retornar: /c/Users/SeuUsuario/.gitignore_global
```

> ⚠️ **Atenção especial ao `.env`** — arquivos de variáveis de ambiente frequentemente contêm senhas e chaves de API. Nunca devem ir para o repositório.

---

## ✅ Resumo da Etapa 1

| Configuração | Comando principal |
|---|---|
| Verificar instalação | `git --version` |
| Definir nome | `git config --global user.name "Seu Nome"` |
| Definir e-mail | `git config --global user.email "seu@email.com"` |
| Definir editor (VS Code) | `git config --global core.editor "code --wait"` |
| Definir branch padrão | `git config --global init.defaultbranch main` |
| Gerar chave SSH | `ssh-keygen -t ed25519 -C "seu@email.com"` |
| Testar conexão SSH | `ssh -T git@github.com` |
| Configurar aliases | `git config --global alias.st status` |
| Criar .gitignore global | `touch ~/.gitignore_global` |
| Verificar tudo | `git config --list` |

---

> 📚 Documentação oficial: [git-scm.com](https://git-scm.com/doc)
