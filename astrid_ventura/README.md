# Examen Unidad 3: Modelos Supervisados con Ventas

## Asignatura
Extraccion de conocimiento en bases de datos

## Grupo
9A

## Objetivo

Desarrollar un flujo practico de aprendizaje supervisado usando el dataset de ventas.

Este examen evalua que puedas:

- cargar y revisar un dataset;
- crear una variable objetivo;
- preparar variables de entrada;
- entrenar un modelo supervisado de clasificacion;
- evaluar resultados;
- guardar y cargar el modelo;
- preparar datos nuevos;
- alinear columnas;
- generar predicciones;
- auditar resultados;
- explicar limitaciones.

No se evaluara ETL.  
No se evaluara KMeans.  
No se evaluara aprendizaje no supervisado.

---

## Reglas De Entrega

El examen se entrega en este repositorio central.

Cada estudiante debe crear su propia rama.

Formato obligatorio de la rama:

```text
examen-u3-nombre-apellido
```

Ejemplo:

```text
examen-u3-ana-lopez
```

Dentro de tu rama debes crear una carpeta con tu nombre:

```text
nombre_apellido/
```

Ejemplo:

```text
ana_lopez/
```

Todo tu examen debe quedar dentro de tu carpeta.

No subas archivos en la raiz del repositorio.

---

## Regla Importante Sobre Main

Esta prohibido subir cambios directamente a `main`.

Si subes tu entrega a `main`, el examen se considera no entregado correctamente.

```text
Push a main = 0
```

Tambien esta prohibido:

- modificar archivos de otros companeros;
- borrar archivos del repositorio;
- subir tu entrega fuera de tu carpeta;
- cambiar el dataset oficial del repositorio base.

---

## Archivos Base Del Repositorio

El repositorio ya incluye:

```text
README.md
ventas_ecommerce_limpio.csv
AYUDA_CODIGO.txt
```

El archivo oficial de datos es:

```text
ventas_ecommerce_limpio.csv
```

Debes usar ese archivo para entrenar el modelo.

No uses otro dataset.

---

## Entregables Finales

Dentro de tu carpeta personal deben quedar estos archivos:

```text
nombre_apellido/
|-- README.md
|-- ventas_ecommerce_limpio.csv
|-- Examen_U3_ModeloSupervisado_NombreApellido.ipynb
|-- modelo_examen_venta_alta.pkl
|-- columnas_examen_modelo.pkl
|-- examen_ventas_nuevas.csv
|-- examen_predicciones.csv
```

Ejemplo:

```text
ana_lopez/
|-- README.md
|-- ventas_ecommerce_limpio.csv
|-- Examen_U3_ModeloSupervisado_AnaLopez.ipynb
|-- modelo_examen_venta_alta.pkl
|-- columnas_examen_modelo.pkl
|-- examen_ventas_nuevas.csv
|-- examen_predicciones.csv
```

El notebook debe estar ejecutado y ordenado.

---

## Flujo Inicial

Al inicio del examen:

1. Clona este repositorio.
2. Crea tu rama personal.
3. Crea tu carpeta personal.
4. Copia `ventas_ecommerce_limpio.csv` dentro de tu carpeta.
5. Copia este `README.md` dentro de tu carpeta.
6. Desconecta internet cuando el docente lo indique.
7. Trabaja el examen de forma local.

Comandos de referencia:

```bash
git checkout -b examen-u3-nombre-apellido
```

---

## Parte 1. Conceptos

Responde con tus palabras en esta seccion o en una celda Markdown de tu notebook.

1. Que es aprendizaje supervisado?
2. Que es una variable objetivo?
3. Que significa `venta_alta`?
4. Que diferencia hay entre `X` e `y`?
5. Que es clasificacion?
6. Por que este problema es de clasificacion?
7. Que hace `DecisionTreeClassifier`?
8. Para que sirve separar entrenamiento y prueba?
9. Que significa exactitud?
10. Para que sirve una matriz de confusion?
11. Que significa `fit()`?
12. Que significa `predict()`?
13. Por que se usa `pd.get_dummies()`?
14. Por que se usa `reindex()` al predecir ventas nuevas?
15. Por que una prediccion no es una verdad absoluta?

### Respuestas Del Estudiante

Escribe aqui tus respuestas:

```text
1. Que el modelo aprenda y se evalue con datos conocidos, comprobando las respuestas de forma manual o con alguna regla
2. El dato o variable que el modelo debe aprender a predecir
3. La variable objetivo, clasificando ventas en altas (> 1000) o no altas
4. X son las variables de entrada (pistas para que el modelo haga las predicciones) y la variable objetivo (lo que se desea predecir)
5. Clasificar en categorias (venta alta o no alta)
6. Por que en este caso se clasifican las ventas en altas (con un 1) o no altas (con un 0) y no se predice por ejemplo el precio de la venta o un numero exacto
7. Hace preguntas como un arbol de desicion de si y no hasta llegar a un resultado
8. Para que con unos datos el modelo aprenda un patron y con los otros evalue para ver si aprendio a predecir
9. Que tan exacto es el modelo al predecir datos
10. Para ver mediante una matriz en que falllo, que tipos de errores tuvo el modelo etc.
11. es la funcion con la que se entrena el modelo
12. Es la funcion con la que el modelo predice
13. Para convertir valores a numericos y que el modelo pueda usarlos
14. Para alinear los nuevos datos (nuevas ventas) a las mismas columnas con las que trabaja el modelo (mismo tamaño y orden)
15. Por que el modelo puede fallar, mas si no tiene varios ejemplos de entrenamiento o en casos que no vio antes o se acercan al limite
```

---

## Parte 2. Revision Del Dataset

Carga el dataset con pandas.

Realiza lo siguiente:
import pandas as pd
import joblib

from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score, confusion_matrix

df = pd.read_csv("ventas_ecommerce_limpio.csv")

1. Muestra las primeras filas.
df.head()

2. Revisa las columnas.
df.columns

3. Muestra cuantas filas y columnas tiene.
df.shape

4. Revisa si hay valores nulos.
print (df.isnull().sum()
)
5. Verifica que exista la columna `total_venta`.

"total_venta" in df.columns

6. Verifica que `total_venta` coincida con `cantidad * precio_unitario`.

df["check_total"] = df["cantidad"] * df["precio_unitario"]
coincide_total = (df["check_total"] == df["total_venta"]).all()
print("Coincide en todas las filas:", coincide_total)
df.drop(columns=["check_total"], inplace=True)

7. Escribe una observacion breve sobre el estado del dataset.
El dataset tiene 60 filas y 10 columnas, no contiene valores nulos en ninguna columna, y la columna `total_venta` coincide en el 100% de las filas con `cantidad * precio_unitario`. Esto confirma que el dataset esta limpio y es consistente, listo para construir la variable objetivo y entrenar el modelo.
---

## Parte 3. Variable Objetivo

Crea la columna:

```text
venta_alta
```

Regla:

```text
Si total_venta >= 1000, venta_alta = 1
Si total_venta < 1000, venta_alta = 0
```

Indicaciones:

1. Puedes usar una funcion normal o una lambda.
def clasificar_venta(total):
    return 1 if total >= 1000 else 0

df["venta_alta"] = df["total_venta"].apply(clasificar_venta)

2. Cuenta cuantas ventas quedaron como 1 y cuantas como 0.
print(df["venta_alta"].value_counts())

Pregunta obligatoria:

```text
Por que venta_alta es la variable objetivo?
Por que se quiere aprender a predecir si una venta sera alta o no
```

---

## Parte 4. Variables De Entrada

Usa como variables de entrada:

```text
cantidad
precio_unitario
categoria
metodo_pago
ciudad
```

Usa como variable objetivo:

```text
venta_alta
```

Indicaciones:

1. Crea `X`.
columnas_entrada = ["cantidad", "precio_unitario", "categoria", "metodo_pago", "ciudad"]

X = df[columnas_entrada]

2. Crea `y`.
y = df["venta_alta"]

3. Convierte variables categoricas con `pd.get_dummies()`.
X = pd.get_dummies(X)

4. Guarda la lista de columnas generadas.
columnas_modelo = X.columns.tolist()


5. Muestra las primeras filas de `X` despues de `get_dummies()`.
print(columnas_modelo)
X.head()

Pregunta obligatoria:

```text
Por que no se debe usar total_venta como variable de entrada si venta_alta se creo a partir de total_venta?
Si el modelo tuviera `total_venta` como entrada, podria "hacer trampa" aprendiendo la regla exacta en lugar de encontrar patrones reales en las demas variables.
```

---

## Parte 5. Entrenamiento Y Evaluacion

# Entrena un modelo con `DecisionTreeClassifier`.

# Indicaciones:

1. Divide los datos en entrenamiento y prueba.
2. Usa 80% entrenamiento y 20% prueba.
3. Usa `random_state=42`.
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

4. Entrena el modelo.
modelo = DecisionTreeClassifier(random_state=42)
modelo.fit(X_train, y_train)

5. Genera predicciones con los datos de prueba.
y_pred = modelo.predict(X_test)
y_pred

6. Calcula exactitud.
exactitud = accuracy_score(y_test, y_pred)
print("Exactitud:", exactitud)

7. Muestra matriz de confusion.
matriz = confusion_matrix(y_test, y_pred)
print("Matriz de confusion:")
print(matriz)

8. Crea una tabla llamada `resultados_prueba` con:
```text
valor_real
prediccion
coincide
```

resultados_prueba = pd.DataFrame({
    "valor_real": y_test.values,
    "prediccion": y_pred
})
resultados_prueba["coincide"] = resultados_prueba["valor_real"] == resultados_prueba["prediccion"]
resultados_prueba


9. Cuenta cuantos aciertos y cuantos errores hubo.
aciertos = resultados_prueba["coincide"].sum()
errores = (~resultados_prueba["coincide"]).sum()
print("Aciertos:", aciertos)
print("Errores:", errores)

Preguntas obligatorias:

1. Cual fue la exactitud? La exactitud fue de 1.0 (100%)
2. Cuantos aciertos tuvo el modelo? 12 aciertos, de los 12 datos de prueba (20% de 60 filas = 12 filas)
3. Cuantos errores tuvo el modelo? 0 errores
4. Que indica la matriz de confusion? 
La matriz fue `[[4, 0], [0, 8]]`: de las 4 ventas reales no altas, el modelo predijo correctamente las 4 como no altas (0 falsos positivos); de las 8 ventas reales altas, el modelo predijo correctamente las 8 como altas (0 falsos negativos). En este conjunto de prueba el modelo no cometio ningun error
5. Una buena exactitud significa que el modelo ya es perfecto? Explica.
Una exactitud alta en un conjunto de prueba pequeno no garantiza que el modelo funcione igual de bien con datos nuevos, con categorias o ciudades no vistas, o con casos cercanos al limite de 1000

---

## Parte 6. Guardar Modelo Y Columnas

Guarda:

```text
modelo_examen_venta_alta.pkl
columnas_examen_modelo.pkl
```

Indicaciones:

1. Usa `joblib`.
2. Guarda el modelo entrenado.
joblib.dump(modelo, "modelo_examen_venta_alta.pkl")

3. Guarda la lista de columnas usadas durante el entrenamiento.
joblib.dump(columnas_modelo, "columnas_examen_modelo.pkl")

4. Verifica que los archivos aparezcan en tu carpeta.
import os
print(os.listdir("."))

Preguntas obligatorias:

1. Para que sirve guardar el modelo? Para poder reutilizarlo sin entrelarlo cada vez
2. Para que sirve guardar las columnas del entrenamiento?  Para saber exactamente que variables (y en que orden) espera el modelo al momento de predecir, lo que permite alinear correctamente los datos nuevos antes de pasarlos al modelo
3. Que problema puede aparecer si no guardas las columnas?Si no se guardan, al aplicar `get_dummies()` sobre datos nuevos se podrian generar columnas distintas a las del entrenamiento (por ejemplo, si aparece una categoria o ciudad nueva, o si falta alguna que si estaba antes)

---

## Parte 7. Ventas Nuevas

Crea un archivo llamado:

```text
examen_ventas_nuevas.csv
```

Debe contener al menos 10 ventas nuevas.

Columnas obligatorias:

```text
id_venta
fecha
cliente
producto
categoria
cantidad
precio_unitario
metodo_pago
ciudad
```

Condiciones obligatorias:

1. Incluye al menos 4 ventas claramente altas.
2. Incluye al menos 4 ventas claramente no altas.
3. Incluye al menos 2 ventas cercanas al limite de 1000.
4. Incluye al menos 1 categoria nueva.
5. Incluye al menos 1 ciudad nueva.
6. No copies exactamente las ventas del pre examen.
7. No uses las mismas ventas que otro companero.

ventas_nuevas = pd.DataFrame([
    {"id_venta": 2001, "fecha": "2026-08-01", "cliente": "Ana Bermudez", "producto": "Laptop Gamer",
     "categoria": "Electronica", "cantidad": 1, "precio_unitario": 1850, "metodo_pago": "Tarjeta", "ciudad": "Cuernavaca"},
    {"id_venta": 2002, "fecha": "2026-08-01", "cliente": "Luis Cabrera", "producto": "Refrigerador",
     "categoria": "Electrodomesticos", "cantidad": 1, "precio_unitario": 1600, "metodo_pago": "Transferencia", "ciudad": "Jiutepec"},
    {"id_venta": 2003, "fecha": "2026-08-02", "cliente": "Sofia Nava", "producto": "PC Escritorio",
     "categoria": "Electronica", "cantidad": 1, "precio_unitario": 1200, "metodo_pago": "Tarjeta", "ciudad": "Temixco"},
    {"id_venta": 2004, "fecha": "2026-08-02", "cliente": "Pedro Solis", "producto": "Sofa",
     "categoria": "Muebles", "cantidad": 1, "precio_unitario": 2100, "metodo_pago": "Transferencia", "ciudad": "Cuautla"},
    {"id_venta": 2005, "fecha": "2026-08-03", "cliente": "Laura Cano", "producto": "Mouse Pad",
     "categoria": "Accesorios", "cantidad": 2, "precio_unitario": 120, "metodo_pago": "Efectivo", "ciudad": "Cuernavaca"},
    {"id_venta": 2006, "fecha": "2026-08-03", "cliente": "Carlos Rios", "producto": "Playera Deportiva",
     "categoria": "Ropa", "cantidad": 3, "precio_unitario": 180, "metodo_pago": "Efectivo", "ciudad": "Jiutepec"},
    {"id_venta": 2007, "fecha": "2026-08-04", "cliente": "Maria Torres", "producto": "Cable HDMI",
     "categoria": "Accesorios", "cantidad": 2, "precio_unitario": 150, "metodo_pago": "Efectivo", "ciudad": "Emiliano Zapata"},
    {"id_venta": 2008, "fecha": "2026-08-04", "cliente": "Jorge Mata", "producto": "Cargador",
     "categoria": "Accesorios", "cantidad": 2, "precio_unitario": 380, "metodo_pago": "Tarjeta", "ciudad": "Temixco"},
    {"id_venta": 2009, "fecha": "2026-08-05", "cliente": "Valeria Ruiz", "producto": "Bocina Bluetooth",
     "categoria": "Electronica", "cantidad": 1, "precio_unitario": 1000, "metodo_pago": "Tarjeta", "ciudad": "Cuernavaca"},
    {"id_venta": 2010, "fecha": "2026-08-05", "cliente": "Diego Ponce", "producto": "Escritorio",
     "categoria": "Muebles", "cantidad": 1, "precio_unitario": 990, "metodo_pago": "Transferencia", "ciudad": "Cuautla"},
])

ventas_nuevas.to_csv("examen_ventas_nuevas.csv", index=False)
ventas_nuevas

Importante:

El dataset `ventas_ecommerce_limpio.csv` se usa para entrenar.

Tus ventas nuevas son solamente para probar el modelo ya entrenado.

No las agregues al dataset de entrenamiento.

---

## Parte 8. Cargar Modelo Y Predecir

Usa el modelo guardado para predecir tus ventas nuevas.

Indicaciones:

1. Carga `modelo_examen_venta_alta.pkl`.
modelo_cargado = joblib.load("modelo_examen_venta_alta.pkl")

2. Carga `columnas_examen_modelo.pkl`.
columnas_cargadas = joblib.load("columnas_examen_modelo.pkl")

3. Carga `examen_ventas_nuevas.csv`.
nuevas = pd.read_csv("examen_ventas_nuevas.csv")
nuevas.head()

4. Selecciona las mismas variables de entrada usadas en entrenamiento.
X_nuevas = nuevas[["cantidad", "precio_unitario", "categoria", "metodo_pago", "ciudad"]]

5. Aplica `pd.get_dummies()`.
X_nuevas = pd.get_dummies(X_nuevas)

6. Alinea columnas con `reindex()`.
X_nuevas = X_nuevas.reindex(columns=columnas_cargadas, fill_value=0)
X_nuevas.head()

7. Genera predicciones.
predicciones = modelo_cargado.predict(X_nuevas)

8. Agrega la columna:

```text
prediccion_venta_alta
```
nuevas["prediccion_venta_alta"] = predicciones
9. Agrega la columna:

```text
interpretacion_prediccion
```

donde:

```text
0 = Venta no alta
1 = Venta alta


```

nuevas["interpretacion_prediccion"] = nuevas["prediccion_venta_alta"].map({
    0: "Venta no alta",
    1: "Venta alta"
})

10. Guarda el resultado como:

```text
examen_predicciones.csv
```

nuevas.to_csv("examen_predicciones.csv", index=False)
nuevas

Pregunta obligatoria:

```text
Que podria pasar si no usas reindex antes de predecir? los datos nuevos podrian no coincidir en cantidad, nombre u orden con las columnas que el modelo uso durante el entrenamiento. 
```

---

## Parte 9. Auditoria Del Modelo

Revisa si las predicciones coinciden con una regla manual.

Indicaciones:

1. Calcula:

```text
total_estimado = cantidad * precio_unitario
```
nuevas["total_estimado"] = nuevas["cantidad"] * nuevas["precio_unitario"]

2. Crea:

```text
venta_alta_real_estimada
```

usando:

```text
Si total_estimado >= 1000, entonces 1
Si total_estimado < 1000, entonces 0
```
nuevas["venta_alta_real_estimada"] = nuevas["total_estimado"].apply(lambda t: 1 if t >= 1000 else 0)
nuevas[["id_venta", "total_estimado", "venta_alta_real_estimada"]]


3. Compara:

```text
prediccion_venta_alta
venta_alta_real_estimada
```

4. Crea la columna:

```text
coincide
```
nuevas["coincide"] = nuevas["prediccion_venta_alta"] == nuevas["venta_alta_real_estimada"]

nuevas[["id_venta", "categoria", "ciudad", "total_estimado",
        "prediccion_venta_alta", "venta_alta_real_estimada", "coincide"]]

5. Cuenta cuantas predicciones coincidieron y cuantas no.
nuevas["coincide"].value_counts()

6. Filtra las ventas que no coincidieron.
errores = nuevas[nuevas["coincide"] == False]
print(errores)


7. Revisa si los errores estan cerca del limite de 1000.
nuevas["distancia_a_1000"] = (nuevas["total_estimado"] - 1000).abs()
cerca_limite = nuevas[nuevas["distancia_a_1000"] <= 200]

cerca_limite

8. Guarda de nuevo `examen_predicciones.csv` con las columnas de auditoria.
nuevas.to_csv("examen_predicciones.csv", index=False)
nuevas

Preguntas obligatorias:

1. Cuantas ventas nuevas evaluaste? 10
2. Cuantas fueron predichas como venta alta? 7
3. Cuantas fueron predichas como venta no alta? 3
4. Cuantas coincidieron con la regla manual? 8/10
5. Cuantas no coincidieron? 2
6. Que ventas no coincidieron? los ids 2006 y 2010
7. Los errores estuvieron cerca del limite de 1000? solo uno de los dos: el id 2010 (total 990) esta muy cerca del limite de 1000
8. Que paso con la categoria nueva? La categoria `Ropa` no existia en el entrenamiento, asi que al aplicar `get_dummies()` y `reindex()` no se genero ninguna columna para ella: la fila quedo con todas las columnas de categoria en 0
9. Que paso con la ciudad nueva? La ciudad `Cuautla` tampoco existia en el entrenamiento, por lo que sus dos ventas (ids 2004 y 2010) tambien quedaron sin ninguna columna de ciudad activada tras el `reindex()`

---

## Resumen Para README

Completa esta seccion al final:

```text
Nombre: Astrid Valeria Ventura Gil
Grupo: A
Materia: Extrac. Conocimientos en Base de datos
Exactitud obtenida: 1.0
Ventas nuevas evaluadas: 10
Coincidencias:8
Errores: 2
Conclusion breve: El modelo evalua las ventas clasificandolas como alta, no alta dependiendo de los parametros con DesicionTreeClasifie una vez entrenado, con las nuevas ventas, tiene una exactitud actualmente del 100 %
```

---

## Penalizaciones

- Si subes a `main`, calificacion 0.
- Si no entregas notebook, calificacion maxima 50.
- Si no entregas archivos `.pkl`, calificacion maxima 75.
- Si no entregas `examen_predicciones.csv`, calificacion maxima 80.
- Si el notebook no esta ejecutado, calificacion maxima 70.
- Si no hay interpretacion escrita, calificacion maxima 75.
- Si se detecta copia entre notebooks, se revisara manualmente y puede anularse la parte copiada.
