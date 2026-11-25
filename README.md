# Indicadores – Pesca e Aquicultura (RESEX/UCs)

Caderno Jupyter que lê a planilha consolidada de cadastro das famílias de Unidades de Conservação (UCs/RESEX) e calcula indicadores de **pesca** e **aquicultura**, com recorte por **sexo** e por posse do **RGP** (Registro Geral da Atividade Pesqueira). Ao final, gera um quadro-resumo, gráficos (`.png`) e uma planilha Excel com os resultados.

## Sumário

- [O que o projeto calcula](#o-que-o-projeto-calcula)
- [Tecnologias](#tecnologias)
- [Estrutura do repositório](#estrutura-do-repositório)
- [Como executar](#como-executar)
- [Configuração](#configuração)
- [Saídas geradas](#saídas-geradas)

## O que o projeto calcula

| Indicador | Origem (aba da planilha) |
|---|---|
| Total de famílias | *Dados Gerais* |
| Famílias com atividade pesqueira como atividade principal | *Atividades Produtivas* |
| Pessoas adultas na pesca, por sexo e por RGP (Sim / Não / Não sabe) | *Membros Familiares* (coluna *Principais Ocupações*) |
| Famílias com aquicultura como atividade principal | *Atividades Produtivas* (colunas *Produto* e *Outro Produto*) |
| Familiares adultos dessas famílias, por sexo e por RGP | *Membros Familiares* |

Apenas pessoas com idade mínima de 18 anos (configurável) entram na contagem de pessoas.

## Tecnologias

| Tecnologia | Uso |
|---|---|
| Python 3.12+ | Linguagem |
| Jupyter Notebook | Execução do caderno |
| pandas | Leitura, limpeza e cálculo dos indicadores |
| matplotlib | Gráficos |
| openpyxl | Escrita do Excel de resultados (e leitura, se o calamine não estiver disponível) |
| python-calamine | Leitura mais rápida de `.xlsx` grandes |

## Estrutura do repositório

```
.
├── indicadores_pesca_aquicultura.ipynb   # caderno principal
├── requirements.txt                      # dependências
└── README.md
```

## Como executar

### 1. Pré-requisitos

- [Python 3.12 ou superior](https://www.python.org/downloads/)
- [Git](https://git-scm.com/)

### 2. Clonar o repositório e instalar as dependências

```bash
git clone <url-do-repositorio>
cd <pasta-do-repositorio>

python -m venv .venv

# Windows (PowerShell)
.venv\Scripts\Activate.ps1
# Windows (cmd)
.venv\Scripts\activate.bat
# Linux / macOS
source .venv/bin/activate

pip install -r requirements.txt
```

### 3. Colocar a planilha de dados

Copie o arquivo `.xlsx` consolidado para a mesma pasta do caderno. Ele precisa conter as abas:

- `Dados Gerais`
- `Membros Familiares`
- `Atividades Produtivas`

### 4. Abrir e executar o caderno

```bash
jupyter notebook indicadores_pesca_aquicultura.ipynb
```

1. Na **célula 1 (Configuração)**, ajuste `ARQUIVO` para o nome da sua planilha.
2. Execute tudo: menu **Run → Run All Cells** (ou `Shift+Enter` célula a célula, em ordem).

Também é possível abrir o `.ipynb` diretamente na sua IDE, selecionando o kernel da `.venv`.

## Configuração

Todas as opções ficam na célula 1 do caderno.

| Variável | O que é | Exemplo |
|---|---|---|
| `ARQUIVO` | Caminho do Excel consolidado | `"tabela.xlsx"` |
| `ABA_DADOS_GERAIS`, `ABA_MEMBROS`, `ABA_ATIVIDADES` | Nomes das abas (maiúsculas, acentos e espaços extras não importam) | `"Membros Familiares"` |
| `PRODUTOS_AQUICULTURA` | Produtos que contam como aquicultura | `["Peixes (piscicultura)", ...]` |
| `TEXTOS_OUTRO_AQUICULTURA` | Textos procurados em *Outro Produto* quando o produto é "Outro" | `["PSICULTURA", ...]` |
| `FAM`, `INTEG`, `SEXO`, `RGP`, `OCUP`, `NASC`, `ATV`, `PROD`, `OUTRO` | Nomes das colunas usadas (altere só se os cabeçalhos forem diferentes) | `"Código Familiar"` |
| `IDADE_MINIMA` | Idade mínima para contar uma pessoa | `18` |
| `DATA_REFERENCIA` | Data usada para calcular a idade (`None` = hoje) | `None` |

## Saídas geradas

Criadas na pasta do caderno após a execução:

- `resultados_indicadores.xlsx` – abas: *Resumo*, *Pesca sexo x RGP*, *Aquicultura sexo x RGP*
- `grafico_atividades.png`
- `grafico_pesca_sexo_rgp.png`
- `grafico_aquicultura_sexo_rgp.png`
