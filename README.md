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
- Prometheus: Instalación y configuración. Recopilación de métricas del sistema. Configuración de objetivos (targets). Comporbación de disponibilidad y consultas PromQL.
- Grafana: Conectar Prometheus como fuente de datos. Crear un dashboard con CPU, RAM, disco y red. Añadir paneles con métricas relevantes. Configurar umbrales y alertas.
- Node Exporter: Exponer métricas de Ubuntu. Conectar Node Exporter con Prometheus. Verificar que las métricas llegan correctamente. Provocar carga controlada y observar los cambios.
- Pruebas de diagnóstico: Generar consumo de CPU de forma controlada. Simular la caída de un servicio de monitorización. Identificar el problema en las métricas. Documentar la incidencia y su solución.

## Arquitectura
Ubuntu Server 20.04 -> Node Exporter: Métricas de CPU, memoria, disco, red – Puerto 9100 -> Prometheus: Recopila y almacena las métricas – Puerto 9090 -> Grafana: Dashboards y alertas – Puerto 3000

## Requisitos
Software utilizado para la ejecución de las pruebas:
- VirtualBox para la preparación del entorno y una imagen Ubuntu 20.04.1 para la instalación del sistema.
- Terminal de comandos de Ubuntu para la ejecución de los comandos, instalaciones y creación de archivos.
- Prometheus para recoger y almacenar métricas de monitorización.
- Grafana para conectarse a Prometheus y presentar métricas en dashboards con gráficos y estadísticas.
- Node Exporter para exponer información sobre CPU, memoria RAM, disco, sistema de archivos y red para que Prometheus pueda recopilarla.

## Instalación
Primeramente, actualizamos el sistema de la máquina Ubuntu mediante apt update y apt upgrade.

<img width="886" height="469" alt="image" src="https://github.com/user-attachments/assets/db5217f9-0da0-47f5-8cd3-7b579019498e" />

<img width="886" height="585" alt="image" src="https://github.com/user-attachments/assets/238f2149-103e-470d-8c92-fea3a7382274" />

Instalamos Grafana, haciendo la instalación on-premises (en las instalaciones), utilizando la versión Open Source:
sudo apt-get install -y apt-transport-https

<img width="886" height="219" alt="image" src="https://github.com/user-attachments/assets/6294b77d-014a-4f66-98b9-0602df9474fe" />

sudo apt-get install -y software-properties-common wget

<img width="886" height="219" alt="image" src="https://github.com/user-attachments/assets/2febf9be-f16f-467e-8781-91f3ee13a287" />

wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -

<img width="886" height="36" alt="image" src="https://github.com/user-attachments/assets/7d3ab6c5-88c8-47b7-ac78-0fe57e116a6f" />

Agregamos el siguiente repositorio para versiones más estables:

echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list

<img width="886" height="44" alt="image" src="https://github.com/user-attachments/assets/0977d87b-e5ae-419a-943f-a0824a5660db" />

Descargar y añadir la clave GPG de Grafana:

sudo mkdir -p /etc/apt/keyrings

wget -q -O - https://api.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null

<img width="886" height="54" alt="image" src="https://github.com/user-attachments/assets/2f4f0d95-2867-45cc-9dad-dc00a80dd0d0" />

Añadimos el repositorio oficial de Grafana:

echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://googleapis.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list

<img width="886" height="44" alt="image" src="https://github.com/user-attachments/assets/2ab6899c-4519-478b-b89b-3202b298311c" />

sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"

<img width="886" height="309" alt="image" src="https://github.com/user-attachments/assets/ec69046a-e3cd-423b-94c8-f2d31c2242a6" />

Finalmente instalamos Grafana:

sudo apt install grafana -y

<img width="886" height="183" alt="image" src="https://github.com/user-attachments/assets/d3bab1fe-6c09-4076-a276-e887d6af2f30" />

Después de la instalación, iniciamos el daemon del servidor Grafana y configuramos para que se lance automáticamente al iniciar el sistema:

sudo systemctl start grafana-server

sudo systemctl enable grafana-server

<img width="886" height="42" alt="image" src="https://github.com/user-attachments/assets/b46760cb-f2a7-4a49-9b9a-35a334b0f388" />


Verificamos que el servicio se esté ejecutando correctamente:

sudo systemctl status grafana-server

<img width="886" height="401" alt="image" src="https://github.com/user-attachments/assets/019b800e-f4e7-42c6-a503-2d7bdaeb846d" />

Accedemos a la Interfaz Web de Grafana
Por defecto, Grafana escucha en el puerto 3000. Abrimos navegador web y escribimos lo siguiente:
Nuestra dirección IP de la máquina o hostname y el puerto 3000 de grafana.

<img width="886" height="489" alt="image" src="https://github.com/user-attachments/assets/abd36212-9cec-41ac-afad-e819f113a337" />

Una vez logeados con el usuario y contraseña por defecto, grafana nos pide cambiar la contraseña por una nueva por motivos de seguridad.

En el primer inicio de sesión, Grafana te pedirá inmediatamente que establezca una contraseña nueva y segura. No omitir este paso -- dejar las credenciales predeterminadas es un riesgo de seguridad significativo.


<img width="886" height="470" alt="image" src="https://github.com/user-attachments/assets/01bc851d-4cc9-4289-bf5e-4fe388739a3e" />

Instalamos Prometheus:

<img width="886" height="337" alt="image" src="https://github.com/user-attachments/assets/37245243-7a41-4426-a9bd-8761bff00c8d" />

Verificamos que el servicio se esté ejecutando correctamente:

sudo systemctl status prometheus 

<img width="886" height="288" alt="image" src="https://github.com/user-attachments/assets/75ac3ec9-c9f9-4d8f-8be9-6e915ed690c3" />

El puerto de Prometheus es 9090.
Acceso vía web con nuestra ip o nuestro hostname.

<img width="886" height="437" alt="image" src="https://github.com/user-attachments/assets/f95bb68a-300b-41f5-913e-80d46eed0a3e" />


