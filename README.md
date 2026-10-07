# databricks-lakehouse
 
Lakehouse end-to-end no Databricks que integra dados públicos de clima e produção
agrícola do Paraná para analisar a relação entre chuva, temperatura e produtividade
de soja, milho e trigo por município e safra.
 
> Status: em desenvolvimento. Veja o [roadmap](#roadmap).
 
## Escopo
 
- **Região:** Paraná, um dos maiores produtores de grãos do Brasil
- **Culturas:** soja, milho e trigo
- **Período:** 2015 a 2024
 
## Fontes de dados
 
| Fonte | Conteúdo | Formato | Granularidade |
|---|---|---|---|
| INMET, dados históricos | Chuva, temperatura, umidade, radiação | CSV (um ZIP por ano) | Horária, por estação |
| IBGE, PAM (API SIDRA) | Área plantada e colhida, produção, rendimento | JSON | Anual, por município e cultura |
| IBGE, malha municipal | Polígonos dos municípios | GeoJSON | Município |
 
## Desafio
 
O clima é medido por hora e por estação; a produção é anual e por município,
e nem todo município tem estação. O pipeline agrega o clima até o nível de safra
e associa cada município à estação mais próxima do seu centroide.
 
## Arquitetura
 
- **Bronze:** dados crus com metadados de ingestão. Carga incremental com Auto Loader.
- **Silver:** tipagem, padronização, tratamento de valores inválidos e regras de qualidade com quarentena.
- **Gold:** modelo dimensional (`fato_produtividade_clima`, `dim_municipio`, `dim_cultura`, `dim_safra`, `dim_estacao`).
 
*Diagrama em breve.*
 
## Stack
 
Databricks (Free Edition) · Delta Lake · Unity Catalog · Lakeflow · PySpark · GeoPandas ·
Databricks Asset Bundles · GitHub Actions · pytest
 
## Limitações conhecidas
 
- A PAM é divulgada por ano civil, que não coincide exatamente com o ano-safra.
- A estação mais próxima é uma aproximação do clima do município.
- O resultado indica correlação, não causalidade.
 
## Roadmap
 
- [ ] Estrutura do repositório e conexão com o Databricks
- [ ] Ingestão e camada bronze
- [ ] Camada silver com qualidade de dados
- [ ] Join geoespacial e camada gold
- [ ] Orquestração com Jobs
- [ ] Asset Bundles, testes e CI/CD
- [ ] Dashboard
- [ ] Fase 2: crédito rural (Banco Central, Sicor)
