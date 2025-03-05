# dashboard-inventario-powerbi
Este proyecto utiliza Power BI y DAX para analizar la gestión de inventario de una empresa, evaluando el stock disponible, el consumo promedio y la duración del inventario en días.
📂 Archivos incluidos:

    📊 Dashboard en Power BI con visualizaciones clave
    📄 Medidas DAX implementadas para análisis de stock
    📜 Dataset de Entradas y Salidas de Productos

🔍 Métricas Implementadas en DAX
📦 Inventario y Stock
Cantidad de productos = SUM(Registro_Entradas_Salidas[Existencia])
Inventario final = SUMX(Registro_Entradas_Salidas, Registro_Entradas_Salidas[Existencia] * RELATED(Precio_Compra[Precio de Compra]))

📉 Análisis de Consumo y Rotación
Duración de Inventario(días) = 
IF(HASONEVALUE(Promedio_Consumo_Ventas[Productos]), 
    DIVIDE([Inventario final], Promedio_Consumo_Ventas[Ventas promedio],0), 
    0
)
Ventas promedio = SUM(Promedio_Consumo_Ventas[Ventas promedio diarias])
Consumo promedio = SUMX(Promedio_Consumo_Ventas, VALUE(Promedio_Consumo_Ventas[Consumo promedio diario]))

⚠️ Estatus y Alertas
Stock mínimo = 
IF(HASONEVALUE(Promedio_Consumo_Ventas[Productos]), 
    [Tiempo de entrega habitual del proveedor] * Promedio_Consumo_Ventas[consumo promedio], 
    0
)
Stock Máximo = [Stock mínimo] * 2
Stock de Seguridad = 
IF(HASONEVALUE(Promedio_Consumo_Ventas[Productos]), 
    [Stock mínimo] + ([Tiempo de entrega con retraso] - [Tiempo de entrega habitual del proveedor]) * Promedio_Consumo_Ventas[consumo promedio], 
    0
)
Estatus = IF([Cantidad de productos] <= [Stock mínimo], "Fuera de Stock", "Stock Óptimo")
Estatus con emoji = IF([Cantidad de productos] <= [Stock mínimo], "❗", "✅")

📈 Parámetros de Tiempo de Entrega
Tiempo de entrega habitual del proveedor = 10
Tiempo de entrega con retraso = 15

💡 Insights Clave Extraídos

✔️ Reducción de stock de $45.8M debido a alta demanda
✔️ Ajuste negativo de -284 unidades, indicando posible merma
✔️ Stock de seguridad optimizado para reducir riesgos de quiebres de stock
✔️ Identificación de productos de mayor valor: Impresoras y Laptops



