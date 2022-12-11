# Actividad 3 Diego Medina
## Actividad de Web Scraping en Phyton

***

El objetivo de esta actividad fue hacer web scraping de la página web del El País. El web scraping es una forma de encontar y extraer información en diferentes paginas webs.
La idea princicpal de este ejercicio es extraer algunos titulares especificos de esta web y hacer un paso a paso de este proceso.

## Actividad 3

Primeros pasos: Se relizó la instalación de las librerias


```python
!pip install requests bs4 pandas termcolor
```

    Requirement already satisfied: requests in c:\users\luisa\anaconda3\lib\site-packages (2.28.1)
    Collecting bs4
      Downloading bs4-0.0.1.tar.gz (1.1 kB)
      Preparing metadata (setup.py): started
      Preparing metadata (setup.py): finished with status 'done'
    Requirement already satisfied: pandas in c:\users\luisa\anaconda3\lib\site-packages (1.4.4)
    Requirement already satisfied: termcolor in c:\users\luisa\anaconda3\lib\site-packages (2.1.1)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in c:\users\luisa\anaconda3\lib\site-packages (from requests) (1.26.11)
    Requirement already satisfied: charset-normalizer<3,>=2 in c:\users\luisa\anaconda3\lib\site-packages (from requests) (2.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in c:\users\luisa\anaconda3\lib\site-packages (from requests) (2022.9.14)
    Requirement already satisfied: idna<4,>=2.5 in c:\users\luisa\anaconda3\lib\site-packages (from requests) (3.3)
    Requirement already satisfied: beautifulsoup4 in c:\users\luisa\anaconda3\lib\site-packages (from bs4) (4.11.1)
    Requirement already satisfied: python-dateutil>=2.8.1 in c:\users\luisa\anaconda3\lib\site-packages (from pandas) (2.8.2)
    Requirement already satisfied: numpy>=1.18.5 in c:\users\luisa\anaconda3\lib\site-packages (from pandas) (1.21.5)
    Requirement already satisfied: pytz>=2020.1 in c:\users\luisa\anaconda3\lib\site-packages (from pandas) (2022.1)
    Requirement already satisfied: six>=1.5 in c:\users\luisa\anaconda3\lib\site-packages (from python-dateutil>=2.8.1->pandas) (1.16.0)
    Requirement already satisfied: soupsieve>1.2 in c:\users\luisa\anaconda3\lib\site-packages (from beautifulsoup4->bs4) (2.3.1)
    Building wheels for collected packages: bs4
      Building wheel for bs4 (setup.py): started
      Building wheel for bs4 (setup.py): finished with status 'done'
      Created wheel for bs4: filename=bs4-0.0.1-py3-none-any.whl size=1257 sha256=b872bcda154077af0d1dc6245cad85d0f8620b7047b850a77e537e94ddd6273c
      Stored in directory: c:\users\luisa\appdata\local\pip\cache\wheels\73\2b\cb\099980278a0c9a3e57ff1a89875ec07bfa0b6fcbebb9a8cad3
    Successfully built bs4
    Installing collected packages: bs4
    Successfully installed bs4-0.0.1
    
Tras la instalación me enfoqué en importar las librerias instaladas y las nativas de Python

```python
import requests
import time
import csv
import re
from bs4 import BeautifulSoup
import os
import pandas as pd
from termcolor import colored
```

Despues de importalras, realicé la primer petición GET a la URL "https://resultados.elpais.com", utilizando la libreria requests


```python
resultados = []
```



```python
req = requests.get("https://resultados.elpais.com")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup = BeautifulSoup(req.text, 'html.parser')
```

Luego de obtener el texto de la URL dada, procedo a utilizar la libreria BeatifulSoup para realizar el web scraping, del HTML entregado por la libreria me encargo de buscar los tags H2 y mostrarlos en consola


```python
tags = soup.findAll("h2")
```


```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```

    El Mundial, en vivo
    Universo Mundial
    El calendario a seguir desde América
    ¿Quién va a ganar? Simulador y pronóstico de cada selección 
    La ‘newsletter’
    Nueva York internará en psiquiátricos contra su voluntad a las personas sin hogar con trastornos mentales graves
    Boric y Castillo acuerdan celebrar en Lima la cumbre de la Alianza del Pacífico suspendida 
    El líder del grupo ultraderechista Oath Keepers, culpable por sedición por el asalto al Capitolio
    La doble respuesta de China a las protestas: acelerar la vacunación de los mayores y descabezar las manifestaciones 
    Rusia culpa a EE UU de la cancelación del encuentro bilateral sobre control de armas nucleares
    El autor cubano Carlos Manuel Álvarez gana el Premio Anagrama de Crónica
    Lectura del tarot con Gioconda Belli y Julián Herbert: “Una transmisión de energía en las cartas”
    Brenda Ríos: “Hace falta reírse de ‘Cien años de soledad”
    Una radiografía (incompleta) para descubrir qué está leyendo Latinoamérica
    El imperio contraataca: EE UU derrota a Irán y se mete en octavos
    Varvaridades
    Inglaterra golea a Gales (3-0) y se clasifica para octavos como primera de grupo
    Ecuador queda eliminada y Senegal  devuelve a África a los octavos
    Qatar se despide del Mundial con tres derrotas, la peor anfitriona de la historia
    ¿Qué opciones tiene cada selección para pasar a octavos de final del Mundial?
    Kirchner, a los jueces que la investigan: “Este tribunal es un pelotón de fusilamiento”
    Ideas neonazis y víctima de acoso: así es el joven que asesinó a cuatro personas en dos colegios de Brasil  
    ¿Dónde está La Barbie? El misterio revive el mito criminal de Édgar Valdez Villarreal
    Venezuela anuncia un crecimiento de 18 puntos del PIB para 2022
    Asesinado en Nariño el director de un canal de televisión regional durante un reportaje
    Una banda secuestra durante horas un hospital de Ecuador
    Darío Villamizar: “Es el final del ciclo guerrillero en América Latina”
    Gardel y el Barça, un idilio cien años escondido
    En busca de una solución para los dolores que se creían inventados
    Encontrarse en la transexualidad: la odisea de Harry, el chico que quiso ser monja
    Tragedia en la frontera de Melilla: el papel de Marruecos y España en las muertes del 24-J
    Agonía a ambos lados de la frontera de Melilla
    ‘Podcast’ | Cinco meses investigando la tragedia de Melilla
    Ideas de regalos para el jardinero
    Guía de regalos para el ‘foodie’
    Pon nombre a lo que te pasa para sentirte mejor
    Volver a empezar
    Borges, 'Funes el memorioso' y la neurona Jennifer Aniston
    Joseph Henrich, antropólogo: “El mejor antídoto contra el supremacismo blanco es discutir ideas”
    Así acaba el dinero de los fondos de inversión más verdes en empresas contaminantes
    Estados Unidos prepara una ley para evitar una huelga de ferrocarriles en vísperas de Navidad
    Una autopista para dejarse llevar
    Los ciberdelincuentes aprovechan el caos en Twitter para lanzar campañas de ‘phishing’
    Síntomas de rebeldía en China
    “¿De qué color es el hambre, mamá?”
    Enzensberger y la distancia crítica
    El siglo de María Pimentel
    ‘Que sepan que sabemos’: traducen los megaproyectos de López Obrador a lenguas indígenas
    América Latina puede convertirse en un referente mundial de la transición energética justa
    De luz y oscuridad: el 70% de los casos de ceguera infantil en México son prevenibles
    Las guatemaltecas que estudian ingeniería para llevar luz a sus comunidades
    Eliseo, ‘el encargado’ corrupto que seduce y repele en Argentina 
    ‘Canina’: la maternidad con pelo y colmillos
    Bono se desnuda emocionalmente en su concierto más atípico 
    Rolling Blackouts Coastal Fever, la banda de rock más chisporroteante y luminosa de Australia
    La lección magistral de Bob Dylan sobre Elvis
    ¿Selfie sí o selfie no en Art Basel?
    De las galerías a las calles: así se hizo importante Miami para el arte del mundo
    Jennifer Lopez pensó que “iba a morir” después de su ruptura con Ben Affleck
    Por qué encontrar pareja parece hoy misión imposible
    ¿A qué sabe un vino de 160 años? Apuntes de una cata histórica en Marqués de Riscal
    Camila rompe una de las tradiciones históricas de la monarquía: no tendrá damas de compañía 
    ‘Argentina, 1985’, un debate nacional entre la ficción y la memoria
    Madrid, la nueva Miami: así se han hecho con la capital española los ricos latinoamericanos
    Neofascistas, nacionalpopulistas, tecnosoberanistas. ¿Cómo llamamos a las nuevas derechas radicales? 
    Macondo Inundado: el pueblo donde nunca deja de llover 
    Qatar 2022, el único Mundial donde es posible ver dos partidos en un mismo día
    ¿Qué estrella tuvo el mejor debut en el Mundial? El ‘tier list’ de Jesús Gallego, Bruno Alemany y Aritz Gabilondo
    Enredados en el Mundial | Los jugadores iraníes vuelven a cantar el himno y ha nacido una estrella en EE UU
    Migrantes haitianos crean microeconomía en Tapachula
    Puerto Rico lucha por mantener sus playas públicas
    3 claves para entender las protestas por el censo en Bolivia
    Protests spread across China as anger builds over Xi Jinping’s zero-Covid policy 
    Tracing Columbus’s DNA: Two exhumations in Spain seek to determine the explorer’s lineage once and for all
    Europe says goodbye to ‘airplane mode’: Passengers can talk on the phone while flying
    Parasite makes wolves bolder, more likely to become pack leaders
    Americanas: Reportajes y noticias sobre feminismo e historias con enfoque de género de la región
    Toda la actualidad científica en el boletín de Materia
    Letras Americanas: la actualidad literaria de un continente vista por el escritor Emiliano Monge
    Ideas: reportajes y entrevistas para entender el mundo
    El Kremlin silencia el dolor de las mujeres rusas que critican la guerra de Ucrania 
    El primer ministro de Rumania: “Necesitamos un esfuerzo común en la OTAN para defender cada centímetro de territorio frente a Rusia”  
    La justicia investiga si el Gobierno de Macron favoreció a consultoras privadas en las dos últimas campañas presidenciales en Francia
    Aquí puede buscar si ha sido engañado por la estafa internacional de las criptomonedas
    El secreto de la pirámide
    El futuro de las compras: más redes sociales, pago a plazos y vuelta de la tienda física
     El Papa detecta fallos en Caritas Internationalis y cesa a sus responsables 
    La Iglesia italiana da por primera vez cifras de los abusos cometidos por el clero
    El Senado de EE UU da un paso clave para blindar el matrimonio homosexual frente al Supremo
    Muere Hans Magnus Enzensberger, gran intelectual alemán del siglo XX
    ‘Close’, una mirada conmovedora a la fragilidad de la preadolescencia
    La gran retrospectiva de Vermeer en Ámsterdam aviva la polémica sobre la autoría de ‘Muchacha con flauta’
    “Houston, tenemos un nuevo récord”: ‘Orion’ llega más lejos que ninguna otra nave diseñada para llevar astronautas
    Joseph Henrich, antropólogo evolutivo: “El mejor antídoto contra el supremacismo blanco es más ciencia y discutir ideas”
    Infarto, cáncer, alzhéimer… Así aumenta la mala salud de sus encías el riesgo de sufrir otras enfermedades
    La Juventus cierra por sorpresa una era con un cambio de su cúpula en medio de investigaciones judiciales
    Dimite Mattia Binotto, director de Ferrari
    James Rhodes: “Me pilló fuera de juego meterme en política”
    Operación ‘OS35’: cómo rescatar un gigante de 40.288 toneladas varado a la sombra de Gibraltar
    ¿Podré matar a una rata que entre en mi casa? Claves de la reforma del Código Penal para castigar más el maltrato animal
    La izquierda retira por sorpresa y a última hora la propuesta para abolir los toros en Francia 
    La mitad de las mujeres programadoras ha renunciado a un puesto por no haber recibido apoyo 
    Damian Burns, director de Twitch Europa: “No me importaría que mi hija fuera ‘streamer’”
    Los ciberdelincuentes aprovechan el caos en Twitter para lanzar campañas de ‘phishing’
    El Supremo avala reducir las penas cuando lo permita la ‘ley del solo sí es sí’, pero insta a analizar cada a caso
    Unidas Podemos advierte con “preocupación” que el PSOE bloquea leyes sociales por cálculos políticos
    La división entre Arrimadas y Bal se agudiza: el diputado abre la puerta a concurrir a las primarias de Ciudadanos
    Radiografía de ‘El hormiguero’: entretenimiento blanco de éxito… y polémicas por machismo
    ‘No me gusta conducir’: en qué se parece hacer una serie a aprender a conducir’, por Paloma Rando
    ‘The Peripheral’, realidad, ficción y un futuro ciberpunk con los creadores de ‘Westworld’
    Windsor desvela su decoración navideña en las primeras festividades sin Isabel II y con Carlos III como rey
    Camila y los 1.000 osos Paddington: la reina entrega a una ONG infantil los peluches en homenaje a Isabel II
    Stallone vs. Schwarzenegger, la “competición” eterna: “Éramos antagonistas. Soñaba con pegarle y él con pegarme a mí”
    Muchos hombres, una sola mujer
    El misterio de Manuel Carrasco: cómo salir de una barriada de pescadores y acabar siendo un cantante que llena estadios
    Neofascistas, nacionalpopulistas, tecnosoberanistas. ¿Cómo llamamos a las nuevas derechas radicales? 
    Marina Otero, la bailarina argentina que lleva al límite el cuerpo y la provocación
    Madrid, la nueva Miami: así se han hecho con la capital los ricos latinoamericanos
    Si no tiene ahorros, esta es una de las pocas alternativas para comprar casa
    Cuando la furia de Twitter empaña un (interesante) debate narrativo
    Las nuevas coordenadas culturales de Latinoamérica
    Abuelas empoderadas para sobrevivir en los países jóvenes del sur 
    Al-Hol: la prisión al aire libre a la que nadie quiere mirar
    Oyambre, Son Bou y otras nueve playas españolas para un baño de historia
    Cómo actuar ante el caos de los aeropuertos: qué hacer frente a retrasos y cancelaciones de un vuelo
    La bestial tensión sexual entre Bruce Willis y Cybill Shepherd en ‘Luz de luna’, la serie en la que la actriz tuvo mucho que decir
    Samantha Hudson: “Me va bien económicamante, pero no soy Paula Echevarría”
    Una Heidi matanazis y un Winnie the Pooh asesino en serie: ¿por qué los clásicos infantiles se han vuelto ultraviolentos?
    “Tu canción es una basura”: así juzgamos la música por motivos que nada tienen que ver con la calidad 
    Para qué sirve cada tipo de cebolla (y un truco para no llorar al picarla)
    Pollo asado con pomelo y dátiles
    Blackstone pone a prueba al mercado con la venta de viviendas de alquiler de Testa
    Los satélites de Vox: cómo la ultraderecha utiliza una red de asociaciones para aparentar apoyo social a sus postulados
    Suárez lllana anuncia que deja su escaño y Oskar Matute no defrauda con su reacción
    Un nuevo lesionado en Brasil
    Lobos de Ámsterdam. Booking.com, el monopolio de la última habitación
    Así era el final original (y emotivo) de The Walking Dead que se rodó y se cambió a última hora
    

El codigo se enfocó en realizar varias peticiones a distintas URLs, continuo a mostrar cada peticion realizada por el codigo.


```python
req2 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup2 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```

    Bruselas promueve un tribunal especial para juzgar a la cúpula de Putin por sus crímenes en Ucrania
    Bruselas exige congelar fondos europeos a Hungría por las violaciones del Estado de derecho
    Hungría acelera las medidas anticorrupción para rebajar la magnitud del castigo europeo
    Autocracia en crisis
    “¿De qué color es el hambre, mamá?”
    China: una protesta nacional
    Publicar no es un delito
    La vida bajo los bombardeos rusos
    China relaja las medidas anticovid en Guangzhou para frenar nuevas protestas
    Muere el expresidente de China Jiang Zemin
    La doble respuesta de China a las protestas: acelerar la vacunación de los mayores y descabezar las manifestaciones 
    La selección del jurado popular marca el inicio del juicio por los atentados yihadistas de 2016 en Bruselas
    La OTAN redobla su alerta sobre una China en ebullición
    La tragedia de los menores en zona de guerra: 230 millones de niños viven en los conflictos más cruentos del mundo
    Macron viaja a Washington para “resincronizarse” con Biden ante el impacto de la guerra en Ucrania
    Rusia culpa a EE UU de la cancelación del encuentro bilateral sobre control de armas nucleares
    Hojas en blanco contra el cerrojo de la política de covid cero en China
    Rishi Sunak anuncia el fin de la “era dorada” en la relación con China
    Los medios reclaman el fin del abuso del sistema judicial del Reino Unido por parte de los oligarcas rusos
    La hija de Hugo Chávez califica de “grotesco” un video oficialista en homenaje a su padre
    Nueva York internará en psiquiátricos contra su voluntad a las personas sin hogar con trastornos mentales graves
    El líder del grupo ultraderechista Oath Keepers, culpable de sedición por el asalto al Capitolio
    Estados Unidos prepara una ley para evitar una huelga de ferrocarriles en vísperas de Navidad
    “¿De qué color es el hambre, mamá?”
    
    



```python
req3 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup3 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```

    Autocracia en crisis
    Planes para frenar el suicidio
    La celebración de dar gracias
    Brasil en transición
    ‘A compás’
    Cadáveres exquisitos
    Pedirle cuentas a un pijo
    Despolitizar politizando
    Entender al otro empieza a ser intolerable
    Luis Enrique y Mariano Rajoy ante Qatar
    Argentina, 1985
    Síntomas de rebeldía en China
    Las familias también cambian
    Ciclos conservadores
    Millones de razones de cambio en China
    El Roto
    Flavita Banana
    Riki Blanco
    Peridis
    Sciammarella
    Envía tu carta
    Así funciona la corrupción
    Mucho texto
    El desprestigio de la sanidad pública va calando
    Noticias positivas en tiempos caóticos
    Bomba nuclear en Barcelona
    Una maldita lista en tiempos airados
    El defensor del lector contesta


```python
req4 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup4 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```

    Todos los partidos coinciden en acusar a Marlaska de mentir y engañar en su versión sobre la tragedia de Melilla el 24-J
    Nueva trifulca en el Congreso: Irene Montero pasa de atacada a provocadora
    El embajador de Ucrania y una empresa de armas reciben sendas cartas con sustancias deflagrantes
    Alberto Núñez Feijóo, nacionalista
    La caverna
    Aunque nos tiemble la barbilla
    Orfandad representativa
    Otra forma de violencia política
    Acnur cuestiona la legalidad de las devoluciones a Marruecos durante la tragedia de Melilla 
    El Constitucional rechaza celebrar ya un pleno para examinar a los dos magistrados designados por el Gobierno
    Los ciberataques más peligrosos a las redes públicas casi se duplican en España durante el último año
    Lambán afirma que “mejor le habría ido a España” si Javier Fernández hubiese liderado al PSOE
    Los tres polizones llegados a Gran Canaria piden asilo 
    Visto para sentencia el juicio por malversación a un expresidente socialista de la Diputación de Valencia
    La escasez de pruebas convierte en “fracaso” la primera gran causa contra los reyes del hachís
    Pedro Sánchez renueva el Tribunal Constitucional con el exministro de Justicia Juan Carlos Campo
    Documental | Tragedia en la frontera de Melilla: el papel de Marruecos y España en las muertes del 24-J
    El Supremo avala reducir las penas cuando lo permita la ‘ley del solo sí es sí’, pero insta a analizar cada a caso
    Interior insiste en que no hubo ningún muerto en suelo español en la tragedia de Melilla
    Hay casi 4.000 cervezas artesanas españolas. ¿Cómo probarlas todas?
    El cambio hacia un consumo sostenible empieza por los pies


```python
req5 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup5 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```

    Breton exige a Musk que refuerce su política de moderación de contenidos y desinformación en Twitter
    El BBVA gana a Merlin el arbitraje sobre la Operación Chamartín
    España, en el podio de países de la OCDE con mayor alza de la presión fiscal en una década
    Arabia Saudí encarga otros cinco buques de guerra a Navantia 
    La moda de la sostenibilidad
    Impuesto erróneo a la banca
    Sufrimientos evitables
    El coste de la vida
    La intermediación del ahorro y su evolución
    El gigante chino Alibaba lanza en España una nueva plataforma de comercio electrónico
    Powell sugiere que la Reserva Federal frenará el ritmo de subidas de tipos en diciembre
    Los ingresos tributarios hasta octubre ya superan lo recaudado en todo 2021
    La inflación en la zona euro se frena por primera vez en 17 meses con España como el país donde menos suben los precios
    OHLA ingresa un 15% más pero pierde 88 millones por ajustes contables 
    Alemania suaviza su ley de inmigración para atraer trabajadores extranjeros de fuera de la UE
    Las renovables y el gas dan un respiro en la factura de la luz: noviembre fue el mes menos caro desde el inicio de la crisis
    Telefónica subirá en enero todas las tarifas de fibra y móvil hasta 13 euros
    Mutua entra en el negocio del ‘carsharing’ en Madrid con Voltio
    La Inspección considera que Ryanair vulneró el derecho a huelga de sus trabajadores
    La CNMC congela las tarifas aeroportuarias para 2023
    Madrid, la nueva Miami: así se han hecho con la capital los ricos latinoamericanos
    Si no tiene ahorros, esta es una de las pocas alternativas para comprar casa
    Los efectos de una economía de andar por casa
    Caballos al galope: un negocio con un impacto económico de 7.400 millones de euros
    Los impuestos que se necesitan para construir la sociedad del futuro
    A quién perjudica y a quién beneficia el plan de Escrivá para calcular las futuras pensiones
    ING lanza la venta de Fintonic, la primera 'fintech' con licencia del Banco de España
    ¿Quiere cobrar dividendo antes de fin de año? Las cotizadas reparten 2.500 millones y éstos son algunos pagos
    Pulso España-Francia por el sobrecoste del 50% de la interconexión eléctrica submarina
    Por primera vez un juez declara nulo un contrato de alquiler por aplicación de la perspectiva de género
    Visitas a centros de acogida y muchas horas de estudio: así se forman los jueces en materia de género
    La justicia obliga a Iberia a controlar el peso de las maletas para que los azafatos no se lesionen
    ‘Phishing’, ‘malware’ o ‘ransomware’: el reto de formar en ciberseguridad tras la pandemia
    La difícil tarea de recuperar el talento investigador que un día hizo las maletas
    Descubre las formaciones de marketing ‘online’ más buscadas (y económicas) de 2023
    Logística de ida y vuelta, cuando la solución está en reciclar menos y reutilizar más
    Logística y digitalización, el manual de supervivencia para pymes
    Empleados satisfechos, empresas de éxito
    Diez claves del bienestar corporativo para empresas de éxito
    Las aventuras de un par de calcetines que dan empleo a todo un pueblo
    Cultura financiera como punto de partida para volver a empezar
    Cómo aplicar el ‘design thinking’ a cualquier negocio
    Las soluciones digitales que necesita mi negocio 
    Alexia Putellas, un Balón de Oro a base de “esfuerzo, constancia, disciplina y saber reinventarse”
    El método Muñoz para triunfar en cada reto que se propone
    ¿Por qué muchos inversores están volviendo a confiar en Japón?
    ¿Por qué se está enfriando la economía china?
    No una, sino cinco ‘startups’ para la esperanza
    Si tú lo imaginas, yo te lo imprimo
    Cómo garantizar la seguridad en un mundo de amenazas híbridas
    ¿Cuáles son los dilemas éticos del uso de la inteligencia artificial?
    La educación financiera se convierte en la llave de la inclusión
    Hacia un 2025 más justo, más conectado y más sostenible
    ‘La mirada del paciente’: historias sin filtro detrás de las personas
    Cómo conseguir ayudas a la digitalización para autónomos y pymes con Kit Digital
    Un brindis con vino español por la sostenibilidad, la economía y el empleo


```python
req6 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup6 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```
    
  ```  
    PSOE y Unidas Podemos aplazan el debate sobre la autodeterminación de género en la ‘ley trans’ 
    Un plan de choque de 356 millones para intentar salvar Doñana de su creciente declive 
    España gana medio millón de habitantes en una década, pero siete de cada 10 municipios pierden población 
    Qué es la cultura de la violación de la que habla Irene Montero
    La caja de resonancia de la angustia vital
    Para acabar con la violencia de género, necesitamos más mujeres en posiciones de poder 
    Los secretos atronadores del secretario general
    El Senado de Estados Unidos aprueba una ley para blindar el matrimonio homosexual
    El retraso en resolver contratos predoctorales deja en el limbo a cientos de jóvenes: “Es la desesperación” 
    Bruselas quiere que todos los embalajes de la UE sean reciclables para 2030
    Qué es la cultura de la violación de la que habla Irene Montero
    ¿Adiós a los anuncios con coches para niños y cocinas para niñas? Los jugueteros pactan desterrar los estereotipos sexistas en la publicidad
    Portugal, el secreto de un país sin bicicletas que se convirtió en el mayor fabricante europeo
    Claves de la ley de trata: centrada en las víctimas y protección integral
    El estrés académico lastra el aprendizaje de los niños de nivel socioeconómico bajo
    Pilar Bonet dedica a los periodistas que cubren la guerra de Ucrania el premio Francisco Cerecedo de periodismo 
    Vila-real concede la medalla de oro al colegio de los carmelitas dos meses después de destaparse un caso de abusos
    Cinco jóvenes procesados en Murcia por una agresión sexual en grupo que además grabaron y difundieron por redes sociales 
    Unas gafas que cambian la forma de ver el mundo
    Cambia tu relación con los papeles de la cocina
    Social, clínica y en primera persona. Tres visiones del VIH 
    Enfermería, los profesionales de las ‘curas invisibles’ en las personas con VIH
    Sostenibilidad y... ¡acción! 
    La huella de carbono, el rastro que marca el futuro del planeta
    Psoriasis, en las profundidades de la piel
    Contar para sanar
    Qué hacer con tres millones de colchones
    La nueva revolución agrícola 
    Lo lejos que se puede llegar con otra metodología educativa 
    Cuando el empleo se convierte en la mejor terapia 
    A solas con la obesidad
    Freno y marcha atrás en la obesidad infantil
    “La ópera, como la naturaleza, está viva, y debe evolucionar para ser eterna” 
    ¿Hasta dónde puede llegar el ser humano? El viaje a la oscuridad más absoluta
    El futuro de la alimentación se encuentra en el agua
    Acuicultura: la importancia de comer ‘azul’ para vivir en verde
    El futuro de la aviación verdeEn busca de la eficiencia energética en los aeropuertos
    Un día en el servicio de ayuda a domicilio
    Trabajadores esenciales. Los que nunca paran
    Consejos para alimentar correctamente a los animales de compañía
    Cuidados y necesidades de perros y gatos en su etapa sénior
    Combatir el desperdicio alimentario desde la cesta de la compra
    Kilómetro cero para llenar la despensa
    Acompañamiento, el modelo alternativo de acogida de refugiados
    El metro como refugio. De los andenes de Madrid en 1936 a los de Kiev en 2022
    Yogures con menos azúcar, paso firme hacia la alimentación infantil del futuro
    Invisible, pero vital: el ciclo de las aguas subterráneas
    La bomba de calor, el sistema de climatización más sostenible y eficiente
```

```python
req7 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup7 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```
    El retraso en resolver contratos predoctorales deja en el limbo a cientos de jóvenes: “Es la desesperación” 
    El estrés académico lastra el aprendizaje de los niños de nivel socioeconómico bajo
    ‘Phishing’, ‘malware’ o ‘ransomware’: el reto de formar en ciberseguridad tras la pandemia
    La escuela concertada matricula a la mitad del alumnado desfavorecido que le correspondería
    El Gobierno dará un subsidio de 400 euros anuales a los alumnos con necesidades especiales
    La expulsión de una clase en un colegio de Palma tras colocar una bandera de España deriva en graves amenazas a una profesora 
    Pobreza en el Olimpo académico de EE UU: la huelga en la Universidad de California es ya una de las más grandes del país
    El primer Mundial en horario lectivo y con ‘smartphones’ se juega también en los institutos
    El cabecilla de los cánticos machistas del Elías Ahuja vuelve al colegio mayor pese a anunciarse su expulsión definitiva
    La nueva ley de enseñanzas artísticas endurecerá los requisitos para ser profesor y hará más homogéneas las pruebas de acceso del alumnado
    Una madre denuncia insultos homófobos a su hija en un colegio de Madrid: “Su tutor nos dijo que tenemos que respetar la opinión de los demás”
    Evaluar competencias
    La Selectividad a los 50: ¿cirugía mayor o estiramiento facial?
    La libertad de elección de centro educativo y los cheques escolares en Madrid
    Selectividad: Desatar el nudo gordiano
    El nuevo sistema para evaluar los conocimientos digitales de los profesores valdrá en toda España
    Ofrecer comedor gratis en todos los colegios públicos es “alcanzable y urgente”: costaría 1.664 millones al año, según la ONG Educo  
    Una fórmula para que la escuela pública compita mejor con la concertada
    La pérdida de alumnos por el descenso de la natalidad está afectando con más fuerza a la escuela pública que a la concertada
    La disparidad de resultados entre autonomías en la EVAU se origina en la escuela, no en el examen 
    Las autonomías del PP critican la nueva Selectividad porque no prevé un examen único para todo el Estado
    La escalada vertiginosa de notas en Bachillerato: los sobresalientes de los que llegan a Selectividad se doblan en seis años
    El caso de Georgia, en EE UU: becar sin importar la renta agranda la desigualdad
    El techo de cristal del grado medio de FP: candidatos demasiado preparados se quedan con los puestos
    César Rendueles: “Hay universidades privadas que son como academias de conducir con pretensiones”
    La fuga de miles de médicos agrava el déficit de especialistas en España
    Jóvenes tutelados en la Universidad: “Tu pasado no tiene por qué condicionar tu futuro”
    Si tu familia está desahogada estudiarás Medicina y dobles grados, si no, Óptica o Educación 
    Pon nombre a lo que te pasa para sentirte mejor
    Volver a empezar
    Borges, 'Funes el memorioso' y la neurona Jennifer Aniston


```python
req8 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup8 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```
    Portugal, el secreto de un país sin bicicletas que se convirtió en el mayor fabricante europeo
    Bruselas quiere que todos los embalajes de la UE sean reciclables para 2030
    La misteriosa llegada del pájaro parecido a un pingüino que preocupa en las costas mediterráneas
    Alex Rafalowicz, abogado: “Los combustibles fósiles son una amenaza existencial como las armas nucleares”
    El problema con los suelos: un mundo vivo, desconocido y muy desprotegido
    Los agujeros legales de las finanzas sostenibles: así acaba el dinero de los fondos de inversión más verdes en empresas contaminantes
    Gobierno y Junta chocan por Doñana mientras se agrava el deterioro ecológico 
    Linces y águilas imperiales se refugian en fincas privadas que se alían con conservacionistas  
    ¿Podré matar a una rata que entre en mi casa? Claves de la reforma del Código Penal para castigar más el maltrato animal
    El precio del Ave Madrid-Barcelona cae un 43% pero sube un 14% en las líneas sin competencia 
    El plan del Gobierno para las reservas de agua mantiene intacta la alta presión del regadío
    El cambio climático perjudica su salud
    Por qué comer animales puede ayudar a luchar contra el cambio climático
    Por qué hay que dejar de comer animales para luchar contra el cambio climático
    Bombas de carbono 
    Crisis de los insectos: esto no es un armagedón sino una debacle provocada por los humanos
    El quebrantahuesos reconquista los cielos 
    Un año de crisis climática sin fin
    Ríos imposibles: las 171.000 barreras que rompen el curso de agua en España
    Pon nombre a lo que te pasa para sentirte mejor
    Volver a empezar
    Borges, 'Funes el memorioso' y la neurona Jennifer Aniston


```python
req9 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup9 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```
    La primera simulación cuántica de un agujero de gusano abre una nueva puerta para entender el universo
    Un fármaco experimental contra el alzhéimer confirma efectos positivos, pero puede estar detrás de la muerte de dos pacientes 
    Escrito en las estrellas, ¿qué hemos aprendido de ellas?
    El problema con los suelos: un mundo vivo, desconocido y muy desprotegido
    Los hogares más pobres reducen su consumo de refrescos casi 11 litros en un año por la subida del IVA
    Ronna, ronto, quetta y quecto, los nuevos prefijos para magnitudes extraordinarias
    Por qué unas personas resisten el estrés mejor que otras 
    Joseph Henrich, antropólogo evolutivo: “El mejor antídoto contra el supremacismo blanco es más ciencia y discutir ideas”
    “Houston, tenemos un nuevo récord”: ‘Orion’ llega más lejos que ninguna otra nave diseñada para llevar astronautas
    En busca de una solución para los dolores que se creían inventados
    Un parásito ‘elige’ qué lobo será el líder de la manada
    Pobreza en el Olimpo académico de EE UU: la huelga en la Universidad de California es ya una de las más grandes del país
    Josefa Ros Velasco, filósofa: “Si alguien se aburre suele darse a la botella, cuando le pasa a un país suele darse una revuelta”
    La insólita historia de la mujer que descubrió el primer antibiótico español
    Hilda Hudson, la primera conferenciante en el gran congreso internacional de matemáticas
    De León al espacio, pasando por una pequeña universidad pública: “Invirtiendo en ciencia se puede llegar a lo más alto”
    Escrito en las estrellas, ¿qué hemos aprendido de ellas?
    Hilda Hudson, la primera conferenciante en el gran congreso internacional de matemáticas
    Por qué el entrelazamiento cuántico revoluciona nuestro entendimiento de la naturaleza
    La órbita del telescopio ‘James Webb’ y el problema de los tres cuerpos
    Cien años de las ecuaciones que expandieron el universo
    Sangaku
    El tamiz de Apolonio
    La respuesta a la gran pregunta
    El aroma de la inspiración y la piedra de la locura
    ‘Los hijos de Hansen’ y la marginación social provocada por la lepra
    Los fractales y su estructura narrativa como parte del relato
    El amanecer de todo y el dinosaurio como animal modernista
    Los universos paralelos de un visionario científico llamado Philip K. Dick
    ¿Por qué son rocosos los satélites de los planetas gaseosos?
    ¿Es nueva la producción de alimentos en macrogranjas? 
    ¿Cómo se ve el cielo nocturno desde el ecuador?
    ¿Por qué las olas van siempre hacia la playa?
    Listeria: el patógeno que trae de cabeza a la industria alimentaria
    Ciencia para derrumbar el mito de que la soja es mala para prevenir el cáncer de mama
    Cuidado con las conservas caseras: es importante hacerlas bien
    Una propuesta alternativa, barata y saludable a la nueva cesta de la compra
    Aditivos, propiedades saludables y fechas de caducidad: guía para entender las etiquetas alimentarias


```python
req10 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup10 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```
    Muere Christine McVie, cantante y teclista de la legendaria banda Fleetwood Mac, a los 79 años
    El toque manual de campana español, declarado Patrimonio Cultural Inmaterial por la Unesco
    Lo más escuchado en Spotify en 2022: solo Rosalía se coloca en España en una lista dominada por hombres latinos
    ¿Cómo empezó?
    ‘Company’ y el olor de la gran manzana
    Charlie Watts: el bicho más raro de The Rolling Stones
    Meter la mano en la vaca 
    ¿Quedan cines de barrio? ¿Toda España se hizo artista en el confinamiento? ¿Importan los premios? Los números responden
    ¿Se imaginan a la Generalitat criticando el día de Sant Jordi?
    Almodóvar, en la lectura de la obra de Paco Bezerra: “Nunca imaginé que iba a asistir hoy a un acto contra la censura”
    Un tatuaje y un bebé en el nombre de Mircea Cărtărescu
    Angustias y vivencias de dos revolucionarios entre rejas 
    Carolina Bang: “Los productores somos los andamios de la obra”
    El ritmo de la revolución en Sudán salta de TikTok a Nueva York 
    ¿Especulación, algoritmos locos o codicia? El misterio de los libros de segunda mano con precios desorbitados
    Adonis vuelve a México de la mano de Sharjah
    ‘Canina’: la maternidad con pelo y colmillos
    Bono se desnuda emocionalmente en Madrid en su concierto más atípico
    Cartel de Primavera Sound 2023, tanto Barcelona como Madrid: Rosalía, Kendrick Lamar, Blur o Depeche Mode
    Rolling Blackouts Coastal Fever, la banda de rock más chisporroteante y luminosa de Australia
    Santo Estevo, el monasterio al que todos le tienen fe
    La Calahorra de siempre, más viva que nunca
    La novela negra del siglo XXI no existe (sin Michael Connelly)
    Disfruta en familia de 'El pequeño Mozart'
    El MET Opera en Cine Yelmo a través de +Que Cine
    Descubre la exposición 'Hijas del Nilo'
    'Un animal en mi almohada' en el Teatro Fernán Gómez
    La temida voltereta de la abolición de los toros en el sur de Francia quedó en un susto
    Las obras de mejora de la plaza de toros de Las Ventas comienzan en diciembre
    Actos a favor y en contra de los toros en Francia antes de que se debata su prohibición
    Luis Miguel Leiro, picador premiado y desencantado: “Amo y respeto a los animales, pero el toro ha nacido para la lidia”
    
```python
req11 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup11 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```
    Lo nuevo de Rihanna, Bruce Springsteen, Niño de Elche (con Rosalía) y otras canciones de noviembre
    ‘La última vez’, de Guillermo Martínez: un laberinto con final milagrosamente imprevisible
    La lección magistral de Bob Dylan sobre Elvis
    Cuando la furia de Twitter empaña un (interesante) debate narrativo
    La curva de la semana: sube Cristina Morales, está aquí Aldo Manucio, baja Rosalía
    Las nuevas coordenadas culturales de Latinoamérica
    Las más bellas cartas de amor en castellano, las memorias de Marguerite Duras y otros libros de la semana
    Amor mío queridísimo: la delicadeza sentimental de Felisberto Hernández
    Ellas fueron modernas: cuatro artistas alemanas en la vorágine del cambio de siglo
    Anacrónicas y audaces: las cantautoras latinas que renuevan la música de raíz
    Los mejores libros infantiles y juveniles de noviembre
    Lo que jode es la respuesta: la diferencia entre crítica, cancelación y censura
    Edmonia, Frida y Amrita: tres mestizas
    La basura se lee con anteojos
    Mi otoño alemán
    ‘Percusión’: una novela del pasado para recordar el futuro
    Pere Gimferrer dentro del laberinto
    ‘La brecha’: cómo cambiar un mundo mal hecho
    ‘Tu sueño imperios han sido’: la historia boca arriba
    Nona Fernández: “En Chile intentan que conciliemos el sueño otra vez”
    Cristina Lucas: “Todo lo que se publicita está sobrevalorado”
    Enrique Gracián: “Hay infinitos más grandes que otros”
    Àlex Brendemühl: “La serie sobre el rey emérito no te deja indiferente”
    Marta Etura: “De no ser actriz, habría sido matrona”
    
    
```python
req12 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup12 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```
    Última hora
    El calendario
    Universo Mundial
    Las predicciones
    La ‘newsletter’
    Argentina se gana los octavos 
    “Si Lewandowski fuera argentino, igual mete cinco”
    México se queda a un gol del pase 
    El mejor Messi juega, no golea 
    Enrique ‘doce Copas’ Bermúdez
    Los primeros 45 minutos
    El estimulante legado de la Canadá campeona de la Davis
    Tata Martino lleva a México a su gran fracaso en el Mundial
    ¡Viva mi desgracia!
    Así le hemos contado la eliminación de México del Mundial pese a ganarle a Arabia Saudí
    Túnez desnuda a los suplentes de Francia
    Una indesmayable Australia acaba con la insustancial Dinamarca y se mete en octavos
    Mundial de Qatar 2022, ultimas noticias en directo | Francia-Polonia y Argentina-Australia, emparejamientos de octavos de final
    Pelé se mantiene estable tras ser hospitalizado de manera inesperada en São Paulo
    Dani Olmo: “Al fútbol se gana con la pelota en los pies”
    Tragedia en el ciclismo: Davide Rebellin muere atropellado por un camión
    La liga saudí de golf aterrizará el próximo año en España con un torneo en el histórico campo de Valderrama 
    El imperio contraataca: EE UU derrota a Irán y se mete en octavos
    Luis Enrique: “No vamos a especular contra Japón”
    Las lecturas de Luis Enrique para el Mundial de Qatar (y la vida): un testimonio de Auschwitz y la sabiduría del estoicismo
    Última hora del Mundial de Qatar: vídeo en directo | Fútbol Sapiens
    Los seleccionadores de otros países se han dado cuenta:  “Nadie juega como España” 
    ¿Por qué Argentina vive una situación límite contra Polonia?
    La moneda que cambió para siempre el destino de España en los Mundiales
    Cómo no perderte ni un solo minuto del Mundial
    Las promesas del baloncesto español piden consejo a Sergio Scariolo
    A toda mecha hacia el mundial de la consagración
    Cómo disfrutar de la montaña con todas las garantías
    Así te hemos contado la victoria de Australia sobre Dinamarca en el partido de la fase de grupos del Mundial de Qatar 2022
    Una indesmayable Australia acaba con la insustancial Dinamarca y se mete en octavos
    Enzo Fernández, el niño que pidió compasión para Messi
    Bai, chino de doble oro
    Argentina - Polonia: dónde ver este y todos los partidos desde el país latino
    Los seleccionadores de otros países se han dado cuenta:  “Nadie juega como España” 
    ¿Por qué Argentina vive una situación límite contra Polonia?
    Tragedia en el ciclismo: Davide Rebellin muere atropellado por un camión

```python
req13 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup13 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```
    La primera simulación cuántica de un agujero de gusano abre una nueva puerta para entender el universo
    La Policía de San Francisco contará con robots con capacidad de matar
    Ronna, ronto, quetta y quecto, los nuevos prefijos para magnitudes extraordinarias
    El problema de la cultura ‘brogrammer’: “Se está excluyendo la mirada femenina de las soluciones tecnológicas que van a modelar el futuro”
    Los ciberdelincuentes aprovechan el caos en Twitter para lanzar campañas de ‘phishing’
    Damian Burns, director de Twitch Europa: “No me importaría que mi hija fuera ‘streamer’”
    Adrian Hon, diseñador de videojuegos: “Las empresas y gobiernos usan juegos para controlarnos”
    Mastodon: qué es y cómo funciona la red social en la que los usuarios deciden qué está permitido
    Así serán las nuevas marcas de verificación en Twitter: azul, oro y gris
    Gafas con funciones de un móvil, ‘Photoshop’ instantáneo y otros inminentes avances gracias a los nuevos chips
    Elon Musk ya edita tuits y Twitter no se ha hundido aún, ¿qué hará en el futuro?
    Elon Musk declara una “amnistía general” en Twitter para las cuentas suspendidas
    Así está fallando Twitter: racismo, derechos de autor y desprotección en dictaduras
    Quo Vadis, Elon Musk: por qué el colapso de Twitter no es un divertimento
    Candid Wüest: “Si alguien ‘apaga’ Ucrania, probablemente haya una respuesta y eso no interesa porque todos los países son vulnerables”
    La banca confía en tumbar en los tribunales el nuevo impuesto al sector
    Los cuatro coches que llegaron a España en el año de los Juegos y la Expo
    Ocho mil millones de cursis
    Guía de viaje a la nube: una aventura con escalas
    Árboles tuiteros contra la ceguera botánica
    De Baudelaire a Midjourney: los nuevos “enemigos mortales del arte”
    
```python
req14 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup14 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```
    La primera simulación cuántica de un agujero de gusano abre una nueva puerta para entender el universo
    La Policía de San Francisco contará con robots con capacidad de matar
    Ronna, ronto, quetta y quecto, los nuevos prefijos para magnitudes extraordinarias
    El problema de la cultura ‘brogrammer’: “Se está excluyendo la mirada femenina de las soluciones tecnológicas que van a modelar el futuro”
    Los ciberdelincuentes aprovechan el caos en Twitter para lanzar campañas de ‘phishing’
    Damian Burns, director de Twitch Europa: “No me importaría que mi hija fuera ‘streamer’”
    Adrian Hon, diseñador de videojuegos: “Las empresas y gobiernos usan juegos para controlarnos”
    Mastodon: qué es y cómo funciona la red social en la que los usuarios deciden qué está permitido
    Así serán las nuevas marcas de verificación en Twitter: azul, oro y gris
    Gafas con funciones de un móvil, ‘Photoshop’ instantáneo y otros inminentes avances gracias a los nuevos chips
    Elon Musk ya edita tuits y Twitter no se ha hundido aún, ¿qué hará en el futuro?
    Elon Musk declara una “amnistía general” en Twitter para las cuentas suspendidas
    Así está fallando Twitter: racismo, derechos de autor y desprotección en dictaduras
    Quo Vadis, Elon Musk: por qué el colapso de Twitter no es un divertimento
    Candid Wüest: “Si alguien ‘apaga’ Ucrania, probablemente haya una respuesta y eso no interesa porque todos los países son vulnerables”
    La banca confía en tumbar en los tribunales el nuevo impuesto al sector
    Los cuatro coches que llegaron a España en el año de los Juegos y la Expo
    Ocho mil millones de cursis
    Guía de viaje a la nube: una aventura con escalas
    Árboles tuiteros contra la ceguera botánica
    De Baudelaire a Midjourney: los nuevos “enemigos mortales del arte”

```python
req15 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup15 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```
    Spotify Wrapped, el selfi musical que monopoliza la conversación en Twitter y acaricia el ego
    Buckingham fuerza la dimisión de una asistente de la reina Camila por sus comentarios racistas
    Los actores de ‘Love Actually’ se reencuentran: ¿qué ha sido de ellos 19 años después?
    El último desfile de Antonio Alvarado se expone en el Museo del Traje
    Antonio Alvarado, el museo como pasarela: su restrospectiva, en fotos
    Tres reinas, una princesa y dos primeras damas se reúnen en Buckingham contra la violencia de género
    Acuerdo de divorcio entre Kim Kardashian y Kanye West: custodia compartida y 200.000 dólares de pensión
    La ‘baguette’ francesa, declarada patrimonio inmaterial de la Unesco
    Los chocolates y bombones de Jordi Roca ponen un pie en Barcelona con una tienda efímera de Casa Cacao en un hotel de lujo
    ¿Se puede ahorrar energía colocando alfombras, cortinas o mantas en el sofá? Los expertos responden
    Xuso Jones: “Tengo muchos más seguidores por mis ‘tips’ de limpieza que por mi música. Y me encanta”
    Sara Carbonero, tras recibir el alta hospitalaria por su operación: “Me siento en paz y agradecida con la vida”
    La jueza archiva la denuncia de María León contra los policías que la detuvieron en Sevilla
    Una selección imprescindible de productos con descuento: comprar en Black Friday nunca fue tan fácil
    Achilles Ion Gabriel, el surrealista de Camper: “No hago zapatos para museos, sino para la gente” 
    El regreso discreto de Mario Testino: “A mi edad, no creo que mi percepción de la moda sea la correcta ni la que el mundo necesita”
    Manuel Outumuro toca cumbre
    Diane de Beauvau-Craon, la última princesa rebelde: “Me drogué mucho, bebí mucho alcohol, me acosté con muchos gais y aquí sigo”
    En los secaderos ancestrales del pimentón de la Vera, el oro rojo de Cáceres
    Las abuelas que han viralizado el arte de elaborar la pasta italiana 
    Humaredas mediáticas, monotonía, discursos vacíos y otras amenazas y retos de la gastronomía 
    Sumer, el ‘kebab’ artesano de barrio en Madrid que acoge tertulias filosóficas
    El robot de cocina más famoso del mundo ha conquistado ya tres millones de hogares españoles
    
    
```python
req16 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup16 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```
    Lorena Castell, ganadora de ‘MasterChef Celebrity’: “Me gusta trabajar bajo presión, me pone las pilas”
    Borja Cobeaga: “Tardé más en sacarme el carné que en hacer esta serie”
    Lorena Castell, ganadora de ‘MasterChef Celebrity 7’
    ‘El presidente: juego de la corrupción’, los entresijos del fútbol
    ‘No me gusta conducir’: en qué se parece hacer una serie a aprender a conducir
    ‘Ummo’, un guirigay intergaláctico
    Adiós a Ellen Pompeo, la funcionaria de la tele
    Radiografía de ‘El hormiguero’: entretenimiento blanco de éxito… y polémicas por machismo
    Eliseo, ‘el encargado’ corrupto que seduce y repele en Argentina 
    ‘Masterchef Celebrity’ ya tiene a sus dos finalistas, tras la “rendición” de Patricia Conde
    ‘The Peripheral’, realidad, ficción y un futuro ciberpunk con los creadores de ‘Westworld’
    El ‘bon vivant’ acorralado de la FIFA que grabó a sus amigos
    Leonor Lavado: “Somos como hablamos: la voz es nuestro ADN psicológico”
    ¿Qué ocurre tras la edad de oro de la televisión? El mismo oro, aunque enterrado bajo una oferta masiva
    ‘La ruta’: por qué nos invade la nostalgia del ‘after’
    La veteranía de María del Monte y Francisco se mide con la juventud de María Terremoto y Juanlu Montoya en los premios Radiolé  
    Regreso al hospital terrorífico de Lars von Trier
    ¿Qué ver hoy en TV? Miércoles 30 de noviembre de 2022
    Las series de agosto de 2022: ‘La casa del dragón’ en HBO Max; ‘Sandman’ en Netflix y otras
    Nueve capítulos para recordar ‘The Wire’ en su 20º aniversario
    Harry Palmer: el tercer vértice del mágico triángulo de espías británicos
    Las series de junio de 2022: ‘The Boys’ en Amazon Prime Video; ‘Peaky Blinders’ en Netflix y otras
    
```python
req17 = requests.get("https://elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup17 = BeautifulSoup(req2.text, 'html.parser')
```



```python
tags = soup2.findAll("h2")
```



```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
    

os.system("clear")

print(colored("A continuación se muestran los titulares de las páginas principales del diario El País que contienen las siguientes palabras:", 'blue', attrs=['bold']))
print(colored("Feminismo", 'green', attrs=['bold']))

str_match = [s for s in resultados if "feminismo" in s]
print("\n".join(str_match))

print(colored("Igualdad", 'green', attrs=['bold']))

str_match = [s for s in resultados if "igualdad" in s]
print("\n".join(str_match))

print(colored("Mujeres", 'green', attrs=['bold']))

str_match = [s for s in resultados if "mujeres" in s]
print("\n".join(str_match))

print(colored("Mujer", 'green', attrs=['bold']))

str_match = [s for s in resultados if "mujer" in s]
print("\n".join(str_match))

print(colored("Brecha salarial", 'green', attrs=['bold']))

str_match = [s for s in resultados if "brecha salarial" in s]
print("\n".join(str_match))

print(colored("Machismo", 'green', attrs=['bold']))

str_match = [s for s in resultados if "machismo" in s]
print("\n".join(str_match))

print(colored("Violencia", 'green', attrs=['bold']))

str_match = [s for s in resultados if "violencia" in s]
print("\n".join(str_match))

print(colored("Maltrato", 'green', attrs=['bold']))

str_match = [s for s in resultados if "maltrato" in s]
print("\n".join(str_match))

print(colored("Homicidio", 'green', attrs=['bold']))

str_match = [s for s in resultados if "homicidio" in s]
print("\n".join(str_match))

print(colored("Género", 'green', attrs=['bold']))

str_match = [s for s in resultados if "género" in s]
print("\n".join(str_match))

print(colored("Asesinato", 'green', attrs=['bold']))

str_match = [s for s in resultados if "asesinato" in s]
print("\n".join(str_match))

print(colored("Sexo", 'green', attrs=['bold']))

str_match = [s for s in resultados if "sexo" in s]
print("\n".join(str_match))

```
En el texto anterior, ademas de imprimir la informacion, guarde toda la información obtenida por las URLs en una variable llamada Results. A continuación vamos buscar palabras claves dentro de los textos obtenidos de antemano, además, usaré la libreria Termcolor para modificar el color de los textos
```


    Pilar Bonet dedica a los periodistas que cubren la guerra de Ucrania el premio Francisco Cerecedo de periodismo 
    Vila-real concede la medalla de oro al colegio de los carmelitas dos meses después de destaparse un caso de abusos
    Cinco jóvenes procesados en Murcia por una agresión sexual en grupo que además grabaron y difundieron por redes sociales 
    La caja de resonancia de la angustia vital
    Para acabar con la violencia de género, necesitamos más mujeres en posiciones de poder 
    Los secretos atronadores del secretario general
    Balance de la COP27: un fondo para los más vulnerables mientras la meta de los 1,5 grados se aleja
    La ley de trata garantizará la asistencia y protección de las víctimas sin necesidad de denuncia
    EE UU ordena retirar un innovador tratamiento contra el cáncer que España acaba de incorporar a la sanidad pública 
    Derechos Sociales culpa al ala socialista del Gobierno de “constantes retrasos” en la aprobación de la ley de familias
    La escuela concertada matricula a la mitad del alumnado desfavorecido que le correspondería
    Alex Rafalowicz, abogado: “Los combustibles fósiles son una amenaza existencial como las armas nucleares”
    Igualdad negocia a contra reloj con Interior y Justicia el texto de la ley de trata que irá mañana a Consejo de Ministros 
    La expulsión de una clase en un colegio de Palma tras colocar una bandera de España deriva en graves amenazas a una profesora 
    Los médicos privados se sienten “humillados” por las aseguradoras: los técnicos cobran 60 euros por arreglar una lavadora y los sanitarios, 10 por consulta
    Los ayuntamientos de Madrid se sitúan un año más a la cola en servicios sociales
    Kelley Robinson: “Han declarado una guerra cultural contra nuestros hijos”
    La caja de resonancia de la angustia vital
    Unas gafas que cambian la forma de ver el mundo
    Cambia tu relación con los papeles de la cocina
    Social, clínica y en primera persona. Tres visiones del VIH 
    Enfermería, los profesionales de las ‘curas invisibles’ en las personas con VIH
    Sostenibilidad y... ¡acción! 
    La huella de carbono, el rastro que marca el futuro del planeta
    Psoriasis, en las profundidades de la piel
    Contar para sanar
    La nueva revolución agrícola 
    Nuevos talentos de la economía circular
    Lo lejos que se puede llegar con otra metodología educativa 
    Cuando el empleo se convierte en la mejor terapia 
    A solas con la obesidad
    Freno y marcha atrás en la obesidad infantil
    “La ópera, como la naturaleza, está viva, y debe evolucionar para ser eterna” 
    ¿Hasta dónde puede llegar el ser humano? El viaje a la oscuridad más absoluta
    Acuicultura: la importancia de comer ‘azul’ para vivir en verde
    El cultivo de pescado español, a la vanguardia del planeta
    El futuro de la aviación verde
    En busca de la eficiencia energética en los aeropuertos
    Un día en el servicio de ayuda a domicilio
    Trabajadores esenciales. Los que nunca paran
    Consejos para alimentar correctamente a los animales de compañía
    Cuidados y necesidades de perros y gatos en su etapa sénior
    Combatir el desperdicio alimentario desde la cesta de la compra
    Kilómetro cero para llenar la despensa
    Acompañamiento, el modelo alternativo de acogida de refugiados
    El metro como refugio. De los andenes de Madrid en 1936 a los de Kiev en 2022
    Yogures con menos azúcar, paso firme hacia la alimentación infantil del futuro
    Invisible, pero vital: el ciclo de las aguas subterráneas
    La bomba de calor, el sistema de climatización más sostenible y eficiente
    La escuela concertada matricula a la mitad del alumnado desfavorecido que le correspondería
    El Gobierno dará un subsidio de 400 euros anuales a los alumnos con necesidades especiales
    La expulsión de una clase en un colegio de Palma tras colocar una bandera de España deriva en graves amenazas a una profesora 
    Pobreza en el Olimpo académico de EE UU: la huelga en la Universidad de California es ya una de las más grandes del país
    El primer Mundial en horario lectivo y con ‘smartphones’ se juega también en los institutos
    El cabecilla de los cánticos machistas del Elías Ahuja vuelve al colegio mayor pese a anunciarse su expulsión definitiva
    La nueva ley de enseñanzas artísticas endurecerá los requisitos para ser profesor y hará más homogéneas las pruebas de acceso del alumnado
    Una madre denuncia insultos homófobos a su hija en un colegio de Madrid: “Su tutor nos dijo que tenemos que respetar la opinión de los demás”
    Cataluña aprueba un complemento para ampliar la jornada a los profesores de informática de FP
    Más de 6.000 estudiantes de FP empezaron tarde el curso por el largo proceso de inscripción en Cataluña
    Cuando la escuela se convierte en un gueto: América Latina tiene las primarias más segregadas del mundo
    Evaluar competencias
    La Selectividad a los 50: ¿cirugía mayor o estiramiento facial?
    La libertad de elección de centro educativo y los cheques escolares en Madrid
    Selectividad: Desatar el nudo gordiano
    El nuevo sistema para evaluar los conocimientos digitales de los profesores valdrá en toda España
    Ofrecer comedor gratis en todos los colegios públicos es “alcanzable y urgente”: costaría 1.664 millones al año, según la ONG Educo  
    Una fórmula para que la escuela pública compita mejor con la concertada
    La pérdida de alumnos por el descenso de la natalidad está afectando con más fuerza a la escuela pública que a la concertada
    La disparidad de resultados entre autonomías en la EVAU se origina en la escuela, no en el examen 
    Las autonomías del PP critican la nueva Selectividad porque no prevé un examen único para todo el Estado
    La escalada vertiginosa de notas en Bachillerato: los sobresalientes de los que llegan a Selectividad se doblan en seis años
    El caso de Georgia, en EE UU: becar sin importar la renta agranda la desigualdad
    El techo de cristal del grado medio de FP: candidatos demasiado preparados se quedan con los puestos
    César Rendueles: “Hay universidades privadas que son como academias de conducir con pretensiones”
    La fuga de miles de médicos agrava el déficit de especialistas en España
    Jóvenes tutelados en la Universidad: “Tu pasado no tiene por qué condicionar tu futuro”
    Si tu familia está desahogada estudiarás Medicina y dobles grados, si no, Óptica o Educación 
    Pon nombre a lo que te pasa para sentirte mejor
    Volver a empezar
    Borges, 'Funes el memorioso' y la neurona Jennifer Aniston
    Alex Rafalowicz, abogado: “Los combustibles fósiles son una amenaza existencial como las armas nucleares”
    Los agujeros legales de las finanzas sostenibles: así acaba el dinero de los fondos de inversión más verdes en empresas contaminantes
    Gobierno y Junta chocan por Doñana mientras se agrava el deterioro ecológico 
    Linces y águilas imperiales se refugian en fincas privadas que se alían con conservacionistas  
    ¿Podré matar a una rata que entre en mi casa? Claves de la reforma del Código Penal para castigar más el maltrato animal
    El precio del Ave Madrid-Barcelona cae un 43% pero sube un 14% en las líneas sin competencia 
    El plan del Gobierno para las reservas de agua mantiene intacta la alta presión del regadío
    Operación ‘OS35’: cómo rescatar un gigante de 40.288 toneladas varado a la sombra de Gibraltar
    Balance de la COP27: un fondo para los más vulnerables mientras la meta de los 1,5 grados se aleja
    Clemente Álvarez, premio a la conservación de la biodiversidad de la Fundación BBVA: “Debemos difundir la voz de alarma de los científicos”
    Investigan la brutalidad en una montería en la Sierra de Segura de Jaén 
    El cambio climático perjudica su salud
    Por qué comer animales puede ayudar a luchar contra el cambio climático
    Por qué hay que dejar de comer animales para luchar contra el cambio climático
    Bombas de carbono 
    Crisis de los insectos: esto no es un armagedón sino una debacle provocada por los humanos
    El quebrantahuesos reconquista los cielos 
    Un año de crisis climática sin fin
    Ríos imposibles: las 171.000 barreras que rompen el curso de agua en España
    Pon nombre a lo que te pasa para sentirte mejor
    Volver a empezar
    Borges, 'Funes el memorioso' y la neurona Jennifer Aniston
    Los hogares más pobres reducen su consumo de refrescos casi 11 litros en un año por la subida del IVA
    Por qué unas personas resisten el estrés mejor que otras 
    Joseph Henrich, antropólogo evolutivo: “El mejor antídoto contra el supremacismo blanco es más ciencia y discutir ideas”
    “Houston, tenemos un nuevo récord”: ‘Orion’ llega más lejos que ninguna otra nave diseñada para llevar astronautas
    En busca de una solución para los dolores que se creían inventados
    Un parásito ‘elige’ qué lobo será el líder de la manada
    Pobreza en el Olimpo académico de EE UU: la huelga en la Universidad de California es ya una de las más grandes del país
    Josefa Ros Velasco, filósofa: “Si alguien se aburre suele darse a la botella, cuando le pasa a un país suele darse una revuelta”
    La insólita historia de la mujer que descubrió el primer antibiótico español
    Hilda Hudson, la primera conferenciante en el gran congreso internacional de matemáticas
    De León al espacio, pasando por una pequeña universidad pública: “Invirtiendo en ciencia se puede llegar a lo más alto”
    “Soñamos con ir a la Luna, pero no llegaremos a Marte” 
    Pablo Álvarez y Sara García, primeros astronautas españoles de la ESA en 30 años
    Una vacuna universal contra la gripe que usa la tecnología de ARN, probada con éxito en ratones
    Palabras y árboles
    Europa resucita su misión para taladrar Marte en busca de vida en 2028  
    ¿Cómo nos afecta en la Tierra la formación de un agujero negro?: tres grandes físicos lo desvelan
    Hilda Hudson, la primera conferenciante en el gran congreso internacional de matemáticas
    Por qué el entrelazamiento cuántico revoluciona nuestro entendimiento de la naturaleza
    La órbita del telescopio ‘James Webb’ y el problema de los tres cuerpos
    Cien años de las ecuaciones que expandieron el universo
    Sangaku
    El tamiz de Apolonio
    La respuesta a la gran pregunta
    El aroma de la inspiración y la piedra de la locura
    ‘Los hijos de Hansen’ y la marginación social provocada por la lepra
    Los fractales y su estructura narrativa como parte del relato
    El amanecer de todo y el dinosaurio como animal modernista
    Los universos paralelos de un visionario científico llamado Philip K. Dick
    ¿Por qué son rocosos los satélites de los planetas gaseosos?
    ¿Es nueva la producción de alimentos en macrogranjas? 
    ¿Cómo se ve el cielo nocturno desde el ecuador?
    ¿Por qué las olas van siempre hacia la playa?
    Listeria: el patógeno que trae de cabeza a la industria alimentaria
    Ciencia para derrumbar el mito de que la soja es mala para prevenir el cáncer de mama
    Cuidado con las conservas caseras: es importante hacerlas bien
    Una propuesta alternativa, barata y saludable a la nueva cesta de la compra
    Aditivos, propiedades saludables y fechas de caducidad: guía para entender las etiquetas alimentarias
    Almodóvar, en la lectura de la obra de Paco Bezerra: “Nunca imaginé que iba a asistir hoy a un acto contra la censura”
    ¿Especulación, algoritmos locos o codicia? El misterio de los libros de segunda mano con precios desorbitados
    ‘Canina’: la maternidad con pelo y colmillos
    ¿Cómo empezó?
    ‘Company’ y el olor de la gran manzana
    Charlie Watts: el bicho más raro de The Rolling Stones
    Meter la mano en la vaca 
    Bono se desnuda emocionalmente en Madrid en su concierto más atípico
    Cartel de Primavera Sound 2023, tanto Barcelona como Madrid: Rosalía, Kendrick Lamar, Blur o Depeche Mode
    Rolling Blackouts Coastal Fever, la banda de rock más chisporroteante y luminosa de Australia
    ¿Cómo empezó?
    Viviendas de lujo sostenible
    Así era el rostro de san Isidro Labrador, muerto hace diez siglos   
    La encrucijada del Reina Sofía: ¿quién sucederá a Borja-Villel en la dirección de uno de los museos más importantes del mundo?
    La cultura abraza las historias trans 
    Momias a dos velas: la excavación estrella de la egiptología española afronta una crítica falta de fondos en su momento más decisivo
    El monasterio burgalés que falsificó en el siglo XII una herencia para quedarse con una valiosa iglesia
    La FIL pone una vela al libro y otra al balón
    Videoanálisis | La FIL mastodóntica y popular está de vuelta
    Carmen Machi: “Que te metieran mano en el metro nos parecía normal”
    Santo Estevo, el monasterio al que todos le tienen fe
    La Calahorra de siempre, más viva que nunca
    La novela negra del siglo XXI no existe (sin Michael Connelly)
    'Company, el Musical' llega a Madrid
    Disfruta de 'El Cascanueces' en Cine Yelmo
    Fernando Cayo es Scrooge en 'Cuento de Navidad'
    'El Guardaespaldas, el musical' llega a tu ciudad
    La temida voltereta de la abolición de los toros en el sur de Francia quedó en un susto
    Las obras de mejora de la plaza de toros de Las Ventas comienzan en diciembre
    Actos a favor y en contra de los toros en Francia antes de que se debata su prohibición
    Luis Miguel Leiro, picador premiado y desencantado: “Amo y respeto a los animales, pero el toro ha nacido para la lidia”
    La lección magistral de Bob Dylan sobre Elvis
    Cuando la furia de Twitter empaña un (interesante) debate narrativo
    La curva de la semana: sube Cristina Morales, está aquí Aldo Manucio, baja Rosalía
    Las nuevas coordenadas culturales de Latinoamérica
    Las más bellas cartas de amor en castellano, las memorias de Marguerite Duras y otros libros de la semana
    Amor mío queridísimo: la delicadeza sentimental de Felisberto Hernández
    Ellas fueron modernas: cuatro artistas alemanas en la vorágine del cambio de siglo
    Anacrónicas y audaces: las cantautoras latinas que renuevan la música de raíz
    Los mejores libros infantiles y juveniles de noviembre
    Palabra de Kafka
    Una ‘Yerma’ con poco nervio y zozobra
    Lo que jode es la respuesta: la diferencia entre crítica, cancelación y censura
    Edmonia, Frida y Amrita: tres mestizas
    La basura se lee con anteojos
    Mi otoño alemán
    ‘Percusión’: una novela del pasado para recordar el futuro
    Pere Gimferrer dentro del laberinto
    ‘La brecha’: cómo cambiar un mundo mal hecho
    ‘Tu sueño imperios han sido’: la historia boca arriba
    Nona Fernández: “En Chile intentan que conciliemos el sueño otra vez”
    Cristina Lucas: “Todo lo que se publicita está sobrevalorado”
    Enrique Gracián: “Hay infinitos más grandes que otros”
    Àlex Brendemühl: “La serie sobre el rey emérito no te deja indiferente”
    Marta Etura: “De no ser actriz, habría sido matrona”
    Última hora
    El calendario
    Universo Mundial
    Las predicciones
    La ‘newsletter’
    El imperio contraataca: EE UU derrota a Irán y se mete en octavos
    Inglaterra marca distancias
    Gareth Bale, lesionado y eliminado del Mundial: “Seguiré mientras pueda y me quieran” 
    Difícil separar la política del deporte
    Inglaterra - Gales, matar al hermano
    España eleva su crédito por derecho
    La elevación del delito
    Bélgica, bajo fuego amigo en el Mundial
    Senegal derrota a una reservona Ecuador y devuelve a África a los octavos de final de un Mundial
    Qatar se despide del Mundial con tres derrotas, la peor anfitriona de la historia
    Qatar reconoce la muerte de al menos 400 trabajadores migrantes en la preparación del Mundial
    Resumen de la jornada 10 del Mundial de Qatar 2022 
    Todos los ofendidos de Irán, contra EE UU 
    La fascinante vida del último buscador de perlas de Qatar  
    Dimite Mattia Binotto, director de Ferrari
    James Rhodes: “Me pilló fuera de juego meterme en política”
    Gareth Bale, lesionado y eliminado del Mundial: “Seguiré mientras pueda y me quieran” 
    Bruno Fernandes somete a Uruguay 
    Gareth Bale, lesionado y eliminado del Mundial: “Seguiré mientras pueda y me quieran” 
    Bruno Fernandes somete a Uruguay 
    Los aficionados opinan sobre el Mundial de Qatar: de “Se celebra en un lugar triste” a “La culpa no es del fútbol” 
    Última hora del Mundial de Qatar: vídeo en directo | Fútbol Sapiens
    Día 10 de Qatar 2022 | Iturralde analiza el no gol de Cristiano Ronaldo
    La moneda que cambió para siempre el destino de España en los Mundiales
    Cómo no perderte ni un solo minuto del Mundial
    Las promesas del baloncesto español piden consejo a Sergio Scariolo
    A toda mecha hacia el mundial de la consagración
    Cómo disfrutar de la montaña con todas las garantías
    Así hemos contado la victoria de Brasil ante Suiza en la segunda jornada de la fase de grupos del Mundial de Qatar 2022
    Enredados en el Mundial: Füllkrug, el nuevo héroe de Alemania
    Rubiales, sobre el futuro de Luis Enrique: “Nunca podremos pagarle 20 millones de euros. Hablaremos tras el Mundial, pero no se puede dilatar”
    ¿Quién de España jugó mejor contra Alemania? El ‘tier list’ de Jesús Gallego, Antón Meana y Joaquín Maroto
    ¿Quién va a ganar el Mundial de Qatar? Simulador y pronóstico actualizado de cada selección 
    Difícil separar la política del deporte
    El ‘Chucky’ Lozano, Alexis Vega y las cadenas del gol en México
    Dimite Mattia Binotto, director de Ferrari
    El problema de la cultura ‘brogrammer’: “Se está excluyendo la mirada femenina de las soluciones tecnológicas que van a modelar el futuro”
    Los ciberdelincuentes aprovechan el caos en Twitter para lanzar campañas de ‘phishing’
    Damian Burns, director de Twitch Europa: “No me importaría que mi hija fuera ‘streamer’”
    Adrian Hon, diseñador de videojuegos: “Las empresas y gobiernos usan juegos para controlarnos”
    Mastodon: qué es y cómo funciona la red social en la que los usuarios deciden qué está permitido
    Así serán las nuevas marcas de verificación en Twitter: azul, oro y gris
    Gafas con funciones de un móvil, ‘Photoshop’ instantáneo y otros inminentes avances gracias a los nuevos chips
    Elon Musk ya edita tuits y Twitter no se ha hundido aún, ¿qué hará en el futuro?
    Elon Musk declara una “amnistía general” en Twitter para las cuentas suspendidas
    Así está fallando Twitter: racismo, derechos de autor y desprotección en dictaduras
    Quo Vadis, Elon Musk: por qué el colapso de Twitter no es un divertimento
    Candid Wüest: “Si alguien ‘apaga’ Ucrania, probablemente haya una respuesta y eso no interesa porque todos los países son vulnerables”
    El reclutamiento de personal se reinventa en las redes: “Se crea una relación de confianza entre los candidatos y las empresas mucho antes de haber contacto directo”
    El biógrafo de Elon Musk: “Para él, el caos es el procedimiento operativo estándar”
    Aplicaciones, relojes y etiquetas QR ayudan a evitar la desaparición de personas con demencia
    La banca confía en tumbar en los tribunales el nuevo impuesto al sector
    Los cuatro coches que llegaron a España en el año de los Juegos y la Expo
    Ocho mil millones de cursis
    Guía de viaje a la nube: una aventura con escalas
    Árboles tuiteros contra la ceguera botánica
    De Baudelaire a Midjourney: los nuevos “enemigos mortales del arte”
    El problema de la cultura ‘brogrammer’: “Se está excluyendo la mirada femenina de las soluciones tecnológicas que van a modelar el futuro”
    Los ciberdelincuentes aprovechan el caos en Twitter para lanzar campañas de ‘phishing’
    Damian Burns, director de Twitch Europa: “No me importaría que mi hija fuera ‘streamer’”
    Adrian Hon, diseñador de videojuegos: “Las empresas y gobiernos usan juegos para controlarnos”
    Mastodon: qué es y cómo funciona la red social en la que los usuarios deciden qué está permitido
    Así serán las nuevas marcas de verificación en Twitter: azul, oro y gris
    Gafas con funciones de un móvil, ‘Photoshop’ instantáneo y otros inminentes avances gracias a los nuevos chips
    Elon Musk ya edita tuits y Twitter no se ha hundido aún, ¿qué hará en el futuro?
    Elon Musk declara una “amnistía general” en Twitter para las cuentas suspendidas
    Así está fallando Twitter: racismo, derechos de autor y desprotección en dictaduras
    Quo Vadis, Elon Musk: por qué el colapso de Twitter no es un divertimento
    Candid Wüest: “Si alguien ‘apaga’ Ucrania, probablemente haya una respuesta y eso no interesa porque todos los países son vulnerables”
    El reclutamiento de personal se reinventa en las redes: “Se crea una relación de confianza entre los candidatos y las empresas mucho antes de haber contacto directo”
    El biógrafo de Elon Musk: “Para él, el caos es el procedimiento operativo estándar”
    Aplicaciones, relojes y etiquetas QR ayudan a evitar la desaparición de personas con demencia
    La banca confía en tumbar en los tribunales el nuevo impuesto al sector
    Los cuatro coches que llegaron a España en el año de los Juegos y la Expo
    Ocho mil millones de cursis
    Guía de viaje a la nube: una aventura con escalas
    Árboles tuiteros contra la ceguera botánica
    De Baudelaire a Midjourney: los nuevos “enemigos mortales del arte”
    Xuso Jones: “Tengo muchos más seguidores por mis ‘tips’ de limpieza que por mi música. Y me encanta”
    Sara Carbonero, tras recibir el alta hospitalaria por su operación: “Me siento en paz y agradecida con la vida”
    La jueza archiva la denuncia de María León contra los policías que la detuvieron en Sevilla
    Jennifer Lopez pensó que “iba a morir”después de su ruptura con Ben Affleck
    Por qué encontrar pareja parece hoy misión imposible
    Dwayne Johnson vuelve a la tienda donde robó chocolatinas de niño y se gasta 300 dólares 
    Camila rompe una de las tradiciones históricas de la monarquía: no tendrá damas de compañía 
    Helena Bonham Carter defiende a su amigo Johnny Depp y critica la cultura de la cancelación: “Hay una caza de brujas”
    El silencio es el nuevo lujo: cómo vivir en un mundo cada vez más ruidoso nos ha hecho pagar por algo gratuito
    Confesiones de la mujer que lleva 40 años decidiendo quién será James Bond
    Atún rojo: el inesperado nexo cultural que une a Japón con el Mediterráneo
    Las excentricidades imposibles de las Kardashian: de escáneres cerebrales a 700 dólares en gominolas de cannabis
    Mis séptimos Premios Icon
    Achilles Ion Gabriel, el surrealista de Camper: “No hago zapatos para museos, sino para la gente” 
    El regreso discreto de Mario Testino: “A mi edad, no creo que mi percepción de la moda sea la correcta ni la que el mundo necesita”
    Manuel Outumuro toca cumbre
    Diane de Beauvau-Craon, la última princesa rebelde: “Me drogué mucho, bebí mucho alcohol, me acosté con muchos gais y aquí sigo”
    Marta Ortega reúne en A Coruña a grandes protagonistas de la moda internacional en la inauguración de la muestra sobre Steven Meisel
    En los secaderos ancestrales del pimentón de la Vera, el oro rojo de Cáceres
    Las abuelas que han viralizado el arte de elaborar la pasta italiana 
    Humaredas mediáticas, monotonía, discursos vacíos y otras amenazas y retos de la gastronomía 
    Sumer, el ‘kebab’ artesano de barrio en Madrid que acoge tertulias filosóficas
    El robot de cocina más famoso del mundo ha conquistado ya tres millones de hogares españoles
    Lorena Castell, ganadora de ‘MasterChef Celebrity 7’
    Radiografía de ‘El hormiguero’: entretenimiento blanco de éxito… y polémicas por machismo
    Eliseo, ‘el encargado’ corrupto que seduce y repele en Argentina 
    ‘No me gusta conducir’: en qué se parece hacer una serie a aprender a conducir
    ‘Ummo’, un guirigay intergaláctico
    Adiós a Ellen Pompeo, la funcionaria de la tele
    ‘This England’
    ‘Masterchef Celebrity’ ya tiene a sus dos finalistas, tras la “rendición” de Patricia Conde
    ‘The Peripheral’, realidad, ficción y un futuro ciberpunk con los creadores de ‘Westworld’
    El ‘bon vivant’ acorralado de la FIFA que grabó a sus amigos
    Leonor Lavado: “Somos como hablamos: la voz es nuestro ADN psicológico”
    ¿Qué ocurre tras la edad de oro de la televisión? El mismo oro, aunque enterrado bajo una oferta masiva
    ‘La ruta’: por qué nos invade la nostalgia del ‘after’
    La veteranía de María del Monte y Francisco se mide con la juventud de María Terremoto y Juanlu Montoya en los premios Radiolé  
    La violencia machista entre los adolescentes, el drama que ni ellos quieren reconocer
    ‘La Sagrada Familia’ rescata el mito Pujol y disecciona su caída
    Eurovisión permitirá votos de espectadores de todo el mundo a partir de 2023
    ¿Qué ver hoy en TV? Martes 29 de noviembre de 2022
    Las series de agosto de 2022: ‘La casa del dragón’ en HBO Max; ‘Sandman’ en Netflix y otras
    Nueve capítulos para recordar ‘The Wire’ en su 20º aniversario
    Harry Palmer: el tercer vértice del mágico triángulo de espías británicos
    Las series de junio de 2022: ‘The Boys’ en Amazon Prime Video; ‘Peaky Blinders’ en Netflix y otras
    ¿A qué sabe un vino de 160 años? Apuntes de una cata histórica en Marqués de Riscal
    Las abuelas que han viralizado el arte de elaborar la pasta italiana 
    David LaChapelle, en busca de Dios
    Muchos hombres, una sola mujer
    El misterio de Manuel Carrasco: cómo salir de una barriada de pescadores y acabar siendo un cantante que llena estadios
    Ishida, los ‘dioses’ de la cocina japonesa: “Tenemos que dejar de desear tantas cosas, y hay que hacerlo ya”  
    Los guardianes de la ensaladilla rusa, la tapa popular que subió a los altares ‘gourmet’ 
    Humaredas mediáticas, monotonía, discursos vacíos y otras amenazas y retos de la gastronomía 
    Cerdo pío negro, el inesperado rival del ibérico
    Narcisistas, ansiosos, pasivo-agresivos… Cinco claves para tratar con personas difíciles
    Al fondo, invisibles, las pirámides
    Todo es mejor con chocolate y estas cinco recetas lo demuestran
    Sangre, sudor y petróleo: la materia prima del Mundial de Qatar según un artista ruso
    Los Audi RS 6 Avant y RS 7 más potentes de la historia
    El milagro alemán
    Consuelo
    Sin tiempo
    La bióloga que se propuso repoblar los bosques de Brasil
    Un parche para conseguir que las vacunas sean más baratas y accesibles
    Cerdo pío negro, el inesperado rival del ibérico
    Ishida, los ‘dioses’ de la cocina japonesa: “Tenemos que dejar de desear tantas cosas, y hay que hacerlo ya”  
    El secreto del maíz gigante mexicano que bajó del volcán
    ‘Mis primeros recuerdos’, por Daniel Barenboim
    El indómito espíritu creativo de Picasso
    Medio siglo sin Picasso
    Paloma Picasso se confiesa con Nuccio Ordine
    El amigo de Picasso sin el que no estaría en España el ‘Guernica’
    Tommy Hilfiger: “Muchos de nuestros clientes van a vivir en el metaverso. De hecho, ya lo hacen”
    Storm Pablo: el estilista que convirtió a Bad Bunny en icono también de la moda
    El hombre que cultiva las flores más caras para los perfumes más especiales
    Una casa junto al lago Como que es pura diversión 
    Desayuno a la mexicana entre hípsters de mañaneo y polis con más apetito que buena fama
    El misterio Picasso
    Isabel II: el reinado de la imagen
    Un día en la vida de un país: San Sebastián-Cádiz en 16 horas
    Luces y sombras de Robert Mapplethorpe
    A continuación se muestran los titulares de las páginas principales del diario El País que contienen las siguientes palabras:
    Feminismo
    Americanas: Reportajes y noticias sobre feminismo e historias con enfoque de género de la región
    Igualdad
    El caso de Georgia, en EE UU: becar sin importar la renta agranda la desigualdad
    Mujeres
    El Kremlin silencia el dolor de las mujeres rusas que critican la guerra de Ucrania 
    La mitad de las mujeres programadoras ha renunciado a un puesto por no haber recibido apoyo 
    Para acabar con la violencia de género, necesitamos más mujeres en posiciones de poder 
    Mujer
    El Kremlin silencia el dolor de las mujeres rusas que critican la guerra de Ucrania 
    La mitad de las mujeres programadoras ha renunciado a un puesto por no haber recibido apoyo 
    Muchos hombres, una sola mujer
    La tertuliana de ultraderecha y el bulo tránsfobo contra Begoña Gómez, la mujer de Pedro Sánchez
    Para acabar con la violencia de género, necesitamos más mujeres en posiciones de poder 
    La insólita historia de la mujer que descubrió el primer antibiótico español
    Confesiones de la mujer que lleva 40 años decidiendo quién será James Bond
    Muchos hombres, una sola mujer
    Brecha salarial
    
    Machismo
    Radiografía de ‘El hormiguero’: entretenimiento blanco de éxito… y polémicas por machismo
    Radiografía de ‘El hormiguero’: entretenimiento blanco de éxito… y polémicas por machismo
    Violencia
    Otra forma de violencia política
    Para acabar con la violencia de género, necesitamos más mujeres en posiciones de poder 
    La violencia machista entre los adolescentes, el drama que ni ellos quieren reconocer
    Maltrato
    ¿Podré matar a una rata que entre en mi casa? Claves de la reforma del Código Penal para castigar más el maltrato animal
    ¿Podré matar a una rata que entre en mi casa? Claves de la reforma del Código Penal para castigar más el maltrato animal
    Homicidio
    
    Género
    Americanas: Reportajes y noticias sobre feminismo e historias con enfoque de género de la región
    Por primera vez un juez declara nulo un contrato de alquiler por aplicación de la perspectiva de género
    Visitas a centros de acogida y muchas horas de estudio: así se forman los jueces en materia de género
    Para acabar con la violencia de género, necesitamos más mujeres en posiciones de poder 
    Asesinato
    
    Sexo
    
    
