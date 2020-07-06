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
En las siguientes líneas
```
cases_str = str(cases.string)
index = cases_str.find("Highcharts.chart('coronavirus-cases-daily', {")
daily_cases = cases_str[index:]
```

```
cat_start_index = daily_cases.find("categories: ")
temp =daily_cases[cat_start_index:]
cat_end_index = temp.find("]")
dataString = temp[len("categories: ") : cat_end_index+1]
```

```
data_start_index = daily_cases.find("data: ")
temp2 =daily_cases[data_start_index:]
data_end_index = temp2.find("]")
casesString = temp2[len("data: ") : data_end_index+1]
```

```
Date = ast.literal_eval(dataString)
print(Date)
```

```
casesString = casesString.replace('null','0')
Cases = ast.literal_eval(casesString.strip())
print(Cases)
```
