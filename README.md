# plot-advisor

Repositorio principal del proyecto **Plot Advisor**. Este repositorio funciona como contenedor de los submódulos donde vive cada parte de la solución.

## Resumen

Plot Advisor ayuda a agricultores a conocer mejor sus terrenos y a tomar decisiones sobre sus cultivos.
Sus balizas con sensores recogen información del campo y del entorno para mostrar qué está ocurriendo en cada zona.
Con estos datos, ofrece recomendaciones que facilitan la planificación y el seguimiento de las cosechas.

## Resumen técnico

**Plot Advisor** es una plataforma para monitorizar cultivos y ayudar a decidir qué plantar.
Los nodos ESP32 recogen datos ambientales mediante sensores y los envían a una Raspberry Pi.
La infraestructura almacena y visualiza las mediciones con MQTT, Telegraf, InfluxDB y Grafana.
El backend procesa los datos, consulta información externa y genera predicciones y recomendaciones.
El frontend ofrece un mapa y un panel web para consultar zonas, cultivos, balizas y resultados.

## Estructura del repositorio

- `crops-analyzer-frontend`: interfaz de usuario del proyecto.
- `crops-analyzer-backend`: API y lógica de servidor.
- `crops-analyzer-module`: código para los nodos ESP32.
- `crops-analyzer-infra`: infraestructura en contenedores (docker compose completo y despliegue en la Raspberry en http://plotadvisor.local).

## Levantar todo con Docker

```bash
git clone --recurse-submodules https://github.com/Payasos-del-Goya/plot-advisor
cd plot-advisor/crops-analyzer-infra
docker compose up -d --build
```

## Estudio económico de una implementación mínima con 3 nodos

### Alcance y arquitectura

Se considera una instalación mínima con **tres balizas de campo** y un
**puente**. Las balizas forman la malla `painlessMesh` y envían sus lecturas
al puente; este las publica por MQTT en la Raspberry Pi. La Raspberry ejecuta
Mosquitto, Telegraf, InfluxDB, Grafana, PostgreSQL, el backend y el frontend
mediante Docker, tal como se describe en el
[`crops-analyzer-infra`](https://github.com/Payasos-del-Goya/plot-advisor/tree/main/crops-analyzer-infra).

Esta es una reducción de la finca `c001` del diseño de infraestructura, que
contempla cuatro balizas reales y un puente. Se mantienen los mismos
componentes funcionales y se reduce únicamente el número de balizas a tres.

### Coste de las balizas

El cálculo parte del estudio de ensamblaje del repositorio
[`crops-analyzer-module`](https://github.com/Payasos-del-Goya/crops-analyzer-module):

| Concepto | Coste unitario | Unidades | Subtotal |
|---|---:|---:|---:|
| Materiales de una baliza Sensor | 59,23 € | 3 | 177,69 € |
| Reserva del 10 % para consumibles y montaje físico | 5,92 € | 3 | 17,76 € |
| **Total recomendado de las tres balizas** | **65,15 €** | **3** | **195,45 €** |

Cada baliza incluye una placa ESP32, sensores FC-28, DHT11 y LDR, batería
18650 protegida, cargador solar CN3065, elevador a 5 V, panel solar,
portabatería, caja IP65, cableado, módulo LoRa y sensor de pH. El estudio
incluye el módulo de pH aunque el firmware actual todavía simula esa lectura.
Los precios son orientativos, no incluyen transporte, IVA ni mano de obra y
se deben volver a comprobar antes de comprar.

### Coste del puente y de la infraestructura

El puente es otra placa ESP32, pero no necesita repetir el conjunto de sensores
de una baliza. Tomando como referencia la placa del estudio:

| Elemento | Coste considerado |
|---|---:|
| ESP32 del puente | 8,49 € |
| Tres balizas autónomas | 195,45 € |
| Raspberry Pi 5 Starter Kit (4 GB, 64 GB, fuente de 27 W) | 189,99 € |
| LoRa + antena para la Raspberry Pi | 8,59 € |
| 3G/4G LTE HAT Mini para la Raspberry Pi | 25,57 € |
| **Coste conocido de la implementación mínima** | **428,09 €** |

Este total supone que el puente dispone de alimentación y protección física en
su ubicación. Si también debe funcionar como un equipo exterior autónomo,
conviene asignarle el mismo kit completo de una baliza (65,15 €); en ese caso,
el coste conocido de balizas, puente y Raspberry asciende a **484,75 €**.

La Raspberry Pi 5 se valora con el
[Starter Kit de Amazon](https://amzn.eu/d/04FdHqNl), de 4 GB de RAM, 64 GB de
almacenamiento y fuente de 27 W, por **189,99 €**. El precio puede cambiar y
se toma como referencia de compra, no como una tarifa fija. Se añade un
[3G/4G LTE HAT Mini](https://es.aliexpress.com/item/1005007875411312.html)
por **25,57 €** para proporcionar conectividad móvil a la Raspberry Pi. El software
descrito (Docker, PostgreSQL, InfluxDB, Mosquitto, Telegraf, Grafana, Icinga y
la aplicación) no añade licencias de pago. Tampoco se incluyen posibles costes
recurrentes de Internet, Meteomatics, mapas o notificaciones, porque no están
valorados en las fuentes.

### Resultado y limitaciones

El **coste mínimo conocido** para tres balizas, un puente alimentado por una
instalación existente, la Raspberry Pi, el segundo set de LoRa y el HAT LTE es
**428,09 €**. Para una instalación
exterior completamente autónoma, incluyendo el kit completo para el puente,
la referencia es **484,75 €**.

El estudio energético estima unos 31,24 mA por ESP32 y una autonomía real de
aproximadamente dos o tres días con una placa de desarrollo y una batería
18650 de 3000 mAh. Por ello, el panel solar, el cargador y la protección de la
batería no son accesorios opcionales en una instalación exterior. Estas
autonomías son estimaciones teóricas y deben validarse midiendo el consumo
real de cada montaje.
