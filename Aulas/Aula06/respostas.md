#### Passo 1. Gere um DataFrame (dfMerged), com as seguintes colunas:
    
state/region, ages, year, population, state

```python

# Código do programa de merge dos 03 arquivos
dfMerged = pd.merge(pop, abbrevs, how='outer',left_on='state/region', right_on='abbreviation')
dfMerged = dfMerged.drop('abbreviation', axis= 1)

# Verificação do DataFrame gerado
dfMerged.head()

# Verificação se há algum dado faltando
dfMerged.isnull().any()

# Verificação de quais itens de "population" estão faltando
dfMerged[dfMerged['population'].isnull()].head()

# Verificação de quais state/region têm elementos na coluna "state" com data missing
dfMerged.loc[dfMerged['state'].isnull(), 'state/region'].unique()

# Para resolver o problema de associar Porto Rico com um estado dos EU.
# Só fica faltando resolver a população de Porto Rico (data missing)

dfMerged.loc[dfMerged['state/region'] == 'PR', 'state'] = 'Puerto Rico'
dfMerged.loc[dfMerged['state/region'] == 'USA', 'state'] = 'United States'
dfMerged.isnull().any()
```

### Passo 2. Gere um DataFrame (dfFinal), com as seguintes colunas:

state/region, ages, year, population, state, area (sq. mi)

```python

# Código de merge para gerar o DataFrame dfFinal
dfFinal = pd.merge(dfMerged, areas, on='state', how='left')

# Verificação do resultado
dfFinal.head()

# Verificação se há data missing
dfFinal.isnull().any()
```

### Passo 3. Deve haver nulos na coluna da área.
Verifique quais regiões foram ignoradas.

```python

# Código de verificação de quais estados têm data missing nos campos de área
dfFinal['state'][dfFinal['area (sq. mi)'].isnull()].unique()
```

### Passo 4. Há campos na coluna de área no DataFrame com data missing.
Para resolver esse tipo de problema, podemos inserir o valor apropriado (usando a soma de todas as áreas de estado, por exemplo), mas neste caso, simplesmente elimite os campos com valores nulos.

```python

# código de eliminação de campos com data missing
dfFinal.dropna(inplace=True)
dfFinal.head()
```

### Passo 5. Obtenha um DataFrame (data2010) da população dos EU em 2010 (apenas o total por estados).
- data2010(state/region, ages, year, population, state,area (sq. mi))

```python

# Código

data2010 = dfFinal.query("year == 2010 & ages == 'total'")
data2010.head()
```