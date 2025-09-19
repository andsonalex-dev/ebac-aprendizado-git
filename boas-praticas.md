# Boas Práticas com Git e GitHub

## Índice
- [Configuração Inicial](#configuração-inicial)
- [Commits](#commits)
- [Branches](#branches)
- [Pull Requests](#pull-requests)
- [Repositórios](#repositórios)
- [Segurança](#segurança)
- [Colaboração](#colaboração)
- [Ferramentas e Automação](#ferramentas-e-automação)

---

## Configuração Inicial

### Configuração Básica do Git
```bash
# Configurar nome e email globalmente
git config --global user.name "Seu Nome"
git config --global user.email "seu.email@exemplo.com"

# Configurar editor padrão
git config --global core.editor "code --wait"

# Configurar tratamento de line endings
git config --global core.autocrlf true  # Windows
git config --global core.autocrlf input # macOS/Linux
```

### Configuração de SSH
```bash
# Gerar chave SSH
ssh-keygen -t ed25519 -C "seu.email@exemplo.com"

# Adicionar chave ao ssh-agent
ssh-add ~/.ssh/id_ed25519

# Testar conexão
ssh -T git@github.com
```

---

## Commits

### Mensagens de Commit

#### Boas Práticas
- **Use o imperativo**: "Adiciona" em vez de "Adicionado"
- **Seja conciso**: Primeira linha com até 50 caracteres
- **Seja descritivo**: Explique o "o quê" e o "por quê"
- **Use prefixos convencionais**:
  - `feat:` nova funcionalidade
  - `fix:` correção de bug
  - `docs:` documentação
  - `style:` formatação, espaços em branco
  - `refactor:` refatoração de código
  - `test:` adição ou correção de testes
  - `chore:` tarefas de build, configuração

#### Exemplo de Mensagem Bem Estruturada
```
feat: adiciona validação de email no formulário de cadastro

- Implementa regex para validar formato de email
- Adiciona mensagens de erro personalizadas
- Inclui testes unitários para validação

Closes #123
```

### Estrutura do Commit
```bash
# Fazer commits atômicos (uma funcionalidade por vez)
git add arquivo-especifico.py
git commit -m "feat: adiciona função de cálculo de impostos"

# Revisar antes de commitar
git diff --staged

# Usar git add interativo para commits precisos
git add -p
```

---

## Branches

### Estratégias de Branching

#### Git Flow
```
main/master     # Produção
develop         # Desenvolvimento
feature/        # Novas funcionalidades
release/        # Preparação para release
hotfix/         # Correções urgentes
```

#### GitHub Flow (Mais Simples)
```
main           # Produção
feature/       # Todas as funcionalidades
```

### Nomenclatura de Branches
```bash
# Boas práticas
feature/user-authentication
bugfix/login-error-handling
hotfix/security-vulnerability
docs/api-documentation

# Evitar
branch1
fix
temp
test-stuff
```

### Comandos Essenciais
```bash
# Criar e mudar para nova branch
git checkout -b feature/nova-funcionalidade

# Listar branches
git branch -a

# Deletar branch local
git branch -d nome-da-branch

# Deletar branch remota
git push origin --delete nome-da-branch

# Sincronizar com remoto
git fetch --prune
```

---

## Pull Requests

### Estrutura de um Bom Pull Request

#### Template Sugerido
```markdown
## Descrição
Breve descrição das mudanças implementadas.

## Motivação
Por que essa mudança é necessária?

## Checklist
- [ ] Código testado
- [ ] Documentação atualizada
- [ ] Não há conflitos
- [ ] Segue padrões do projeto


Closes #123
```

### Boas Práticas para PRs
- **Mantenha pequeno**: Foque em uma funcionalidade
- **Descrição clara**: Explique o que e por que
- **Testes**: Inclua testes adequados
- **Review próprio**: Revise seu código antes de abrir o PR
- **Responda feedback**: Seja receptivo a sugestões

---

## Repositórios

### Estrutura de Arquivos Essenciais

#### README.md
```markdown
# Nome do Projeto

## Descrição
Breve descrição do que o projeto faz.

## Como executar
```bash
# Passos para executar o projeto
```

## Tecnologias
- Python 3.x
- Django
- PostgreSQL

## Licença
MIT License
```

#### .gitignore
```bash
# Python
__pycache__/
*.pyc
*.pyo
*.pyd
.Python
env/
venv/
.venv/

# IDE
.vscode/
.idea/
*.swp
*.swo

# Sistema
.DS_Store
Thumbs.db

# Dependências
node_modules/
```

#### .gitattributes
```bash
# Tratamento de line endings
* text=auto
*.py text eol=lf
*.js text eol=lf
*.css text eol=lf
*.html text eol=lf

# Arquivos binários
*.png binary
*.jpg binary
*.pdf binary
```

---

## Segurança

### Proteção de Dados Sensíveis

#### Boas Práticas
- **Nunca commite**:
  - Senhas, tokens, chaves API
  - Certificados privados
  - Informações pessoais
  - Configurações de produção

#### Usando Variáveis de Ambiente
```python
# settings.py
import os
from dotenv import load_dotenv

load_dotenv()

SECRET_KEY = os.getenv('SECRET_KEY')
DATABASE_URL = os.getenv('DATABASE_URL')
```

#### .env (não versionar!)
```bash
SECRET_KEY=sua-chave-secreta
DATABASE_URL=postgresql://user:pass@localhost/db
API_KEY=sua-api-key
```

### GitHub Secrets
- Configure secrets no repositório
- Use em GitHub Actions
- Acesse via `${{ secrets.SECRET_NAME }}`

---

## Colaboração

### Workflow de Equipe

#### 1. Clone e Setup
```bash
git clone git@github.com:usuario/repositorio.git
cd repositorio
git remote add upstream git@github.com:original/repositorio.git
```

#### 2. Sincronização Regular
```bash
# Buscar mudanças do repositório principal
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

#### 3. Desenvolvimento
```bash
# Criar branch para feature
git checkout -b feature/minha-feature

# Trabalhar e commitar
git add .
git commit -m "feat: adiciona nova funcionalidade"

# Push da branch
git push origin feature/minha-feature
```

### Code Review

#### Para Reviewers
- **Seja construtivo**: Foque no código, não na pessoa
- **Seja específico**: Aponte problemas e sugira soluções
- **Priorize**: Distinga entre bugs críticos e melhorias
- **Reconheça**: Elogie bom código também

#### Para Autores
- **Seja receptivo**: Aceite feedback construtivo
- **Explique decisões**: Quando necessário
- **Teste sugestões**: Antes de implementar
- **Agradeça**: Reconheça o tempo do reviewer

---

## Ferramentas e Automação

### GitHub Actions

#### Exemplo: CI/CD Básico
```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.9'
    
    - name: Install dependencies
      run: |
        pip install -r requirements.txt
        pip install pytest
    
    - name: Run tests
      run: pytest
    
    - name: Check code style
      run: flake8 .
```

### Hooks Úteis

#### Pre-commit Hook
```bash
# .git/hooks/pre-commit
#!/bin/sh
echo "Executando testes antes do commit..."
python -m pytest
if [ $? -ne 0 ]; then
    echo "Testes falharam. Commit abortado."
    exit 1
fi
```

### Aliases Úteis
```bash
# Adicionar ao ~/.gitconfig
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    ca = commit --amend
    lg = log --oneline --graph --all
    unstage = reset HEAD --
```

---

## Comandos Git Essenciais

### Básicos
```bash
# Status do repositório
git status

# Adicionar arquivos
git add arquivo.py          # Específico
git add .                   # Todos os arquivos
git add -A                  # Incluindo removidos

# Commit
git commit -m "mensagem"
git commit --amend         # Editar último commit

# Push/Pull
git push origin main
git pull origin main
```

### Intermediários
```bash
# Ver histórico
git log --oneline
git log --graph --all

# Desfazer mudanças
git checkout -- arquivo.py      # Desfaz mudanças não staged
git reset HEAD arquivo.py       # Remove do staging
git reset --soft HEAD~1         # Desfaz último commit (mantém mudanças)
git reset --hard HEAD~1         # Desfaz último commit (remove mudanças)

# Stash (guardar mudanças temporariamente)
git stash
git stash pop
git stash list
```

### Avançados
```bash
# Rebase interativo
git rebase -i HEAD~3

# Cherry-pick
git cherry-pick <commit-hash>

# Bisect (encontrar bug)
git bisect start
git bisect bad HEAD
git bisect good <commit-hash>

# Blame (rastrear mudanças)
git blame arquivo.py
```

---

## Resumo das Principais Práticas

### FAZER
- Commits pequenos e atômicos
- Mensagens de commit descritivas
- Branches com nomes significativos
- Pull Requests pequenos e focados
- Revisar código próprio antes de PR
- Usar .gitignore apropriado
- Proteger informações sensíveis
- Sincronizar regularmente com main/master
- Escrever documentação clara

### EVITAR
- Commits gigantes com muitas mudanças
- Mensagens vagas como "fix stuff"
- Commitar arquivos de configuração local
- Deixar branches antigas sem deletar
- Fazer force push em branches compartilhadas
- Commitar dados sensíveis
- Trabalhar sempre na branch main
- Ignorar conflitos de merge

---

## Recursos Adicionais

- [Git Documentation](https://git-scm.com/doc)
- [GitHub Docs](https://docs.github.com/)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/)
- [GitHub Flow](https://guides.github.com/introduction/flow/)

---
