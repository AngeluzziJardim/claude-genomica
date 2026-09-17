# Ferramenta canônica — use a flag, não escreva script

**Regra nº 1:** se a ferramenta padrão já faz, use a ferramenta padrão. Código novo em Python/R só quando nenhuma ferramenta cobre, e mesmo assim: o menor script possível, um arquivo, sem classes, sem framework.

**Regra nº 2:** nunca parseie VCF, BAM, BED ou FASTA "na mão" (`open()` + `split('\t')`). Use `bcftools`, `samtools`, `cyvcf2`, `scikit-allel` ou `pysam`.

## Catálogo

| Preciso de... | Use | Não use |
|---|---|---|
| Filtrar / subsetar / converter VCF | `bcftools view/filter/query` | script Python |
| Estatística por variante (freq, HWE, missing) | `plink2 --freq --hardy --missing` | loop manual |
| Conversão VCF ↔ PLINK | `plink2 --vcf ... --make-bed` | — |
| PCA | `plink2 --pca` (rápido) ou `smartpca`/EIGENSOFT (projeção, outliers) | PCA na mão em numpy |
| Poda de LD antes de PCA/ADMIXTURE | `plink2 --indep-pairwise` | — |
| Estrutura populacional / ancestralidade | `ADMIXTURE` (ou `fastSTRUCTURE`) | — |
| Fst | `plink2 --fst` ou `vcftools --weir-fst-pop` | — |
| π, dxy, Fst **com sítios invariantes** | `pixy` | vcftools (subestima π) |
| Estatísticas em Python | `scikit-allel` | parser próprio |
| f3 / f4 / D / qpAdm / qpGraph | `ADMIXTOOLS 2` (R) | — |
| Árvore / migração | `TreeMix`, `qpGraph` | — |
| Seleção (iHS, XP-EHH, nSL) | `selscan` + `norm` | — |
| Dados de baixa cobertura / genotype likelihoods | `ANGSD` (+ `PCAngsd`, `NGSadmix`) | chamar genótipo duro |
| Fasear | `SHAPEIT5` ou `Beagle` | — |
| Imputar | `Beagle` ou `Minimac4` | — |
| Parentesco | `KING` ou `plink2 --make-king` | — |
| ROH / consanguinidade | `plink2 --homozyg` ou `bcftools roh` | — |
| Demografia / tamanho efetivo | `SMC++`, `MSMC2`, `Stairway Plot` | — |
| Simulação | `msprime` (coalescente), `SLiM` (forward) | — |
| Alinhamento / BAM | `bwa-mem2`, `samtools` | — |
| Chamada de variantes | `GATK` ou `bcftools call` | — |
| Anotação | `VEP`, `SnpEff`, `bcftools csq` | — |
| Gráficos | `R` + `ggplot2` (ou `matplotlib`) | biblioteca nova |

> Se o projeto dela usa uma ferramenta diferente para alguma dessas linhas, isso fica registrado em `PROJETO.md` e **`PROJETO.md` ganha desta tabela**.

## Ambiente

- Um ambiente conda/mamba só, o que já existe. **Não criar ambiente novo, não instalar pacote, sem perguntar.**
- Antes de dizer "a ferramenta não está instalada", rode `which <ferramenta>` e `<ferramenta> --version`. Metade das vezes está lá com outro nome (`plink2`, `plink`, `bcftools`).
- Versão de tudo que rodou vai pro log.

## Performance — a ordem certa

1. Use a flag de paralelismo que a ferramenta já tem (`--threads`).
2. Trabalhe por cromossomo antes de trabalhar no genoma inteiro.
3. Só então pense em outra coisa.

Nunca reescreva algo em outra linguagem "pra ficar mais rápido" sem ela pedir.
