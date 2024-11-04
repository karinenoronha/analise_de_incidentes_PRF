# analise_de_incidentes_PRF

# Análise de Acidentes nas Rodovias Brasileiras com Dados da PRF

Este projeto realiza uma análise exploratória dos dados de acidentes em rodovias federais no Brasil,
fornecidos pelo site de Dados Abertos do Governo Federal juntamente  Polícia Rodoviária Federal (PRF).
Utilizando dados reais, buscamos identificar padrões, causas e características dos incidentes para contribuir com insights que possam guiar campanhas de conscientização e políticas de segurança no trânsito.


# Fontes dos Dados
Os dados foram obtidos do portal de Dados Abertos do Governo Federal (https://www.gov.br/prf/pt-br/acesso-a-informacao/dados-abertos/dados-abertos-da-prf). Essa base contém informações detalhadas sobre acidentes em rodovias federais, incluindo localização, causas, tipos de veículos envolvidos e condições de trânsito no momento do acidente.

# Bibliotecas Utilizadas
Para a análise e visualização dos dados, foram utilizadas as seguintes bibliotecas:

- pandas: Manipulação e análise de dados.

- seaborn: Visualização de dados.
- numpy: Operações numéricas e manipulação de arrays.
- geopandas: Análise de dados geoespaciais.
- matplotlib.pyplot: Visualização de dados.
- folium: Visualização geográfica interativa.


'''
def plot_categorical_frequency_pt(df, corte_cardinalidade=30, graficos_por_linha=2):
    """
    Plota a frequência de categorias para variáveis categóricas em um DataFrame.

    Parâmetros:
    - df: DataFrame para plotagem.
    - corte_cardinalidade: Cardinalidade máxima para uma coluna ser considerada (padrão é 30).
    - graficos_por_linha: Quantidade de gráficos por linha (padrão é 3).

    Retorna:
    - Exibe os gráficos de barras.
    """

    # Gera metadados para o DataFrame
    metadados = []
    for coluna in df.columns:
        metadados.append({
            'Variável': coluna,
            'Tipo': df[coluna].dtype,
            'Cardinalidade': df[coluna].nunique()
        })

    df_metadados = pd.DataFrame(metadados)

    # Filtra colunas com cardinalidade maior que o corte e tipos não numéricos
    variaveis_categoricas = df_metadados[(df_metadados['Cardinalidade'] <= corte_cardinalidade) & (df_metadados['Tipo'] == 'object')]

    # Calcula o número de linhas e colunas para os subplots
    n_linhas = -(-len(variaveis_categoricas) // graficos_por_linha)  # Ceiling division
    n_colunas = min(len(variaveis_categoricas), graficos_por_linha)

    # Plota as variáveis categóricas
    fig, axs = plt.subplots(nrows=n_linhas, ncols=n_colunas, figsize=(15, 5 * n_linhas))

    for i, (idx, linha) in enumerate(variaveis_categoricas.iterrows()):
        var = linha['Variável']
        ax = axs[i // graficos_por_linha, i % graficos_por_linha]
        df[var].value_counts().sort_index().plot(kind='bar', ax=ax, color='skyblue')
        ax.set_title(f'Frequência em {var}')
        ax.set_ylabel('Frequência')
        ax.set_xlabel(var)

    # Remove os eixos vazios, se houver
    for j in range(i + 1, n_linhas * n_colunas):
        axs[j // graficos_por_linha, j % graficos_por_linha].axis('off')

    plt.tight_layout()
    plt.show()

# Testa a função com os dados e o corte padrão de cardinalidade
plot_categorical_frequency_pt(df_prf_oco_01, corte_cardinalidade=30, graficos_por_linha=2)

'''


  ![image](https://github.com/user-attachments/assets/69582c98-312e-4b14-a407-b6f3b42e6891)


  ![image](https://github.com/user-attachments/assets/63a7b417-e136-4f48-ae4c-1aae830dc3ad)


 # Principais Insights
1. Dias da Semana (dia_semana)
A maioria dos acidentes ocorre nos finais de semana (sábado e domingo), sugerindo um aumento no tráfego ou em atividades de risco durante esses dias.

2. Estados (UF)
Estados como Minas Gerais, Paraná e Santa Catarina lideram em frequência de acidentes, indicando possíveis focos de atenção para segurança em rodovias nessas regiões.

3. Causa dos Acidentes (causa_acidente)
"Colisão traseira" e "Tombamento" são causas recorrentes, sugerindo a necessidade de campanhas de conscientização e, possivelmente, de melhorias na sinalização e fiscalização.

4. Classificação dos Acidentes (classificacao_acidente)
Grande parte dos acidentes resulta em feridos leves. Entretanto, ainda há uma quantidade preocupante de casos graves e fatais, ressaltando a importância da segurança e prevenção no trânsito.

5. Fase do Dia (fase_dia)
Acidentes ocorrem com mais frequência durante o dia, especialmente em horários de amanhecer e anoitecer, que podem coincidir com picos de tráfego e condições de visibilidade variável.

6. Condições Meteorológicas (condicao_metereologica)
A maioria dos acidentes ocorre em condições de tempo claro, indicando que condições adversas, embora menos frequentes, podem potencializar a gravidade dos acidentes.

7. Tipo de Pista (tipo_pista)
Rodovias de pista simples concentram a maioria dos registros de acidentes, sugerindo um risco potencialmente maior ou uma maior prevalência desse tipo de pista nas áreas analisadas.

8. Tipo de Veículos (tipo_veiculo)
Automóveis e motocicletas são os veículos mais envolvidos em acidentes, refletindo sua popularidade e a exposição ao risco de motoristas de motocicletas.

9. Sexo
O sexo masculino é predominante entre as vítimas, sugerindo um perfil de risco maior associado a homens no contexto dos acidentes.


# Visualização no Mapa
Por fim, mapeamos os pontos de acidentes fatais nas rodovias brasileiras, incluindo locais com presença de radares. Essa visualização ajuda a entender a distribuição dos acidentes fatais e como esses pontos se relacionam com a presença de radares.
Legenda:
- Pins Azuis- Radares
- Pontos vermelhos- Vitimas fatais

![image](https://github.com/user-attachments/assets/550dafb3-a398-4fc4-b990-38e7f74eb3b5)



# Conclusão
Este projeto visa contribuir para uma maior compreensão dos fatores e padrões que influenciam a ocorrência de acidentes nas rodovias federais brasileiras. Com análises e visualizações, espero fornecer insights valiosos para a PRF e outros órgãos de segurança, ajudando a tomar decisões informadas e salvar vidas.
