# ubuntu-prometheus-grafana-monitoring
## Título
Linux Server Monitoring

## Descripción
Monitorización con Prometheus, Grafana y Node Exporter que permite observar el estado de los servicios, detectar problemas y diagnosticar incidencias.

## Objetivos
-	Monitorizar una máquina Linux.
-	Recoger métricas de CPU, memoria y disco.
-	Crear un dashboard en Grafana.
-	Configurar alertas básicas.
Prometheus: Instalación y configuración. Recopilación de métricas del sistema. Configuración de objetivos (targets). Comporbación de disponibilidad y consultas PromQL.
Grafana: Conectar Prometheus como fuente de datos. Crear un dashboard con CPU, RAM, disco y red. Añadir paneles con métricas relevantes. Configurar umbrales y alertas.
Node Exporter: Exponer métricas de Ubuntu. Conectar Node Exporter con Prometheus. Verificar que las métricas llegan correctamente. Provocar carga controlada y observar los cambios.
Pruebas de diagnóstico: Generar consumo de CPU de forma controlada. Simular la caída de un servicio de monitorización. Identificar el problema en las métricas. Documentar la incidencia y su solución.

## Arquitectura
Ubuntu Server 20.04 -> Node Exporter: Métricas de CPU, memoria, disco, red – Puerto 9100 -> Prometheus: Recopila y almacena las métricas – Puerto 9090 -> Grafana: Dashboards y alertas – Puerto 3000

## Requisitos
Software utilizado para la ejecución de las pruebas:
VirtualBox para la preparación del entorno y una imagen Ubuntu 20.04.1 para la instalación del sistema.
Terminal de comandos de Ubuntu para la ejecución de los comandos, instalaciones y creación de archivos.
Prometheus para recoger y almacenar métricas de monitorización.
Grafana para conectarse a Prometheus y presentar métricas en dashboards con gráficos y estadísticas.
Node Exporter para exponer información sobre CPU, memoria RAM, disco, sistema de archivos y red para que Prometheus pueda recopilarla.
