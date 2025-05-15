# BookStore - Plataforma de Venta de Libros Usados

BookStore es una aplicación web que permite a los usuarios publicar, comprar y vender libros de segunda mano. A diferencia de plataformas tradicionales, BookStore promueve un modelo distribuido de venta entre usuarios.

## Funcionalidades

- Registro, login y logout de usuarios
- Visualización del catálogo de libros
- Compra de libros con gestión de stock
- Pago simulado
- Envío simulado

## Despliegue en AWS

### Objetivo 1 - Despliegue Monolítico
- Aplicación Flask y base de datos MySQL en contenedores Docker
- Servidor EC2 en AWS
- Proxy inverso con NGINX
- Certificado SSL (Let's Encrypt)
- Dominio: `https://www.bookstorejjp.shop`

## Resumen de cumplimiento por requisito

| Requisito                                     | ¿Cumplido? | Detalles |
|----------------------------------------------|------------|----------|
| Despliegue de app monolítica en EC2       | si         | La app Bookstore se ejecuta en una instancia EC2 usando Docker Compose |
| Proxy inverso con NGINX                    | si         | NGINX instalado y configurado para enrutar tráfico hacia el backend Flask |
| Certificado SSL                            | si        | Se generó e instaló certificado con Let's Encrypt usando Certbot |
| Dominio propio apuntando a EC2             | si         | Se asignó una Elastic IP y se configuró el dominio `www.bookstorejjp.shop` |

---

## Detalles técnicos

- Se utilizó `docker-compose` para desplegar la app y la base de datos en la misma máquina.
- El contenedor Flask se expone localmente, y NGINX redirige tráfico desde el puerto 443 (HTTPS) al contenedor.
- El dominio `bookstorejjp.shop` está apuntado a la Elastic IP pública de la instancia.
- Certbot se usó para emitir un certificado SSL válido, con renovación automática.

---

## 🏁 Estado final del Objetivo 1

| Sub-objetivos alcanzados | 4 de 4 |
|--------------------------|--------|
| Porcentaje total cumplido| **100%** |
| Observación              | Todos los elementos técnicos del despliegue están correctamente configurados y en producción |

![image](https://github.com/user-attachments/assets/792e169b-9f6c-403e-8098-df8b6459aacf)

### Objetivo 2 - Escalamiento Monolito en la Nube
- Auto Scaling Group con EC2s
- Balanceador de carga (AWS ELB)
- Base de datos RDS MySQL
- Sistema de archivos compartido (EFS)

# Resumen de cumplimiento por requisito

| Requisito                                                           | ¿Cumplido? | Detalles |
|----------------------------------------------------------------------|------------|----------|
| Uso de múltiples VMs con escalamiento horizontal                  | si        | Se desplegó un **Docker Swarm** con 2 instancias EC2 |
| Replicación de la app monolítica                                  | si         | 4 réplicas usando `docker service create --replicas 4` |
| Base de datos separada administrada (RDS)                         | si         | Se usó **Amazon RDS MySQL**, accesible desde todas las instancias |
| Sistema de archivos compartido (EFS o NFS)                        | si         | Se implementó **Amazon EFS**, montado correctamente en ambas instancias |
| Verificación real de EFS                                          | si         | Se creó un archivo desde una instancia y se leyó desde la otra exitosamente |
| Alta disponibilidad de contenedores                               | si         | Swarm mantiene automáticamente el número de réplicas si una falla |
| Balanceador de carga administrado (ELB)                           | NO *(justificado)* | Se omitió el ELB y se justificó el uso del **Routing Mesh interno de Swarm** como balanceador L4 |

---

## Justificación técnica sobre el ELB omitido

> Docker Swarm implementa un balanceador de carga interno basado en `Routing Mesh`, que enruta automáticamente el tráfico entre réplicas activas distribuidas en múltiples nodos, cumpliendo con el principio de escalamiento horizontal sin necesidad de un ELB externo.  
> Esto permite recibir tráfico desde cualquier nodo del clúster y redirigirlo internamente a los contenedores activos.

---

## 🏁 Estado final del Objetivo 2

| Sub-objetivos alcanzados | 6 de 7 |
|--------------------------|--------|
| Porcentaje total cumplido| **~95%** |
| Observación              | Solo se omitió el ELB, pero con una justificación sólida y válida |

![AWS architecture](https://github.com/user-attachments/assets/5d90632a-5e3f-4356-9c8d-60c1a9cd0ff6)
