---
name: setup-projeto
description: Primeira configuração do projeto — faz um punhado de perguntas objetivas sobre dados, organismo, stack e objetivo, e escreve o PROJETO.md que todas as outras skills leem depois. Use quando a pessoa disser "/setup-projeto", "configura o projeto", "primeira vez aqui", ou quando PROJETO.md não existir ou estiver com os campos em branco.
---

# /setup-projeto — rodar uma vez, no começo

Objetivo: preencher `PROJETO.md` para que, daqui em diante, ninguém precise repetir contexto.

## Como perguntar

Faça as perguntas **de uma vez só, numeradas**, e aceite respostas curtas. Não faça entrevista de dez rodadas.

1. Organismo e o que é o dataset, em uma frase (ex: "150 indivíduos de Drosophila, 6 populações do litoral").
2. Qual é a **pergunta principal** do projeto?
3. Onde estão os dados e em que formato? (caminho + VCF / PLINK / BAM / genotype likelihoods)
4. Build ou genoma de referência. Tipo de dado: cobertura alta, baixa cobertura, RADseq, exoma, array?
5. Onde roda: PC local, WSL, servidor ou cluster (qual gerenciador de fila)?
6. Ambiente: conda/mamba? Qual env? Quais ferramentas já estão instaladas?
7. Tem arquivo de metadados de amostra? Caminho e colunas.
8. Que análises já rodaram (para não refazer) e quais estão previstas.
9. Alguma ferramenta obrigatória do laboratório — ou proibida?

Depois, **confira o ambiente de verdade** (sem instalar nada):

```bash
which bcftools plink2 plink vcftools admixture samtools R python
bcftools --version | head -1
plink2 --version
```

Registre o que existe. Se faltar algo, apenas anote como "faltando" — não instale.

## O que escrever

Crie `PROJETO.md` com estas seções, preenchidas com as respostas e com o que você conferiu:

```markdown
# PROJETO

## O que é
## Pergunta principal
## Dados        (caminho, formato, N amostras, M variantes, build, tipo de cobertura)
## Metadados    (caminho, colunas, chave de cruzamento)
## Ambiente     (máquina, env, ferramentas + versões conferidas)
## Ferramentas obrigatórias / proibidas deste laboratório
## Já foi feito (não refazer)
## Próximas análises
## Convenções deste projeto  (o que diverge do CLAUDE.md padrão)
```

Crie também as pastas que faltarem: `data/raw`, `data/interim`, `scripts`, `results`, `logs`.

Ao terminar, mostre o `PROJETO.md` e peça para ela corrigir o que estiver errado. **Não comece nenhuma análise.**
