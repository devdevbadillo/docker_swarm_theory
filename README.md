# Docker Swarm

## Tabla de Contenido

- [Introducción a la Orquestación de Contenedores y Docker Swarm](#introduccion-orquestacion-docker-swarm)
  - [¿Qué es la Orquestación de Contenedores?](#que-es-orquestacion-contenedores)
  - [¿Por qué Docker Swarm?](#por-que-docker-swarm)
  - [Docker Swarm vs. Docker Compose](#docker-swarm-vs-docker-compose)
  - [Docker Swarm vs. Kubernetes (Visión General)](#docker-swarm-vs-kubernetes)

- [Conceptos Centrales de Docker Swarm](#conceptos-centrales-docker-swarm)
  - [Nodos (Nodes): Manager y Worker](#nodos-manager-worker)
  - [Servicios (Services)](#servicios-docker-swarm)
  - [Tareas (Tasks)](#tareas-docker-swarm)
  - [Stacks](#stacks-docker-swarm)
  - [Redes Overlay](#redes-overlay-docker-swarm)
  - [Balanceo de Carga](#balanceo-carga-docker-swarm)
  - [Modo Swarm](#modo-swarm)

- [Configuración de un Cluster Docker Swarm](#configuracion-cluster-docker-swarm)
  - [Requisitos Previos](#requisitos-previos-docker-swarm)
  - [Inicializar el Swarm (`docker swarm init`)](#inicializar-swarm)
  - [Unir Nodos Worker (`docker swarm join`)](#unir-nodos-worker)
  - [Unir Nodos Manager Adicionales](#unir-nodos-manager-adicionales)
  - [Listar y Gestionar Nodos (`docker node ls`)](#listar-gestionar-nodos)
  - [Drenar y Eliminar Nodos](#drenar-eliminar-nodos)

- [Despliegue y Gestión de Servicios](#despliegue-gestion-servicios)
  - [Crear un Servicio (`docker service create`)](#crear-servicio)
    - [Definición de Imágenes](#definicion-imagenes-servicio)
    - [Publicación de Puertos](#publicacion-puertos-servicio)
    - [Variables de Entorno](#variables-entorno-servicio)
    - [Volúmenes](#volumenes-servicio)
  - [Escalar Servicios (`docker service scale`)](#escalar-servicios)
  - [Actualizar Servicios (`docker service update`)](#actualizar-servicios)
    - [Estrategias de Despliegue (Rolling Update)](#estrategias-despliegue)
    - [Rollbacks](#rollbacks-servicio)
  - [Eliminar Servicios (`docker service rm`)](#eliminar-servicios)
  - [Inspeccionar Servicios (`docker service inspect`)](#inspeccionar-servicios)
  - [Logs de Servicios (`docker service logs`)](#logs-servicios)

- [Despliegue y Gestión de Stacks (Docker Compose en Swarm)](#despliegue-gestion-stacks)
  - [¿Qué es un Stack?](#que-es-stack)
  - [Archivo `docker-compose.yml` para Swarm](#docker-compose-yml-swarm)
    - [Versión 3 del Formato](#version-3-compose)
    - [Sintaxis Específica de Swarm](#sintaxis-especifica-swarm)
  - [Desplegar un Stack (`docker stack deploy`)](#desplegar-stack)
  - [Listar Stacks y Servicios de un Stack](#listar-stacks-servicios)
  - [Actualizar Stacks](#actualizar-stacks)
  - [Eliminar Stacks (`docker stack rm`)](#eliminar-stacks)

- [Redes en Docker Swarm](#redes-docker-swarm)
  - [Conceptos de Redes de Contenedores](#conceptos-redes-contenedores)
  - [Redes Overlay (`overlay`)](#redes-overlay)
    - [Creación de Redes Overlay](#creacion-redes-overlay)
    - [Conexión de Servicios a Redes Overlay](#conexion-servicios-overlay)
  - [Red de Ingress (`ingress network`)](#red-ingress)
    - [Publicación de Puertos Externos](#publicacion-puertos-externos)
    - [Modo de Publicación (Ingress vs Host)](#modo-publicacion-ingress-host)
  - [Balanceo de Carga Interno y Externo](#balanceo-carga-interno-externo)
    - [DNS Round Robin](#dns-round-robin)
    - [Routing Mesh](#routing-mesh)

- [Almacenamiento en Docker Swarm](#almacenamiento-docker-swarm)
  - [Volúmenes (`volumes`)](#volumenes-docker-swarm)
    - [Volúmenes Nombrados](#volumenes-nombrados)
    - [Volúmenes Anónimos](#volumenes-anonimos)
  - [Bind Mounts](#bind-mounts-docker-swarm)
  - [Plugins de Volúmenes (Volume Plugins)](#plugins-volumenes)
    - [Almacenamiento Compartido (NFS, GlusterFS)](#almacenamiento-compartido)
  - [Consideraciones de Persistencia en un Cluster](#consideraciones-persistencia-cluster)

- [Seguridad en Docker Swarm](#seguridad-docker-swarm)
  - [Comunicación TLS entre Nodos](#comunicacion-tls-nodos)
  - [Gestión de Secretos (`docker secret`)](#gestion-secretos)
    - [Crear y Usar Secretos](#crear-usar-secretos)
    - [Actualizar y Eliminar Secretos](#actualizar-eliminar-secretos)
  - [Configuraciones (`docker config`)](#configuraciones-docker-swarm)
    - [Crear y Usar Configuraciones](#crear-usar-configuraciones)
  - [Seguridad de Imágenes (Image Signing)](#seguridad-imagenes)
  - [Control de Acceso Basado en Roles (RBAC - Integración)](#rbac-docker-swarm)

- [Monitorización y Logging](#monitorizacion-logging-docker-swarm)
  - [Monitorización Básica de Docker Swarm](#monitorizacion-basica-docker-swarm)
    - [`docker stats`](#docker-stats)
    - [`docker node ps`](#docker-node-ps)
  - [Herramientas de Monitorización Externas](#herramientas-monitorizacion-externas)
    - [Prometheus y Grafana](#prometheus-grafana-docker-swarm)
    - [ELK Stack (Elasticsearch, Logstash, Kibana)](#elk-stack-docker-swarm)
  - [Estrategias de Logging](#estrategias-logging-docker-swarm)
    - [Drivers de Logging](#drivers-logging-docker-swarm)
    - [Centralización de Logs](#centralizacion-logs-docker-swarm)

- [Temas Avanzados y Mejores Prácticas](#temas-avanzados-mejores-practicas-docker-swarm)
  - [Alta Disponibilidad de Managers](#alta-disponibilidad-managers)
    - [Quorum y Tolerancia a Fallos](#quorum-tolerancia-fallos)
    - [Recuperación de Managers Caídos](#recuperacion-managers-caidos)
  - [Actualizaciones de Docker Engine en el Swarm](#actualizaciones-docker-engine)
  - [Troubleshooting Común](#troubleshooting-comun-docker-swarm)
    - [Problemas de Red](#problemas-red-docker-swarm)
    - [Problemas de Servicio](#problemas-servicio-docker-swarm)
    - [Problemas de Nodos](#problemas-nodos-docker-swarm)
  - [Integración con CI/CD](#integracion-ci-cd-docker-swarm)
    - [Pipelines de Despliegue](#pipelines-despliegue-docker-swarm)


<a id="introduccion-orquestacion-docker-swarm"></a>
## Introducción a la Orquestación de Contenedores y Docker Swarm

<a id="que-es-orquestacion-contenedores"></a>
### ¿Qué es la Orquestación de Contenedores?

<a id="por-que-docker-swarm"></a>
### ¿Por qué Docker Swarm?

<a id="docker-swarm-vs-docker-compose"></a>
### Docker Swarm vs. Docker Compose

<a id="docker-swarm-vs-kubernetes"></a>
### Docker Swarm vs. Kubernetes (Visión General)

<a id="conceptos-centrales-docker-swarm"></a>
## Conceptos Centrales de Docker Swarm

<a id="nodos-manager-worker"></a>
### Nodos (Nodes): Manager y Worker

<a id="servicios-docker-swarm"></a>
### Servicios (Services)

<a id="tareas-docker-swarm"></a>
### Tareas (Tasks)

<a id="stacks-docker-swarm"></a>
### Stacks

<a id="redes-overlay-docker-swarm"></a>
### Redes Overlay

<a id="balanceo-carga-docker-swarm"></a>
### Balanceo de Carga

<a id="modo-swarm"></a>
### Modo Swarm

<a id="configuracion-cluster-docker-swarm"></a>
## Configuración de un Cluster Docker Swarm

<a id="requisitos-previos-docker-swarm"></a>
### Requisitos Previos

<a id="inicializar-swarm"></a>
### Inicializar el Swarm (`docker swarm init`)

<a id="unir-nodos-worker"></a>
### Unir Nodos Worker (`docker swarm join`)

<a id="unir-nodos-manager-adicionales"></a>
### Unir Nodos Manager Adicionales

<a id="listar-gestionar-nodos"></a>
### Listar y Gestionar Nodos (`docker node ls`)

<a id="drenar-eliminar-nodos"></a>
### Drenar y Eliminar Nodos

<a id="despliegue-gestion-servicios"></a>
## Despliegue y Gestión de Servicios

<a id="crear-servicio"></a>
### Crear un Servicio (`docker service create`)

<a id="definicion-imagenes-servicio"></a>
#### Definición de Imágenes

<a id="publicacion-puertos-servicio"></a>
#### Publicación de Puertos

<a id="variables-entorno-servicio"></a>
#### Variables de Entorno

<a id="volumenes-servicio"></a>
#### Volúmenes

<a id="escalar-servicios"></a>
### Escalar Servicios (`docker service scale`)

<a id="actualizar-servicios"></a>
### Actualizar Servicios (`docker service update`)

<a id="estrategias-despliegue"></a>
#### Estrategias de Despliegue (Rolling Update)

<a id="rollbacks-servicio"></a>
#### Rollbacks

<a id="eliminar-servicios"></a>
### Eliminar Servicios (`docker service rm`)

<a id="inspeccionar-servicios"></a>
### Inspeccionar Servicios (`docker service inspect`)

<a id="logs-servicios"></a>
### Logs de Servicios (`docker service logs`)

<a id="despliegue-gestion-stacks"></a>
## Despliegue y Gestión de Stacks (Docker Compose en Swarm)

<a id="que-es-stack"></a>
### ¿Qué es un Stack?

<a id="docker-compose-yml-swarm"></a>
### Archivo `docker-compose.yml` para Swarm

<a id="version-3-compose"></a>
#### Versión 3 del Formato

<a id="sintaxis-especifica-swarm"></a>
#### Sintaxis Específica de Swarm

<a id="desplegar-stack"></a>
### Desplegar un Stack (`docker stack deploy`)

<a id="listar-stacks-servicios"></a>
### Listar Stacks y Servicios de un Stack

<a id="actualizar-stacks"></a>
### Actualizar Stacks

<a id="eliminar-stacks"></a>
### Eliminar Stacks (`docker stack rm`)

<a id="redes-docker-swarm"></a>
## Redes en Docker Swarm

<a id="conceptos-redes-contenedores"></a>
### Conceptos de Redes de Contenedores

<a id="redes-overlay"></a>
### Redes Overlay (`overlay`)

<a id="creacion-redes-overlay"></a>
#### Creación de Redes Overlay

<a id="conexion-servicios-overlay"></a>
#### Conexión de Servicios a Redes Overlay

<a id="red-ingress"></a>
### Red de Ingress (`ingress network`)

<a id="publicacion-puertos-externos"></a>
#### Publicación de Puertos Externos

<a id="modo-publicacion-ingress-host"></a>
#### Modo de Publicación (Ingress vs Host)

<a id="balanceo-carga-interno-externo"></a>
### Balanceo de Carga Interno y Externo

<a id="dns-round-robin"></a>
#### DNS Round Robin

<a id="routing-mesh"></a>
#### Routing Mesh

<a id="almacenamiento-docker-swarm"></a>
## Almacenamiento en Docker Swarm

<a id="volumenes-docker-swarm"></a>
### Volúmenes (`volumes`)

<a id="volumenes-nombrados"></a>
#### Volúmenes Nombrados

<a id="volumenes-anonimos"></a>
#### Volúmenes Anónimos

<a id="bind-mounts-docker-swarm"></a>
### Bind Mounts

<a id="plugins-volumenes"></a>
### Plugins de Volúmenes (Volume Plugins)

<a id="almacenamiento-compartido"></a>
#### Almacenamiento Compartido (NFS, GlusterFS)

<a id="consideraciones-persistencia-cluster"></a>
### Consideraciones de Persistencia en un Cluster

<a id="seguridad-docker-swarm"></a>
## Seguridad en Docker Swarm

<a id="comunicacion-tls-nodos"></a>
### Comunicación TLS entre Nodos

<a id="gestion-secretos"></a>
### Gestión de Secretos (`docker secret`)

<a id="crear-usar-secretos"></a>
#### Crear y Usar Secretos

<a id="actualizar-eliminar-secretos"></a>
#### Actualizar y Eliminar Secretos

<a id="configuraciones-docker-swarm"></a>
### Configuraciones (`docker config`)

<a id="crear-usar-configuraciones"></a>
#### Crear y Usar Configuraciones

<a id="seguridad-imagenes"></a>
### Seguridad de Imágenes (Image Signing)

<a id="rbac-docker-swarm"></a>
### Control de Acceso Basado en Roles (RBAC - Integración)

<a id="monitorizacion-logging-docker-swarm"></a>
## Monitorización y Logging

<a id="monitorizacion-basica-docker-swarm"></a>
### Monitorización Básica de Docker Swarm

<a id="docker-stats"></a>
#### `docker stats`

<a id="docker-node-ps"></a>
#### `docker node ps`

<a id="herramientas-monitorizacion-externas"></a>
### Herramientas de Monitorización Externas

<a id="prometheus-grafana-docker-swarm"></a>
#### Prometheus y Grafana

<a id="elk-stack-docker-swarm"></a>
#### ELK Stack (Elasticsearch, Logstash, Kibana)

<a id="estrategias-logging-docker-swarm"></a>
### Estrategias de Logging

<a id="drivers-logging-docker-swarm"></a>
#### Drivers de Logging

<a id="centralizacion-logs-docker-swarm"></a>
#### Centralización de Logs

<a id="temas-avanzados-mejores-practicas-docker-swarm"></a>
## Temas Avanzados y Mejores Prácticas

<a id="alta-disponibilidad-managers"></a>
### Alta Disponibilidad de Managers

<a id="quorum-tolerancia-fallos"></a>
#### Quorum y Tolerancia a Fallos

<a id="recuperacion-managers-caidos"></a>
#### Recuperación de Managers Caídos

<a id="actualizaciones-docker-engine"></a>
### Actualizaciones de Docker Engine en el Swarm

<a id="troubleshooting-comun-docker-swarm"></a>
### Troubleshooting Común

<a id="problemas-red-docker-swarm"></a>
#### Problemas de Red

<a id="problemas-servicio-docker-swarm"></a>
#### Problemas de Servicio

<a id="problemas-nodos-docker-swarm"></a>
#### Problemas de Nodos

<a id="integracion-ci-cd-docker-swarm"></a>
### Integración con CI/CD

<a id="pipelines-despliegue-docker-swarm"></a>
#### Pipelines de Despliegue


