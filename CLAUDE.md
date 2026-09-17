# CLAUDE.md — Projeto de Genômica Populacional

> **Este arquivo manda.** Se o que está aqui conflita com o seu instinto de "seria melhor se...", este arquivo ganha. Sempre.
> Este arquivo é um **resolver**: ele tem as regras que valem sempre e aponta pro doc certo conforme a situação. Antes de agir, carregue o doc que se aplica — são curtos, leia inteiro.

---

## As 7 regras que valem sempre

### 1. Entregue o X que foi pedido. Só o X.
Se pediram Fst entre duas populações, entregue Fst entre duas populações. Não entregue PCA + ADMIXTURE + gráfico de missingness "porque ajuda a interpretar".
Se você acha que falta um passo antes (ex: "esse VCF não passou por QC"), **diga em uma frase e pergunte** — não faça por conta própria.

### 2. Orçamento de desvio: 1.
Um desvio do caminho principal é permitido (um diagnóstico, uma checagem, uma tentativa alternativa).
**No segundo desvio, pare e reporte.** Diga: o que tentei, o erro literal, qual é a minha hipótese, e as 2 opções de caminho. Deixe a decisão com ela.
Rodar cinco ferramentas diferentes até uma funcionar é proibido.

### 3. Contrato antes de rodar.
Antes de executar qualquer coisa que leia dados, escreva **3 linhas**:
```
ENTRADA:  <arquivo exato>  (N amostras, M variantes, build)
SAÍDA:    <arquivo exato>  (o que vai ter dentro)
COMANDO:  <a ferramenta e as flags>
```
Se qualquer uma das três linhas tiver um "acho que", **pergunte antes de rodar**.

### 4. Subconjunto primeiro, dataset depois.
Todo comando novo roda primeiro em uma fatia pequena (um cromossomo, 1.000 variantes, 20 amostras).
Só depois de a fatia sair certa é que roda no dataset inteiro — e aí **avisa quanto tempo deve levar**.

### 5. Ferramenta canônica, não código novo.
Se `bcftools`, `plink2`, `vcftools`, `ADMIXTURE`, `pixy` ou `ANGSD` já fazem, **use a flag**. Não escreva script.
Nunca parseie VCF/BAM na mão. Catálogo: [`docs/regras/ferramentas.md`](docs/regras/ferramentas.md).

### 6. Nada de instalar, baixar ou apagar sem perguntar.
Zero `conda install`, `pip install`, `apt`, download de referência ou painel, e zero `rm`/sobrescrita de arquivo existente sem confirmação explícita.
`data/raw/` é **somente leitura**. Sempre.

### 7. Relatório curto, em português, com o que você NÃO fez.
Formato fixo em [`docs/regras/relatorio.md`](docs/regras/relatorio.md). Nada de textão. Nada de afirmar resultado sem ter aberto o arquivo de saída.

---

## Mais duas regras de postura

- **Pergunte, não assuma.** Incerteza sobre intenção, formato de arquivo, população de referência ou parâmetro: **pergunte antes**, não chute em silêncio.
- **Sinalize incerteza.** Se você não tem certeza se o método é o adequado, diga isso **antes** de gastar tempo dela. Confiança sem certeza causa dano em análise científica.

---

## Resolver — leia o doc da sua situação

| Situação | Leia |
|---|---|
| Vou tocar em dados (VCF, PLINK, BAM, metadados) | [`docs/regras/dados.md`](docs/regras/dados.md) |
| Vou escolher/rodar uma ferramenta | [`docs/regras/ferramentas.md`](docs/regras/ferramentas.md) |
| Vou rodar uma análise de ponta a ponta | [`docs/regras/analise.md`](docs/regras/analise.md) |
| Vou responder/entregar o resultado | [`docs/regras/relatorio.md`](docs/regras/relatorio.md) |
| Algo quebrou | [`docs/regras/analise.md`](docs/regras/analise.md) → seção "Quando quebra" |
| Preciso saber o stack/dados deste projeto | [`PROJETO.md`](PROJETO.md) |

---

## Estrutura do projeto (não invente outra)

```
data/raw/        # imutável. Nunca escrever aqui.
data/interim/    # arquivos intermediários (filtrados, subsets)
scripts/         # 01-qc.sh, 02-pca.sh, ... numerados, um por etapa
results/<AAAA-MM-DD>-<slug>/   # saídas finais + figuras
logs/            # log de cada rodada (comando + versão + tempo)
PROJETO.md       # o que é este projeto, stack, dados, convenções
```

---

## Proibido neste repositório

- Criar pipeline framework (Snakemake, Nextflow, Makefile orquestrador) — **a menos que ela peça com essas palavras**.
- Refatorar, "limpar" ou renomear script que já existe e funciona.
- Melhorar um gráfico que ninguém pediu pra melhorar.
- Rodar QC completo quando pediram uma estatística.
- Mexer em arquivo que não faz parte da tarefa atual — mesmo que dê pra melhorar.
- Mudar parâmetro de filtro (MAF, missingness, HWE) sem falar qual mudou e por quê.
- Gerar resultado e descrever a conclusão sem ter lido o arquivo de saída.

---

## Skills desta casa (invoque pela `/`)

- **`/setup-projeto`** — primeira vez no projeto: faz ~8 perguntas e preenche o `PROJETO.md`.
- **`/analise <pergunta>`** — o fluxo com freios: contrato → plano → subconjunto → rodar → sanidade → relatório.
- **`/checar-dados <arquivo>`** — sanidade de um VCF/PLINK antes de confiar nele.
- **`/explicar <coisa>`** — explicação em 3 níveis, sem código, sem rodar nada.
