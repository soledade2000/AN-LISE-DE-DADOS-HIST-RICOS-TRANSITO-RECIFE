# Análise de Dados Históricos de Velocidade — Recife

Projeto de Trabalho de Conclusão de Curso (TCC) voltado à análise de dados históricos de velocidade das vias do Recife, com foco na identificação de trechos com recorrência de baixa velocidade e no apoio à análise de rotas alternativas.

## Objetivo

O projeto busca investigar como dados históricos de velocidade podem ser utilizados para:

- identificar trechos com recorrência de baixa velocidade;
- caracterizar o comportamento da velocidade ao longo do dia;
- estimar velocidades médias a partir das faixas de velocidade disponíveis;
- analisar a recorrência de condições de baixa velocidade;
- relacionar os registros aos equipamentos de monitoramento;
- representar espacialmente os pontos analisados;
- integrar os resultados à rede viária;
- comparar rotas a partir da distância e do tempo estimado de deslocamento.

## Pergunta de pesquisa

> Como dados históricos de velocidade podem ser utilizados para identificar trechos com recorrência de baixa velocidade e apoiar a identificação de rotas alternativas com menor tempo estimado de deslocamento no Recife?

## Dados

Os dados utilizados são provenientes de bases públicas da CTTU/Prefeitura do Recife, disponibilizadas por meio do Portal de Dados Abertos do Recife.

A base principal contém registros de velocidade organizados em intervalos de 15 minutos e distribuídos em faixas de velocidade.

O estudo considera:

- dados referentes ao ano de 2025;
- dados de janeiro a julho de 2026;
- identificação dos equipamentos de monitoramento;
- localização dos equipamentos;
- faixas de velocidade;
- quantidade de veículos observados.

Para comparação entre os anos, é considerada a mesma janela temporal disponível para 2026, evitando comparar um ano completo com apenas parte de outro.

## Tratamento dos dados

O notebook realiza etapas de:

1. carregamento das bases;
2. consolidação dos dados;
3. verificação da estrutura dos registros;
4. tratamento de inconsistências identificadas;
5. análise das faixas de velocidade;
6. cálculo de indicadores de baixa velocidade;
7. análise temporal;
8. identificação dos pontos críticos;
9. análise por avenida;
10. representação espacial;
11. integração com a rede viária;
12. análise de rotas.

Os registros acima de 100 km/h não são preenchidos artificialmente com zero quando não há observação correspondente.

## Indicadores

Um dos principais indicadores utilizados é a proporção de veículos observados até 30 km/h.

### Quantidade de veículos até 30 km/h

São consideradas as faixas:

- 0–10 km/h;
- 11–20 km/h;
- 21–30 km/h.

O cálculo utilizado é:

```text
Q30 = Q0-10 + Q11-20 + Q21-30
```

A proporção de veículos até 30 km/h é calculada por:

```text
P30 = (Q30 / Qtotal) × 100
```

Também é considerado o indicador de veículos até 40 km/h:

```text
Q40 = Q30 + Q31-40

P40 = (Q40 / Qtotal) × 100
```

## Critério exploratório de baixa velocidade

Para a análise de recorrência, é utilizado como critério exploratório o seguinte:

> um dia é considerado de baixa velocidade quando pelo menos 50% dos veículos observados em determinado equipamento estão nas faixas de velocidade até 30 km/h.

Também são consideradas análises de sensibilidade com outros limiares, quando aplicável.

A recorrência representa a proporção de dias que atendem ao critério. Ela não deve ser interpretada como duração de congestionamento.

## Velocidade média estimada

A base utilizada não fornece necessariamente uma velocidade média diretamente observada para cada registro.

Por isso, quando necessário, é calculada uma **velocidade média estimada** a partir dos pontos médios das faixas:

| Faixa | Valor utilizado |
|---|---:|
| 0–10 km/h | 5 km/h |
| 11–20 km/h | 15 km/h |
| 21–30 km/h | 25 km/h |
| 31–40 km/h | 35 km/h |
| 41–50 km/h | 45 km/h |
| 51–60 km/h | 55 km/h |
| 61–70 km/h | 65 km/h |
| 71–80 km/h | 75 km/h |
| 81–90 km/h | 85 km/h |
| 91–100 km/h | 95 km/h |

Registros acima de 100 km/h não são utilizados nesse cálculo.

Essa medida deve ser interpretada como **estimativa baseada nas faixas de velocidade**, e não como uma média de velocidade diretamente fornecida pela CTTU.

## Análise temporal

O projeto analisa a variação da baixa velocidade ao longo das horas do dia.

Entre os resultados exploratórios obtidos no notebook, destacam-se diferentes padrões horários entre as avenidas analisadas. Por exemplo, a Av. Rui Barbosa apresentou um pico de aproximadamente 66,3% de veículos até 30 km/h às 7h no gráfico analisado.

Os valores apresentados no trabalho devem ser interpretados de acordo com os filtros, período e equipamentos considerados em cada análise.

## Análise espacial

Os equipamentos de monitoramento são associados às suas coordenadas geográficas para permitir:

- visualização dos pontos;
- identificação de pontos críticos;
- associação entre equipamento e avenida;
- preparação da análise com a rede viária.

O projeto utiliza mapas para representação dos pontos analisados.

## Análise de rotas

A etapa de roteamento utiliza uma representação da rede viária e considera:

```text
tempo = distância / velocidade
```

Para uma rota composta por vários segmentos:

```text
T = Σ(di / vi)
```

onde:

- `di` = distância do segmento;
- `vi` = velocidade estimada no segmento;
- `T` = tempo estimado da rota.

A análise busca comparar distância, velocidade estimada e tempo de deslocamento.

### Estudo de caso

Um dos estudos de caso utiliza a Av. Rui Barbosa como rota de referência e considera uma alternativa associada às ruas do Futuro e Bruno Maia.

Na versão atual do notebook, a rota de referência calculada na rede viária apresentou aproximadamente:

- **Distância:** 7,94 km;
- **Velocidade histórica estimada:** 32,11 km/h;
- **Tempo estimado:** 14,83 minutos.

A alternativa não deve receber uma velocidade artificial apenas para completar a comparação. Quando não houver dado histórico equivalente, isso deve ser indicado explicitamente no resultado.

## Estrutura do projeto

Uma organização sugerida para o repositório é:

```text
.
├── README.md
├── tcc_transito_recife.ipynb
├── dados/
│   └── arquivos de dados utilizados
├── resultados/
│   ├── mapas/
│   ├── tabelas/
│   └── estudos de caso/
└── figuras/
    └── gráficos utilizados no trabalho
```

Os nomes e arquivos podem variar conforme a organização final do repositório.

## Tecnologias utilizadas

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- NetworkX
- OSMnx
- Folium
- Scikit-learn

## Execução

### 1. Clonar o repositório

```bash
git clone URL_DO_REPOSITORIO
```

### 2. Acessar a pasta

```bash
cd NOME_DO_REPOSITORIO
```

### 3. Instalar as dependências

```bash
pip install pandas numpy matplotlib seaborn networkx osmnx folium scikit-learn
```

### 4. Executar o notebook

Abra o arquivo:

```text
tcc_transito_recife.ipynb
```

e execute as células na ordem apresentada.

## Reprodutibilidade

Para reproduzir os resultados apresentados no TCC, recomenda-se registrar no GitHub:

- versão do notebook utilizada na entrega;
- arquivos de dados utilizados;
- data/período dos dados;
- versão do Python;
- principais bibliotecas utilizadas;
- resultados gerados;
- commit correspondente à versão avaliada.

### Identificação da versão avaliada

No momento da entrega, o commit utilizado para gerar os resultados finais deve ser informado aqui:

```text
Commit da versão avaliada: COLOCAR_HASH_DO_COMMIT_AQUI
```

Exemplo:

```text
Commit da versão avaliada: a1b2c3d4
```

Isso permite identificar exatamente qual versão do código corresponde aos resultados apresentados no TCC.

## Limitações

Os resultados de baixa velocidade não permitem, isoladamente, afirmar a causa do comportamento observado.

Por exemplo, os dados não devem ser utilizados para afirmar que uma determinada ocorrência foi causada por:

- semáforos;
- acidentes;
- obras;
- chuva;
- alagamentos;
- eventos;
- outros fatores externos,

quando essas informações não estiverem disponíveis na base analisada.

Outra limitação importante é que os equipamentos representam pontos de observação. Portanto, um ponto de monitoramento não corresponde necessariamente a todo o segmento da via.

A estimativa de velocidade também depende da distribuição dos veículos nas faixas de velocidade disponíveis.

## Resultados

Os resultados gerados pelo notebook podem incluir:

- tabelas de indicadores;
- ranking de avenidas;
- gráficos horários;
- identificação de pontos críticos;
- mapas dos equipamentos;
- análise de recorrência;
- estimativas de velocidade;
- análise de rotas;
- tempos estimados de deslocamento.

Os resultados finais utilizados no TCC devem corresponder ao notebook e ao commit registrados na versão de entrega.

## Autoria

Projeto desenvolvido como parte de Trabalho de Conclusão de Curso.

**Tema:** Análise de dados históricos de velocidade para identificação de trechos com baixa velocidade e apoio à seleção de rotas alternativas no Recife.

---

**Observação:** este README descreve a metodologia e a organização do projeto. Valores finais, tabelas e conclusões devem ser conferidos diretamente na versão do notebook utilizada para a entrega do TCC.
