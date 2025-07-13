# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## PRINCÍPIOS IMPORTANTES
- **SEMPRE responder em português brasileiro**
- Usar terminologia técnica em português quando possível
- Manter consistência com o idioma do projeto (site em português)

## Visão Geral do Projeto
Este é um site estático para o evento de conferência jovem "ONE WAY" (31 de julho - 2 de agosto de 2025). É uma aplicação de página única construída com HTML, CSS e JavaScript vanilla.

## Comandos de Desenvolvimento

### Execução Local
```bash
# Método 1: Servidor Python (recomendado)
python -m http.server 8000
# Acesse: http://localhost:8000

# Método 2: Servidor Node.js
npx serve .
# Acesse: http://localhost:3000

# Método 3: Abertura direta (funciona mas pode ter limitações)
open index.html
```

### Versionamento de Cache
Para forçar atualização do cache durante desenvolvimento:
```bash
# Adicione parâmetro de versão nas URLs das imagens/CSS
# Exemplo: src="./img/exemplo.jpg?v=1"
```

## Arquitetura e Componentes Principais

### Estrutura de Arquivos
- `index.html` - Página única contendo todo o conteúdo
- `Css/style.css` - Todos os estilos em um arquivo
- `img/` - Todos os assets de imagem incluindo imagens de ingressos em `img/ingressos/`

### Arquitetura do HTML (Fluxo de Seções)
```
index.html (página única)
├── section_home: Hero banner com CTAs
├── section_date: Card glassmorphism com data/local
├── class_valores: Seção de ingressos (4 lotes)
├── section_produtos: Cards de produtos (opcional)
├── div_note_sobre: Banner sobre (desktop)
├── div_cel_sobre: Banner sobre (mobile)
├── saq: FAQ com accordion
└── div_rodape: Rodapé com logo
```

### Sistema de Imagens Responsivas
O projeto usa uma arquitetura específica para desktop/mobile:
```css
.img-desktop { display: block; } /* > 768px */
.img-mobile { display: none; }

@media (max-width: 768px) {
  .img-desktop { display: none; }
  .img-mobile { display: block; }
}
```

### Estado dos Lotes de Ingressos
Os lotes são controlados via display CSS nos elementos `<a>`:
- `display: none` = Lote oculto
- `display: block` = Lote visível
- Imagens: `LOTE[N].png` (ativo), `LOTE[N]ESGOTADO.png`, `LOTE[N]APAGADO.png`

### Integração Externa
- **Ingressos**: tiketo.com.br (links diretos)
- **Pagamentos**: Stripe/MercadoPago (implementação placeholder)
- **Sem backend**: Site estático puro

## Patterns e Convenções Específicas

### Gerenciamento de Lotes
Para ativar/desativar lotes, modifique os styles inline:
```html
<!-- Lote ativo -->
<a href="https://tiketo.com.br/evento/3063">
  <img src="./img/ingressos/LOTE3.png" alt="">
</a>

<!-- Lote esgotado -->
<a href="https://tiketo.com.br/evento/3063" style="display: none;">
  <img src="./img/ingressos/LOTE3.png" alt="">
</a>
<a href="https://tiketo.com.br/evento/3063">
  <img src="./img/ingressos/LOTE3ESGOTADO.png" alt="">
</a>
```

### Cache Control Durante Desenvolvimento
O projeto inclui headers para evitar cache agressivo:
```html
<meta http-equiv="Cache-Control" content="no-cache, no-store, must-revalidate">
<meta http-equiv="Pragma" content="no-cache">
<meta http-equiv="Expires" content="0">
```

### Comandos Específicos do Projeto

#### Gestão de Imagens
```bash
# Listar todas as imagens de lotes
ls -la img/ingressos/

# Verificar imagens responsivas principais
ls -la img/ | grep -E "(ONE WAY|site - vertical)"

# Verificar se existe imagem antes de usar
test -f img/exemplo.jpg && echo "Existe" || echo "Não existe"
```

#### Debug de Layout Responsivo
```bash
# Testar em diferentes resoluções (via DevTools)
# Desktop: > 768px usa .img-desktop
# Mobile: <= 768px usa .img-mobile

# Verificar breakpoints no CSS
grep -n "768px" Css/style.css
```

#### Manipulação de Lotes (Tarefas Comuns)
```bash
# Buscar todos os elementos de lote no HTML
grep -n "LOTE[0-9]" index.html

# Verificar status atual dos lotes
grep -A2 -B2 "display.*none" index.html
```

### Observações Críticas
- Todo JavaScript está inline no index.html - sem arquivos JS separados
- Headers de segurança configurados em meta tags (CSP, X-Frame-Options, etc.)
- Sem variáveis de ambiente ou arquivos de configuração
- Links para tiketo.com.br são hardcoded (vendas de ingressos externas)
- Cache pode ser problemático: sempre use parâmetros de versão (?v=1) em desenvolvimento

---

# Template IAFirst - Extensão para Desenvolvimento com IA

## 🌐 IMPORTANTE: Idioma das Respostas

**SEMPRE responda em PORTUGUÊS BRASILEIRO** ao trabalhar neste repositório, independentemente do idioma da pergunta.

## 🛠️ Comandos Comuns

### Docker (Método Recomendado)

```bash
# Setup completo
docker compose up -d

# Status dos serviços
docker compose ps

# Logs
docker compose logs -f [serviço]

# Parar ambiente
docker compose down
```

### Desenvolvimento Local

```bash
# Sempre use ambiente virtual (Python)
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows

# Node.js
npm install
npm run dev
npm run build
```

### Comandos de Qualidade

```bash
# Python
black .
flake8 .
bandit -r .
mypy .
pytest

# Node.js
npm run lint
npm run type-check
npm run test
```

## 🤖 Workflow IAFirst para Desenvolvimento com IA

### Fluxo Completo de Desenvolvimento

1. **📋 Implementação (IA/Claude executa)**
   - Desenvolve a solução conforme especificado na issue
   - Marca checkboxes das tarefas técnicas (✅) conforme implementa
   - Segue rigorosamente o escopo definido

2. **🔄 Finalização (IA/Claude executa automaticamente)**
   - Move issue de "IA Ready" → "REVIEW" no GitHub Project
   - Adiciona comentário de resumo na issue com:
     ```markdown
     ## 🤖 Implementação realizada por Claude Code (IA)
     
     ### 📋 O que foi feito:
     - Lista detalhada das implementações
     
     ### 🎯 Status Atual:
     - Status da issue e arquivos criados/modificados
     
     ### ⏳ Próximos Passos (para você):
     - Instruções claras para validação
     
     ---
     *Este código foi desenvolvido por **Claude Code** (Assistente de IA da Anthropic) 
     seguindo o padrão IAFirst de desenvolvimento.*
     ```

3. **✅ Validação (Humano/Usuário executa)**
   - Testa a implementação
   - Marca checkboxes dos critérios de aceitação
   - Move issue de "REVIEW" → "DONE" quando satisfeito

### Checklist IAFirst (para IA seguir)

- [ ] Implementar solução dentro do escopo
- [ ] Marcar checkboxes das tarefas técnicas conforme implementa
- [ ] Mover issue para coluna REVIEW após conclusão
- [ ] Adicionar comentário de resumo identificando desenvolvimento por IA
- [ ] Aguardar validação humana (nunca mover para DONE)

### Benefícios do Padrão IAFirst

- **Transparência**: Código claramente identificado como gerado por IA
- **Qualidade**: Validação humana obrigatória antes de conclusão
- **Rastreabilidade**: Histórico completo no GitHub
- **Eficiência**: IA foca em implementação, humano em validação

## 🔒 Considerações de Segurança

- **Variáveis de Ambiente**: Todos os segredos em `.env` usando bibliotecas de configuração
- **CORS**: Restrito apenas a origens permitidas
- **Autenticação**: Baseada em token para APIs
- **Logging**: Logs estruturados para trilhas de auditoria
- **Rate Limiting**: Configurado para proteção de APIs

## 📝 Convenções de Código

### Python
- **Formato**: Black com linhas de 96 caracteres
- **Imports**: isort com perfil black
- **Tipos**: Tipagem completa com `from __future__ import annotations`
- **Docstrings**: Estilo Google apenas para APIs públicas

### JavaScript/TypeScript
- **Formato**: Prettier + ESLint
- **Tipos**: TypeScript em modo strict
- **Hooks**: ESLint rules para React Hooks

## 🚫 Regras Críticas para IA

### Nunca Alterar Sem Confirmar
1. **Contratos de API** - Quebra frontend/integrações
2. **Schemas de banco de dados** - Risco de perda de dados
3. **Arquivos de configuração Docker** - Quebra ambiente
4. **Configurações de CI/CD** - Quebra pipeline

### Credenciais e Segurança
- **Nunca commitar** arquivos `.env` (apenas `.env.example`)
- **URLs vs credenciais**: URLs ficam no `.env`, credenciais na interface
- **Rate limiting**: Sempre preservar configurações

### Workflow e Aprovação  
- **NUNCA aprovar ou fazer merge de PRs** - Aprovação é EXCLUSIVA do usuário
- **Pipeline de Issues**: IA move para REVIEW, usuário move para DONE
- **🚨 SEMPRE criar issues no GitHub Project** - NUNCA diretamente no repositório
- **Comando obrigatório**: `gh issue create` (integra automaticamente com projeto)

## 📋 Processo de Criação de Issues (OBRIGATÓRIO)

### Comando Correto
```bash
gh issue create --title "Título da Issue" --body "Descrição detalhada"
```

### Template de Issues Focadas

Todas as issues seguem template rigoroso com:
- 🎯 **Escopo específico** (o que EXATAMENTE fazer)
- 🚫 **Lista de proibições** (seguindo regras CLAUDE.md)
- ✅ **Interfaces permitidas** (usar apenas código existente)
- 🎯 **Arquivos permitidos** (o que pode criar/modificar)
- ✅ **Critérios de aceitação** (checklist obrigatório)

### Workflow de Issues
```
Issue criada → "Backlog" → IA move para "IA Ready" → implementa → "REVIEW" → usuário valida → "DONE"
```

## 🔄 CI/CD e GitHub Actions

### Filosofia: Qualidade Gradual
- **Warnings não bloqueiam** CI - focado em desenvolvimento ágil
- **Configurações permissivas** inicialmente
- **Melhoria incremental** da qualidade ao longo do tempo

### Workflows Essenciais

#### 1. CI Pipeline
```yaml
# .github/workflows/ci.yml
- Testes automatizados
- Verificação de qualidade (permissiva)
- Build e validação
```

#### 2. CD Pipeline  
```yaml
# .github/workflows/cd.yml
- Deploy automático staging (push main)
- Deploy produção (tags v*)
- Health checks
```

#### 3. PR Checks
```yaml
# .github/workflows/pr-checks.yml  
- Validação de títulos
- Auto-labeling
- Preview builds
```

### ⚠️ REGRAS CRÍTICAS: Workflow e Aprovações

#### Aprovação de PRs
- **APENAS o usuário** pode aprovar e fazer merge de PRs
- **Claude NUNCA** deve aprovar, fazer merge ou sugerir aprovação
- **Claude** pode apenas criar PRs e aguardar aprovação

#### Workflow de Issues
- **Claude**: implementa funcionalidades → move para **REVIEW**
- **Usuário**: valida implementação → move para **DONE**
- **NUNCA** mover issues diretamente para DONE sem validação do usuário

## 🐳 Ambiente Docker

### Estrutura Mínima
```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
    volumes:
      - .:/app
      - /app/node_modules
```

### Health Checks
```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
  interval: 30s
  timeout: 10s
  retries: 3
```

## 📁 Estrutura de Projeto Recomendada

```
projeto/
├── .github/workflows/     # CI/CD
├── src/                   # Código fonte
├── tests/                 # Testes
├── docs/                  # Documentação
├── .env.example          # Template de configuração
├── .gitignore            # Arquivos ignorados
├── CLAUDE.md             # Este arquivo
├── docker-compose.yml    # Ambiente Docker
└── README.md             # Documentação do projeto
```

## 🎯 Princípios de Desenvolvimento

### Para IA (Claude)
1. **Transparência**: Sempre identificar código gerado por IA
2. **Escopo limitado**: Seguir exatamente o que foi solicitado
3. **Validação humana**: Nunca assumir que implementação está pronta
4. **Qualidade incremental**: Melhorar código aos poucos

### Para Humano (Usuário)
1. **Issues claras**: Definir escopo específico e critérios de aceitação
2. **Validação obrigatória**: Sempre testar antes de aprovar
3. **Feedback construtivo**: Orientar melhorias quando necessário
4. **Aprovação explícita**: Controle total sobre o que vai para produção

## 🚀 Comandos Úteis para Gestão

```bash
# Criar issue no projeto
gh issue create --title "Título" --body "Descrição"

# Criar PR
gh pr create --title "Título" --body "Descrição"

# Verificar status do projeto
gh project list

# Ver issues do projeto
gh issue list --assignee "@me"
```

## 💡 Dicas de Uso

1. **Comece pequeno**: Issues simples e bem definidas
2. **Valide sempre**: Teste tudo antes de aprovar
3. **Documente decisões**: Use comments nas issues para justificar escolhas
4. **Melhore incrementalmente**: Qualidade evolui ao longo do tempo
5. **Mantenha histórico**: GitHub como fonte da verdade

---

*Este template foi criado baseado no padrão IAFirst de desenvolvimento, 
promovendo colaboração eficiente entre IA e desenvolvedores humanos.*