# Glosario técnico: EDA, DDD, BFF, IaC, TDD, BDD, ATDD, SLA, MLOps, CI/CD y CORS

---

## 3. 

### EDA: Event-Driven Architecture (Arquitectura orientada a eventos)

**Qué es:** un estilo de arquitectura en el que los componentes se comunican produciendo y consumiendo **eventos**, es decir, registros de algo que ya ocurrió (por ejemplo, "pedido creado" o "pago aprobado").

**Idea central:** el productor no sabe quién reaccionará al evento. Esto desacopla los servicios: se pueden agregar nuevos consumidores sin tocar al productor.

**Ejemplo:** en una tienda en línea, al crearse un pedido se publica el evento `PedidoCreado`. El servicio de facturación, el de inventario y el de envíos reaccionan cada uno por su lado.

**Cuándo aparece:** sistemas distribuidos, microservicios, procesamiento en tiempo real, colas de mensajes (Kafka, RabbitMQ, Amazon SNS/SQS).

### DDD: Domain-Driven Design (Diseño guiado por el dominio)

**Qué es:** un enfoque de diseño, propuesto por Eric Evans, que organiza el software alrededor del **dominio del negocio** y del lenguaje que usan los expertos de ese negocio.

**Conceptos clave:**

- **Lenguaje ubicuo:** el código usa los mismos términos que el equipo de negocio. Si ellos dicen "reserva", el código tiene una clase `Reserva`, no `Registro2`.
- **Contexto acotado (*bounded context*):** límite explícito dentro del cual un modelo tiene un significado único. "Cliente" en ventas puede significar algo distinto a "cliente" en soporte.
- **Entidades, objetos de valor y agregados:** un agregado es un grupo de objetos que se tratan como una unidad para mantener reglas de consistencia.

**Ejemplo:** en un sistema de salud, "Paciente" en el contexto de *agendamiento* tiene datos de contacto, mientras que en el contexto de *historia clínica* tiene diagnósticos. Son dos contextos acotados con modelos separados.

**Cuándo aparece:** dominios de negocio complejos donde las reglas importan más que la técnica.

### BFF: Backend For Frontend (Backend para el frontend)

**Qué es:** un patrón en el que se crea **una capa de backend por cada tipo de cliente** (web, app móvil, panel de administración), en lugar de un único API genérico para todos.

**Idea central:** cada BFF expone exactamente los datos que su cliente necesita, en el formato y la cantidad adecuados. Suele agregar varias llamadas a servicios internos en una sola respuesta.

**Ejemplo:** la app móvil recibe un BFF que devuelve un resumen compacto de la pantalla de inicio en una sola llamada. La versión web, con más espacio, recibe un BFF distinto con más detalle.

**Cuándo aparece:** microservicios con varios clientes que tienen necesidades diferentes de datos.

---

## 4.
### IaC: Infrastructure as Code (Infraestructura como código)

**Qué es:** definir servidores, redes, bases de datos y demás recursos de nube mediante **archivos de configuración versionados**, en lugar de configurarlos a mano en una consola.

**Idea central:** la infraestructura se trata como cualquier otro código: se revisa, se versiona, se prueba y se puede reproducir exactamente.

**Ejemplo:** un archivo de Terraform que declara una base de datos PostgreSQL, dos servidores y un balanceador. Ejecutarlo en otra cuenta crea el mismo entorno.

**Herramientas comunes:** Terraform, AWS CloudFormation, Pulumi, Ansible.

**Beneficio:** evita la "configuración manual" que nadie recuerda, y permite recrear entornos de desarrollo, pruebas y producción de forma consistente.

### TDD: Test-Driven Development (Desarrollo guiado por pruebas)

**Qué es:** una práctica en la que se escribe **primero la prueba** y después el código que la hace pasar. El ciclo se conoce como **rojo-verde-refactor**:

1. **Rojo:** escribir una prueba que falla.
2. **Verde:** escribir el mínimo código para que pase.
3. **Refactor:** mejorar el código manteniendo las pruebas en verde.

**Ejemplo:** antes de implementar una función `calcularDescuento`, se escribe una prueba que verifica que un cliente con más de 1000 puntos obtiene 10 %. La prueba falla hasta que la función se implementa.

**Enfoque:** es técnico y se centra en las unidades de código. Lo escriben principalmente los desarrolladores.

### BDD: Behavior-Driven Development (Desarrollo guiado por comportamiento)

**Qué es:** una evolución de TDD que describe el software en términos de **comportamiento observable**, usando un lenguaje cercano al natural. Un formato común es **Gherkin**, con la estructura *Dado / Cuando / Entonces* (*Given / When / Then*).

**Ejemplo:**

```gherkin
Escenario: Aplicar cupón válido
  Dado que el carrito tiene un total de 100 dólares
  Cuando el cliente aplica el cupón "VERANO10"
  Entonces el total a pagar es 90 dólares
```

**Enfoque:** su objetivo es que desarrolladores, testers y personas de negocio compartan el mismo entendimiento del comportamiento esperado. Las especificaciones en Gherkin pueden ejecutarse como pruebas automatizadas 

### ATDD: Acceptance Test-Driven Development (Desarrollo guiado por pruebas de aceptación)

**Qué es:** una práctica en la que se definen **pruebas de aceptación** antes de desarrollar, de forma colaborativa entre negocio, QA y desarrollo. Esas pruebas describen cuándo una historia de usuario se considera terminada.

**Diferencia con BDD:** ATDD se enfoca en validar que el sistema cumple lo acordado con el cliente o el negocio. BDD se enfoca en describir el comportamiento de forma que sea entendible y ejecutable. En la práctica se solapan mucho, y muchos equipos usan los términos casi como sinónimos.

**Ejemplo:** antes de codificar la funcionalidad "recuperar contraseña", el equipo acuerda con el negocio los criterios de aceptación: el enlace expira en 30 minutos, solo se puede usar una vez y se envía a un correo verificado.

### SLA: Service Level Agreement (Acuerdo de nivel de servicio)

**Qué es:** un **contrato** entre proveedor y cliente que define el nivel de servicio comprometido, normalmente con métricas medibles y, a veces, con penalizaciones si no se cumple.

**Métricas típicas:**

- **Disponibilidad:** por ejemplo, 99.9 % de tiempo activo al mes (unas 43 minutos de caída al mes).
- **Tiempo de respuesta:** por ejemplo, el 95 % de las peticiones responde en menos de 300 ms.
- **Tiempo de resolución de incidentes.**

**Términos relacionados:** SLO (*Service Level Objective*), el objetivo interno, y SLI (*Service Level Indicator*), la medición real. El SLA es el compromiso externo, y suele ser menos exigente que el SLO interno.

---

## 5.

### MLOps: Machine Learning Operations

**Qué es:** el conjunto de prácticas que aplica los principios de DevOps al ciclo de vida de modelos de machine learning: preparación de datos, entrenamiento, evaluación, despliegue, monitoreo y reentrenamiento.

**Problemas que resuelve:** un modelo que funciona en un notebook suele fallar en producción por datos cambiantes, versiones inconsistentes o falta de monitoreo.

**Elementos típicos:**

- Versionado de datos y de modelos.
- Pipelines de entrenamiento reproducibles.
- Registro de modelos y experimentos (MLflow, por ejemplo).
- Monitoreo de *drift*, es decir, cuando los datos reales se alejan de los datos de entrenamiento.

### CI/CD: Continuous Integration / Continuous Delivery (o Deployment)

**Qué es:** dos prácticas que automatizan el camino del código desde el repositorio hasta producción.

- **CI (Integración continua):** cada cambio se integra en la rama principal y se compila y prueba automáticamente, varias veces al día.
- **CD (Entrega o despliegue continuo):** tras pasar las pruebas, el código queda listo para desplegarse. En *Continuous Delivery* un humano aprueba el paso a producción; en *Continuous Deployment* el despliegue es automático.

**Ejemplo de flujo:** un `git push` dispara pruebas unitarias, análisis estático, construcción de una imagen Docker y despliegue a un entorno de pruebas. Si todo pasa, se despliega a producción.

**Herramientas comunes:** GitHub Actions, GitLab CI, Jenkins, Azure DevOps.

### CORS: Cross-Origin Resource Sharing (Intercambio de recursos de origen cruzado)

**Qué es:** un mecanismo del navegador, basado en cabeceras HTTP, que decide si una página cargada desde un **origen** (dominio, protocolo y puerto) puede hacer peticiones a un **origen distinto**.

**Por qué existe:** el navegador aplica la política de *mismo origen* (*Same-Origin Policy*) para proteger al usuario. CORS es la forma controlada en que un servidor dice "confío en este otro origen".

**Ejemplo:** la app en `https://miapp.com` llama a la API en `https://api.miservicio.com`. La API debe responder con una cabecera como `Access-Control-Allow-Origin: https://miapp.com`. Si no la envía, el navegador bloquea la respuesta, aunque el servidor la haya procesado.


