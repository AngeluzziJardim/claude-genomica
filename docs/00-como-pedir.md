# Como pedir (este doc é para você, não para o Claude)

Metade da divergência do Claude nasce no pedido. As regras deste repositório seguram a outra metade. Este doc é a sua metade — e é a que dá mais resultado por minuto investido.

## A fórmula de um pedido bom

> **[o que] + [com qual arquivo] + [por qual método, se você já sabe] + [o que NÃO fazer]**

Comparando:

| Pedido ruim | Pedido bom |
|---|---|
| "Analisa a estrutura populacional desse dataset" | "Roda PCA no `data/raw/coast.vcf.gz` com plink2, depois de poda de LD. Só o PCA — não roda ADMIXTURE ainda." |
| "Vê se tem seleção nessas populações" | "Calcula Fst por janela de 50kb entre POP_A e POP_B com vcftools e me dá as 20 janelas mais altas. Sem gráfico." |
| "Melhora esse script" | "No `scripts/03-pca.sh`, troca o filtro de MAF de 0.01 para 0.05. Só isso." |
| "Por que meu resultado deu estranho?" | "O Fst entre POP_A e POP_B deu 0.42, esperava ~0.05. Antes de rodar qualquer coisa, me dá 3 hipóteses do que pode ter acontecido." |

O `+ [o que NÃO fazer]` é o item que mais economiza tempo. É a frase que impede as dez análises extras.

## Quatro frases que funcionam como freio

Cole quando ele começar a se espalhar:

- **"Só o que eu pedi. Pare quando entregar."**
- **"Antes de rodar, me mostra o contrato (entrada, saída, comando)."**
- **"Não tenta outra ferramenta. Me mostra o erro e para."**
- **"Para. O que você já tem? Me resume em 5 linhas."**

## Use as skills — elas já carregam as regras

| Você quer | Digite |
|---|---|
| Rodar uma análise | `/analise calcula Fst entre POP_A e POP_B no coast.vcf.gz` |
| Conferir um arquivo antes de confiar | `/checar-dados data/raw/coast.vcf.gz` |
| Entender um método ou um resultado | `/explicar por que o PC1 separou por sexo` |
| Configurar o projeto (uma vez só) | `/setup-projeto` |

## Três hábitos que mudam o jogo

**1. Uma tarefa por conversa.** Conversa longa é conversa que dispersa: ele carrega o contexto de tudo que já tentou. Terminou uma análise? `/clear` e começa a próxima limpa.

**2. Aprove o plano antes de gastar dados.** Quando a tarefa for grande, peça primeiro: *"me dá o plano em passos numerados, não roda nada"*. Você lê em 20 segundos e corta os passos que não quer. É aí que se evita a divergência — não depois.

**3. Peça para conferir, não para concluir.** *"Abre o arquivo de saída e me diz o que tem nele"* vale mais do que *"o que esse resultado significa"*. Modelo que não olhou o arquivo inventa a interpretação com convicção.

## O que não vale a pena pedir

- "Faz um pipeline pra isso" — a menos que você realmente queira um pipeline. Do contrário você ganha Snakemake onde precisava de três linhas.
- "Faz do jeito mais robusto" — vira engenharia, não ciência. Peça o resultado; robustez você pede depois, onde importar.
- "Roda tudo e me avisa" — é exatamente o modo em que ele se perde. Fatie.

## Quando ele errar, corrija a regra, não só o resultado

Se o Claude fez algo que não devia e você teve que corrigir na mão: abra o `CLAUDE.md` (ou o doc da regra) e escreva a regra que teria evitado aquilo. Uma linha basta. O arquivo vai ficando mais afiado a cada semana — é assim que este kit foi construído.
