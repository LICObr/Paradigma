# Exercícios Resolvidos — Aula 02

**Tema:** Evolução das Principais Linguagens de Programação
**Referência:** Sebesta, *Concepts of Programming Languages*, Capítulo 2

Resolução de 10 das 20 questões da lista (5 entre as 10 primeiras + 5 entre as 10 últimas).

---

## Questão 1

**A genealogia das linguagens não é uma escada de progresso. Explique essa afirmação e apresente dois fatores históricos que fazem uma linguagem influenciar outra sem necessariamente substituí-la.**

A evolução das linguagens de programação costuma ser representada como uma árvore genealógica ramificada, e não como uma sequência linear em que cada linguagem nova torna a anterior obsoleta. Linguagens contemporâneas coexistem, atendem domínios diferentes e influenciam-se mutuamente sem que uma substitua a outra — Fortran continua em uso em computação científica décadas depois de linguagens funcionais terem introduzido ideias como recursão e funções de primeira classe, sem que isso tenha eliminado seu uso.

Dois fatores históricos que explicam esse padrão:

1. **Domínio de aplicação distinto** — cada linguagem nasce para resolver um tipo de problema (científico, comercial, de IA, de sistemas) e permanece relevante em seu nicho mesmo quando surgem linguagens voltadas a outros domínios. COBOL sobrevive no processamento comercial, Fortran na computação numérica.
2. **Restrições de hardware e contexto histórico** — cada linguagem reflete as limitações tecnológicas de sua época (memória, velocidade de processamento). Uma linguagem posterior pode reaproveitar ideias de uma anterior (a estrutura de blocos de ALGOL é usada por C, Pascal e Java) sem eliminar o uso da linguagem original, pois código legado em produção e o custo de reescrita e retreinamento de equipes tornam a substituição total inviável.

---

## Questão 4

**Explique por que o projeto Fortran precisou convencer programadores de que código traduzido podia competir com código de máquina escrito à mão. Relacione desempenho, custo de programação e adoção.**

Em meados dos anos 1950, praticamente todo o código era escrito diretamente em linguagem de montagem, pois os programadores acreditavam — com razão, para os padrões da época — que apenas código escrito manualmente exploraria plenamente a arquitetura da máquina, evitando desperdício de memória e ciclos de processamento, recursos extremamente escassos nos primeiros computadores (como o IBM 704). Um compilador que gerasse código ineficiente aumentaria o custo de execução dos programas, o que, na época em que o tempo de máquina era alugado por hora, representava um argumento econômico decisivo contra as linguagens de alto nível.

A equipe de John Backus sabia que o Fortran só seria adotado se o compilador gerasse código objeto comparável em eficiência ao de um programador experiente em assembly. Por isso, grande parte do esforço de desenvolvimento (cerca de três anos, muito além do previsto) foi dedicada à otimização do compilador, não à definição da linguagem. Esse sucesso — o compilador de fato gerava código eficiente — permitiu provar que a produtividade de programação (escrever mais rápido, com menos erros, em nível mais alto de abstração) podia ser obtida sem sacrificar desempenho, o que abriu caminho para a aceitação de linguagens de alto nível em geral.

---

## Questão 6

**Avalie três contribuições de ALGOL 60 que ultrapassaram sua adoção comercial. Por que uma linguagem pode ser muito influente sem dominar o mercado?**

ALGOL 60 nunca alcançou grande adoção comercial (foi ofuscado por Fortran e COBOL e sofreu com a ausência de uma estrutura de E/S padronizada e apoio fraco da indústria), mas é considerada uma das linguagens mais influentes da história por introduzir conceitos estruturais que se tornaram padrão em praticamente todas as linguagens imperativas posteriores:

1. **Notação BNF (Backus-Naur Form)** para definição formal de sintaxe — pela primeira vez uma linguagem teve sua gramática descrita de forma rigorosa e não ambígua, fundando o campo de definição formal de linguagens.
2. **Estrutura de blocos** (`begin...end`) com escopo léxico e variáveis locais — base do escopo estático usado hoje em C, Pascal, Java etc.
3. **Recursão e estruturas de controle estruturadas** (`if-then-else`, `for`) — uma das primeiras linguagens a suportar chamadas recursivas explicitamente e a reduzir o uso extensivo de `goto`.

Isso mostra que influência não se mede apenas por adoção comercial: uma linguagem pode funcionar como um "laboratório de ideias" cujo impacto se propaga por meio de outras linguagens que adotam seus conceitos — ALGOL influenciou diretamente Pascal, C e Simula, e indiretamente quase toda linguagem estruturada moderna.

---

## Questão 7

**COBOL foi desenhada para processamento comercial. Mostre como domínio e público influenciaram sua legibilidade, seus registros e sua relação com FLOW-MATIC.**

COBOL (1959–60) foi desenvolvida pelo comitê CODASYL sob forte influência do Departamento de Defesa dos EUA, tendo como base direta o FLOW-MATIC, linguagem criada por Grace Hopper para a Remington Rand/Univac especificamente para processamento de dados comerciais (folha de pagamento, faturamento, controle de estoque).

O domínio comercial trouxe exigências diferentes das da computação científica: grandes volumes de dados estruturados (registros com múltiplos campos, como nome, endereço, valor), pouca computação matemática complexa, e programas que precisam ser lidos por gerentes e auditores não necessariamente programadores. Isso levou a decisões de projeto voltadas à legibilidade: sintaxe verbosa e próxima do inglês (`ADD FIELD-A TO FIELD-B GIVING FIELD-C`) e divisão do programa em quatro *divisions* (IDENTIFICATION, ENVIRONMENT, DATA, PROCEDURE), que separam claramente descrição de dados e lógica.

A herança do FLOW-MATIC aparece diretamente na estrutura de registros do COBOL (`record`), que descreve hierarquicamente campos de dados de tamanho fixo e tipo definido (cláusulas `PICTURE`) — recurso essencial para representar formulários comerciais reais, que o FLOW-MATIC já esboçava e o COBOL formalizou e expandiu. Assim, o público-alvo (empresas, não cientistas) e o domínio (dados estruturados e volumosos) moldaram tanto a legibilidade extrema da sintaxe quanto o subsistema de registros do COBOL.

---

## Questão 9

**APL, SNOBOL e SIMULA 67 seguiram direções distintas. Associe cada linguagem ao seu foco e identifique uma contribuição duradoura de cada uma.**

- **APL** (Kenneth Iverson, início dos anos 1960) — concebida originalmente como notação matemática para descrever algoritmos, depois implementada como linguagem interpretada. Foco: manipulação compacta de arrays e vetores multidimensionais por meio de operadores matemáticos especiais. **Contribuição duradoura:** operadores de array de alto nível (operações vetoriais aplicadas sem laços explícitos), ideia que reaparece hoje em bibliotecas como NumPy e em linguagens como R e MATLAB.

- **SNOBOL** (1962, Bell Labs) — voltada para processamento de strings e reconhecimento de padrões em texto, muito usada antes da popularização de expressões regulares embutidas em linguagens de propósito geral. **Contribuição duradoura:** mecanismos sofisticados de casamento de padrões em strings, que influenciaram o desenvolvimento de expressões regulares e de linguagens de processamento de texto como Perl.

- **SIMULA 67** (Ole-Johan Dahl e Kristen Nygaard) — criada para simulação de eventos discretos, introduziu o conceito de *classe* como estrutura que agrupa dados e procedimentos, junto com herança. **Contribuição duradoura:** é reconhecida como a origem da programação orientada a objetos, influenciando diretamente Smalltalk e, por extensão, C++, Java e praticamente toda linguagem OO moderna.

Esses três exemplos mostram como direções de pesquisa aparentemente isoladas (matemática/array, texto, simulação) geraram mecanismos ou paradigmas que se tornaram centrais décadas depois.

---

## Questão 11

**Construa uma cadeia de influência que passe por ALGOL, Pascal e C. Depois contraste essa linhagem imperativa com a proposta declarativa de Prolog.**

**Cadeia de influência imperativa:**

**ALGOL 60** introduziu estrutura de blocos, tipagem e estruturas de controle estruturadas (substituindo o uso extensivo de `goto`). **→ Pascal** (Niklaus Wirth, 1971): Wirth discordou da complexidade excessiva de ALGOL 68 e criou Pascal preservando a estrutura de blocos e controle de ALGOL, mas com um sistema de tipos mais rico (enumerados, registros, subranges), voltado ao ensino de programação estruturada. **→ C** (Dennis Ritchie, início dos anos 1970): herdou a estrutura de blocos e controle de fluxo dessa linhagem (via B e BCPL), mas foi desenhada com foco em eficiência e acesso de baixo nível ao hardware (ponteiros, aritmética de endereços), pois seu propósito era reescrever o UNIX em uma linguagem portátil.

Essa linhagem é fundamentalmente **imperativa**: o programa é uma sequência de comandos que alteram o estado da máquina, e o programador especifica explicitamente o "como" resolver o problema, passo a passo.

**Contraste com Prolog:** Prolog (Alain Colmerauer, 1972) representa uma ruptura de paradigma. Um programa Prolog é uma base de fatos e regras lógicas; o programador descreve "o que" é verdadeiro sobre o domínio, e a execução consiste no interpretador buscar, por resolução e unificação, valores que satisfaçam uma consulta. Não há atribuição de variáveis nem fluxo de controle definido pelo programador — a ordem de execução é decidida pelo motor de inferência. Enquanto ALGOL-Pascal-C refinam "como" controlar o estado da máquina, Prolog desloca o foco para a especificação declarativa do problema.

---

## Questão 13

**Ada resultou de requisitos e projeto em grande escala. Analise como confiabilidade, tipos, pacotes e concorrência se relacionam ao domínio de sistemas críticos.**

Ada foi encomendada pelo Departamento de Defesa dos EUA para substituir as centenas de linguagens então usadas em sistemas embarcados militares. O processo foi incomum: o DoD publicou requisitos formais (documentos Strawman a Steelman) e promoveu uma competição pública entre propostas, da qual o design de Jean Ichbiah foi escolhido. O domínio-alvo eram sistemas embarcados e de missão crítica (radares, controle de mísseis, sistemas de bordo), que precisam funcionar de forma confiável por longos períodos, muitas vezes sem intervenção humana, rodando em hardware com múltiplos processos concorrentes.

- **Confiabilidade** — como falhas em software militar podem ter consequências catastróficas, Ada foi projetada com verificação forte em tempo de compilação, checagem de faixa de valores em tempo de execução e tratamento de exceções embutido na linguagem, para detectar erros o mais cedo possível.
- **Tipos** — sistema de tipos estático e forte, com tipos derivados e subtipos que impedem, por exemplo, que uma variável de "velocidade" seja somada a uma de "temperatura", reduzindo erros lógicos comuns em sistemas complexos.
- **Pacotes** — permitem encapsulamento e ocultamento de informação, essencial para que equipes grandes (múltiplos contratantes militares) desenvolvessem módulos independentes de um mesmo sistema de forma organizada.
- **Concorrência** — Ada introduziu *tasks* como unidade nativa da linguagem para expressar processos concorrentes com sincronização (*rendezvous*), refletindo a necessidade de controlar múltiplos sensores/atuadores operando simultaneamente em sistemas embarcados de tempo real, sem depender de bibliotecas externas do sistema operacional.

Cada uma dessas características foi resposta direta a requisitos documentados de um domínio de sistemas críticos de grande escala e longa vida útil.

---

## Questão 14

**Compare o papel dos objetos em Smalltalk, C++ e Java. Inclua na resposta o compromisso de C++ com C e a estratégia de portabilidade de Java.**

- **Smalltalk** (Alan Kay e equipe, Xerox PARC, anos 1970) consolidou e popularizou a orientação a objetos "pura", herdada de Simula 67. Nela, absolutamente tudo é um objeto (incluindo inteiros e classes), e toda computação ocorre exclusivamente por troca de mensagens entre objetos — até um `if` é implementado como envio de mensagem a um objeto booleano. Objetos são o princípio organizador único da linguagem.

- **C++** (Bjarne Stroustrup, início dos anos 1980) nasceu como "C com Classes": um acréscimo de recursos orientados a objetos (inspirados em Simula) sobre a linguagem C já existente. O objetivo era permitir OO sem abrir mão da eficiência e da compatibilidade com o vasto código C já em uso — por isso C++ é uma linguagem híbrida, permitindo tanto objetos quanto programação puramente procedural ao estilo C, com ponteiros e gerenciamento manual de memória. Esse compromisso trouxe vantagens de adoção e desempenho, às custas de maior complexidade e da possibilidade de escapar da disciplina orientada a objetos.

- **Java** (Sun Microsystems, 1995) buscou uma orientação a objetos mais disciplinada que C++ (sem herança múltipla de implementação, sem ponteiros aritméticos, com coleta automática de lixo). Sua decisão de projeto mais marcante foi a estratégia de portabilidade: em vez de compilar para código de máquina nativo, Java compila para *bytecode* executado por uma máquina virtual (JVM), seguindo o lema "write once, run anywhere" — o mesmo bytecode roda em qualquer plataforma com JVM, o que foi decisivo para sua adoção na era da Web.

Em síntese: Smalltalk trata objetos como único mecanismo computacional; C++ soma objetos a C mantendo compatibilidade e desempenho às custas de pureza; Java reforça a disciplina orientada a objetos e resolve a portabilidade via máquina virtual, sacrificando parte do desempenho nativo em troca de portabilidade.

---

## Questão 16

**Compare Perl, JavaScript, PHP, Python, Ruby e Lua usando três eixos: domínio inicial, estruturas de dados e estratégia de implementação. Evite concluir que todas são iguais por serem chamadas de scripting.**

Embora agrupadas sob o rótulo "linguagens de script", tiveram origens e focos bem diferentes:

| Linguagem | Domínio inicial | Estrutura de dados central | Implementação |
|---|---|---|---|
| **Perl** (1987) | Administração de sistemas UNIX e processamento de texto/relatórios | Arrays e *hashes* (arrays associativos), com regex nativo forte | Interpretada, com compilação interna para forma intermediária |
| **JavaScript** (1995) | Interatividade em páginas web no navegador (cliente) | Objetos dinâmicos com herança prototypal, arrays | Interpretada/JIT dentro do motor do navegador (V8, SpiderMonkey) |
| **PHP** (1994) | Geração dinâmica de páginas HTML no servidor | Arrays associativos "ordenados", usados como listas ou mapas | Interpretada, embutida no fluxo HTML, executada por requisição |
| **Python** (1991) | Propósito geral, legibilidade e produtividade | Listas, dicionários, tuplas e conjuntos como cidadãos de primeira classe | Interpretada (CPython compila para bytecode executado por VM) |
| **Ruby** (1995) | Expressividade e "felicidade do programador" (combina Perl + Smalltalk) | Arrays e hashes, com modelo de objetos totalmente puro | Interpretada (posteriormente com VMs como YARV) |
| **Lua** (1993) | Linguagem de extensão embutida em aplicações hospedeiras (jogos, sistemas industriais) | Uma única estrutura universal: a *table* | Interpretador leve embutido via API C, hospedado dentro de outro programa |

Os domínios variam de administração de sistemas (Perl) a Web cliente (JS), Web servidor (PHP), propósito geral (Python), pureza orientada a objetos (Ruby) e extensão embutida (Lua); as estruturas de dados vão de hashes genéricos a uma única estrutura universal (Lua) ou objetos puros (Ruby); as implementações vão de interpretação simples a JIT altamente otimizado (JS) ou interpretador ultraleve embutível (Lua). Chamar todas de "linguagens de script" esconde diferenças de projeto tão grandes quanto as existentes entre linguagens compiladas tradicionais.

---

## Questão 20

**Estudo de caso: uma equipe precisa escolher tecnologias para cálculo científico, regras declarativas, aplicação Web interativa e firmware restrito. Proponha famílias de linguagens, justifique historicamente cada escolha e explicite dois trade-offs.**

**1. Cálculo científico → família Fortran** (Fortran moderno, ou C/C++/Python com bibliotecas numéricas)
*Justificativa histórica:* Fortran foi criado especificamente para computação numérica de alto desempenho e, décadas depois, ainda é referência em computação científica de alto desempenho porque seus compiladores são extremamente otimizados para laços numéricos e arrays — a mesma motivação de eficiência de sua criação em 1957.
*Trade-offs:* (a) desempenho bruto altíssimo vs. produtividade e legibilidade menores; (b) grande base de código legado científico confiável vs. dificuldade de encontrar novos desenvolvedores treinados na linguagem.

**2. Regras declarativas → família Prolog** (linguagens lógicas / motores de regras)
*Justificativa histórica:* Prolog nasceu para expressar problemas como bases de fatos e regras lógicas resolvidas por inferência, sendo a escolha natural quando o problema é melhor descrito como "o que é verdade" do que como uma sequência de passos.
*Trade-offs:* (a) expressividade natural para problemas de busca/restrição vs. dificuldade de prever a ordem e o desempenho da execução (o motor de inferência decide o "como"); (b) código compacto para regras complexas vs. curva de aprendizado alta para equipes acostumadas ao paradigma imperativo.

**3. Aplicação Web interativa → família JavaScript/TypeScript** (cliente) + linguagem de script de servidor
*Justificativa histórica:* JavaScript foi criada especificamente para rodar embutida no navegador e é, hoje, a única linguagem nativamente suportada por todos os navegadores para interatividade no cliente, papel que assumiu desde 1995.
*Trade-offs:* (a) execução universal em qualquer navegador vs. tipagem dinâmica fraca, historicamente fonte de erros em aplicações grandes (mitigado parcialmente por TypeScript); (b) enorme ecossistema de bibliotecas vs. fragmentação e mudança rápida do ecossistema.

**4. Firmware restrito → família C** (ou Ada, para sistemas críticos)
*Justificativa histórica:* C foi desenhada desde o início para acesso de baixo nível ao hardware com overhead mínimo de runtime, papel que assumiu ao reescrever o UNIX e que mantém como padrão para sistemas embarcados; Ada é a alternativa quando confiabilidade crítica pesa mais que familiaridade da equipe, dado seu histórico em sistemas de missão crítica.
*Trade-offs:* (a) C: controle total e desempenho previsível vs. ausência de proteções de memória, exigindo disciplina manual; (b) Ada: forte verificação em tempo de compilação vs. ecossistema e mão de obra disponível muito menores que C.

**Conclusão:** a escolha reflete o mesmo padrão histórico do capítulo — cada família de linguagens foi moldada pelas restrições e pelo domínio para o qual nasceu, e a equipe deve priorizar esse alinhamento histórico de propósito, aceitando conscientemente os trade-offs de cada uma.
