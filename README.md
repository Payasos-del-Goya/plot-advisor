# plot-advisor

Repositorio principal del proyecto **Plot Advisor**. Este repositorio funciona como contenedor de los submódulos donde vive cada parte de la solución.

## Estructura del repositorio

- `crops-analyzer-frontend`: interfaz de usuario del proyecto.
- `crops-analyzer-backend`: API y lógica de servidor.
- `crops-analyzer-module`: módulo compartido con utilidades y lógica de análisis.
- `crops-analyzer-infra`: infraestructura en contenedores (docker compose completo y despliegue en la Raspberry en http://plotadvisor.local).

## README de los submódulos

- Frontend: https://github.com/Payasos-del-Goya/crops-analyzer-frontend/blob/main/README.md
- Backend: https://github.com/Payasos-del-Goya/crops-analyzer-backend/blob/main/README.md
- Módulo de análisis: https://github.com/Payasos-del-Goya/crops-analyzer-module/blob/main/README.md
- Infraestructura: https://github.com/Payasos-del-Goya/crops-analyzer-infra/blob/main/README.md

## Levantar todo con Docker

```bash
git clone --recurse-submodules https://github.com/Payasos-del-Goya/plot-advisor
cd plot-advisor/crops-analyzer-infra
docker compose up -d --build
```

Detalles (servicios, puertos, usuarios de demo) en el README de `crops-analyzer-infra`.