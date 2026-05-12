# SmartSnack Office - Sistema de Suministro Automatizado

**SmartSnack Office** es una plataforma integral diseñada para transformar la gestión de beneficios corporativos. El sistema automatiza la cadena de suministro de snacks saludables, conectando la demanda real de los empleados con la logística de inventario mediante una experiencia premium impulsada por Inteligencia Artificial.

Documentos (disponibles a cambios):

Documento Propuesta del Proyecto: https://docs.google.com/document/d/1XKS81kv_aLzMQ9cQPRSm9Q3XFeSGn83BYzZTp2ekLuE/edit?usp=sharing

Documento levantamiento de requerimientos: https://docs.google.com/document/d/17w5CEMyj6_y3XSgpkHI1FiooFWE0UqpjaNi1agNqeOQ/edit?usp=sharing

---

## 1. Listado de Funcionalidades Principales

El sistema está segmentado en cuatro roles estratégicos y un motor de inteligencia autónomo:

### A. Actor: Comprador (Usuario Final)
*   **Búsqueda Inteligente (TF-IDF):** Localización de snacks mediante términos descriptivos (ej. "energía para la tarde" o "bajo en sodio") en lugar de simples nombres.
*   **Personalización y Compra:** Selección de productos, adquisición de cajas personalizadas y recepción de recomendaciones basadas en IA.
*   **Perfil de Salud:** Configuración de restricciones alimenticias (alergias, dietas veganas) y seguimiento de metas nutricionales.
*   **Feedback de Calidad:** Registro de valoraciones y reseñas que retroalimentan el algoritmo de recomendación.
*   **Lista de Deseos:** Guardado de productos agotados con notificaciones automáticas de reposición de stock.

### B. Actor: Vendedor
*   **Gestión de Productos:** Publicación de catálogo con descripciones técnicas, nutricionales y carga de imágenes.
*   **Inventario en Tiempo Real:** Control dinámico de existencias para evitar discrepancias en las cajas personalizadas.
*   **Operaciones Comerciales:** Gestión de descuentos, promociones y módulo de atención al cliente para resolver dudas técnicas.
*   **Logística Inteligente:** Generación de etiquetas de envío y acceso a pronósticos de demanda basados en IA.

### C. Actor: Administrador (Gobernanza)
*   **Gestión Global:** Creación y moderación de categorías, etiquetas y usuarios mediante el ORM de Django.
*   **Auditoría y Seguridad:** Visualización de flujos financieros, gestión de permisos (RBAC) y revisión de logs de seguridad.
*   **Mantenimiento Operativo:** Monitoreo de salud del sistema, configuración de parámetros de IA y gestión de respaldos en PostgreSQL.

### D. Agente IA (Sistema Inteligente)
*   **Perfilamiento y Conversión:** Análisis de comportamiento para maximizar el Click-Through Rate (CTR).
*   **Motor de Recomendación:** Ejecución de modelos *Item-based Collaborative Filtering* para el descubrimiento de productos.
*   **Detección de Anomalías:** Identificación proactiva de errores en precios o patrones sospechosos de fraude.
*   **Optimización Semántica:** Reordenamiento de resultados de búsqueda por relevancia real para el usuario.

### E. Integraciones Externas (Pasarela de Pago)
*   **Procesamiento Seguro:** Validación y ejecución de transacciones monetarias.
*   **Automatización Logística:** Envío de callbacks para actualizar estados de pedido y disparar la cadena de suministro.

---

## 2. Arquitectura y Stack Tecnológico

| Capa | Tecnología | Propósito |
| :--- | :--- | :--- |
| **Backend** | `Django + DRF` | API REST robusta y gestión de lógica de negocio. |
| **Frontend** | `Next.js (React)` | Interfaz de alto impacto visual y experiencia de usuario premium. |
| **Base de Datos** | `PostgreSQL` | Almacenamiento relacional con control de versiones en `Alembic`. |
| **IA Engine** | `Scikit-learn` | Modelos de recomendación y procesamiento de lenguaje natural. |
| **Asincronismo** | `Celery + Redis` | Procesamiento de tareas pesadas en segundo plano. |
| **Seguridad** | `SimpleJWT` | Autenticación estándar corporativa mediante tokens. |
| **Infraestructura** | `Docker` | Contenedorización para portabilidad total. |
| **CI/CD** | `GitHub Actions` | Automatización de pruebas y despliegue continuo. |

---

## 3. Resumen de Requerimientos No Funcionales (RNF)

*   **Rendimiento:** Búsquedas TF-IDF en < 2s. Recomendaciones en < 1.5s.
*   **Seguridad:** Protección nativa contra SQLi, XSS y CSRF. Aislamiento de pasarela de pagos.
*   **Disponibilidad:** Mínimo 99% en horario corporativo (6 AM – 10 PM).
*   **Escalabilidad:** Arquitectura Docker Compose preparada para escalamiento horizontal.
*   **Mantenibilidad:** Cobertura de pruebas mínima del 70% en módulos críticos.

---

## 4. Equipo y Roles de Ingeniería

*   **Backend:** Juan Sebastián Leguizamón & Juan Roldán.
*   **Frontend:** Nelson Pineda.
*   **Testing / QA:** Pablo Olaya.
*   **DevOps / Integración IA:** Daniel Duitama.
*   **Project Manager & Documentación:** Samuel Castañeda.

---
