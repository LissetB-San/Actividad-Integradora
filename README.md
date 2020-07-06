# Actividad-Integradora 
## Integrantes del equipo
Juan Pablo Ramos Sanabria, 
Diego Alberto Alvarez Rodríguez, 
César Francisco Barraza Aguilar, 
César Buenfil Vázquez y 
Lisset Botello Santiago.

## Planteamiento del problema 
Diseñar un web scrapper para la recolección de datos sobre la cantidad diaria de casos nuevos de coronavirus en el mundo. La información será obtenida de la página: https://www.worldometers.info/coronavirus/

 <p align="center">
  <img src="https://github.com/LissetB-San/Actividad-Integradora/blob/master/A.PNG">
</p>

Se deberán imprimir los datos en dos listas diferentes, la primera tendrá las fechas y se llamará "Date" y la segunda tendrá los casos y se llamará "Cases".

## Explicación del código
Para poder comenzar con el web scrapper, importamos las siguientes librerías:
```
import requests
from bs4 import BeautifulSoup
import ast
import matplotlib.pyplot as plt
```
Una vez importadas las librerías escribirmos el URL de la página y lo mandamos a llamar. Recupera todos los datos HTML que el servidor devuelve y los almacena en un objeto Python.
```
URL = 'https://www.worldometers.info/coronavirus/'
page = requests.get(URL)
```
Creamos un objeto Beautiful Soup, el cual va a tomar el contenido del que raspó como entrada en las líneas anteriores. También le indica a Beautiful Soup que use el analizador apropiado.
```
soup = BeautifulSoup(page.content, 'html.parser')
```
Dentro de la variable **soup** llamamos lo que se encuentra en el script y dentro de la variable **cases** guardamos los datos donde se encuentran solamente los casos.
```
scripts = soup.find_all('script')
cases = scripts[21]
```
En las siguientes líneas convertimos el contenido de **cases** a formato string para poder utilizar la herramienta **.find** y buscar las palabras _'coronavirus-cases-daily'_ dentro del elemento **chart** y definimos **daily_cases** como el contenido de **cases_str** a partir del elemento **index**.
```
cases_str = str(cases.string)
index = cases_str.find("Highcharts.chart('coronavirus-cases-daily', {")
daily_cases = cases_str[index:]
```
Generamos el vector **dataString** con un slip seccionado donde definimos el inicio como **cat_start_index** y el final como **cat_end_index**, todo a partir del contenido de las categories de **daily_cases**.

```
cat_start_index = daily_cases.find("categories: ")
temp =daily_cases[cat_start_index:]
cat_end_index = temp.find("]")
dataString = temp[len("categories: ") : cat_end_index+1]
```

De igual forma que lo anterior, generamos el vector **casesString** con un slip seccionado donde definimos el inicio como **data_start_inde**s y el final como **data_end_index**, todo a partir de la data de **daily_cases**.
```
data_start_index = daily_cases.find("data: ")
temp2 =daily_cases[data_start_index:]
data_end_index = temp2.find("]")
casesString = temp2[len("data: ") : data_end_index+1]
```

Para presentar el contenido de **dataString** primero generamos una evaluación y posteriormente imprimimos el resultado.
```
Date = ast.literal_eval(dataString)
print(Date)
```

Para presentar el contenido de **casesString** primero sustituimos los elementos _'null'_ y los cambiamos por el valor numerico 0 y al igual que el paso anterior generamos una evaluacion y posteriormente imprimimos el resultado.
```
casesString = casesString.replace('null','0')
Cases = ast.literal_eval(casesString.strip())
```
## Resultados
Impresión de la lista **Date**:
```
['Jan 22', 'Jan 23', 'Jan 24', 'Jan 25', 'Jan 26', 'Jan 27', 'Jan 28', 'Jan 29', 'Jan 30', 'Jan 31', 'Feb 01', 'Feb 02', 'Feb 03', 'Feb 04', 'Feb 05', 'Feb 06', 'Feb 07', 'Feb 08', 'Feb 09', 'Feb 10', 'Feb 11', 'Feb 12', 'Feb 13', 'Feb 14', 'Feb 15', 'Feb 16', 'Feb 17', 'Feb 18', 'Feb 19', 'Feb 20', 'Feb 21', 'Feb 22', 'Feb 23', 'Feb 24', 'Feb 25', 'Feb 26', 'Feb 27', 'Feb 28', 'Feb 29', 'Mar 01', 'Mar 02', 'Mar 03', 'Mar 04', 'Mar 05', 'Mar 06', 'Mar 07', 'Mar 08', 'Mar 09', 'Mar 10', 'Mar 11', 'Mar 12', 'Mar 13', 'Mar 14', 'Mar 15', 'Mar 16', 'Mar 17', 'Mar 18', 'Mar 19', 'Mar 20', 'Mar 21', 'Mar 22', 'Mar 23', 'Mar 24', 'Mar 25', 'Mar 26', 'Mar 27', 'Mar 28', 'Mar 29', 'Mar 30', 'Mar 31', 'Apr 01', 'Apr 02', 'Apr 03', 'Apr 04', 'Apr 05', 'Apr 06', 'Apr 07', 'Apr 08', 'Apr 09', 'Apr 10', 'Apr 11', 'Apr 12', 'Apr 13', 'Apr 14', 'Apr 15', 'Apr 16', 'Apr 17', 'Apr 18', 'Apr 19', 'Apr 20', 'Apr 21', 'Apr 22', 'Apr 23', 'Apr 24', 'Apr 25', 'Apr 26', 'Apr 27', 'Apr 28', 'Apr 29', 'Apr 30', 'May 01', 'May 02', 'May 03', 'May 04', 'May 05', 'May 06', 'May 07', 'May 08', 'May 09', 'May 10', 'May 11', 'May 12', 'May 13', 'May 14', 'May 15', 'May 16', 'May 17', 'May 18', 'May 19', 'May 20', 'May 21', 'May 22', 'May 23', 'May 24', 'May 25', 'May 26', 'May 27', 'May 28', 'May 29', 'May 30', 'May 31', 'Jun 01', 'Jun 02', 'Jun 03', 'Jun 04', 'Jun 05', 'Jun 06', 'Jun 07', 'Jun 08', 'Jun 09', 'Jun 10', 'Jun 11', 'Jun 12', 'Jun 13', 'Jun 14', 'Jun 15', 'Jun 16', 'Jun 17', 'Jun 18', 'Jun 19', 'Jun 20', 'Jun 21', 'Jun 22', 'Jun 23', 'Jun 24', 'Jun 25', 'Jun 26', 'Jun 27', 'Jun 28', 'Jun 29', 'Jun 30', 'Jul 01', 'Jul 02', 'Jul 03', 'Jul 04']
```
Impresión de la lista **Cases**:
```
[0, 265, 472, 698, 785, 1781, 1477, 1755, 2010, 2127, 2603, 2838, 3239, 3915, 3721, 3173, 3437, 2676, 3001, 2546, 2035, 14153, 5151, 2662, 2097, 2132, 2003, 1852, 516, 977, 996, 978, 554, 885, 738, 992, 1288, 1509, 1989, 1980, 1862, 2570, 2306, 3090, 3637, 4052, 3894, 4434, 4644, 7232, 8258, 10912, 10959, 13016, 12933, 15763, 20685, 26140, 30665, 29407, 32435, 41546, 43793, 48618, 60940, 64649, 66761, 60465, 64234, 74000, 77121, 80175, 84056, 81569, 70438, 73040, 78346, 84432, 85208, 91553, 79225, 71494, 70044, 73078, 82757, 80777, 84939, 80289, 75160, 73458, 75430, 80071, 84853, 101695, 90253, 73261, 69349, 75444, 79861, 85848, 94982, 82901, 82174, 79429, 81443, 95884, 96330, 96597, 89342, 80209, 74657, 85731, 89767, 97301, 100393, 96957, 82562, 90485, 95859, 103538, 108410, 108938, 100497, 97349, 90910, 93893, 107543, 117751, 126931, 125464, 109915, 105730, 116420, 121370, 131432, 131404, 129633, 114906, 108639, 122007, 136905, 139260, 143400, 135253, 124523, 126072, 144127, 146589, 141605, 182488, 158036, 131075, 140073, 164509, 174307, 181198, 195404, 178390, 164646, 162047, 175349, 197520, 209379, 209028, 189413]
```
