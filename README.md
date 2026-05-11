# SmartSnack Office - Sistema de Suministro Automatizado

**SmartSnack Office** es una plataforma integral diseñada para transformar la gestión de beneficios corporativos. El sistema automatiza la cadena de suministro de snacks saludables, conectando la demanda real de los empleados con la logística de inventario mediante una experiencia personalizada impulsada por Inteligencia Artificial.

---

## Equipo de Desarrollo (2026-I)

| Integrante | Rol Principal |
| :--- | :--- |
| **Juan Sebastián Leguizamón** | Backend Developer |
| **Juan Roldán** | Backend Developer |
| **Nelson Pineda** | Frontend Developer |
| **Pablo Olaya** | Testing & QA |
| **Daniel Duitama** | DevOps & Integración IA |
| **Samuel Castañeda** | Project Manager & Documentación |

---

## Tecnologías y Frameworks

| Capa | Tecnología |
| :--- | :--- |
| **Lenguaje** | Python |
| **Backend** | Django + Django REST Framework (DRF) |
| **Frontend** | Next.js (React) |
| **Base de datos** | PostgreSQL |
| **Autenticación** | SimpleJWT |
| **ML Engine** | Scikit-learn (TF-IDF & Item-based CF) |
| **Task Queue** | Celery + Redis |
| **Contenedores** | Docker & Docker Compose |
| **Gestión** | Jira & GitHub Actions (CI/CD) |

---

## Actores del Sistema

*   **Administrador:** Guardián operativo y de cumplimiento. Gestiona categorías, modera contenido, realiza auditorías transaccionales y monitorea la salud del sistema.
*   **Vendedor:** Motor de la oferta. Gestiona el catálogo de productos, controla el inventario en tiempo real y brinda atención al cliente.
*   **Comprador:** Actor principal. Disfruta de búsquedas inteligentes, adquisición de cajas personalizadas y configuración de perfiles nutricionales.
*   **Agente IA:** Sistema autónomo que optimiza la conversión mediante perfiles de usuario, recomendaciones (CTR) y detección de anomalías.
*   **Pasarela de Pago:** Sistema externo para el procesamiento financiero seguro y confirmación de pedidos vía callbacks.

---

## Componentes de Inteligencia Artificial

El sistema implementa una capa de IA con **Scikit-learn** dividida en:
1.  **Motor de Recomendación:** Filtrado colaborativo basado en ítems para sugerir productos según el historial.
2.  **Búsqueda Inteligente (TF-IDF):** Localización de snacks mediante términos descriptivos (ej: "energía para la tarde").
3.  **Detección de Anomalías:** Identificación proactiva de errores en precios o patrones sospechosos de fraude.

---

## Calidad de Ingeniería e Infraestructura

*   **Validación de Datos:** Uso de `Pydantic` para garantizar esquemas de datos rigurosos.
*   **Gestión de DB:** Control de versiones y migraciones con `Alembic`.
*   **Contenedorización:** Entorno 100% reproducible mediante `Docker`.
*   **CI/CD:** Flujo automatizado en `GitHub Actions` con ejecución de pruebas obligatorias antes de cada aprobación de despliegue.
*   **Rendimiento:** Tareas pesadas gestionadas de forma asíncrona con `Celery` para mantener una respuesta fluida en el frontend.

---

## Requerimientos No Funcionales Clave

*   **Rendimiento:** Búsquedas inteligentes en < 2s; recomendaciones en < 1.5s.
*   **Seguridad:** Control de acceso basado en roles (RBAC) y comunicación cifrada vía HTTPS/TLS.
*   **Disponibilidad:** Mínimo 99% en horario laboral corporativo.
*   **Escalabilidad:** Arquitectura preparada para escalamiento horizontal mediante contenedores.

---
