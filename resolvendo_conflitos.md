# 🔥 Git — Resolução de Conflitos de Merge com Segurança

> Guia completo da Etapa 3 do GitMentor — resolvendo conflitos com calma e sem perder código.

---

## 1. O que é um Conflito de Merge?

Um conflito acontece quando o Git não consegue decidir automaticamente qual versão de um trecho de código deve prevalecer — geralmente porque **duas branches modificaram a mesma linha do mesmo arquivo.**

```
Branch main:    "cor: azul"
Branch feature: "cor: vermelho"

Git: "Qual dos dois eu mantenho?" ← conflito ⚠️
```

---

## 2. Como o Git Sinaliza um Conflito

Quando você tenta fazer um merge e há conflito, o Git exibe:

```bash
Auto-merging arquivo.js
CONFLICT (content): Merge conflict in arquivo.js
Automatic merge failed; fix conflicts and then commit the result.
```

E dentro do arquivo em conflito, ele insere marcadores visuais:

```
<<<<<<< HEAD
cor: azul
=======
cor: vermelho
>>>>>>> feature/cor-botao
```

### Entendendo os marcadores

| Marcador | Significado |
|---|---|
| `<<<<<<< HEAD` | Início do seu código atual (branch destino) |
| `=======` | Separador entre as duas versões |
| `>>>>>>> feature/...` | Código que veio da outra branch |

---

## 3. Passo a Passo para Resolver Conflitos

```bash
# 1. Verifique quais arquivos estão em conflito
git st

# 2. Abra o arquivo em conflito no VS Code
code arquivo.js
```

No VS Code, você verá os conflitos destacados com opções clicáveis:

| Opção | O que faz |
|---|---|
| **Accept Current Change** | Mantém o código do `HEAD` |
| **Accept Incoming Change** | Mantém o código da outra branch |
| **Accept Both Changes** | Mantém os dois |
| **Compare Changes** | Mostra as diferenças lado a lado |

```bash
# 3. Após resolver todos os conflitos, marque como resolvido
git add arquivo.js

# 4. Finalize o merge com um commit
git commit -m "fix: resolve conflito de merge na cor do botão"
```

> 💡 O `git st` durante um conflito mostra os arquivos como `both modified` — isso te ajuda a rastrear quais ainda precisam de atenção.

---

## 4. Git Reflog — Sua Rede de Segurança

**O que é?** O `reflog` é um registro interno do Git que guarda **tudo que aconteceu no seu repositório** — inclusive operações que "apagaram" commits. É sua última linha de defesa quando algo dá errado.

> 💡 Enquanto o `git log` mostra o histórico do projeto, o `git reflog` mostra o histórico das suas **ações** — cada checkout, merge, reset e rebase que você fez.

### Como visualizar o reflog

```bash
git reflog
```

A saída se parece com isso:

```
a1b2c3d HEAD@{0}: commit: fix: resolve conflito no login
e4f5g6h HEAD@{1}: merge feature/cadastro: Merge made by 'ort'
i7j8k9l HEAD@{2}: checkout: moving from main to feature/cadastro
m1n2o3p HEAD@{3}: commit: feat: adiciona tela de cadastro
```

### Entendendo cada linha

| Parte | Significado |
|---|---|
| `a1b2c3d` | Hash do commit |
| `HEAD@{1}` | Posição na linha do tempo |
| `merge feature/...` | Ação que aconteceu |

### Recuperando código "perdido"

```bash
# 1. Veja o reflog para encontrar o ponto que quer recuperar
git reflog

# 2. Volte para aquele ponto
git checkout HEAD@{2}

# 3. Ou crie uma branch a partir daquele ponto para não perder nada
git checkout -b recovery/branch-recuperada HEAD@{2}
```

### Desfazendo um merge errado

```bash
# Identifique o estado antes do merge no reflog
git reflog

# Volte para aquele estado com segurança
git reset --hard HEAD@{1}
```

> ⚠️ O `--hard` descarta alterações não commitadas. Só use quando tiver certeza do que está fazendo — e sempre verifique o reflog antes.

> ⚠️ O reflog fica disponível por **90 dias** por padrão. Após isso, o Git pode limpar entradas antigas automaticamente.

---

## 5. Estratégias para Minimizar Conflitos Futuros

Resolver conflitos é uma habilidade — mas evitá-los é uma arte.

### Faça pulls frequentes

Quanto mais tempo sua branch fica sem sincronizar com a `main`, maior a chance de conflito.

```bash
# Atualize sua branch com a main regularmente
git checkout feature/minha-feature
git rebase main
```

> 💡 Faça isso pelo menos uma vez por dia em projetos colaborativos.

### Mantenha branches de vida curta

Branches que ficam abertas por semanas acumulam divergências. O ideal é:

| Tipo | Tempo de vida ideal |
|---|---|
| `feature/` | 1 a 3 dias |
| `fix/` | Horas |
| `hotfix/` | Minutos a horas |

> 💡 Se uma feature é grande demais para 3 dias, divida em partes menores — cada uma com sua própria branch e entrega.

### Comunique-se antes de mexer em arquivos críticos

Em trabalho colaborativo, avise o time quando for modificar arquivos centrais — arquivos de configuração, rotas, banco de dados. Isso evita que duas pessoas editem o mesmo trecho ao mesmo tempo.

### Faça commits pequenos e frequentes

Commits grandes e espaçados aumentam a chance de conflito. Prefira:

```bash
# ❌ Um commit gigante no final do dia
git commit -m "feat: adiciona módulo inteiro de pagamento"

# ✅ Commits pequenos ao longo do trabalho
git commit -m "feat: adiciona modelo de pagamento"
git commit -m "feat: adiciona rota de criação de pagamento"
git commit -m "feat: adiciona validação de cartão"
```

### Use `.gitignore` corretamente

Conflitos em arquivos gerados automaticamente (logs, builds, dependências) são os mais frustrantes — e os mais evitáveis. Certifique-se que seu `.gitignore` cobre esses arquivos.

---

## ✅ Resumo da Etapa 3

| Habilidade | Comando principal |
|---|---|
| Ver arquivos em conflito | `git st` |
| Abrir arquivo no VS Code | `code arquivo.js` |
| Marcar conflito como resolvido | `git add arquivo.js` |
| Finalizar o merge | `git commit -m "fix: resolve conflito"` |
| Ver histórico de ações | `git reflog` |
| Recuperar estado anterior | `git checkout HEAD@{N}` |
| Criar branch de recuperação | `git checkout -b recovery/nome HEAD@{N}` |
| Desfazer merge errado | `git reset --hard HEAD@{1}` |
| Atualizar branch com a main | `git rebase main` |

---

> 📚 Documentação oficial: [git-scm.com](https://git-scm.com/doc)
