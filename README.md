# ubuntu-prometheus-grafana-monitoring
## Título
Linux Server Monitoring

## Descripción
Monitorización con Prometheus, Grafana y Node Exporter que permite observar el estado de los servicios, detectar problemas y diagnosticar incidencias.

## Objetivos
-	Monitorizar una máquina Linux.
-	Recoger métricas de CPU, memoria RAM, disco y red.
-	Crear un dashboard en Grafana.
-	Configurar alertas básicas.
- Prometheus: Instalación y configuración. Recopilación de métricas del sistema. Configuración de objetivos (targets). Comprobación de disponibilidad y consultas PromQL.
- Grafana: Conectar Prometheus como fuente de datos. Crear un dashboard con CPU, RAM, disco y red. Añadir paneles con métricas relevantes.
- Node Exporter: Exponer métricas de Ubuntu. Conectar Node Exporter con Prometheus.

## Arquitectura
Ubuntu Server 20.04 -> Node Exporter: Métricas de CPU, memoria, disco, red – Puerto 9100 -> Prometheus: Recopila y almacena las métricas – Puerto 9090 -> Grafana: Dashboards y alertas – Puerto 3000

## Requisitos
Software utilizado para la ejecución de las pruebas:
- VirtualBox para la preparación del entorno y una imagen Ubuntu 20.04.1 para la instalación del sistema.
- Terminal de comandos de Ubuntu para la ejecución de los comandos, instalaciones y creación de archivos.
- Prometheus para recoger y almacenar métricas de monitorización.
- Grafana para conectarse a Prometheus y presentar métricas en dashboards con gráficos y estadísticas.
- Node Exporter para exponer información sobre CPU, memoria RAM, disco, sistema de archivos y red para que Prometheus pueda recopilarla.

## Instalación de Grafana y Prometheus
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



## Añadir Prometheus en Grafana como fuente de datos
Configurar Fuentes de Datos en Grafana:
Grafana es una capa de visualización — no almacena métricas por sí mismo. En su lugar, se conecta a fuentes de datos externas como Prometheus, InfluxDB, Elasticsearch, MySQL, PostgreSQL, y muchas otras. Configurar una fuente de datos es el primer paso esencial antes de crear cualquier panel.

Navegar a Fuentes de Datos:
En la barra lateral izquierda, clic en el icono de engranaje (⚙) para abrir el menú de Configuración.
Seleccionamos Fuentes de Datos.

Agregar una Nueva Fuente de Datos:
Hacemos clic en Agregar fuente de datos.

<img width="886" height="433" alt="image" src="https://github.com/user-attachments/assets/69758068-9fd0-44e5-8f26-11cd1ecf3a57" />

Add data source

Examinamos la lista y seleccionamos el tipo de fuente de datos que desees. En este caso Prometheus.

<img width="886" height="438" alt="image" src="https://github.com/user-attachments/assets/de63a9d1-5151-443f-9836-cf6b4b5f8f22" />

<img width="886" height="435" alt="image" src="https://github.com/user-attachments/assets/aac4c909-6525-4565-b90b-981621f14f7c" />

Ingresar Detalles de Conexión:
Completamos los parámetros de conexión para tu fuente de datos elegida. Por ejemplo, si te estás conectando a una instancia de Prometheus ejecutándose localmente:
URL: http://localhost:9090
Acceso: Server (default)
Intervalo de extracción: 15s (coincide con tu configuración de extracción de Prometheus)
Para una instancia remota de InfluxDB, proporcionarías la URL del host, el nombre de la base de datos y las credenciales de autenticación.

<img width="886" height="558" alt="image" src="https://github.com/user-attachments/assets/ee1b6c49-b52d-4f39-97d7-0e119e4c37e3" />

Guardar y Probar la Conexión:
Hacemos clic en Guardar y Probar. Grafana intentará conectarse a la fuente de datos y mostrará un mensaje de éxito o error. Una confirmación verde de “La fuente de datos está funcionando” significa que estás listo para crear paneles.

<img width="886" height="445" alt="image" src="https://github.com/user-attachments/assets/ee5c29c1-1b2a-4a0e-8f64-13c0e72852a5" />


## Creación de Dashboard
Creación de panel de Grafana:
Con una fuente de datos conectada, ahora podemos crear paneles para visualizar métricas en tiempo real.
En la barra lateral izquierda, hacemos clic en el icono + (más).
Seleccionamos Dashboard.
Hacemos clic en Add new panel.

<img width="886" height="401" alt="image" src="https://github.com/user-attachments/assets/5fcbd2c7-548a-4d34-8a20-a6f41c91119a" />

Configuramos el panel:

Se abre el editor de paneles, dándonos el control total sobre qué datos se muestran y cómo se visualizan.
Data Source: Seleccionamos la fuente de datos que configuraste anteriormente.
Query: Escribimos la consulta de métrica. Para Prometheus, usamos PromQL. Por ejemplo, para visualizar el uso de CPU:
100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)

<img width="886" height="424" alt="image" src="https://github.com/user-attachments/assets/a87b785b-115c-4df4-bdbf-c186267bb769" />

Visualization Type: Elegimos entre Time series, Gauge, Bar chart, Table, Stat, Heatmap, etc, dependiendo de la naturaleza de los datos.

<img width="886" height="426" alt="image" src="https://github.com/user-attachments/assets/e00693bd-89d1-4b09-a8e5-39e8aa34f442" />

Panel Title: Dale a tu panel un nombre descriptivo: CPU Usage — Last 1 Hour

Personalizamos y guardamos.

<img width="886" height="424" alt="image" src="https://github.com/user-attachments/assets/534c4aa2-a452-4a36-b72e-0b65ad8f3edc" />

Usamos las opciones del panel de la derecha para configurar umbrales, esquemas de color, etiquetas de ejes y leyendas. Una vez satisfecho:
Hacemos clic en Apply para añadir el panel al panel de control.
Hacemos clic en el icono Save dashboard (disquete) en la esquina superior derecha, escribimos un nombre a tu panel de control y hacemos clic en Save.

<img width="781" height="477" alt="image" src="https://github.com/user-attachments/assets/a503419b-498d-4a81-b34f-3593219a6af6" />

<img width="886" height="420" alt="image" src="https://github.com/user-attachments/assets/6da44bc1-a4d2-446c-8cf2-daba7b106d35" />

Consejo profesional: La biblioteca oficial de paneles de Grafana ofrece cientos de paneles preconstruidos, contribuidos por la comunidad, para pilas populares como Node Exporter, Kubernetes, MySQL y NGINX. Puedes importarlos directamente usando un ID de panel.


## Configuración de Alerta
El motor de alertas integrado de Grafana permite definir reglas basadas en umbrales y recibir notificaciones cuando las métricas superen niveles críticos, manteniéndote informado antes de que los pequeños problemas se conviertan en interrupciones importantes.

Creación de una Regla de Alerta:
Abrimos el panel que deseas monitorear y hacemos clic en Editar.

<img width="883" height="525" alt="image" src="https://github.com/user-attachments/assets/f8aa2b63-e985-4f2d-ae50-0a9c8a47e09d" />

Navegamos a la pestaña Alerta dentro del editor de paneles.


Hacemos clic en Crear Alerta.
Definir Condiciones de Alerta:
Configuramos las condiciones que activan la alerta. Por ejemplo:
Condición: WHEN avg() OF query(A, 5m, now) IS ABOVE 90

<img width="886" height="384" alt="image" src="https://github.com/user-attachments/assets/ed83738f-4b93-43ab-92e8-eb7441e816c2" />

<img width="886" height="374" alt="image" src="https://github.com/user-attachments/assets/24c3fcd7-c79b-4848-a68b-c930308a7e47" />

Esto activa una alerta cuando el valor promedio de tu métrica supera 90 durante los últimos 5 minutos.

Configuración de Canales de Notificación:
Vamos a Alertas → Canales de notificación en la barra lateral izquierda.
Hacemos clic en Agregar canal y elegimos el método preferido:

<img width="886" height="433" alt="image" src="https://github.com/user-attachments/assets/90ebebd5-e3bf-4a87-bee5-cbb72240fd14" />

Correo electrónico — requiere configuración SMTP en grafana.ini
Asignamos el canal de notificación a tu regla de alerta para que se notifique a las personas correctas cuando se superen los umbrales.

<img width="886" height="612" alt="image" src="https://github.com/user-attachments/assets/bde29875-ebd7-45b1-98c5-5105cf5dd614" />

<img width="886" height="440" alt="image" src="https://github.com/user-attachments/assets/12d356cd-de9e-4b6e-956e-06407e4654f6" />



## Instalación de Node Exporter para Prometheus
Node Exporter es el exportador oficial de Prometheus para métricas del sistema: CPU, memoria, disco, sistemas de archivos, red y carga. Expone esas métricas por HTTP en el puerto 9100 para que un servidor Prometheus las recoja periódicamente.

### Creación de un usuario de sistema:
Node Exporter no necesita privilegios, así que se ejecutará con un usuario propio sin shell ni directorio personal:
- sudo useradd --system --no-create-home --shell /usr/sbin/nologin node_exporter

Comprobamos que existe el usuario:
- id node_exporter

<img width="886" height="202" alt="image" src="https://github.com/user-attachments/assets/460dd284-e865-46e2-9180-65b42fdc4b42" />

### Descargar e instalar el binario:
Ubuntu incluye el paquete prometheus-node-exporter, pero trae una versión antigua. El proyecto publica binarios estáticos en GitHub, que es la forma recomendada de tener una versión actual.

<img width="886" height="394" alt="image" src="https://github.com/user-attachments/assets/bcb412b5-7c9c-415f-bf87-09a88dfdd7ff" />

Verifica la suma de comprobación antes de instalar nada:
- sha256sum --check --ignore-missing sha256sums.txt

<img width="886" height="66" alt="image" src="https://github.com/user-attachments/assets/9e6a0de2-4817-4419-92d3-90ee33bf9dca" />

node_exporter-1.12.1.linux-amd64.tar.gz: OK

### Extraemos el archivo e instalamos el binario en /usr/local/bin:
- tar xzf "node_exporter-${VERSION}.linux-${ARCH}.tar.gz"
- sudo install -m 0755 "node_exporter-${VERSION}.linux-${ARCH}/node_exporter" /usr/local/bin/
- node_exporter --version

<img width="886" height="92" alt="image" src="https://github.com/user-attachments/assets/fc46c72f-8323-404e-b12c-8cb38279126b" />

<img width="886" height="180" alt="image" src="https://github.com/user-attachments/assets/802ec67c-8dbe-49b2-a19a-758dbed23d16" />

La salida nos muestra la versión instalada.

Limpiamos los archivos temporales:
- rm -rf /tmp/node_exporter-* /tmp/sha256sums.txt

<img width="886" height="71" alt="image" src="https://github.com/user-attachments/assets/5c0a2765-e531-4517-a12e-e8393fc99997" />

### Creación del servicio systemd:
Creamos también el directorio del recolector textfile, para publicar métricas:
- sudo install -d -o node_exporter -g node_exporter -m 0755 /var/lib/node_exporter/textfile_collector

<img width="886" height="157" alt="image" src="https://github.com/user-attachments/assets/b0e45f2d-da1f-4a29-b41d-2faf95a61dc0" />

Creamos la unidad de systemd:
sudo nano /etc/systemd/system/node_exporter.service
[Unit]
Description=Prometheus Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter \
    --web.listen-address=:9100 \
    --collector.systemd \
    --collector.textfile.directory=/var/lib/node_exporter/textfile_collector
Restart=on-failure
NoNewPrivileges=yes
ProtectSystem=strict
ProtectHome=read-only

[Install]
WantedBy=multi-user.target

<img width="886" height="585" alt="image" src="https://github.com/user-attachments/assets/0a7de49f-efa2-48fc-85e5-f3601dd1481a" />

La mayoría de recolectores (CPU, memoria, disco, red, sistemas de archivos) vienen activados por defecto. --collector.systemd añade el estado de cada unidad de systemd, útil para alertar de servicios caídos. Las opciones NoNewPrivileges y Protect* limitan lo que el proceso puede tocar, ya que solo necesita leer.

Recargamos systemd, habilitamos y arrancamos el servicio:

- sudo systemctl daemon-reload
- sudo systemctl enable --now node_exporter
- systemctl status node_exporter --no-pager

<img width="886" height="312" alt="image" src="https://github.com/user-attachments/assets/34c2bf43-7f76-404f-a9e8-d9f5e8ecc001" />

Comprobamos que responde en su puerto habitual:
- curl http://localhost:9100/metrics

<img width="886" height="437" alt="image" src="https://github.com/user-attachments/assets/38c3b1ee-58ef-4050-b61d-c710ca787cbb" />

### Comprobar las métricas:
Pide el endpoint /metrics desde el propio servidor y filtra algunas métricas conocidas:
- curl -s http://localhost:9100/metrics | grep -E '^node_(load1|memory_MemAvailable_bytes|filesystem_avail_bytes\{.*mountpoint="/")'

<img width="886" height="73" alt="image" src="https://github.com/user-attachments/assets/99f1cc09-b3b5-46f5-8203-39c8879109f3" />

Cada línea es una serie: nombre de la métrica, etiquetas entre llaves y valor actual. Prometheus guardará estos valores cada vez que haga un scrape.

Comprobamos que responde en su puerto habitual:
- curl http://localhost:9100/metrics

<img width="886" height="437" alt="image" src="https://github.com/user-attachments/assets/c14c11f4-1236-400e-b641-eb9b8fcfb2b8" />

Abrimos el archivo de configuración de Prometheus, que se encuentra situado en /etc/prometheus/prometheus.yml, y comprobamos si ya contiene una sección scrape_configs.

- sudo cat /etc/prometheus/prometheus.yml

<img width="886" height="577" alt="image" src="https://github.com/user-attachments/assets/ca29d88b-7da6-454d-b0bd-d9b771c6e3fc" />

<img width="886" height="393" alt="image" src="https://github.com/user-attachments/assets/04a18e36-672b-4966-8a4a-9da69e472167" />

Indica a Prometheus que consulte a Node Exporter en el puerto 9100.

Reiniciamos Prometheus para asegurarnos de que carga la configuración actualizada:
- sudo systemctl restart prometheus

Comprobamos su estado:
- sudo systemctl status prometheus

<img width="886" height="278" alt="image" src="https://github.com/user-attachments/assets/26e60af3-3b5e-4869-ad96-274484728681" />

El servicio está en estado active (running).

Abrimos en el navegador la interfaz de Prometheus:
http://localhost:9090/targets

<img width="886" height="423" alt="image" src="https://github.com/user-attachments/assets/c38649c3-82d3-4e72-b445-b496fb5a65d7" />

Node aparece como UP, así que pasaremos a Grafana para representar métricas reales de CPU, RAM, disco y red.

Ahora vamos a Grafana para aprovechar los datos que ya está recogiendo Prometheus.
Realizamos la siguiente prueba en Grafana:
1. Abrimos el dashboard actual.
2. Editamos el panel donde configuramos anteriormente la consulta de CPU.
3. Comprobamos que la consulta utiliza una métrica real de Node Exporter.
   
Para ver el porcentaje de CPU ocupada, utilizamos esta consulta PromQL:
100 * (1 - avg by (instance) (
  rate(node_cpu_seconds_total{mode="idle"}[5m])
))

<img width="1282" height="631" alt="image" src="https://github.com/user-attachments/assets/1708d542-7a39-4bfd-b390-ce4f9567807f" />

Esta consulta calcula el porcentaje medio de CPU ocupada durante los últimos cinco minutos. Grafana muestra un valor numérico y una gráfica de CPU.

Realizamos unos pequeños ajustes en el Dashboard:
- Renombramos el Título: CPU Usage (%).

<img width="878" height="394" alt="image" src="https://github.com/user-attachments/assets/d52812d1-838c-48a4-8107-f71326012533" />

- Leyenda: {{instance}} para que aparezca localhost:9100 en lugar de {instance="localhost:9100"}.

<img width="886" height="183" alt="image" src="https://github.com/user-attachments/assets/acd09b16-a62f-4a3a-b9e9-5cdd1e2170bd" />

- Visualización: Añadimos un panel tipo Gauge para ver el porcentaje actual.

<img width="886" height="380" alt="image" src="https://github.com/user-attachments/assets/8db336d2-7554-49e4-8151-c7236afff0e0" />


Después completaremos RAM, disco y red, y revisaremos las alertas y las notificaciones.

### Creación de paneles para Memoria RAM, Espacio de disco utilizado y Tráfico de Red en el mismo Dashboard
-	Memoria RAM:
100 * node_memory_MemAvailable_bytes
/ node_memory_MemTotal_bytes

<img width="1279" height="625" alt="image" src="https://github.com/user-attachments/assets/3e1bf1dc-e20e-4ed2-9068-b296fb13d496" />

Este cálculo muestra el porcentaje de RAM disponible. Configura la unidad como porcentaje de 0 a 100.


-	Espacio de disco utilizado:
100 * (1 -
  node_filesystem_avail_bytes{
    mountpoint="/",
    fstype!~"tmpfs|overlay"
  }
  /
  node_filesystem_size_bytes{
    mountpoint="/",
    fstype!~"tmpfs|overlay"
  }
)

<img width="1256" height="591" alt="image" src="https://github.com/user-attachments/assets/056acf9b-25f9-40c4-8d4f-796caeaa65f7" />

Representa el uso del sistema de archivos raíz. Comprueba que el selector devuelve datos en tu Ubuntu.
Muestra el porcentaje ocupado en el sistema de archivos raíz /. Configura la unidad como porcentaje.

-	Tráfico de Red:
rate(node_network_receive_bytes_total{
  device!="lo"
}[5m])

<img width="1265" height="584" alt="image" src="https://github.com/user-attachments/assets/1ad60be4-35f8-48a4-829f-7f94ea474016" />

Muestra los bytes recibidos por segundo en las interfaces de red, excluyendo la interfaz local lo. Configura la unidad como bytes/sec (bytes por segundo).

<img width="886" height="549" alt="image" src="https://github.com/user-attachments/assets/4acfe1f1-0352-4df6-b0f0-563dc79deef5" />

Hemos terminado de completar el Dashboard con sus 4 paneles configurados (CPU, RAM, Disco y Red). También lo hemos renombrado de nombre Ubuntu System Monitoring.

