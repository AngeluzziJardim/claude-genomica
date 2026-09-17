# Kit de guardrails — Claude Code para genômica populacional

Um conjunto de regras e comandos que fazem o Claude Code **parar de divagar**: em vez de "faz o que puder para chegar em X", ele passa a declarar o contrato, rodar a versão pequena, conferir, entregar X e parar — com um orçamento de **1 desvio**, depois do qual é obrigado a perguntar em vez de tentar mais cinco coisas.

São arquivos de texto. Não instala nada, não roda nada, não depende de plugin.

---

## Como instalar

Leva 2 minutos. Não precisa instalar programa nenhum — são arquivos de texto que o Claude Code lê sozinho.

### Passo 1 — baixar

```bash
git clone https://github.com/AngeluzziJardim/claude-genomica.git
```

(Sem Git na máquina? No GitHub: botão verde **Code** → **Download ZIP** → descompactar.)

### Passo 2 — copiar

Copie o **conteúdo** da pasta baixada para dentro da pasta do seu projeto de análise. Ao final, ele deve ficar assim:

```
meu-projeto/
├── CLAUDE.md          ← as regras (o Claude lê automaticamente)
├── PROJETO.md         ← o contexto do seu projeto (você preenche com /setup-projeto)
├── README.md
├── docs/
│   ├── 00-como-pedir.md     ← este é para VOCÊ ler
│   └── regras/
└── .claude/
    └── skills/
```

Se você já tem um `CLAUDE.md` no projeto, não sobrescreva: cole o conteúdo do novo no topo do seu.

### Passo 3 — abrir o Claude Code nessa pasta

No terminal, dentro da pasta do projeto:

```bash
claude
```

O `CLAUDE.md` entra em contexto sozinho, toda sessão. Não precisa fazer nada.

### Passo 4 — rodar a configuração inicial, uma vez

Na primeira conversa, digite:

```
/setup-projeto
```

Ele faz umas 9 perguntas objetivas sobre seus dados, ferramentas e objetivo, confere o que está instalado na máquina e escreve o `PROJETO.md`. A partir daí você não repete contexto.

### Passo 5 — ler o `docs/00-como-pedir.md`

São 3 minutos. É a parte do problema que está do seu lado do teclado: como formular o pedido para ele não se espalhar. Vale mais do que o resto do kit.

---

### Verificar se funcionou

Digite `/` e veja se aparecem `analise`, `checar-dados`, `explicar`, `setup-projeto`.

Se não aparecerem: confirme que os arquivos estão em `.claude/skills/<nome>/SKILL.md` (com o ponto no começo de `.claude`) e reinicie o Claude Code.

---

## O que este kit faz, em uma frase

Ele troca "faz o que puder para chegar em X" por **"declare o contrato, rode a versão pequena, confira, entregue X, e pare"** — com um orçamento de 1 desvio, depois do qual ele é obrigado a parar e perguntar em vez de tentar mais cinco coisas.

## Ajustar com o tempo

Os arquivos são seus. Quando ele fizer algo errado, acrescente a regra que teria evitado — em `CLAUDE.md` se valer sempre, no doc específico em `docs/regras/` se for de um tema só. Regra curta e imperativa funciona melhor que parágrafo explicativo.
