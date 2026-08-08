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

## Parte 1. Conceptos

Responde con tus palabras en esta seccion o en una celda Markdown de tu notebook.

1. Que es aprendizaje supervisado?
es cuando al modelo le cargas respuestas correctas para que valla aprendiendo el patron

2. Que es una variable objetivo?
es aquel dato el cual deseas predecir por ejemplo una venta alta o no alta

3. Que significa `venta_alta`?
es una variable objetivo la cual podemos desear predecir

4. Que diferencia hay entre `X` e `y`?
x  variables de entrada
y son las variables objetivo

5. Que es clasificacion?
es cuando separas los datos en categorias para poder predecir, podria ser (0 y 1)

6. Por que este problema es de clasificacion?
porque se intenta predecir si una venta es alta o no alta 

7. Que hace `DecisionTreeClassifier`?
es un modelo tipo arbol, va preguntando si/no hasta decidir.

8. Para que sirve separar entrenamiento y prueba?
para primero ir preparando al modelo para que aprenda de patrones y despues poder predecir 

9. Que significa exactitud?
cuando el modelo acierta a algo que se predijo 

10. Para que sirve una matriz de confusion?
detalle de en que se equivoco el
modelo

11. Que significa `fit()`?
se usa para entrenar el modelo

12. Que significa `predict()`?
genera predicciones 

13. Por que se usa `pd.get_dummies()`?
convierte texto en columnas numericas.
    
14. Por que se usa `reindex()` al predecir ventas nuevas?
porque acomoda columnas nuevas igual que las del entrenamiento.

15. Por que una prediccion no es una verdad absoluta?
el modelo puede fallar, sobre todo con pocos datos o casos nuevos
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
# 7. Escribe una observacion breve sobre el estado del dataset.
# existe una columna que no coincide ya que en precio_unitario esta en 180 y en total_venta esta en 360
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
Por que venta_alta es la variable objetivo?
```
# Por que no se debe usar total_venta como variable de entrada si venta_alta se creo a partir de total_venta?
# porque la variable que se intenta predecir es venta_alta en este caso es la variable objetivo
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
Por que no se debe usar total_venta como variable de entrada si venta_alta se creo a partir de total_venta?
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

1. Cual fue la exactitud?
2. Cuantos aciertos tuvo el modelo?
3. Cuantos errores tuvo el modelo?
4. Que indica la matriz de confusion?
5. Una buena exactitud significa que el modelo ya es perfecto? Explica.

---
# 1. Cual fue la exactitud?
# 1.0 de exactitud
# 2. Cuantos aciertos tuvo el modelo?
# 12 aciertos en total 
# 3. Cuantos errores tuvo el modelo?
# 0 errores 
# 4. Que indica la matriz de confusion?
# muestra el resultado de las predicciones clasificó correctamente 4 
# de la clase negativa 0 y 8 de la clase positiva 1
# 5. Una buena exactitud significa que el modelo ya es perfecto? Explica.
# No Una exactitud del 100% en un conjunto de prueba pequeño

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

1. Para que sirve guardar el modelo?
2. Para que sirve guardar las columnas del entrenamiento?
3. Que problema puede aparecer si no guardas las columnas?

---
1. Para que sirve guardar el modelo?
para tener prueba del modelo y usarlo para otra prediccion 
2. Para que sirve guardar las columnas del entrenamiento?
para que asi el modelo tenga los datos de entrenamiento 
3. Que problema puede aparecer si no guardas las columnas?
el problema es que si no se encuentra la columna que se intenta predecir no tendra datos el modelo

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
Que podria pasar si no usas reindex antes de predecir?
```
# Que podria pasar si no usas reindex antes de predecir?
# que las columnas nuevas no quedaran alineadas con las columnas del entrenamiento
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

1. Cuantas ventas nuevas evaluaste?
2. Cuantas fueron predichas como venta alta?
3. Cuantas fueron predichas como venta no alta?
4. Cuantas coincidieron con la regla manual?
5. Cuantas no coincidieron?
6. Que ventas no coincidieron?
7. Los errores estuvieron cerca del limite de 1000?
8. Que paso con la categoria nueva?
9. Que paso con la ciudad nueva?

---


1. Cuantas ventas nuevas evaluaste?
10 ventas nuevas

2. Cuantas fueron predichas como venta alta?
6 fueron predichas como venta alta

3. Cuantas fueron predichas como venta no alta?
4 fueron predichas como venta no alta

4. Cuantas coincidieron con la regla manual?
solo las que tienen la misma categoria que existia antes 

5. Cuantas no coincidieron?
4 no coincidieron

6. Que ventas no coincidieron?
los clientes con los id 2004, 2005, 2007, 2009

7. Los errores estuvieron cerca del limite de 1000?
Si los errores estan en ese rango

8. Que paso con la categoria nueva?
Al aplicar pd.get_dummies() y luego .reindex cualquier categoria nueva que no existia en el entrenamiento Electrodomesicos
fue ignorada sus columnas tomaron valor 0, por lo que el modelo no pudo usar esa informacion

9. Que paso con la ciudad nueva?
los nombres de ciudades nuevas o combinadas Jiutepec, Nopalera Tetecalita,Cuernavaca etc
no existian en las columnas del modelo original, asi que al hacer el reindex se rellenaron con 0

## Resumen Para README

Completa esta seccion al final:

```text
Nombre:
Grupo:
Materia:
Exactitud obtenida:
Ventas nuevas evaluadas:
Coincidencias:
Errores:
Conclusion breve:
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
