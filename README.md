# Distribuição Setorial Funcional da Renda e Polarização do Emprego no Brasil

## Descrição

Este repositório contém código, dados e documentação para pesquisa sobre a distribuição setorial funcional da renda na indústria brasileira e o fenômeno da polarização do emprego, utilizando um modelo setorial de simulação baseado em insumo-produto combinado com técnicas de machine learning para classificação de ocupações.

## Contexto Teórico

A polarização do mercado de trabalho, caracterizada pelo crescimento simultâneo de ocupações de alta e baixa qualificação com declínio das ocupações intermediárias, tem sido documentada em economias desenvolvidas desde os anos 1990 (Autor e Dorn, 2013; Goos e Manning, 2007). Este fenômeno está intrinsecamente ligado à mudança tecnológica e à automação de tarefas rotineiras.

Este projeto investiga:
1. A existência e magnitude da polarização do emprego no Brasil
2. A relação entre polarização ocupacional e distribuição funcional da renda
3. Os impactos setoriais de choques econômicos e tecnológicos
4. A aplicação de métodos de machine learning não supervisionado para classificação automática de ocupações

## Objetivos

### Objetivo Geral
Analisar a distribuição funcional da renda entre capital e trabalho na indústria brasileira e sua relação com o fenômeno da polarização do emprego, utilizando um modelo setorial de simulação baseado em insumo-produto.

### Objetivos Específicos
- Classificar ocupações da CBO 2002 segundo conteúdo de tarefas (manual/abstrata, rotineira/não-rotineira) usando Hierarchical Dirichlet Process (HDP)
- Construir séries setoriais de emprego por tipo de tarefa utilizando dados da RAIS
- Desenvolver modelo de insumo-produto integrado com classificação de ocupações
- Simular impactos de choques tecnológicos e de demanda na distribuição de renda e composição ocupacional
- Quantificar o grau de polarização do emprego em diferentes setores industriais

## Metodologia

### 1. Classificação de Ocupações

**Abordagem:** Machine Learning Não Supervisionado

Utilizamos Hierarchical Dirichlet Process (HDP) para classificar automaticamente ocupações da Classificação Brasileira de Ocupações (CBO 2002) em categorias de tarefas, baseando-se em:
- Descrições oficiais de atividades
- Competências requeridas
- Informações complementares da ISCO-08

**Framework teórico:** Autor, Levy e Murnane (2003); Acemoglu e Autor (2011)

**Categorias de tarefas:**
- Manual Rotineira
- Manual Não-Rotineira
- Abstrata Rotineira
- Abstrata Não-Rotineira

### 2. Análise de Dados de Emprego

**Fonte primária:** RAIS (Relação Anual de Informações Sociais)

**Processamento:**
- Cruzamento CBO × CNAE (setor)
- Agregação por tipo de tarefa e setor
- Cálculo de massa salarial por categoria
- Construção de séries temporais 2002-2022

### 3. Modelo Insumo-Produto

**Base:** Tabelas de Recursos e Usos (TRU) do IBGE

**Implementação:**
- Matriz de coeficientes técnicos (A)
- Matriz inversa de Leontief (I-A)^(-1)
- Multiplicadores setoriais de produção e emprego
- Decomposição do valor adicionado

**Simulações:**
- Choques de demanda setorial
- Choques de produtividade
- Mudança tecnológica (automação)
- Políticas de incentivo setorial

### 4. Integração ML + Insumo-Produto

Pipeline de análise:
```
CBO → HDP → Classificação de Tarefas → 
RAIS → Emprego por Setor×Tarefa → 
Modelo I-P → Simulações → 
Impactos na Distribuição de Renda e Polarização
```

## Estrutura do Repositório
```
.
├── data/
│   ├── raw/                      # Dados originais (não versionados)
│   │   ├── cbo/                  # CBO 2002 oficial
│   │   ├── rais/                 # Microdados RAIS
│   │   └── tru/                  # Tabelas de Recursos e Usos IBGE
│   ├── processed/                # Dados processados
│   │   ├── cbo_classificada.csv  # CBO com classificação de tarefas
│   │   ├── emprego_setor_tarefa.csv
│   │   └── matriz_insumo_produto.csv
│   └── output/                   # Resultados finais
│
├── code/
│   ├── 01_data_collection/
│   │   ├── download_cbo.py
│   │   ├── download_rais.py
│   │   └── download_tru.py
│   ├── 02_classification/
│   │   ├── hdp_model.py          # Implementação HDP
│   │   ├── train_classifier.py
│   │   └── validate_classification.py
│   ├── 03_data_processing/
│   │   ├── process_rais.py
│   │   ├── aggregate_employment.py
│   │   └── prepare_io_matrix.py
│   ├── 04_modeling/
│   │   ├── input_output_model.py
│   │   ├── simulations.py
│   │   └── multipliers.py
│   ├── 05_analysis/
│   │   ├── polarization_metrics.py
│   │   ├── income_distribution.py
│   │   └── decomposition.py
│   └── utils/
│       ├── data_utils.py
│       └── visualization.py
│
├── notebooks/
│   ├── exploratory/
│   │   ├── 01_cbo_exploration.ipynb
│   │   ├── 02_hdp_tuning.ipynb
│   │   └── 03_rais_analysis.ipynb
│   └── results/
│       ├── 01_classification_results.ipynb
│       ├── 02_polarization_analysis.ipynb
│       └── 03_simulation_results.ipynb
│
├── docs/
│   ├── methodology.md
│   ├── data_sources.md
│   └── references.bib
│
├── results/
│   ├── figures/
│   ├── tables/
│   └── reports/
│
├── requirements.txt
├── environment.yml
├── LICENSE
└── README.md
```

## Como Usar

### Requisitos

**Python 3.9+**

Principais bibliotecas:
```
pandas >= 1.5.0
numpy >= 1.23.0
gensim >= 4.3.0
scikit-learn >= 1.2.0
scipy >= 1.10.0
matplotlib >= 3.6.0
seaborn >= 0.12.0
```

**R 4.2+** (opcional, para análises complementares)

Pacotes:
```
tidyverse
quanteda
stm
ioanalysis
```

### Instalação
```bash
# Clonar repositório
git clone https://github.com/seu-usuario/distribuicao-renda-polarizacao-brasil.git
cd distribuicao-renda-polarizacao-brasil

# Criar ambiente virtual
python -m venv venv
source venv/bin/activate  # Linux/Mac
# ou
venv\Scripts\activate  # Windows

# Instalar dependências
pip install -r requirements.txt
```

### Reproduzir Análise
```bash
# 1. Baixar dados (requer configuração de credenciais)
python code/01_data_collection/download_cbo.py
python code/01_data_collection/download_rais.py

# 2. Treinar modelo de classificação
python code/02_classification/train_classifier.py

# 3. Processar dados de emprego
python code/03_data_processing/process_rais.py

# 4. Executar simulações
python code/04_modeling/simulations.py

# 5. Gerar relatórios
python code/05_analysis/generate_report.py
```

### Notebooks Interativos
```bash
jupyter notebook notebooks/
```

Recomenda-se seguir a ordem numérica dos notebooks.

## Fontes de Dados

### Classificação Brasileira de Ocupações (CBO 2002)
- **Fonte:** Ministério do Trabalho e Emprego (MTE)
- **URL:** http://www.mtecbo.gov.br
- **Formato:** CSV, PDF
- **Descrição:** Estrutura oficial de ocupações no Brasil

### RAIS (Relação Anual de Informações Sociais)
- **Fonte:** Ministério do Trabalho e Emprego
- **Período:** 2002-2022
- **Formato:** Microdados (TXT)
- **Acesso:** Base dos Dados ou FTP MTE

### Tabelas de Recursos e Usos (TRU)
- **Fonte:** IBGE - Sistema de Contas Nacionais
- **URL:** https://www.ibge.gov.br/estatisticas/economicas/contas-nacionais.html
- **Período:** Séries anuais disponíveis
- **Formato:** XLS

### ISCO-08 (International Standard Classification of Occupations)
- **Fonte:** International Labour Organization (ILO)
- **URL:** https://www.ilo.org/public/english/bureau/stat/isco/
- **Uso:** Enriquecimento de descrições CBO

## Principais Resultados

(A ser preenchido conforme andamento da pesquisa)

- Número de tópicos descobertos pelo HDP:
- Proporção de ocupações por categoria:
- Grau de polarização por setor:
- Impacto de choques na distribuição funcional:

## Validação

### Classificação de Ocupações
- Silhouette Score:
- Comparação com classificação manual (amostra):
- Concordância com literatura internacional:

### Modelo Insumo-Produto
- Verificação de identidades contábeis
- Comparação de multiplicadores com literatura
- Testes de sensibilidade

## Citação

Se você utilizar este código ou metodologia, por favor cite:
```bibtex
@misc{seu_sobrenome2025,
  author = {Seu Nome},
  title = {Distribuição Setorial Funcional da Renda e Polarização do Emprego no Brasil},
  year = {2025},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/seu-usuario/distribuicao-renda-polarizacao-brasil}}
}
```

## Referências Principais

Acemoglu, D., & Autor, D. (2011). Skills, Tasks and Technologies: Implications for Employment and Earnings. In Handbook of Labor Economics (Vol. 4, pp. 1043-1171). Elsevier.

Autor, D. H., & Dorn, D. (2013). The Growth of Low-Skill Service Jobs and the Polarization of the US Labor Market. American Economic Review, 103(5), 1553-1597.

Autor, D. H., Levy, F., & Murnane, R. J. (2003). The Skill Content of Recent Technological Change: An Empirical Exploration. The Quarterly Journal of Economics, 118(4), 1279-1333.

Rocha, G. R. (2021). Mudança Tecnológica e Polarização do Emprego no Brasil. Dissertação de Mestrado, UNIFESP.

Teodoridis, F., Lu, J., & Furman, J. (2022). Measuring the Direction of Innovation: Frontier Tools in Unassisted Machine Learning. Working Paper.

## Licença

MIT License - veja arquivo LICENSE para detalhes

## Contato

**Autor:** Seu Nome  
**Instituição:** Sua Universidade  
**Email:** seu.email@universidade.edu.br  
**Orientador:** Nome do Orientador

## Agradecimentos

Este projeto é desenvolvido como parte de uma Iniciação Científica/Tecnológica em [Sua Universidade], com apoio de [agências de fomento, se houver].

Agradecimentos especiais aos desenvolvedores das bibliotecas open-source utilizadas e aos órgãos públicos que disponibilizam dados abertos.

---

Última atualização: Janeiro 2025
```

# Descrição Curta (para campo "About" do GitHub)
```
Análise da distribuição funcional da renda e polarização do emprego na indústria brasileira usando machine learning (HDP) para classificação de ocupações e modelo insumo-produto para simulações setoriais. Dados: RAIS, CBO, TRU-IBGE.
```

# Alternativa: Descrição Curta Expandida
```
Pesquisa sobre polarização do mercado de trabalho brasileiro e distribuição capital-trabalho. Utiliza Hierarchical Dirichlet Process para classificar ocupações CBO por conteúdo de tarefas e modelo de insumo-produto para simular impactos setoriais. Implementação em Python com dados RAIS 2002-2022.
```

# Tags Sugeridas (GitHub Topics)
```
labor-economics
input-output-analysis
machine-learning
economic-modeling
brazil
polarization
topic-modeling
hdp
occupation-classification
income-distribution
economic-research
econometrics
python
data-science
