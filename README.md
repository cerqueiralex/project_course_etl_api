# Pipeline ETL extraindo dados da API de tempo e clima para base de dados SQLite

Este projeto implementa um pipeline ETL utilizando Airflow e Docker para extrair dados meteorológicos de uma API pública e armazená-los em um banco de dados SQLite. A aplicação realiza requisições periódicas para capturar informações como temperatura, velocidade do vento, condição do tempo, umidade e visibilidade, estruturando e salvando os dados em formato CSV para posterior análise. O ambiente é orquestrado com Docker Compose, e a configuração segue os padrões recomendados pelo Apache Airflow, garantindo fácil reprodutibilidade e escalabilidade do fluxo de dados.

<img src="https://i.imgur.com/0Hn2Fs0.jpeg" style="width:100%;height:auto"/>

Tecnologias:  
* Airflow
* Docker
* SQLite
* Python

## Subindo Ambiente Docker

https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html

Acessando pasta do projeto com cd/path

```
docker compose up airflow-init
```
```
docker compose up
```

## Configurando Airflow

Configuração padrão de pastas do Airflow:

> config  
> dags  
> logs  
> plugins  
> .env  

http://localhost:8080/login

* User: airflow
* Senha: airflow

## Configurando Weather API

Cadastro: https://www.weatherapi.com/  

A API é criada automaticamente após cadastro. 

Ex: a06b8ffd337e47e09df43730251704  

Teste API: https://www.weatherapi.com/api-explorer.aspx

Call (London):

```
http://api.weatherapi.com/v1/current.json?key=a06b8ffd337e47e09df43730251704&q=London&aqi=no
```

Responde Code:
```
200
```

Response Headers:
```
{
  "Transfer-Encoding": "chunked",
  "Connection": "keep-alive",
  "Vary": "Accept-Encoding",
  "CDN-PullZone": "93447",
  "CDN-Uid": "8fa3a04a-75d9-4707-8056-b7b33c8ac7fe",
  "CDN-RequestCountryCode": "GB",
  "x-weatherapi-qpm-left": "5000001",
  "CDN-ProxyVer": "1.23",
  "CDN-RequestPullSuccess": "True",
  "CDN-RequestPullCode": "200",
  "CDN-CachedAt": "04/17/2025 04:41:13",
  "CDN-EdgeStorageId": "1075",
  "CDN-RequestId": "3dd995fa137d00c2cb81fcb2163c67c8",
  "CDN-Cache": "MISS",
  "CDN-Status": "200",
  "CDN-RequestTime": "0",
  "Cache-Control": "public, max-age=180",
  "Content-Type": "application/json",
  "Date": "Thu, 17 Apr 2025 04:41:13 GMT",
  "Server": "BunnyCDN-DE1-1077"
}
```


Response Body:
```
{
    "location": {
        "name": "London",
        "region": "City of London, Greater London",
        "country": "United Kingdom",
        "lat": 51.5171,
        "lon": -0.1062,
        "tz_id": "Europe/London",
        "localtime_epoch": 1744865102,
        "localtime": "2025-04-17 05:45"
    },
    "current": {
        "last_updated_epoch": 1744864200,
        "last_updated": "2025-04-17 05:30",
        "temp_c": 3.3,
        "temp_f": 37.9,
        "is_day": 0,
        "condition": {
            "text": "Clear",
            "icon": "//cdn.weatherapi.com/weather/64x64/night/113.png",
            "code": 1000
        },
        "wind_mph": 2.2,
        "wind_kph": 3.6,
        "wind_degree": 224,
        "wind_dir": "SW",
        "pressure_mb": 1011.0,
        "pressure_in": 29.85,
        "precip_mm": 0.0,
        "precip_in": 0.0,
        "humidity": 93,
        "cloud": 0,
        "feelslike_c": 2.8,
        "feelslike_f": 37.1,
        "windchill_c": 6.2,
        "windchill_f": 43.2,
        "heatindex_c": 6.2,
        "heatindex_f": 43.2,
        "dewpoint_c": 3.6,
        "dewpoint_f": 38.4,
        "vis_km": 10.0,
        "vis_miles": 6.0,
        "uv": 0.0,
        "gust_mph": 6.7,
        "gust_kph": 10.8
    }
}
```
## Resultado

Arquivos gerados após ETL

<img src="https://i.imgur.com/7VxH8mu.png" style="width:100%;height:auto"/>

Arquivo final CSV

```
temperature,wind_speed,condition,precipitation,humidity,feels_like_temp,pressure,visibility,is_day,timestamp,ID
20.0,3.6,Partly cloudy,0.57,88,20.0,1018.0,10.0,False,2025-04-17 05:20:57.901877,2025-04-17 05:20:57.901877-20.0
20.0,3.6,Partly cloudy,0.57,88,20.0,1018.0,10.0,False,2025-04-17 05:22:08.401159,2025-04-17 05:22:08.401159-20.0
```

Visualização no DB SQLite

<img src="https://i.imgur.com/x5d9uVB.png" style="width:100%;height:auto"/>
