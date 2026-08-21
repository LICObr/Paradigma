# Aula 03 — Derivação de código a partir da gramática

**Aluno:** Danilo Mori Schuler
**RA:** 24391649-2
**Disciplina:** Paradigmas de Linguagens de Programação

## O que a atividade pede

Pesquisar a gramática formal de uma linguagem real, escolher as regras de
produção necessárias e usar essas regras para derivar, passo a passo, um trecho
de código válido. No caminho, mostrar na prática o que é símbolo terminal,
símbolo não terminal, produção e derivação.

## Linguagem e fonte da gramática

| Item | Valor |
|---|---|
| Linguagem | Python (CPython 3.13) |
| Gramática sintática | <https://docs.python.org/3/reference/grammar.html> |
| Gramática léxica (tokens) | <https://docs.python.org/3/reference/lexical_analysis.html> |
| Arquivo original da gramática | `Grammar/python.gram` no repositório do CPython |
| Notação | PEG (Parsing Expression Grammar), usada pelo Python desde a versão 3.9 |

Escolhi Python porque foi a linguagem sorteada para o grupo na aula 0 e porque a
gramática oficial fica publicada em um único arquivo, fácil de consultar.

### Como ler a notação PEG

Os símbolos que aparecem nas regras usadas aqui:

```
regra: alternativa        define uma regra
e1 e2                     e1 seguido de e2
e1 | e2                   tenta e1; se falhar, tenta e2 (a ordem importa)
[e]                       e é opcional
e+                        e uma ou mais vezes
e*                        e zero ou mais vezes
!e                        só passa se e NÃO casar aqui (não consome nada)
&e                        só passa se e casar aqui (não consome nada)
'texto'                   terminal literal, exatamente esse texto
NAME, NUMBER, NEWLINE     terminais que vêm do analisador léxico
```

Uma diferença importante da PEG para a BNF vista em aula: a barra `|` é uma
escolha **ordenada**. O analisador tenta a primeira alternativa e só passa para
a próxima se a primeira falhar. Por isso o próprio arquivo da gramática avisa que
`assignment` precisa vir antes de `star_expressions` dentro de `simple_stmt`.

## Código escolhido

```python
media = (a + b) / 2
```

Uma linha só, mas ela cobre bastante coisa: atribuição, parênteses, dois
operadores com precedências diferentes, dois nomes e um número. Os parênteses
são o ponto interessante: sem eles, a divisão seria feita antes da soma.

### Tokens do código

Antes de chegar no analisador sintático, o texto vira uma sequência de tokens.
Peguei a lista rodando `python3 -m tokenize`:

| Ordem | Token | Lexema |
|---|---|---|
| 1 | `NAME` | `media` |
| 2 | `'='` | `=` |
| 3 | `'('` | `(` |
| 4 | `NAME` | `a` |
| 5 | `'+'` | `+` |
| 6 | `NAME` | `b` |
| 7 | `')'` | `)` |
| 8 | `'/'` | `/` |
| 9 | `NUMBER` | `2` |
| 10 | `NEWLINE` | fim da linha |
| 11 | `ENDMARKER` | fim do arquivo |

Para o analisador sintático, esses onze tokens são os **terminais**. Ele nunca
olha letra por letra, só recebe a lista pronta.

## Produções selecionadas

Copiei do arquivo oficial só as regras que a derivação usa, tirando as ações em
C que ficam entre chaves no original (elas montam a árvore, não fazem parte da
gramática em si). Onde uma regra tem muitas alternativas, deixei só as que
importam e marquei com `...`.

### Do arquivo até o comando

```
file:         [statements] ENDMARKER
statements:   statement+
statement:    compound_stmt | simple_stmts
simple_stmts: simple_stmt !';' NEWLINE | ...
simple_stmt:  assignment | star_expressions | ...
```

`file` é o símbolo inicial. Um arquivo é uma lista de comandos e um
`ENDMARKER` no final. Um comando simples termina em `NEWLINE`.

### A atribuição

```
assignment:   (star_targets '=')+ annotated_rhs !'=' [TYPE_COMMENT] | ...
annotated_rhs: yield_expr | star_expressions
```

Lê-se: um ou mais "alvo seguido de `=`", depois o lado direito, e não pode
sobrar outro `=`. O `+` é o que permite `a = b = 0` em Python. Aqui usamos só
um alvo.

### Lado esquerdo (o nome `media`)

```
star_targets:          star_target !','
star_target:           target_with_star_atom | ...
target_with_star_atom: star_atom | ...
star_atom:             NAME | ...
```

Quatro regras para chegar em `NAME`. A cadeia é longa porque o lado esquerdo
de uma atribuição pode ser `x.campo`, `lista[0]`, `a, b`. Para um nome simples
tudo colapsa em `NAME`.

### Lado direito (a expressão)

```
star_expressions: star_expression | ...
star_expression:  expression | ...
expression:       disjunction | ...
disjunction:      conjunction | ...
conjunction:      inversion | ...
inversion:        comparison | ...
comparison:       bitwise_or | ...
bitwise_or:       bitwise_xor | ...
bitwise_xor:      bitwise_and | ...
bitwise_and:      shift_expr | ...
shift_expr:       sum | ...
sum:              sum '+' term | sum '-' term | term
term:             term '*' factor | term '/' factor | factor | ...
factor:           power | ...
power:            await_primary | ...
await_primary:    primary | ...
primary:          atom | ...
atom:             NAME | NUMBER | &'(' (tuple | group | genexp) | ...
group:            '(' (yield_expr | named_expression) ')'
named_expression: expression !':=' | ...
```

Essa escada é a forma que a gramática tem de codificar a **precedência** dos
operadores. Cada degrau é um nível: `or`, `and`, `not`, comparação, operadores
bit a bit, deslocamento, soma, multiplicação, sinal, potência, e por fim os
átomos. Quanto mais baixo na escada, mais forte o operador. `sum` fica acima de
`term`, então `*` e `/` são resolvidos antes de `+` e `-`.

Os parênteses entram por `atom`, na alternativa `group`, que abre `(`, aceita
uma expressão inteira de novo e fecha `)`. É assim que o parêntese "reinicia" a
escada e faz a soma ser avaliada antes da divisão.

## Derivação passo a passo

Derivação mais à esquerda: a cada passo troco o não terminal mais à esquerda
pela alternativa escolhida. A forma sentencial atual fica em uma linha e a regra
usada, na coluna ao lado. Terminais vão aparecendo em ordem, da esquerda para a
direita, até sobrar só terminal.

Para a escada de expressões, que é só descer degraus sem escolher nada, agrupei
os passos em um só para não ficar uma lista de trinta linhas iguais. Onde a
forma sentencial termina em `…`, o final da linha (`NEWLINE ENDMARKER`) é o
mesmo do passo anterior e foi omitido só para caber na tabela.

| # | Forma sentencial | Regra aplicada |
|---|---|---|
| 0 | `file` | símbolo inicial |
| 1 | `statements ENDMARKER` | `file → [statements] ENDMARKER` |
| 2 | `statement ENDMARKER` | `statements → statement+` (uma vez) |
| 3 | `simple_stmts ENDMARKER` | `statement → simple_stmts` |
| 4 | `simple_stmt NEWLINE ENDMARKER` | `simple_stmts → simple_stmt !';' NEWLINE` |
| 5 | `assignment NEWLINE ENDMARKER` | `simple_stmt → assignment` |
| 6 | `star_targets '=' annotated_rhs NEWLINE ENDMARKER` | `assignment → (star_targets '=')+ annotated_rhs !'='` (uma vez) |
| 7 | `star_target '=' annotated_rhs NEWLINE ENDMARKER` | `star_targets → star_target !','` |
| 8 | `target_with_star_atom '=' annotated_rhs …` | `star_target → target_with_star_atom` |
| 9 | `star_atom '=' annotated_rhs …` | `target_with_star_atom → star_atom` |
| 10 | `NAME '=' annotated_rhs …` | `star_atom → NAME` |
| 11 | `NAME '=' star_expressions …` | `annotated_rhs → star_expressions` |
| 12 | `NAME '=' star_expression …` | `star_expressions → star_expression` |
| 13 | `NAME '=' expression …` | `star_expression → expression` |
| 14 | `NAME '=' sum …` | descida: `expression → disjunction → conjunction → inversion → comparison → bitwise_or → bitwise_xor → bitwise_and → shift_expr → sum` |
| 15 | `NAME '=' term …` | `sum → term` |
| 16 | `NAME '=' term '/' factor …` | `term → term '/' factor` |
| 17 | `NAME '=' factor '/' factor …` | `term → factor` |
| 18 | `NAME '=' atom '/' factor …` | descida: `factor → power → await_primary → primary → atom` |
| 19 | `NAME '=' group '/' factor …` | `atom → &'(' group` |
| 20 | `NAME '=' '(' named_expression ')' '/' factor …` | `group → '(' named_expression ')'` |
| 21 | `NAME '=' '(' expression ')' '/' factor …` | `named_expression → expression !':='` |
| 22 | `NAME '=' '(' sum ')' '/' factor …` | descida até `sum` (mesma do passo 14) |
| 23 | `NAME '=' '(' sum '+' term ')' '/' factor …` | `sum → sum '+' term` |
| 24 | `NAME '=' '(' term '+' term ')' '/' factor …` | `sum → term` |
| 25 | `NAME '=' '(' atom '+' term ')' '/' factor …` | descida `term → factor → power → await_primary → primary → atom` |
| 26 | `NAME '=' '(' NAME '+' term ')' '/' factor …` | `atom → NAME` |
| 27 | `NAME '=' '(' NAME '+' atom ')' '/' factor …` | descida `term → … → atom` |
| 28 | `NAME '=' '(' NAME '+' NAME ')' '/' factor …` | `atom → NAME` |
| 29 | `NAME '=' '(' NAME '+' NAME ')' '/' atom …` | descida `factor → … → atom` |
| 30 | `NAME '=' '(' NAME '+' NAME ')' '/' NUMBER NEWLINE ENDMARKER` | `atom → NUMBER` |

No passo 30 não sobrou nenhum não terminal. Trocando cada token pelo seu
lexema:

```
NAME  '='  '('  NAME  '+'  NAME  ')'  '/'  NUMBER  NEWLINE  ENDMARKER
media  =    (    a     +    b     )    /    2       ↵        (fim)
```

Que é exatamente `media = (a + b) / 2`.

### Como as regras foram usadas, em palavras

A gramática começa dizendo que um arquivo é uma lista de comandos. Desci por
`statements` e `statement` até `simple_stmt`, e ali escolhi a alternativa
`assignment`, porque o código é uma atribuição. A regra de atribuição pede um
alvo, um `=` e um lado direito. O alvo, depois de quatro regras que existem só
para permitir alvos mais complicados, virou o `NAME` `media`.

O lado direito é uma expressão, e expressão em Python é uma escada de regras
com um nível por operador. Desci a escada inteira sem usar nada até chegar em
`term`, que é o nível da divisão. Ali escolhi `term '/' factor`, gerando os dois
lados do `/`. O lado esquerdo precisava virar `(a + b)`, então desci até
`atom` e escolhi `group`, que abre parêntese, aceita uma expressão completa e
fecha. Dentro do parêntese a escada recomeçou, e dessa vez parei em `sum`
escolhendo `sum '+' term` para gerar o `+`. Cada lado do `+` desceu até `atom`
e virou um `NAME`. O lado direito do `/` desceu até `atom` e virou `NUMBER`.

No fim, cada símbolo da forma sentencial era um token do código, na mesma
ordem em que o analisador léxico os produz. É isso que mostra que o código é
válido na gramática: existe um caminho de produções que sai do símbolo inicial
e chega exatamente nele.

### Dois passos que merecem comentário

**Passo 16, a divisão.** Em `term` existem duas alternativas possíveis:
`term '/' factor` ou só `factor`. Como o código tem um `/`, precisa ser a
primeira. O `term` da esquerda vira o `(a + b)` e o `factor` da direita vira o
`2`.

**Passo 23, a soma dentro do parêntese.** A mesma coisa acontece em `sum`.
Como estamos dentro de um `group`, a expressão recomeçou do topo da escada, e
por isso a soma consegue aparecer "dentro" de um operando da divisão. Sem o
parêntese, o caminho seria `sum → sum '+' term` já no passo 14, e o `2` ficaria
ligado só ao `b`.

### Onde os tokens NAME e NUMBER vêm

Os tokens `NAME` e `NUMBER` são terminais para o parser, mas o analisador léxico
também tem regras para eles. Da documentação de análise léxica:

```
identifier   ::= xid_start xid_continue*
decinteger   ::= nonzerodigit (["_"] digit)* | "0"+ (["_"] "0")*
```

Ou seja: `media`, `a` e `b` são identificadores (começam com letra, seguem com
letras ou dígitos) e `2` é um inteiro decimal.

## Árvore de derivação

Versão resumida, mostrando só os nós que decidem alguma coisa. Os degraus da
escada que só passam adiante ficaram como `…`.

```
file
└── statements
    └── statement
        └── simple_stmts
            ├── simple_stmt
            │   └── assignment
            │       ├── star_targets … star_atom
            │       │   └── NAME "media"
            │       ├── '='
            │       └── annotated_rhs … sum
            │           └── term
            │               ├── term … atom
            │               │   └── group
            │               │       ├── '('
            │               │       ├── named_expression … sum
            │               │       │   ├── sum … atom
            │               │       │   │   └── NAME "a"
            │               │       │   ├── '+'
            │               │       │   └── term … atom
            │               │       │       └── NAME "b"
            │               │       └── ')'
            │               ├── '/'
            │               └── factor … atom
            │                   └── NUMBER "2"
            └── NEWLINE
```

A forma da árvore já mostra o resultado: o `/` está mais perto da raiz que o
`+`, então a divisão é a última operação feita, e ela recebe a soma inteira
como operando da esquerda.

## Terminais e não terminais

**Não terminais usados** (aparecem à esquerda de uma regra e são expandidos):
`file`, `statements`, `statement`, `simple_stmts`, `simple_stmt`,
`assignment`, `annotated_rhs`, `star_targets`, `star_target`,
`target_with_star_atom`, `star_atom`, `star_expressions`, `star_expression`,
`expression`, `disjunction`, `conjunction`, `inversion`, `comparison`,
`bitwise_or`, `bitwise_xor`, `bitwise_and`, `shift_expr`, `sum`, `term`,
`factor`, `power`, `await_primary`, `primary`, `atom`, `group`,
`named_expression`.

**Terminais usados** (nunca são expandidos, são o texto final):
`NAME` (3 vezes), `'='`, `'('`, `'+'`, `')'`, `'/'`, `NUMBER`, `NEWLINE`,
`ENDMARKER`.

**Símbolos que não produzem texto:** os predicados `!';'`, `!','`, `!'='`,
`!':='` e `&'('`. Eles só conferem o que vem a seguir sem consumir nada, por
isso não aparecem na forma sentencial nem na árvore.

## Verificação

Para conferir que a derivação bate com o que o Python realmente faz, rodei o
código com o módulo `ast`, que imprime a árvore montada pelo parser oficial:

```python
import ast
print(ast.dump(ast.parse("media = (a + b) / 2"), indent=2))
```

Saída (resumida):

```
Assign(
  targets=[Name(id='media')],
  value=BinOp(
    left=BinOp(left=Name(id='a'), op=Add(), right=Name(id='b')),
    op=Div(),
    right=Constant(value=2)))
```

É a mesma estrutura da árvore de derivação: uma atribuição cujo valor é uma
divisão, e o lado esquerdo da divisão é a soma. Executando com `a = 7` e
`b = 3`, o resultado é `5.0`.

## Resumo

- A gramática do Python é uma PEG com escolha ordenada, por isso a ordem das
  alternativas faz parte da definição.
- A precedência dos operadores não é uma tabela separada: está codificada na
  escada de regras `sum → term → factor → … → atom`.
- Parênteses funcionam porque `atom` tem uma alternativa `group` que volta ao
  topo da escada.
- A derivação de uma linha simples usa mais de trinta regras, mas a maioria é só
  passagem. As decisões de verdade acontecem em `assignment`, `term`, `sum`,
  `group` e `atom`.

## Referências

- Python Software Foundation. *Full Grammar specification*.
  <https://docs.python.org/3/reference/grammar.html>
- Python Software Foundation. *Lexical analysis*.
  <https://docs.python.org/3/reference/lexical_analysis.html>
- PEP 617, *New PEG parser for CPython*. <https://peps.python.org/pep-0617/>
- SEBESTA, R. W. *Conceitos de Linguagens de Programação*. 11. ed. Porto
  Alegre: Bookman, 2018. Capítulo 3, Descrevendo a sintaxe e a semântica.
