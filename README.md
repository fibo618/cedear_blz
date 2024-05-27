
# Leer archivo Excel con los datos principales
filename = 'blz.xlsx'  # Reemplaza con el nombre de tu archivo
df = pd.read_excel(filename)

# Leer archivo Excel con la lista de tickers a verificar
file_CED = 'List_CEDEARS.xlsx'  # Reemplaza con el nombre de tu archivo
df_CED = pd.read_excel(file_CED)

# Suponemos que la columna con los tickers en df_CED se llama 'Ticker'
tickers_a_verificar = df_CED['Ticker'].unique().tolist()  # Convertimos a lista

# Eliminar filas donde 'Estado' es 'Cancelada' o 'Rechazada'
df = df[~df['Estado'].isin(['Cancelada', 'Rechazada'])]

# Lista de etiquetas de columnas a eliminar
columnas_a_eliminar = ['id Orden', 'Precio Operado', 'Cantidad Operada', 'Hora', 'Monto', 'Precio', 'Moneda']

# Eliminar las columnas especificadas
df = df.drop(columns=columnas_a_eliminar)

# Paso 2: Definir una lista para almacenar los DataFrames seleccionados
selected_dfs = []

# Paso 3: Iterar por cada valor en la columna 'Ticker' del DataFrame principal y verificar si está en la lista de tickers a verificar
unique_tickers = df['Ticker'].unique()

for ticker in unique_tickers:
    if ticker in tickers_a_verificar:
        # Paso 4: Guardar en la lista todas las filas que contengan el valor actual en la columna 'Ticker'
        selected_rows = df[df['Ticker'] == ticker]
        selected_dfs.append(selected_rows)

# Paso 5: Concatenar los DataFrames seleccionados en uno solo si hay alguno seleccionado
if selected_dfs:
    final_df = pd.concat(selected_dfs)
    # Paso 6: Imprimir el DataFrame final
    print(final_df)
else:
    print("No se seleccionaron DataFrames.")
