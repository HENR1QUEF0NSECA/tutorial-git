# 📚 Guia Completo de Git — Do Zero ao Profissional

> Um tutorial prático e progressivo de Git, criado para desenvolvedores que querem evoluir do básico ao fluxo de trabalho profissional com segurança e boas práticas.

---

## 🎯 Sobre o Projeto

Este repositório nasceu de um processo real de aprendizado e tem como objetivo ser um **material de referência acessível para a comunidade** — do desenvolvedor que acabou de instalar o Git até quem quer adotar práticas profissionais no dia a dia.

O conteúdo está organizado em três guias independentes e progressivos, cobrindo desde a configuração do ambiente até a resolução de conflitos de merge com segurança.

---

## 📂 Estrutura do Repositório

```
📁 git-guide/
├── 📄 README.md               ← Você está aqui
├── 📄 config_inicial.md       ← Etapa 1: Configuração do ambiente
├── 📄 boas_praticas.md        ← Etapa 2: Fluxo de trabalho profissional
└── 📄 resolvendo_conflitos.md ← Etapa 3: Resolução de conflitos
```

---

## 🗺️ Conteúdo

### [Etapa 1 — Configuração Inicial do Ambiente](./config_inicial.md)

Tudo que você precisa configurar antes do primeiro commit:

- ✅ Verificação da instalação do Git
- ✅ Configuração de identidade global (nome e e-mail)
- ✅ Definição do editor padrão (VS Code, Nano, Vim)
- ✅ Geração e vinculação de chave SSH ao GitHub
- ✅ Aliases de produtividade no `.gitconfig`
- ✅ Configuração do `.gitignore` global

---

### [Etapa 2 — Boas Práticas de Fluxo de Trabalho](./boas_praticas.md)

Como trabalhar de forma organizada e profissional:

- ✅ Convenção de nomenclatura de branches (`feature/`, `fix/`, `hotfix/`)
- ✅ Commits semânticos com Conventional Commits
- ✅ Quando usar `merge` vs `rebase` — com exemplos visuais
- ✅ Push seguro com `--force-with-lease`
- ✅ `git stash` para proteger trabalho em progresso

---

### [Etapa 3 — Resolução de Conflitos com Segurança](./resolvendo_conflitos.md)

Como enfrentar e resolver conflitos sem perder código:

- ✅ Como identificar e ler um conflito de merge
- ✅ Passo a passo para resolver conflitos no VS Code
- ✅ `git reflog` como rede de segurança
- ✅ Como recuperar código "perdido"
- ✅ Estratégias para minimizar conflitos futuros

---

## 🚀 Como usar este guia

Este material foi pensado para ser seguido em ordem, mas cada etapa também funciona de forma independente como referência rápida.

```
Iniciante no Git?       → Comece pela Etapa 1
Ambiente já configurado? → Pule para a Etapa 2
Travado em um conflito?  → Vá direto para a Etapa 3
```

---

## 🛠️ Pré-requisitos

- Git instalado ([download aqui](https://git-scm.com/downloads))
- Conta no [GitHub](https://github.com)
- VS Code instalado *(recomendado, mas não obrigatório)*

---

## 🤝 Contribuições

Encontrou um erro, tem uma sugestão ou quer adicionar conteúdo? Contribuições são bem-vindas!

1. Faça um fork do repositório
2. Crie uma branch: `git checkout -b fix/sua-correcao`
3. Faça suas alterações e commit: `git commit -m "fix: corrige exemplo de rebase"`
4. Abra um Pull Request

---

## 📚 Referências

- [Documentação oficial do Git](https://git-scm.com/doc)
- [Conventional Commits](https://www.conventionalcommits.org/pt-br)
- [Pro Git Book](https://git-scm.com/book/pt-br/v2) *(gratuito e em português)*

---

## 👤 Autor

Feito por **Henrique Fonseca** — conecte-se comigo no [GitHub](https://github.com/HENR1QUEF0NSECA)

---

> 💡 *Se este material te ajudou, considere deixar uma ⭐ no repositório — isso ajuda outras pessoas a encontrarem o conteúdo!*
