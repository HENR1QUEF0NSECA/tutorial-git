# 🌿 Git — Boas Práticas de Fluxo de Trabalho

> Guia completo da Etapa 2 do GitMentor — do zero ao fluxo profissional.

---

## 1. Nomenclatura de Branches

### Por que nomear branches importa?

Sem convenção, o histórico vira uma bagunça:

```
minha-feature
teste2
correção-bug-login-final
arrumei
```

Com convenção, fica assim:

```
feature/cadastro-usuario
fix/bug-login-senha
hotfix/erro-pagamento
```

### Prefixos mais usados

| Prefixo | Quando usar | Exemplo |
|---|---|---|
| `feature/` | Nova funcionalidade | `feature/cadastro-usuario` |
| `fix/` | Correção de bug normal | `fix/erro-login` |
| `hotfix/` | Correção urgente em produção | `hotfix/falha-pagamento` |
| `chore/` | Tarefas técnicas sem impacto no usuário | `chore/atualizar-dependencias` |
| `docs/` | Apenas documentação | `docs/readme-instalacao` |

### Regras de ouro

- Sempre **letras minúsculas**
- Palavras separadas por **hífen** (`-`), nunca espaço ou underscore
- **Curto e descritivo** — máximo 3 a 4 palavras após o prefixo
- **Nunca** trabalhe direto na `main` — ela é sagrada ⚠️

### Na prática

```bash
# Sempre crie a branch a partir da main atualizada
git checkout main
git pull origin main

# Crie e já entre na nova branch
git checkout -b feature/nome-da-funcionalidade
```

> 💡 O `-b` cria a branch e já muda para ela em um único comando.

---

## 2. Commits Semânticos — Conventional Commits

### O problema

```bash
# ❌ Commits sem convenção
arrumei o bug
alterações
wip
corrigindo de vez
```

```bash
# ✅ Commits semânticos
fix: corrige erro de validação no login
feat: adiciona cadastro de usuário
docs: atualiza README com instruções de instalação
```

### A estrutura

```
<tipo>(<escopo opcional>): <descrição curta>
```

**Exemplos reais:**

```
feat(auth): adiciona autenticação via Google
fix(login): corrige redirecionamento após senha errada
chore: atualiza dependências do projeto
docs(api): documenta endpoints de usuário
```

### Tipos mais usados

| Tipo | Quando usar |
|---|---|
| `feat` | Nova funcionalidade |
| `fix` | Correção de bug |
| `docs` | Mudança em documentação |
| `style` | Formatação, sem mudança de lógica |
| `refactor` | Refatoração sem corrigir bug ou adicionar feature |
| `test` | Adição ou correção de testes |
| `chore` | Tarefas técnicas (configs, dependências) |

### Regras de ouro para a descrição

- Sempre em **letras minúsculas**
- No **imperativo** — *"adiciona"* e não *"adicionado"* ou *"adicionando"*
- **Sem ponto final**
- **Máximo 72 caracteres** na primeira linha
- Seja específico — *"corrige erro de validação no campo e-mail"* é melhor que *"corrige bug"*

### Na prática

```bash
# Verifique o que será adicionado antes
git st

# Adiciona os arquivos alterados
git add .

# Faz o commit com mensagem semântica
git commit -m "feat: adiciona tela de cadastro de usuário"
```

> ⚠️ Sempre confira com `git st` antes de `git add .` para não commitar arquivos indesejados.

---

## 3. Merge vs Rebase

Ambos servem para integrar mudanças de uma branch em outra — mas fazem isso de formas diferentes.

### 🔀 Merge — "Une preservando a história"

O merge cria um **commit de união** entre duas branches, preservando exatamente o que aconteceu e quando.

```
main:    A --- B --- C ------- M
                      \       /
feature:               D --- E
```

O ponto `M` é o merge commit — registra que as duas branches foram unidas.

**Quando usar:**
- Ao integrar uma `feature/` na `main`
- Em trabalho colaborativo com outras pessoas
- Quando a história do projeto precisa ser preservada fielmente

```bash
git checkout main
git merge feature/cadastro-usuario
```

### ♻️ Rebase — "Reescreve a história linearmente"

O rebase **move seus commits** para o topo de outra branch, como se você tivesse começado a trabalhar a partir dali.

```
# Antes do rebase
main:    A --- B --- C
                      \
feature:               D --- E

# Depois do rebase
main:    A --- B --- C
                      \
feature:               D' --- E'
```

**Quando usar:**
- Para atualizar sua branch com as últimas mudanças da `main`
- Quando quer manter um histórico limpo e linear
- **Apenas em branches que só você está usando**

```bash
git checkout feature/cadastro-usuario
git rebase main
```

### ⚠️ A regra de ouro do Rebase

> **Nunca faça rebase em branches compartilhadas com outras pessoas.**

O rebase reescreve o histórico. Se outra pessoa tem a mesma branch, os históricos vão divergir e causar conflitos sérios.

### Quando usar cada um

| Situação | Use |
|---|---|
| Integrar feature na main | `merge` |
| Atualizar sua branch com a main | `rebase` |
| Branch compartilhada com o time | `merge` sempre |
| Branch só sua, histórico limpo | `rebase` |

---

## 4. Git Push com Segurança

### O perigo do `--force`

Após um `rebase`, o Git vai recusar seu push normal:

```bash
$ git push origin feature/minha-branch
# ❌ erro: Updates were rejected because the tip of your current
# branch is behind its remote counterpart.
```

O `--force` empurra sua versão **sem verificar o que está no remoto.** Se alguém fez push na mesma branch, você **apaga o trabalho dessa pessoa** sem aviso:

```
Remoto:  A --- B --- C (commit do seu colega)
Você:    A --- B --- D (seu rebase)

git push --force → apaga C para sempre ❌
```

### ✅ A alternativa segura — `--force-with-lease`

Só sobrescreve o remoto se ele estiver exatamente como você viu da última vez:

```bash
git push --force-with-lease origin feature/minha-branch
```

Se alguém tiver feito push enquanto você trabalhava, o comando **falha com aviso** — em vez de apagar silenciosamente.

### Regras de ouro

| Situação | Comando |
|---|---|
| Push normal | `git push origin feature/minha-branch` |
| Após rebase na sua branch | `git push --force-with-lease origin feature/minha-branch` |
| Nunca em hipótese alguma | `git push --force` na `main` |

> ⚠️ **Nunca use `--force` ou `--force-with-lease` na `main`** — essa branch deve ser protegida.

---

## 5. Git Stash — Guardando trabalho inacabado

**Cenário clássico:** você está no meio de uma feature e surge uma urgência — precisa trocar de branch agora, mas não quer commitar código incompleto.

### O que o Stash faz?

Guarda temporariamente todas as suas alterações não commitadas em uma pilha, deixando sua branch limpa — sem perder nada.

```
Antes do stash:               Depois do stash:
- arquivo1.js modificado  →   - working directory limpo
- arquivo2.js novo            - alterações guardadas na pilha
```

### Comandos essenciais

**Guardar as alterações:**
```bash
git stash push -m "wip: formulário de cadastro incompleto"
```

> 💡 O `-m` adiciona uma descrição — sempre use quando tiver vários stashes.

**Ver o que está guardado:**
```bash
git stash list
# stash@{0}: wip: formulário de cadastro incompleto
# stash@{1}: wip: ajustes no menu de navegação
```

**Recuperar o último stash (e remover da pilha):**
```bash
git stash pop
```

**Recuperar um stash específico:**
```bash
git stash pop stash@{1}
```

**Manter na pilha ao recuperar:**
```bash
git stash apply stash@{0}
```

**Descartar um stash que não precisa mais:**
```bash
git stash drop stash@{0}
```

### Na prática — o fluxo completo

```bash
# 1. Guarda o trabalho em progresso
git stash push -m "wip: formulário de cadastro incompleto"

# 2. Troca para a branch do bug urgente
git checkout fix/bug-urgente

# 3. Resolve o bug, commita e volta
git checkout feature/cadastro-usuario

# 4. Recupera seu trabalho de onde parou
git stash pop
```

### Incluindo arquivos novos no stash

Por padrão, o stash não guarda arquivos novos ainda não rastreados pelo Git:

```bash
git stash push -m "descrição" --include-untracked
```

---

## ✅ Resumo da Etapa 2

| Prática | Comando principal |
|---|---|
| Criar branch com convenção | `git checkout -b feature/nome` |
| Commit semântico | `git commit -m "feat: descrição"` |
| Integrar feature na main | `git merge feature/nome` |
| Atualizar branch com a main | `git rebase main` |
| Push após rebase | `git push --force-with-lease origin feature/nome` |
| Guardar trabalho inacabado | `git stash push -m "descrição"` |
| Recuperar trabalho guardado | `git stash pop` |

---

> 📚 Documentação oficial: [git-scm.com](https://git-scm.com/doc)
