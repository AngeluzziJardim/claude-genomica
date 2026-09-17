---
name: checar-dados
description: Sanidade de um arquivo genômico (VCF, BCF, PLINK bed/bim/fam, metadados) antes de confiar nele — contagens, build, cromossomos, missingness, descasamento de amostras. Use quando a pessoa disser "/checar-dados", "esse VCF está ok?", "quantas amostras tem aí", "confere esse arquivo", ou antes de qualquer análise nova em um arquivo que ainda não foi conferido nesta sessão.
---

# /checar-dados — conferir antes de confiar

Somente leitura. **Não filtra, não converte, não conserta nada.** Só olha e reporta.

## O que rodar

VCF/BCF:

```bash
bcftools query -l ARQ | wc -l                    # amostras
bcftools index -n ARQ                            # variantes (se houver índice)
bcftools view -h ARQ | grep -E "^##(reference|contig)" | head
bcftools view -h ARQ | grep -E "^##FILTER"       # filtros já aplicados
bcftools query -f "%CHROM\n" ARQ | uniq -c | head -30
bcftools stats ARQ | grep -E "^SN"
```

PLINK:

```bash
wc -l ARQ.fam ARQ.bim
cut -f1 ARQ.bim | uniq -c | head -30
plink2 --bfile ARQ --missing --out /tmp/chk
plink2 --bfile ARQ --freq --out /tmp/chk
```

Adapte ao que existir. Se a ferramenta não estiver instalada, **diga** — não instale.

## O que reportar

```
ARQUIVO:     <caminho>
FORMATO:     <VCF bgzipped / PLINK bed / ...>
BUILD:       <hg38 / hg19 / não declarado no header>
AMOSTRAS:    N
VARIANTES:   M
CROMOSSOMOS: <lista, com o prefixo exato: "chr1..chr22" ou "1..22">
MISSINGNESS: mediana X%, pior amostra Y%, nº de amostras acima de 10%: Z
FILTROS JÁ APLICADOS: <do header, ou "nenhum declarado">
METADADOS:   <quantos IDs casam com samples.tsv; quem sobra de cada lado>

PONTOS DE ATENÇÃO
- <cada armadilha encontrada, uma linha, sem consertar>
```

Armadilhas a procurar (lista completa em `docs/regras/dados.md`): prefixo `chr` vs sem prefixo, build ausente ou divergente entre arquivos, amostras duplicadas, IDs que não casam com os metadados, missingness concentrada em uma população, ausência de sítios invariantes, alelos ambíguos A/T e C/G.

**Nunca conserte nada aqui.** Aponte e proponha. A decisão é dela.
