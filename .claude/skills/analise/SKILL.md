---
name: analise
description: Roda uma análise de genômica populacional do começo ao fim com freios — contrato, plano, subconjunto, execução, checagem de sanidade e relatório curto. Use sempre que a pessoa pedir para calcular, rodar, estimar, comparar ou plotar qualquer coisa a partir dos dados (Fst, PCA, ADMIXTURE, pi, dxy, ROH, seleção, parentesco, filtros, conversões). Use também quando ela disser "/analise", "roda isso", "calcula X", "faz o PCA".
---

# /analise — o fluxo com freios

Você está rodando uma análise. O objetivo é **chegar no que foi pedido pelo caminho mais curto defensável**, não explorar o dataset.

Antes de agir, leia `docs/regras/analise.md`, `docs/regras/dados.md` e `PROJETO.md`.

## Passo 1 — Contrato (primeira coisa que você escreve na tela)

```
PERGUNTA: <pergunta biológica em uma frase>
ENTRADA:  <arquivo> (N amostras, M variantes, build)
SAÍDA:    <arquivo de saída> (+ figura, se pedida)
MÉTODO:   <ferramenta + flags>
NÃO FAZ:  <o que fica de fora de propósito>
```

- Os números de N e M você **confere com um comando barato** (ver `dados.md`), não chuta.
- Se qualquer linha tiver "acho que": **pare aqui e pergunte**. Uma pergunta objetiva, com as opções. Não duas telas de perguntas.

## Passo 2 — Plano (só se forem mais de 3 passos)

Passos numerados, uma linha cada, e **espere o ok**. Até 3 passos: siga sem perguntar.

## Passo 3 — Subconjunto

Rode em uma fatia pequena (1 cromossomo, 1.000 variantes, 20 amostras). Olhe a saída de verdade. Só siga se fizer sentido.

## Passo 4 — Rodar completo

- Estime o tempo. Mais de ~10 min → avise e confirme antes de disparar.
- Script numerado em `scripts/`, log em `logs/`, saída em `results/<AAAA-MM-DD>-<slug>/`.
- Seed fixo quando houver aleatoriedade.

## Passo 5 — Sanidade (obrigatório)

Abra a saída. Confira contagens, faixa de valores, `NA`/`inf`, e se os rótulos de população estão certos. Resultado que não passa na sanidade **não é resultado** — é problema, e se reporta como problema.

## Passo 6 — Relatório

Formato de `docs/regras/relatorio.md`. Curto. Com a seção "O que eu NÃO fiz".

---

## Os freios (valem o tempo todo)

- **Orçamento de desvio: 1.** No segundo desvio: pare, reporte com o erro literal, dê 2 opções, espere.
- **Só o X pedido.** Nada de QC extra, PCA extra, gráfico extra, refatoração.
- **Ferramenta canônica** (ver `docs/regras/ferramentas.md`). Nada de parser próprio de VCF/BAM.
- **Nada de instalar, baixar, apagar ou sobrescrever** sem confirmação.
- **`data/raw/` é somente leitura.**
- Mais de ~10 ações sem falar com ela = você se perdeu. Pare e reporte.
