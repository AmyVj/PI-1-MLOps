![Pandas](https://img.shields.io/badge/-Pandas-333333?style=flat&logo=pandas)
![Numpy](https://img.shields.io/badge/-Numpy-333333?style=flat&logo=numpy)
![Matplotlib](https://img.shields.io/badge/-Matplotlib-333333?style=flat&logo=matplotlib)
![Seaborn](https://img.shields.io/badge/-Seaborn-333333?style=flat&logo=seaborn)
![Scikitlearn](https://img.shields.io/badge/-Scikitlearn-333333?style=flat&logo=scikitlearn)
![FastAPI](https://img.shields.io/badge/-FastAPI-333333?style=flat&logo=fastapi)
![Docker](https://img.shields.io/badge/-Docker-333333?style=flat&logo=docker)
![Render](https://img.shields.io/badge/-Render-333333?style=flat&logo=render)

# Prueba de Concepto: PI/1 MLOps🚀

Hola a todos, bienvenidos soy Mel, 

## Introducción
¡Bienvenidos Este proyecto simula el papel de un ingeniero de MLOps, que combina las habilidades de un ingeniero de datos y un científico de datos, para trabajar en la plataforma de juegos mundialmente conocida, Steam. Se nos proporcionan datos y se nos pide desarrollar un Producto Mínimo Viable que incluya una API desplegada en la nube y la implementación de dos modelos de Machine Learning. Estos modelos realizarán un análisis de sentimientos sobre los comentarios de los usuarios de los juegos y recomendarán juegos basados en el nombre de un juego dado o en las preferencias de un usuario específico.


## Datos
Sumergámonos en la magia de los datos. Para hacer este proyecto, nos pasaron tres archivos JSON:

australian_user_reviews.json: comments de los users sobre los juegos, si los recomiendan o no, si son graciosos o no y otras estadísticas copadas.
australian_users_items.json: data sobre los juegos que juegan los users y cuánto tiempo pasan en cada uno.
output_steam_games.json: info sobre los juegos, como el título, el developer, el precio y más.
En el "Diccionario de datos" están todos los detalles por si querés validar.

## Transformaciones: De Datos a Acción

## ETL (Extracción, Transformación y Carga)

### Extracción de Datos
- Se recopilaron tres conjuntos de datos entregados en formato JSON.

### Transformación de Datos:
- Dos conjuntos de datos presentaban anidamiento, es decir, tenían columnas con diccionarios o listas de diccionarios.
- Se aplicaron diversas estrategias para desanidar los datos, convirtiendo las claves de esos diccionarios en columnas accesibles.
- Se realizaron acciones para rellenar valores nulos en variables necesarias para el proyecto.
- Columnas con muchos valores nulos o que no eran relevantes para el proyecto fueron eliminadas, optimizando así el rendimiento de la API.
- Todo el proceso de transformación se llevó a cabo utilizando la poderosa librería Pandas de Python.

### Carga de Datos
- Los datos transformados se prepararon para su posterior carga, asegurando que estuvieran listos para ser utilizados en el proyecto.

Los detalles del ETL se puede ver en 01a_ETL_steam_games, 01b_ETL_users_items y 01c_ETL_user_reviews


## Ingeniería de Características: Sentimientos en Juego

Para hacer nuestros datos más útiles, creamos una nueva característica llamada 'sentiment_analysis'. Esta característica clasifica los comentarios de los usuarios en tres categorías:

- 0 si es malo,
- 1 si es neutral o no hay comentario,
- 2 si es positivo.

Usamos una herramienta llamada TextBlob para analizar el sentimiento de los comentarios. Esta herramienta asigna un valor numérico a cada comentario, representando si es negativo, neutral o positivo. Para simplificar, si el valor está por debajo de -0.2, es negativo; por encima de 0.2, es positivo; y si está entre -0.2 y 0.2, es neutral.

Para optimizar el rendimiento de nuestra aplicación y tener en cuenta las limitaciones de almacenamiento, creamos dataframes auxiliares para cada función solicitada. Estos dataframes se guardaron en formato parquet, que comprime y codifica eficientemente los datos.

Todos los detalles de este proceso están en la carpeta Ingeniería de datos > EDA and Modelo de aprendizaje > 01d_Feature_eng.

## Análisis Exploratorio: Descubriendo Tesoros
Se realizó un Análisis Exploratorio de Datos (EDA) a los tres conjuntos de datos para identificar las variables útiles en la creación del modelo de recomendación. Se utilizó Pandas para la manipulación de los datos y Matplotlib y Seaborn para la visualización.

Específicamente para el modelo de recomendación, se construyó un dataframe especial con el id del usuario que dejó comentarios, los nombres de los juegos comentados y una columna de rating. Este rating se formó combinando el análisis de sentimiento y la recomendación de los juegos. ¡Cada gráfico cuenta una historia emocionante!

## Modelo de Aprendizaje Automático: ¡Recomendaciones en Acción!
Dos modelos poderosos toman el escenario: ítem-ítem y usuario-juego. Con el poder del filtrado colaborativo, traemos a la vida recomendaciones personalizadas que deleitarán a nuestros usuarios.

## Desarrollo de API: La Puerta a la Diversión
Para el desarrolo de la API se decidió utilizar el framework FastAPI, creando las siguientes funciones:

* userdata: Esta función tiene por parámentro 'user_id' y devulve la cantidad de dinero gastado por el usuario, el porcentaje de recomendaciones que realizó sobre la cantidad de reviews que se analizan y la cantidad de items que consume el mismo.

* countreviews: En esta función se ingresan dos fechas entre las que se quiere hacer una consulta y devuelve la cantidad de usuarios que realizaron reviews entre dichas fechas y el porcentaje de las recomendaciones positivas (True) que los mismos hicieron.

* genre: Esta función recibe como parámetro un género de videojuego y devuelve el puesto en el que se encuentra dicho género sobre un ranking de los mismos analizando la cantidad de horas jugadas para cada uno.

* userforgenre: Esta función recibe como parámetro el género de un videojuego y devuelve el top 5 de los usuarios con más horas de juego en el género ingresado, indicando el id del usuario y el url de su perfil.

* developer: Esta función recibe como parámetro 'developer', que es la empresa desarrolladora del juego, y devuelve la cantidad de items que desarrolla dicha empresa y el porcentaje de contenido Free por año por sobre el total que desarrolla.

* sentiment_analysis: Esta función recibe como parámetro el año de lanzamiento de un juego y según ese año devuelve una lista con la cantidad de registros de reseñas de usuarios que se encuentren categorizados con un análisis de sentimiento, como Negativo, Neutral y Positivo.

* recomendacion_juego: Esta función recibe como parámetro el nombre de un juego y devuelve una lista con 5 juegos recomendados similares al ingresado.

* recomendacion_usuario: Esta función recibe como parámetro el id de un usuario y devuelve una lista con 5 juegos recomendados para dicho usuario teniendo en cuenta las similitudes entre los usuarios.

**Nota:**
- Las funciones recomendacion_juego y recomendacion_usuario están disponibles en la API, pero solo recomendacion_juego se pudo desplegar en Render debido a limitaciones de almacenamiento. Para utilizar recomendacion_usuario, se debe ejecutar la API localmente.

El código para generar la API se encuentra en el archivo main.py y las funciones para su funcionamiento se encuentran en api_functions. En caso de querer ejecutar la API desde localHost se deben seguir los siguientes pasos:

Clonar el proyecto haciendo git clone (https://github.com/AmyVj/PI-1-MLOps.git).
Preparación del entorno de trabajo en Visual Studio Code:
Crear entorno Python -m venv env
Ingresar al entorno haciendo venv\Scripts\activate
Instalar dependencias con pip install -r requirements.txt
Ejecutar el archivo main.py desde consola activando uvicorn. Para ello, hacer uvicorn main:app --reload
Hacer Ctrl + clic sobre la dirección http://127.0.0.1:8000/docs#/ (se muestra en la consola).
Una vez en el navegador, agregar /docs para acceder a ReDoc.
En cada una de las funciones hacer clic en Try it out y luego introducir el dato que requiera o utilizar los ejemplos por defecto. Finalmente Ejecutar y observar la respuesta.

## Deployment: Lanzamiento al Éxito
Con Render como nuestro compañero de viaje, llevamos nuestra API al mundo. Automatización sin complicaciones, ¡todo en el camino hacia la gloria!
 
 Para esto se siguieron estos pasos:

Generación de un Dockerfile cuya imagen es Python 3.10. Esto se hace porque Render usa por defecto Python 3.7, lo que no es compatible con las versiones de las librerías trabajadas en este proyecto, por tal motivo, se optó por deployar el proyecto dentro de este contenedor. Se puede ver el detalle del documento Dockerfile.
Se generó un servicio nuevo en render.com, conectado al presente repositorio y utilizando Docker como Runtime.
Finalmente, el servicio queda corriendo en https://deployment-melannyvega.onrender.com/docs
Como se indicó anteriormente, para el despliegue automático, Render utiliza GitHub y dado que el servicio gratuito cuenta con una limitada capacidad de almacenamiento, se realizó un repositorio exclusivo para el deploy, el cual se encuenta https://github.com/AmyVj/PI_deploy_render.git.

## Video
En este video se explica brevemente este proyecto mostrando el funcionamiento de la API.


## Oportunidades de Mejora: ¡El Juego Continúa!
Nuestro viaje apenas comienza. Con ojos puestos en el futuro, exploramos formas de perfeccionar nuestras recomendaciones, mejorar nuestro análisis y expandir nuestras capacidades de implementación.

¡Únete a nosotros en este emocionante viaje hacia el futuro de los videojuegos con MLOps en Steam! 🎮✨
