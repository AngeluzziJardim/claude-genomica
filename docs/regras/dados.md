# Regras de dados

## Imutabilidade

- `data/raw/` é **somente leitura**. Nenhum comando escreve, renomeia, comprime, reordena ou "conserta" nada lá dentro.
- Saída sempre em `data/interim/` (intermediário) ou `results/<data>-<slug>/` (final).
- Nunca sobrescrever um arquivo que já existe. Se o nome colide, **pare e pergunte**.

## Declarar o que é o dado, antes de usar

Toda análise começa declarando, em uma linha:

```
<arquivo> — <formato> — build <hg19|hg38|T2T|outro> — N amostras — M variantes — populações: <lista>
```

Se algum desses você não sabe, **descubra com um comando barato** (abaixo) ou pergunte. Não assuma.

## Checagens baratas antes de confiar no arquivo

```bash
bcftools query -l file.vcf.gz | wc -l        # nº de amostras
bcftools index -n file.vcf.gz                # nº de variantes (precisa de índice)
bcftools view -h file.vcf.gz | tail -20      # header: build, contigs, filtros já aplicados
zcat file.vcf.gz | grep -v '^#' | head -3    # como são os IDs e os cromossomos de verdade
wc -l file.fam                               # PLINK: nº de amostras
wc -l file.bim                               # PLINK: nº de variantes
```

Se o número **não bate** com o que ela disse esperar: pare, mostre os dois números, pergunte. Não siga "ajustando".

## Armadilhas clássicas de genômica populacional — nunca resolver em silêncio

Cada uma destas, quando aparecer, é **motivo de aviso**, não de conserto automático:

| Armadilha | O que fazer |
|---|---|
| `chr1` vs `1` (prefixo de cromossomo) | avise e proponha o `--rename-chrs`; não renomeie por conta |
| Builds diferentes entre arquivos (hg19 × hg38) | **pare**. Liftover é decisão dela, nunca automática |
| Strand flip / alelo ambíguo (A/T, C/G) | liste quantos são, pergunte a política antes de excluir ou flipar |
| Ordem de amostras diferente entre VCF e metadados | **nunca** case por posição. Case por `sample_id`. Se sobrar/faltar, reporte |
| Amostras duplicadas ou relacionadas (parentesco) | reporte o achado; remover é decisão dela |
| Sítios invariantes descartados | importa muito para π e dxy — use `pixy` e avise |
| Missingness alto em um subgrupo | reporte por população, não só global — vira falso sinal de estrutura |
| Filtro implícito da ferramenta (MAF padrão do PLINK, p.ex.) | escreva a flag explicitamente, sempre |

## Metadados

- Um único arquivo de metadados por projeto (`data/raw/samples.tsv`), com no mínimo: `sample_id`, `population`, `region`, `sex`.
- Todo cruzamento é por `sample_id`. Se um ID não existe dos dois lados, ele entra num relatório de descasamento — não é descartado em silêncio.

## Proveniência

Toda saída final vem acompanhada, no `logs/`, de: comando exato, versão da ferramenta (`--version`), data, arquivo de entrada, e o seed quando houver aleatoriedade.
