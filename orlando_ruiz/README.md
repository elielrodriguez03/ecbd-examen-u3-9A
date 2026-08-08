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
1. Es un tipo de machine learning donde el modelo aprende a partir de ejemplos que ya tienen la respuesta correcta, para después predecir esa respuesta en datos nuevos.
2. Es la columna que el modelo intenta predecir.
3. Es una columna creada como variable objetivo que indica si una venta fue "alta" (1) o "no alta" (0)
4. X son las variables de entrada; Y es la variable objetivo, es decirc lo que el modelo intenta predecir.
5. Es como un tipo de aprendizaje supervisado donde la variable objetivo tiene categorias o clases.
6. Porque la variable objetivo solo puede tomar dos valores discretos (0 o 1), no un numero continuo.
7. Es un modelo que construye un arbol de decisiones, el cual dividde los datos en base a preguntas sobre las variables de entrada, hasta llegar a una predicción de clase.
8. Para poder evaluar el modelo con datos que no vio durante el entrenamiento, y asi saber si realmente aprendio algun patron o no.
9. Es el porcentaje de predicciones correctas del modelo sobre el total de casos evaluados.
10. Para mostrar cuántas predicciones fueron correctas e incorrectas por cada clase.
11. Es el método que entrena el modelo.
12. Es el método que genera predicciones del modelo ya entrenado sobre nuevos datos de entrada.
13. Porque los modelos de machine learning no pueden trabajar directamente con texto y el get_dummies() convierte columnas de texto,  en columnas numéricas de 0 y 1.
14. Por que get_dummies() puede generar columnas distintas en datos nuevos y lo que hace reindex() es que alinea esas columnas con las que el modelo uso al entrenar, rellenando con 0 lo que falte, para que el modelo pueda recibir los datos correctamente.
15. Porque el modelo puede equivocarse.
```

---

## Parte 2. Revision Del Dataset

Carga el dataset con pandas.

Realiza lo siguiente:

1. Muestra las primeras filas.
2. Revisa las columnas.
3. Muestra cuantas filas y columnas tiene.
4. Revisa si hay valores nulos.
5. Verifica que exista la columna `total_venta`.
6. Verifica que `total_venta` coincida con `cantidad * precio_unitario`.
7. Escribe una observacion breve sobre el estado del dataset.

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
2. Cuenta cuantas ventas quedaron como 1 y cuantas como 0.

Pregunta obligatoria:

```text
Por que venta_alta es la variable objetivo? Hay 60 filas, 10 columnas, sin nulos, total_venta sí existe y coincide al 100% con cantidad * precio_unitario
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
2. Crea `y`.
3. Convierte variables categoricas con `pd.get_dummies()`.
4. Guarda la lista de columnas generadas.
5. Muestra las primeras filas de `X` despues de `get_dummies()`.

Pregunta obligatoria:

```text
Por que no se debe usar total_venta como variable de entrada si venta_alta se creo a partir de total_venta?  Porque sería fuga de información
```

---

## Parte 5. Entrenamiento Y Evaluacion

Entrena un modelo con `DecisionTreeClassifier`.

Indicaciones:

1. Divide los datos en entrenamiento y prueba.
2. Usa 80% entrenamiento y 20% prueba.
3. Usa `random_state=42`.
4. Entrena el modelo.
5. Genera predicciones con los datos de prueba.
6. Calcula exactitud.
7. Muestra matriz de confusion.
8. Crea una tabla llamada `resultados_prueba` con:

```text
valor_real
prediccion
coincide
```

9. Cuenta cuantos aciertos y cuantos errores hubo.

Preguntas obligatorias:

1. Cual fue la exactitud? 1.0 (100%).
2. Cuantos aciertos tuvo el modelo? 12 aciertos.
3. Cuantos errores tuvo el modelo? 0 errores.
4. Que indica la matriz de confusion? Muestra aciertos y errores por clase: [[4,0],[0,8]] — 4 "no alta" y 8 "alta" bien clasificadas, sin errores.
5. Una buena exactitud significa que el modelo ya es perfecto? Explica. No. Aquí dio 1.0, pero con datos nuevos (categoría/ciudad desconocidas) el modelo sí falló después.

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
3. Guarda la lista de columnas usadas durante el entrenamiento.
4. Verifica que los archivos aparezcan en tu carpeta.

Preguntas obligatorias:

1. Para que sirve guardar el modelo? Para poder reutilizarlo después sin necesidad de volver a entrenarlo cada vez.
2. Para que sirve guardar las columnas del entrenamiento? Para saber exactamente qué columnas espera el modelo al recibir datos nuevos, y poder alinearlas con reindex()
3. Que problema puede aparecer si no guardas las columnas? Al usar get_dummies() en datos nuevos podrían generarse columnas distintas a las del entrenamiento, y sin las columnas originales guardadas el modelo fallaria

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

Importante:

El dataset `ventas_ecommerce_limpio.csv` se usa para entrenar.

Tus ventas nuevas son solamente para probar el modelo ya entrenado.

No las agregues al dataset de entrenamiento.

---

## Parte 8. Cargar Modelo Y Predecir

Usa el modelo guardado para predecir tus ventas nuevas.

Indicaciones:

1. Carga `modelo_examen_venta_alta.pkl`.
2. Carga `columnas_examen_modelo.pkl`.
3. Carga `examen_ventas_nuevas.csv`.
4. Selecciona las mismas variables de entrada usadas en entrenamiento.
5. Aplica `pd.get_dummies()`.
6. Alinea columnas con `reindex()`.
7. Genera predicciones.
8. Agrega la columna:

```text
prediccion_venta_alta
```

9. Agrega la columna:

```text
interpretacion_prediccion
```

donde:

```text
0 = Venta no alta
1 = Venta alta
```

10. Guarda el resultado como:

```text
examen_predicciones.csv
```

Pregunta obligatoria:

```text
Que podria pasar si no usas reindex antes de predecir?  Las columnas generadas por get_dummies() en los datos nuevos probablemente no coincidirían en cantidad ni en orden con las columnas que el modelo espera, por lo que daria error.
``` 

---

## Parte 9. Auditoria Del Modelo

Revisa si las predicciones coinciden con una regla manual.

Indicaciones:

1. Calcula:

```text
total_estimado = cantidad * precio_unitario
```

2. Crea:

```text
venta_alta_real_estimada
```

usando:

```text
Si total_estimado >= 1000, entonces 1
Si total_estimado < 1000, entonces 0
```

3. Compara:

```text
prediccion_venta_alta
venta_alta_real_estimada
```

4. Crea la columna:

```text
coincide
```

5. Cuenta cuantas predicciones coincidieron y cuantas no.
6. Filtra las ventas que no coincidieron.
7. Revisa si los errores estan cerca del limite de 1000.
8. Guarda de nuevo `examen_predicciones.csv` con las columnas de auditoria.

Preguntas obligatorias:

1. Cuantas ventas nuevas evaluaste? 10 ventas nuevas.
2.  Cuantas fueron predichas como venta alta? 4 ventas (5001, 5002, 5003, 5004).
3. Cuantas fueron predichas como venta no alta? 6 ventas (5005, 5006, 5007, 5008, 5009 y 5010).
4. Cuantas coincidieron con la regla manual? 9 de las 10 predicciones coincidieron.
5. Cuantas no coincidieron? 1 predicción no coincidió.
6. Que ventas no coincidieron? La venta 5010
7. Los errores estuvieron cerca del limite de 1000? Si, el único error está dentro del rango de ±200 del límite de 1000.
8. Que paso con la categoria nueva? La categoría "Electrodomesticos"no existía en el entrenamiento. Al aplicar reindex(), esa columna se descartó y la fila quedó con todas las columnas de categoría en 0.
9. Que paso con la ciudad nueva? La ciudad "Cuautla" no está en columnas_examen_modelo, así que tras reindex() esa fila quedó con todas las columnas de ciudad en 0.

---

## Resumen Para README

Completa esta seccion al final:

```text
Nombre: Orlando Ruiz Santos
Grupo: 9° A
Materia: Extracción de conocimiento de Bases de Datos
Exactitud obtenida: 1.0 (100%)
Ventas nuevas evaluadas: 10
Coincidencias: 9
Errores: 1
Conclusion breve: El modelo funcionó bien en las pruebas iniciales, pero al usarlo con ventas nuevas se equivocó en 1 de 10, justo en una venta con una ciudad que no conocía y un monto cercano al límite. Esto me hizo ver que aunque el modelo tenga buena exactitud, no siempre acierta con datos que nunca ha visto.
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
