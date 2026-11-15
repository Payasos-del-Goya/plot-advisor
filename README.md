# plot-advisor

Repositorio principal del proyecto **Plot Advisor**. Este repositorio funciona como contenedor de los submódulos donde vive cada parte de la solución.

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
