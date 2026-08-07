<h1 align="center">🐍 Tabuada em Python</h1>

<p align="center">
  <b>Paradigmas de Programação — Atividade 1</b><br>
  Programa de geração de tabuada + pesquisa sobre a linguagem e o mercado de trabalho
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.x-3776AB?style=for-the-badge&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/Paradigma-Multiparadigma-yellow?style=for-the-badge">
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge">
</p>

---

## 📌 Sobre o projeto

Este repositório contém a primeira atividade da disciplina de **Paradigmas de Programação**.
O programa desenvolvido gera a **tabuada de um número informado pelo usuário**, aplicando
conceitos básicos de entrada de dados, conversão de tipos e estruturas de repetição.

Além da parte prática, o trabalho traz uma pesquisa sobre os **paradigmas suportados pelo
Python**, sua **relação com outras linguagens** e o **mercado de trabalho** da linguagem no Brasil.

---

## 💻 O Código

```python
numero = int(input("Digite um número: "))

print(f"\nTabuada do {numero}")

for i in range(1, 11):
    print(f"{numero} x {i} = {numero * i}")
```

### ▶️ Exemplo de execução

```
Digite um número: 7

Tabuada do 7
7 x 1 = 7
7 x 2 = 14
7 x 3 = 21
7 x 4 = 28
7 x 5 = 35
7 x 6 = 42
7 x 7 = 49
7 x 8 = 56
7 x 9 = 63
7 x 10 = 70
```

### 🧠 Conceitos aplicados

| Conceito | Onde aparece |
|---|---|
| Entrada de dados | `input()` |
| Conversão de tipo (*casting*) | `int()` |
| Saída formatada (*f-string*) | `f"{numero} x {i} = {numero * i}"` |
| Estrutura de repetição | `for i in range(1, 11)` |
| Operador aritmético | `numero * i` |

> 💡 O `range(1, 11)` gera os números de **1 a 10** — o limite final é sempre exclusivo em Python.

### ⚙️ Como executar

```bash
python3 Main.py
```

---

<br>

# 🐍 A Linguagem Python

## Características gerais

| Característica | Descrição |
|---|---|
| **Interpretada** | Executa linha a linha, sem compilação prévia |
| **Alto nível** | Não exige gerenciamento de memória ou ponteiros |
| **Tipagem dinâmica** | Não é necessário declarar o tipo (`x = 10`) |
| **Tipagem forte** | Não converte tipos automaticamente (`"5" + 3` gera erro) |
| **Multiplataforma** | Roda em Windows, Linux e macOS |
| **Multiparadigma** | Suporta diferentes estilos de programação |
| **Gerenciamento automático de memória** | Possui *Garbage Collector* |
| **Indentação obrigatória** | O recuo faz parte da sintaxe — não utiliza `{ }` |

📅 Criada por **Guido van Rossum** em **1991** · Atualmente na versão **3.x** ·
Mantida pela *Python Software Foundation*

---

## 🔀 Paradigmas Suportados

Python é uma linguagem **multiparadigma** — ela não obriga o programador a seguir um único
estilo, permitindo **escolher e até combinar** abordagens no mesmo projeto.

<br>

### 1️⃣ Procedural / Imperativo
Descreve **como** resolver o problema, por meio de uma sequência de instruções.
*(É o paradigma utilizado no exercício da tabuada.)*

```python
def calcular_media(notas):
    soma = 0
    for nota in notas:
        soma += nota
    return soma / len(notas)

print(calcular_media([7, 8, 9]))
```

### 2️⃣ Orientado a Objetos (POO)
Em Python, **tudo é objeto**. Suporta classes, herança, encapsulamento e polimorfismo.

```python
class Aluno:
    def __init__(self, nome, nota):
        self.nome = nome
        self.nota = nota

    def aprovado(self):
        return self.nota >= 6

print(Aluno("Maria", 8).aprovado())   # True
```

### 3️⃣ Funcional
Funções são tratadas como valores (*funções de primeira classe*). Descreve **o que**
deve ser feito, evitando alteração de estado.

```python
notas = [7, 4, 9, 3]

aprovados = list(filter(lambda n: n >= 6, notas))
dobradas  = [n * 2 for n in notas]

print(aprovados)   # [7, 9]
```

### 4️⃣ Reflexivo (Metaprogramação)
O programa consegue inspecionar e modificar a si mesmo em tempo de execução.

```python
texto = "olá"
print(type(texto))   # <class 'str'>
print(dir(texto))    # lista os métodos do objeto
```

> 🗣️ **Em resumo:** Java obriga o uso de objetos, C obriga o estilo procedural.
> **Python deixa você escolher.**

---

## 🔗 Semelhança com outras linguagens

| Linguagem | Relação com Python |
|---|---|
| **ABC** | Principal inspiração — indentação obrigatória e foco na simplicidade |
| **C** | Estrutura geral e operadores (o interpretador CPython é escrito em C) |
| **Modula-3** | Sistema de módulos e tratamento de exceções (`try/except`) |
| **Lisp / Haskell** | Recursos funcionais (`lambda`, `map`, `filter`) |
| **Ruby** | Mais parecida em **filosofia** — dinâmica, legível e multiparadigma |
| **JavaScript** | Mais parecida no **uso** — interpretada e de tipagem dinâmica |

### Python × Java

```python
# Python — sem tipos, sem ponto e vírgula, blocos por indentação
def soma(a, b):
    return a + b
```

```java
// Java — tipagem estática, verboso, blocos com chaves
public int soma(int a, int b) {
    return a + b;
}
```

➡️ Python é o **oposto** de linguagens como **Java, C e C++**, que são compiladas,
de tipagem estática e utilizam `{ }` e `;`.

---

## 🧘 Filosofia — *The Zen of Python*

Executando `import this` no interpretador, são exibidos os princípios de design da linguagem:

```
Bonito é melhor que feio.
Explícito é melhor que implícito.
Simples é melhor que complexo.
Legibilidade conta.
Deve haver um — e preferencialmente apenas um — modo óbvio de fazer algo.
```

---

## ⚠️ Limitações

- Desempenho inferior a linguagens compiladas (C, C++, Java, Go)
- **GIL** (*Global Interpreter Lock*) dificulta o paralelismo real com threads
- Maior consumo de memória
- Erros de tipo aparecem apenas em tempo de execução *(mitigado por type hints)*
- Não é indicada para apps mobile nativos ou jogos de alto desempenho

---

<br>

# 💼 Mercado de Trabalho

## 📊 Panorama (2026)

<table>
<tr>
<td align="center"><b>1º lugar</b><br>Índice TIOBE</td>
<td align="center"><b>25,3%</b><br>de participação<br><i>(recorde histórico)</i></td>
<td align="center"><b>45,7%</b><br>das vagas de TI<br>pedem Python</td>
<td align="center"><b>+2.000</b><br>vagas abertas<br>no Brasil</td>
</tr>
</table>

**Volume de vagas por plataforma (Brasil):**

| Plataforma | Vagas de Python |
|---|---|
| LinkedIn | **+2.000** vagas de Desenvolvedor Python *(592 delas 100% remotas)* |
| Glassdoor | **1.962** vagas *(598 remotas)* |
| Python Brasil (portal especializado) | **1.079** vagas |

---

## 🎯 Áreas que mais contratam

| Área | O que se faz | Tecnologias comuns |
|---|---|---|
| 🤖 **Inteligência Artificial / Machine Learning** | Modelos, LLMs, visão computacional, agentes | PyTorch, TensorFlow, LangChain |
| 📊 **Ciência de Dados / Analytics** | Análise, dashboards, previsões | Pandas, NumPy, Scikit-learn |
| ⚙️ **Backend / APIs** | Serviços e integrações — **área com mais vagas absolutas** | Django, Flask, **FastAPI** |
| 🔁 **Automação / RPA** | Robôs de tarefas repetitivas, web scraping | Selenium, BeautifulSoup |
| ☁️ **DevOps / SRE** | Infraestrutura, CI/CD, monitoramento | AWS, Docker, Kubernetes |
| 🧪 **QA / Testes** | Automação de testes | Pytest, Robot Framework |

> 📈 A demanda por profissionais com **FastAPI** cresce cerca de **40% ao ano**.

---

## 🏢 Empresa pesquisada — NTT DATA

A **NTT DATA** é uma das maiores empresas de consultoria e serviços de TI do mundo,
presente em mais de **70 países** e com mais de **190.000 profissionais**. Contrata
desenvolvedores Python com frequência para projetos de engenharia de software, backend,
cloud, engenharia de dados e inteligência artificial.

🔗 **Carreiras:** https://careers.services.global.ntt/br/pt/

### Vaga de exemplo — *Desenvolvedor(a) Python*

**Requisitos comuns:**

```
✔ Python 3                      ✔ SQL e bancos relacionais
✔ APIs REST                     ✔ Django / Flask / FastAPI
✔ Git e versionamento           ✔ Cloud (AWS, Azure ou GCP)
✔ Metodologias ágeis            ✔ Trabalho em equipe e comunicação
```

---

## 🌐 Vagas reais abertas (agosto/2026)

### Empresas contratando no Brasil

| Empresa | Cargo | Nível | Modalidade | Local |
|---|---|---|---|---|
| **IBM** | SRE Specialist | Pleno | Híbrido | São Paulo — SP |
| **Bosch** | Workload Automation (WLA) Senior Analyst | Sênior | Híbrido | Campinas — SP |
| **PicPay** | Engenheiro(a) de Machine Learning | Pleno | Remoto | São Paulo — SP |
| **Stone** | Senior Data Scientist | Sênior | Remoto | Brasil |
| **Dell Technologies** | Cientista de Dados | Sênior | Remoto | Eldorado do Sul — RS |
| **CI&T** | Senior AI Engineer | Sênior | Remoto | Brasil / Colômbia |
| **EPAM Systems** | Senior Python Backend Developer | Sênior | Remoto | Múltiplas regiões |
| **MetLife** | Forward Deployed AI Engineer | Sênior | Híbrido | São Paulo — SP |
| **Fortive** | Software Engineer, Data & AI Applications | Pleno | Remoto | São Paulo — SP |
| **Ebury** | Senior Security Engineer | Sênior | Híbrido | São Paulo — SP |
| **Inter&Co** | Software Developer Analyst I | 🟢 **Júnior** | Presencial | Belo Horizonte — MG |

### Vagas internacionais com pagamento em moeda estrangeira

| Empresa | Cargo | Modalidade | Observação |
|---|---|---|---|
| **Turing** 🇺🇸 | Software Engineer — Python + Docker | Remoto | Pagamento em **USD** |
| **Qdrant** | Senior SWE — Cloud Platform Infrastructure | Remoto | Brasília ou San Francisco |
| **lemon.io** | Senior Full-stack React & Python Developer | Remoto | Acima de **R$ 18.000** |

### Startups e consultorias (vagas 100% remotas)

| Empresa | Cargo | Contrato | Faixa |
|---|---|---|---|
| **STRIDER** | Full-stack Engineer Python + AI Agent | PJ | Até R$ 18.000 |
| **Pareto** | AI Engineer | PJ | Até R$ 10.000 |
| **Melhor Plano** | Engenheiro(a) de Dados Pleno | CLT | — |
| **FLYTTR do Brasil** | Software Development Coordinator | CLT | Acima de R$ 18.000 |

---

## 🇧🇷 Grandes empresas brasileiras que utilizam Python

| Empresa | Onde usa Python | Carreiras |
|---|---|---|
| **Nubank** | Backend, dados, ciência de dados e IA | [Vagas em Tecnologia](https://international.nubank.com.br/pt-br/companhia/nubank-vagas-abertas-em-tecnologia/) · [Estágio](https://estagio.nubank.com.br/) |
| **iFood** | Recomendação, logística e machine learning | carreiras.ifood.com.br |
| **Mercado Livre** | Backend, engenharia de dados e DevOps | mercadolibre.com/jobs |
| **Itaú Unibanco** | Back-end Python/Java/Kafka, Riscos e Tesouraria | [carreiras.itau.com.br](https://carreiras.itau.com.br/busca-de-vagas) |
| **Bradesco** | IA generativa, dados e automação | bradesco.com.br/carreiras |
| **B3** | Engenharia de software e dados | [vagas.b3.com.br](https://vagas.b3.com.br/) |
| **Stone / PicPay / Inter** | Antifraude, dados e machine learning | Sites próprios |
| **Magalu** | E-commerce, dados e automação | carreiras.magazineluiza.com.br |

> 💡 **Nubank, iFood, Mercado Livre e Itaú** oferecem pacotes com **stock options ou RSUs**,
> que podem **dobrar a remuneração total** anual.

---

## 🌎 Empresas globais que utilizam Python

| Empresa | Como usa |
|---|---|
| **Google** | Processamento de dados e infraestrutura de busca — um dos maiores adeptos do mundo |
| **Instagram** | A **maior implantação Django do planeta**, escrita inteiramente em Python |
| **Netflix** | CDN Open Connect e ferramentas internas de engenharia |
| **Meta (Facebook)** | Gerenciamento de infraestrutura |
| **Uber / Dropbox / Pinterest / Reddit** | Backend, dados e automação |
| **NASA** | Computação científica e simulações |

> 🌍 Mais de **8.000 empresas** no mundo declaram usar Python em seu stack tecnológico.

---

## 💰 Faixa Salarial no Brasil (2026)

### Por senioridade — regime CLT

| Nível | Experiência | Salário mensal |
|---|---|---|
| 🎓 Estágio / Trainee | — | R$ 1.500 – R$ 2.500 |
| 🟢 **Júnior** | 0 – 2 anos | **R$ 3.000 – R$ 6.000** |
| 🔵 **Pleno** | 2 – 5 anos | **R$ 6.000 – R$ 10.000** |
| 🟣 **Sênior** | 5+ anos | **R$ 10.000 – R$ 16.000** |
| 🔴 Especialista / Tech Lead | 8+ anos | **Acima de R$ 20.000** |

> 📈 **Média geral do mercado (misturando todos os níveis):** R$ 7.000 a R$ 11.000/mês
> *Faixa típica do sênior: R$ 7.929 (percentil 25) a R$ 14.017 (percentil 75).*

### Por área de especialização

| Área | Pleno | Sênior |
|---|---|---|
| ⚙️ Backend / APIs | R$ 6.000 – R$ 10.000 | R$ 10.000 – R$ 16.000 |
| 📊 Engenharia de Dados | R$ 8.000 – R$ 14.000 | R$ 15.000 – R$ 22.000 |
| 🤖 **IA / Machine Learning** | **R$ 12.000 – R$ 20.000** | **R$ 25.000 – R$ 35.000** |
| 🌎 Remoto internacional (USD) | — | Equivalente a R$ 20.000+ |

> 💡 Em **IA, dados, fintechs e vagas remotas internacionais**, a remuneração ultrapassa
> facilmente os **R$ 20.000/mês** — podendo ser paga em dólar.

### CLT × PJ

| | CLT | PJ |
|---|---|---|
| **Valor bruto** | Menor | **Maior** (geralmente +20% a +40%) |
| **Benefícios** | Férias, 13º, FGTS, VA/VR, plano de saúde | Nenhum garantido |
| **Estabilidade** | Maior | Menor |
| **Impostos** | Retidos na fonte | Por conta do profissional |

---

## 📈 Fatores que influenciam a remuneração

- 🧠 **Especialização** — IA, Machine Learning e Engenharia de Dados pagam bem acima da média
- 🏦 **Setor** — fintechs, bancos digitais e startups pagam mais que empresas tradicionais
- 📄 **Regime de contratação** — PJ tem valor bruto maior; CLT embute férias, 13º e benefícios
- 🌎 **Idioma** — inglês fluente abre vagas remotas internacionais com pagamento em dólar
- 📍 **Região** — capitais (SP, BH, POA) pagam mais, mas o remoto vem nivelando a diferença
- 📦 **Pacote total** — empresas de tecnologia somam bônus, PLR e ações ao salário base

---

## 🔎 Onde buscar vagas de Python

| Portal | Link | Destaque |
|---|---|---|
| **Python Brasil** | [python.dev.br/vagas](https://python.dev.br/vagas/) | Exclusivo de Python, atualizado diariamente |
| **LinkedIn Jobs** | [br.linkedin.com/jobs](https://br.linkedin.com/jobs/desenvolvedor-python-vagas) | Maior volume de vagas |
| **ProgramaThor** | [programathor.com.br](https://programathor.com.br/jobs-python/remoto) | Foco em vagas remotas de tech |
| **Glassdoor** | [glassdoor.com.br](https://www.glassdoor.com.br/) | Vagas + avaliações e salários das empresas |
| **Indeed / Gupy / Jooble** | — | Agregadores com grande volume |

---

## 🛠️ Tecnologias Utilizadas

<p>
  <img src="https://img.shields.io/badge/Python_3-3776AB?style=flat-square&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/OneCompiler-2E7D32?style=flat-square&logo=codeforces&logoColor=white">
  <img src="https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white">
  <img src="https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white">
</p>

---

## ✅ Conclusão

Python consolidou-se como a linguagem mais requisitada do mercado por unir uma
**sintaxe simples e legível** a um **ecossistema extremamente amplo**. Por ser
**multiparadigma**, permite ao programador escolher a abordagem mais adequada a cada
problema — procedural, orientada a objetos ou funcional.

Essa flexibilidade, somada ao domínio da linguagem nas áreas de Inteligência Artificial e
Ciência de Dados, explica o alto volume de vagas (**mais de 2.000 abertas no Brasil**) e a
boa remuneração observados nesta pesquisa — de **R$ 3.000** no nível júnior a mais de
**R$ 35.000** para especialistas em IA.

---

## 📚 Referências

- [TIOBE Index](https://www.tiobe.com/tiobe-index/)
- [Python Brasil — Salários](https://python.dev.br/carreira/salarios-python-brasil/) · [Vagas](https://python.dev.br/vagas/)
- [Glassdoor Brasil — Vagas de Desenvolvedor Python](https://www.glassdoor.com.br/Vaga/brasil-desenvolvedor-python-vagas-SRCH_IL.0,6_KO7,27.htm)
- [LinkedIn Jobs — Desenvolvedor Python](https://br.linkedin.com/jobs/desenvolvedor-python-vagas)
- [ProgramaThor — Vagas Python Remotas](https://programathor.com.br/jobs-python/remoto)
- [GeekHunter — Salário de Programador Python](https://blog.geekhunter.com.br/salario-programador-python/)
- [NTT DATA — Carreiras](https://careers.services.global.ntt/br/pt/)
- [Nubank — Vagas em Tecnologia](https://international.nubank.com.br/pt-br/companhia/nubank-vagas-abertas-em-tecnologia/)
- [Itaú Carreiras](https://carreiras.itau.com.br/busca-de-vagas)
- [Documentação oficial do Python](https://docs.python.org/pt-br/3/)

---

<p align="center">
  <sub>⚠️ Vagas e salários coletados em <b>agosto de 2026</b> — valores são médias de mercado
  e variam conforme empresa, região e experiência.</sub>
</p>
