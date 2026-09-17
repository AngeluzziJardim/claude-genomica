# Como rodar uma análise (o fluxo com freios)

O objetivo é chegar no X pedido pelo caminho mais curto defensável. Não é explorar o dataset.

## Os 6 passos

### 1. Contrato (sempre, antes de qualquer comando)
```
PERGUNTA: <a pergunta biológica em uma frase>
ENTRADA:  <arquivo exato> (N amostras, M variantes, build)
SAÍDA:    <arquivo exato> + <figura, se pedida>
MÉTODO:   <ferramenta + flags principais>
NÃO FAZ:  <o que fica de fora de propósito>
```
Qualquer linha com "acho que" → **pergunte antes de rodar**.

### 2. Plano — só se forem mais de 3 passos
Liste os passos numerados, em uma linha cada, e **espere o ok** dela.
3 passos ou menos: não peça aprovação, faça.

### 3. Subconjunto
Rode em uma fatia (1 cromossomo, ou `--thin-count 1000`, ou 20 amostras).
Olhe a saída de verdade (`head`, `wc -l`, faixa dos valores). Faz sentido? Só então siga.

### 4. Dataset completo
Antes: **estime o tempo**. Se passar de ~10 minutos ou for pesado no cluster, avise e confirme antes de disparar.
Um script por etapa, numerado, em `scripts/` (`03-pca.sh`), com log em `logs/`.
Seed fixo sempre que houver aleatoriedade (ADMIXTURE, bootstrap, subamostragem).

### 5. Checagem de sanidade — obrigatória, antes de reportar
Olhe o arquivo de saída. Pergunte-se:
- O número de linhas/amostras é o esperado?
- Os valores estão na faixa possível? (Fst entre 0 e 1; π pequeno e positivo; PC1 explicando mais que PC2; soma das ancestralidades = 1)
- Tem `NA`/`nan`/`inf`? Quantos? Por quê?
- O resultado mudaria se eu tivesse trocado as populações de lugar? (sinal de erro de rótulo)

Se algo não bate: isso **não** é um resultado. É um problema. Reporte como problema.

### 6. Relatório
Formato de [`relatorio.md`](relatorio.md). Curto.

---

## Quando quebra

1. **Leia a mensagem de erro inteira.** Não a primeira linha — a inteira. Bioinformática coloca a causa real no meio.
2. **Uma hipótese, um teste.** Escreva a hipótese antes de testar.
3. Não funcionou? **Pare.** Reporte assim:
   ```
   TENTEI:     <comando>
   ERRO:       <mensagem literal, sem parafrasear>
   HIPÓTESE:   <o que eu acho que é>
   OPÇÕES:     A) ...  B) ...
   ```
   E espere.

**Proibido quando quebra:** trocar de ferramenta, inventar um workaround em Python, mudar os dados de entrada, afrouxar filtro, ou tentar a terceira coisa. O orçamento de desvio é 1.

---

## Sinais de que você está divergindo (pare na hora)

- Você abriu um arquivo que não está no contrato.
- Você está rodando a terceira ferramenta seguida sem nada ter dado certo.
- Você está escrevendo uma função "genérica pra reaproveitar depois".
- Você está fazendo QC quando pediram uma estatística.
- Você está gerando um gráfico que ninguém pediu.
- Você está há mais de ~10 ações sem falar com ela.
- Você pensou "já que estou aqui, aproveito e...".

Em qualquer um destes: pare, diga onde está, entregue o que já tem, pergunte.

---

## Proibições específicas

- Não criar Snakemake/Nextflow/Makefile orquestrador sem pedido explícito com esse nome.
- Não refatorar script existente que funciona.
- Não mudar parâmetro de filtro sem declarar qual e por quê.
- Não escrever teste automatizado em projeto de análise, a menos que peçam.
- Não "arrumar" caminho de arquivo hardcoded de outro script.
