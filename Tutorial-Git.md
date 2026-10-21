# Tutorial de Git para projeto em equipe

## 1. Conceitos básicos

- **Repositório**: a pasta do projeto com todo o histórico de alterações.
- **Commit**: um "ponto salvo" com uma mensagem explicando o que mudou.
- **Branch**: uma linha de trabalho paralela. Cada pessoa trabalha na sua sem atrapalhar as outras.
- **Remote (GitHub)**: a cópia do repositório na nuvem, que todos compartilham.
- **Pull Request (PR)**: um pedido para juntar seu trabalho ao principal, com revisão dos colegas.

## 2. Configuração inicial (uma vez por pessoa)

Instale o Git e crie uma conta no GitHub. Depois configure:

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu-email@exemplo.com"
git config --global init.defaultBranch main
```

Use o mesmo e-mail da conta do GitHub, para que seus commits apareçam com seu nome.

```bash
git clone https://github.com/UzzyF/FBD-2026.2.git
cd FBD-2026.2
```

## 4. O fluxo de trabalho diário

Este é o ciclo que vocês vão repetir sempre (chamado de *GitHub Flow*):

### Passo 1: atualize a main local

```bash
git checkout main
git pull
```

### Passo 2: crie uma branch para sua tarefa

```bash
git checkout -b feature/criar-tabelas-clientes
```

Use nomes descritivos: `feature/views`, `fix/chave-estrangeira-pedido`, `docs/diagrama-er`.

### Passo 3: trabalhe e faça commits pequenos

```bash
git status                  # vê o que mudou
git add sql/01_criacao_tabelas.sql
git commit -m "Cria tabelas Cliente e Endereco"
```

Evite `git add .` sem olhar antes o `git status`, para não incluir arquivos que não deveria.

### Passo 4: envie a branch para o GitHub

```bash
git push -u origin feature/criar-tabelas-clientes
```

Nas próximas vezes basta `git push`.

### Passo 5: abra um Pull Request

No GitHub vai aparecer um botão **Compare & pull request**. Descreva o que foi feito e peça revisão a pelo menos um colega.

### Passo 6: revisão e merge

Um colega revisa, comenta se necessário e clica em **Merge pull request**. Depois todos voltam ao passo 1.

## 6. Boas mensagens de commit

Seja específico e use o verbo no imperativo:

- Bom: `Adiciona chave estrangeira entre Pedido e Cliente`
- Ruim: `alterações`, `fix`, `teste 2`

## 7. Resolvendo conflitos

Conflito acontece quando duas pessoas mudam as mesmas linhas do mesmo arquivo. O Git avisa e marca o arquivo assim:

```
<<<<<<< HEAD
CREATE TABLE cliente (id INT, nome VARCHAR(100));
=======
CREATE TABLE cliente (id INT PRIMARY KEY, nome VARCHAR(150));
>>>>>>> feature/outra-branch
```

Para resolver:

1. Abra o arquivo (o VS Code mostra botões "Accept Current / Incoming / Both").
2. Escolha ou combine as versões e apague as linhas `<<<<`, `====`, `>>>>`.
3. Depois:

```bash
git add arquivo.sql
git commit -m "Resolve conflito na tabela cliente"
```

Para reduzir conflitos: faça `git pull` na main com frequência, mantenha branches curtas (1 a 2 dias) e combine quem mexe em qual arquivo.

## 8. Atualizando sua branch com novidades da main

Se a main andou enquanto você trabalhava:

```bash
git checkout main
git pull
git checkout sua-branch
git merge main
```

## 9. Arquivo .gitignore

Nunca suba senhas, dados pessoais ou arquivos gerados. Exemplo:

```
.env
*.log
*.bak
.DS_Store
.vscode/
*.db
```

Se o projeto usar conexão com banco (Python, Java etc.), as credenciais devem ficar em um `.env` ignorado, nunca no código.

## 10. Comandos úteis para o dia a dia

| Comando | Para que serve |
|---|---|
| `git status` | Ver o que mudou |
| `git log --oneline --graph` | Ver o histórico resumido |
| `git diff` | Ver as alterações linha a linha |
| `git branch` | Listar branches locais |
| `git restore arquivo` | Descartar mudanças não commitadas num arquivo |
| `git stash` / `git stash pop` | Guardar mudanças temporariamente |
| `git commit --amend` | Corrigir a última mensagem (só antes do push) |

## 11. Regras de ouro para a equipe

1. **Nunca commitem direto na `main`**: sempre branch + PR.
2. **Sempre `git pull` antes de começar** a trabalhar.
3. **Commits pequenos e frequentes**, com mensagens claras.
4. **Dividam tarefas** usando a aba **Issues** do GitHub (ex: "Criar tabelas de Produto", atribuída a uma pessoa). O quadro **Projects** funciona como um Kanban.
5. **Avisem no grupo** quando mexerem em algo grande, como mudar a estrutura de uma tabela usada por todos.
6. **Não usem `git push --force`** na main ou em branches compartilhadas.

## 12. Exemplo de divisão para 4 pessoas

- **Pessoa A**: modelagem (diagramas ER e dicionário de dados), na pasta `docs/`
- **Pessoa B**: criação das tabelas e restrições (`01_criacao_tabelas.sql`)
- **Pessoa C**: inserção de dados de teste (`02_insercao_dados.sql`)
- **Pessoa D**: consultas, views e relatório (`03_consultas.sql`, `04_views.sql`)

Cada um trabalha na sua branch e abre PRs; os revisores podem ser rotativos.

## 13. Dica extra: use uma interface gráfica se preferir

Se a linha de comando assustar no começo, o **VS Code** tem Git integrado (aba "Source Control"), e o **GitHub Desktop** é bem amigável. Os conceitos são os mesmos.
