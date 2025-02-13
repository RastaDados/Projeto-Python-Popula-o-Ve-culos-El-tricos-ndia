<h1>Projeto de Dados - População de veículos elétricos nos Estados Unidos</h1>

<h1>Introdução</h1>

Este projeto exibe informções e análises da população de veículos elétricos nos Estados Unidos.

<hr>
<br>

<h1>Objetivo</h1>

O objetivo principal do projeto é fornecer uma visão detalhada sobre a distribuição dos veículos elétricos, suas características, preços, autonomia e análise geográfica da frota.

<hr>
<br>

<h1>Entendendo a base de dados</h1>
Este conjunto de dados fornece informações abrangentes sobre a população de Veículos Elétricos (VEs) em várias regiões e períodos de tempo. Inclui várias medições relacionadas ao registro de veículos, elegibilidade para programas de veículos de combustível alternativo limpo e sua autonomia elétrica e preços. Os dados abrangem vários estados e marcas de veículos, fornecendo uma visão ampla do mercado de VEs e seu desenvolvimento.

<h3><b>Colunas:</b></h3>

<b>Estado:</b> O estado dos EUA onde o veículo está registrado.

<b>Ano do modelo:</b> ano em que o modelo do veículo foi fabricado.

<b>Marca:</b> O fabricante do veículo, como:
TESLA

- BMW

- CHEVROLET

- VOLKSWAGEN

- RIVIAN

- TOYOTA

- NISSAN

- … (e mais)

<b>Tipo de veículo elétrico:</b> Tipo de veículo elétrico, categorizado como:

Veículo elétrico a bateria (BEV)

Veículo elétrico híbrido plug-in (PHEV)

<b>Alcance elétrico:</b> Alcance do veículo somente com energia elétrica, medido em milhas.

Preço de varejo sugerido pelo fabricante (MSRP) do veículo .

<b>Distrito legislativo:</b> distrito legislativo associado ao local de registro do veículo.

<b>Elegibilidade CAFV Simples:</b> Um indicador simplificado da elegibilidade do veículo para o programa Clean Alternative Fuel Vehicle (CAFV). Os valores possíveis são:

Veículos com combustível alternativo limpo elegíveis

Elegibilidade desconhecida devido à autonomia da bateria não pesquisada

Não elegível devido à baixa autonomia da bateria



<h1>Tecnologias Utilizadas</h1>

<b>Linguagem:</b> Python

<br>

<h3><b>Bibliotecas:</b></h3>

<b>Pandas:</b> Manipulação e limpeza dos dados

<b>Plotly.express:</b> Criação de gráficos 

<b>Streamlit:</b> Construção do dashboard 

<b>Numpy:</b> Suporte a operações matemáticas

<hr>
<br>

<h1>Processo de ETL</h1>

<h3><b>Extração</b></h3>

O dataset utilizado tem o nome de EV_Population.csv, contendo informações sobre a população de veículos elétricos registrados nos EUA. A base de dados é carregada no Streamlit através da biblioteca Pandas.

```python
@st.cache_data
def load_data():
    file_path = "EV_Population.csv"
    df = pd.read_csv(file_path)
```

<h3><b>Transformação</b></h3>

Remoção de espaços extras nos nomes das colunas

Tratamento de valores nulos

Verificação de colunas disponíveis para evitar erros

```python
    df.columns = df.columns.str.strip()  # Remover espaços extras nos nomes das colunas
    df.dropna(inplace=True)  # Remover valores nulos
    return df
```

<h3><b>Carga</b></h3>

Os dados tratados são utilizados para gerar os gráficos no dashboard do Streamlit.

```python
df = load_data()
```

<hr>
<br>

<h1>Estrutura do Dashboard</h1>

O Dashboard é dividido em três páginas, cada página foca em um aspecto específico da análise.

<h2><b>Distribuição de Veículos</b></h2>

Mostra a distribuição de veículos elétricos por fabricante, tipo de veículo e ano do modelo.

<h3><b>Número de veículos por fabricante</h3></b>
Exibe os fabricantes que possuem mais veículos registrados.

```python
fabricantes = df["Make"].value_counts().reset_index()
    fabricantes.columns = ["Fabricante", "Quantidade"]

    fig1 = px.bar(fabricantes, x="Fabricante", y="Quantidade",
                  labels={"Fabricante": "Fabricante", "Quantidade": "Número de Veículos"},
                  title="Veículos por Fabricante")
    st.plotly_chart(fig1)
```
![Graf 1 pag 1](https://github.com/user-attachments/assets/8a26eceb-7ba3-486a-ac2d-1314daef64bd)

<br>

<h3><b>Distribuição por tipo de veículo elétrico</h3></b>
Mostra a proporção entre os diferentes tipos de veículos elétricos.

```python
 fig2 = px.pie(df, names="Electric Vehicle Type", title="Distribuição por Tipo de Veículo Elétrico")
    st.plotly_chart(fig2)
```
![Graf 2 pag 1](https://github.com/user-attachments/assets/faa7f4d3-041f-4f5a-9138-1aaf170b6246)

<br>

<h3><b>Distribuição de veículos por ano de modelo</b></h3>
Exibe a evolução dos veículos elétricos ao longo dos anos.

```python
fig3 = px.histogram(df, x="Model Year", title="Distribuição dos Veículos por Ano de Modelo")
    st.plotly_chart(fig3)

elif escolha == "Autonomia e Preço":
    st.title("Relação entre Autonomia e Preço")
```
![Graf 3 pag 1](https://github.com/user-attachments/assets/fe3ee820-126e-42ac-8596-4548c5fb264c)


<hr>
<br>

<h2><b>Autonomia e Preço</b></h2>
Explorar a relação entre autonomia dos veículos e seu preço.

<h2><b>Preço vs Autonomia</b></h2>
Mostra como o preço de um veículo elétrico varia com sua autonomia.

```python
 fig4 = px.scatter(df, x="Electric Range", y="Base MSRP", color="Make",
                      title="Preço vs Autonomia", labels={"Electric Range": "Autonomia (milhas)", "Base MSRP": "Preço ($)"})
    st.plotly_chart(fig4)
    
```
![Graf 1 pag 2](https://github.com/user-attachments/assets/7ecdd283-962b-4558-92bc-2c62bb032832)

<br>

<h3><b>Distribuição da autonomia por tipo de veículo</b></h3>
Exibe a dispersão dos valores de autonomia entre diferentes categorias de veículos elétricos.

```python
fig5 = px.box(df, x="Electric Vehicle Type", y="Electric Range", title="Distribuição da Autonomia por Tipo de Veículo")
    st.plotly_chart(fig5)
```
![Graf 2 pag 2](https://github.com/user-attachments/assets/0f796cb4-d03d-44fc-b144-c1a4084ede6c)

<h3><b>Evolução da autonomia ao longo dos anos</b></h3>
Mostra como a autonomia dos veículos elétricos tem evoluído ao longo dos anos de fabricação.

```python
 fig6 = px.scatter(df, x="Model Year", y="Electric Range", color="Make",
                      title="Evolução da Autonomia ao Longo dos Anos", labels={"Model Year": "Ano do Modelo", "Electric Range": "Autonomia (milhas)"})
    st.plotly_chart(fig6)
```
![Graf 3 pag 2](https://github.com/user-attachments/assets/d28bec09-f525-4f76-908f-b8cf9564b50e)

<hr>
<br>

<h2><b>Análise Geográfica</b></h2>
Explorar a distribuição dos veículos elétricos por região.

<h3><b>Distribuição por estado</b></h3>
Indica quais estados possuem mais veículos elétricos registrados.

```python
estados = df["State"].value_counts().reset_index()
    estados.columns = ["Estado", "Quantidade"]

    fig7 = px.bar(estados, x="Estado", y="Quantidade",
                  labels={"Estado": "Estado", "Quantidade": "Número de Veículos"},
                  title="Veículos por Estado")
    st.plotly_chart(fig7)
```
![Graf 1 pag 3](https://github.com/user-attachments/assets/ddb17e72-da01-4c12-b4f7-284a38e224af)

<br>

<h3><b>Distribuição por distrito legislativo</b></h3>
Permite avaliar a presença de veículos elétricos em diferentes regiões legislativas.

```python
fig8 = px.histogram(df, x="Legislative District", title="Distribuição por Distrito Legislativo")
    st.plotly_chart(fig8)   
```
![Graf 2 pag 3](https://github.com/user-attachments/assets/8bff5bfe-c1a0-4030-8bf6-1d480cf571b5)

<hr>
<br>

<h1>Possíveis Pontos a Melhorar</h1>

Incluir análise de tendências futuras utilizando Machine Learning.

Adicionar integração com APIs para obter dados atualizados.

Melhorar o layout visual do dashboard para torná-lo ainda mais intuitivo.

<hr>
