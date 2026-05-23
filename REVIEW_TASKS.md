# Revisão rápida da base e tarefas sugeridas

## 1) Erro de digitação (copy/UI)
**Problema:** o texto do botão **"Recarregar Dados"** está associado à ação de restauração de backup via upload de arquivo JSON. Em português de produto, "restaurar" comunica melhor que os dados serão substituídos pelo conteúdo do backup.

**Tarefa sugerida:**
- Alterar o rótulo para **"Restaurar Dados"** (ou "Restaurar Backup").
- Validar consistência com o feedback exibido ao usuário após o restore.

**Referência:** botão que dispara `restore-input` na seção de Configurações.

---

## 2) Bug funcional (lógica de ordenação de clientes inativos)
**Problema:** em `renderReativacao()`, a lista de clientes inativos usa `slice(0, 30)` sem ordenar por valor total, apesar do subtítulo da tela dizer "Clientes inativos por valor histórico". Na prática, o resultado depende da ordem do array original e pode mostrar clientes menos relevantes antes dos maiores.

**Tarefa sugerida:**
- Ordenar explicitamente por `total` decrescente antes do `slice(0, 30)`.
- Exemplo: `.sort((a, b) => b.total - a.total).slice(0, 30)`.

**Referência:** função `renderReativacao()`.

---

## 3) Comentário/documentação desalinhado com comportamento
**Problema:** o comentário "Enter no login" sugere que Enter funcionaria no login em geral, mas o listener é registrado apenas no campo de senha (`#login-pass`). Usuário pressionando Enter no campo de email não dispara login.

**Tarefa sugerida:**
- Ajustar comentário para refletir o comportamento atual (**"Enter no campo de senha"**), **ou**
- Implementar listener também no campo de email para alinhar comportamento à expectativa do comentário.

**Referência:** bloco de inicialização no final do script.

---

## 4) Melhoria de teste (regressão de regra de negócio)
**Problema:** não há evidência de testes automatizados para regras críticas de cálculo/segmentação.

**Tarefa sugerida:**
- Criar testes unitários (ex.: Vitest/Jest) para funções puras:
  - `statusOf(dias)` (30/60 como fronteiras)
  - `daysBetween(d1, d2)` (normalização por dia e timezone)
  - `getUnCx(tamanho)` (mapeamento 5L/1L/default)
- Adicionar teste de regressão para reativação garantindo ordenação por maior `total`.

**Referência:** funções utilitárias e `renderReativacao()`.
