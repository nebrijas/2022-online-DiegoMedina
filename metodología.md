## Metodología
# Periodismo de Datos II: Herramientas Digitales para la Visualización y Presentación de Datos


A través de las diferentes sesiones de la clase Periodismo de datos II recibimos un mejor entendimiento de la informática, gracias a esto, pudimos potenciar nuestro quehacer periodístico y obtener conocimientos y herramientas que a futuro serán un diferencia ante otros profesionales en nuestro campo.

Un periodista se nutre de datos, en ellos está la verdad y esta materia nos dio los conocimientos necesarios para buscarlos, entenderlos y visualizarlos. los temas manejados en esta asignatura se complementan de una excelente forma con lo aprendido en las materias de programación, los lenguajes tales como HTML y Python lograron ser un reto para mi, pero después de estas clases y del juicioso desarrollo de las actividades dirigidas, logré comprenderlos y aplicarlos en temas relacionados a mi profesión.

Esta memoria tiene como fin recopilar los pasos que seguí para realizar las actividades dirigidas, siguiendo los pasos de programación literal, o, expresado de mejor manera, un archivo de texto y de código.

***

## Actividad dirigida 1

Esta primera actividad consistió en la escritura de un comentario periodístico acerca de un artículo de periodismo de datos, el objetivo era familiarizarnos con la forma de redactar  y comprimir los datos que se encuentra en los medios de comunicación, pero además, se esperaba que nos familiarizamos con la escritura en  editores de texto o pad para así recordar los códigos que aprendimos en programación y en periodismo de datos I.

Para la entrega de esta actividad redactamos el comentario en una aplicación que nos permite escribir código de forma colaborativa, esto con el objetivo de conocer herramientas de programación grupal y poder conocer el trabajo de nuestros compañeros.

Un requisito que estuvo presente en esta actividad fue la redacción de la crítica en escritura de tipo Markdown, presente principalmente en el código fuente de páginas webs o cuadernos de Python.

Gracias a esto, recordamos como ciertos caracteres pueden transformar la visualización de nuestros textos, símbolos como el numeral (#), el asterisco (*) pueden transformar la presentación de un texto y adecuarlo para una mejor visualización y jerarquización de la información.

Otro aspecto retador de esta actividad fue la introducción a GitHub, plataforma que hace la vez de un repositorio en el cual se puede editar grupalmente proyectos, albergar información y crear código desde 0.

Tras familiarizarnos con esta plataforma el profesor nos enseñó a enlazar proyectos, crear nuestras propias páginas y nos dió las instrucciones para crear el archivo README.md, con el cual podríamos tener acceso a las futuras actividades y condensarla en una sola vista.

Aquí es posible ver la [ad1.](ad1.md)

***

## Actividad dirigida 2


Para esta segunda actividad se creó un nuevo comentario enfocado al periodismo de datos, pero esta vez se le buscó dar un enfoque que demostrara la experiencia de análisis y visualización aprendidos con la actividad II.

Al igual que en la primera actividad, esta también se redactó en Markdown  y se subió a Github, esta actividad tenía como finalidad mostrarnos páginas como From data to viz, herramienta utilizada para crear visualizaciones de datos y también, se buscaba que corrigieron los comentarios que vimos en la actividad I y agregaremos nuevos elementos, tales como el enlace de palabras. 

En esta actividad decidí hacer mi redacción acerca del informe de desempleo de septiembre en Colombia. Para realizarlo, decidí remitirme a la fuente oficial del DANE, entidad gubernamental encargada de los datos del país, así como a informes realizados por entes internacionales y artículos donde se compara este dato con el resto del continente americano.

Debido a la naturaleza de esta actividad y para demostrar un crecimiento respecto a la actividad anterior, decidí hacer una mayor utilización de los enlaces, las fuentes oficiales y las imágenes de referencia.

Aquí es posible ver la [ad2.](ad2.md) 

***

## Actividad dirigida 3

Esta actividad marcó nuestra entrada en el lenguaje de programación Python como tal, para esto, fue necesario recordar conocimientos aprendidos en programación y aprender nuevas maneras de ejecutar y redactar código.

El ejercicio consistía en hacer scraping de una web, hacer que el código funcionara y explicar en un comentario el paso a paso que seguimos para alcanzar estos objetivos.

Para este punto, el profesor nos explicó que scraping es buscar y extraer información de valor de una página, en este caso, comenzamos descargando herramientas como Anaconda y Jupyter.


Con el objetivo de aprender a usar Jupyter, dentro de esta actividad se usó como ejemplo la página del diario El país de españa ya que ellos tiene una forma muy particular de seccionar sus artículos según la jerarquía de sus titulares.
La base y la columna vertebral que permite la realización de este trabajo son las librerías, ya que estas permiten que el código funcione de manera correcta y son el primer paso para hacer el scraping.

Después de importarlas, realicé la primer petición GET a la URL de el país, utilizando la librería requests, Luego de obtener el texto de la URL dada, procedí a utilizar la librería BeatifulSoup para realizar el web scraping, del HTML entregado por la librería para después  encargarme de buscar los tags H2 y mostrarlos en consola. 

Al finalizar el paso anterior, El código se enfocó en realizar varias peticiones a distintas URLs, luego continúe a mostrar cada petición realizada por el código. Para finalizar  se hizo una búsqueda de palabras claves y se usó la librería Termcolor para cambiar el color de los textos. 

Aquí es posible ver la [ad3.](ad.3)

***

## Actividad dirigida 4


Con la actividad 4 se cerró este ciclo, una de las cosas que pude notar al finalizar todas las actividades fue lo conectadas que estaban una con la otra, ya que cada una expandia y profundizaba más lo aprendió en la anterior.

Esta última actividad consistía en extraer y visualizar datos de una web, y su objetivo era enseñarnos formas, a través de programación,  de extraer y visualizar datos de una tabla con gran cantidad de entradas.

Para esto, regresamos a Jupyter y conectamos esta herramienta a la API de datos del covid 19, después, seguimos las instrucciones que se nos dió en clase, ya que en esta se explicó gran parte del ejercicio, e incluso se realizó un ejemplo, con el que me pude guiar en el paso a paso y así finalizar esta actividad.

La librería que se utilizó en esta ocasión fue Panda, de Python, y de la misma forma que en la actividad anterior, se explicó el paso a paso en una serie de comentarios redactados en Jupyter.

Como se mencionó anteriormente, Panda es la librería principal en este ejercicio, por eso, el primer paso fue instalarla, después se creó una acción para llamarla y  se creó la variable myurl. Esto es vital para que esta biblioteca sepa de dónde solicitar la información.

Más adelante aprendimos a organizar la información en en un marco de datos llamada dataframe, esto nos permite analizar la información dentro de este marco gracias a diversas funciones, según el objetivo que se quiera lograr y los datos que se quieran aislar. 

Para finalizar, con la función Plot se logró generar gráficas que permiten visualizar los datos obtenidos en tiempo real.

Aquí es posible ver la [ad4.](ad.4)

***

La materia de Periodismo de datos II probó ser una de las más desafiantes, pero, también fue una de las más importantes de este máster, ya que me dió un enfoque diferente de lo que es el periodismo y me enseñó herramientas que me diferenciarán de otros profesionales, me permiten hacer un trabajo mucho más juicioso en menor tiempo.
