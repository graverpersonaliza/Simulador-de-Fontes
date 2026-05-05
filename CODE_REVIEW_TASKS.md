# Revisão rápida da base e tarefas sugeridas

## 1) Erro de digitação / rótulo (baixa complexidade)
**Problema observado**
- O botão principal de criação de texto aparece como **"Add"** em uma UI toda em português.

**Tarefa sugerida**
- Trocar o rótulo do botão `#addTextBtn` de `Add` para `Adicionar`.
- Validar visualmente se o texto continua cabendo no botão em desktop e mobile.

**Onde**
- `index.html` (markup do botão na barra de ações de texto).

---

## 2) Bug funcional (média complexidade)
**Problema observado**
- No gesto de **pinça em texto** (`textPinch`), o tamanho (`sizeMM`) é atualizado, mas **não há clamp por razão de escala** como já existe em outros caminhos de resize (mouse e touch por alça). Isso pode gerar saltos de tamanho em gestos extremos.

**Tarefa sugerida**
- Padronizar o cálculo de resize por pinça para texto usando os mesmos limites de razão aplicados em outros fluxos (`0.2` a `8`) antes de aplicar limites absolutos de tamanho (`5` a `150`).
- Cobrir com teste (unitário da função de clamp extraída, ou teste E2E simulando gesto).

**Onde**
- `index.html` (bloco de `touchmove`, ramo `dragging.type === 'textPinch'`).

---

## 3) Comentário/documentação inconsistente (baixa complexidade)
**Problema observado**
- O texto de ajuda lista atalhos com `Ctrl/Cmd+Y` para refazer, porém o código também aceita `Ctrl/Cmd+Shift+Z`.

**Tarefa sugerida**
- Atualizar a linha de ajuda de atalhos para refletir **todas** as combinações suportadas, evitando discrepância com o comportamento real.

**Onde**
- `index.html` (nota de atalhos) e listener global de teclado.

---

## 4) Melhoria de teste (média complexidade)
**Lacuna observada**
- Não há suíte visível cobrindo fluxos críticos de edição (atalhos, undo/redo, drag/resize, autosave).

**Tarefa sugerida**
- Criar teste E2E (Playwright/Cypress) para um fluxo mínimo:
  1. adicionar texto;
  2. mover com seta;
  3. duplicar com `Ctrl/Cmd+D`;
  4. desfazer/refazer (`Ctrl/Cmd+Z`, `Ctrl/Cmd+Y` e `Ctrl/Cmd+Shift+Z`);
  5. validar persistência básica após salvar/carregar projeto.
- Adicionar assertions para contagem de camadas (`texts.length`) e estado de seleção após cada ação.

**Onde**
- Nova pasta de testes (ex.: `tests/e2e/`) + possível extração de utilitários de estado para facilitar inspeção nos testes.
