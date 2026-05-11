# Análisis Del Proyecto - Django sin cadenas 

Este documento resume el análisis de requisitos para el proyecto **Django sin cadenas**, un sistema avanzado de venta de snacks en línea con integración de Inteligencia Artificial.

---

## Equipo de Desarrollo
* **Integrantes:** * Pablo Esteban Olaya Arias
    * Juan José Roldán Garay
    * Sebastian Leguizamon Silva
    * Samuel David Castañeda Mora
    * Nelson Enrique Pineda Tellez
    * Daniel Alejandro Duitama Correa
* **Periodo:** 2026-I

---

## 1. Listado de Funcionalidades Principales

El sistema está segmentado en cuatro roles principales y un sistema externo de integración.

### A. Funcionalidades del Comprador (Usuario Final)
- **Búsqueda Inteligente (TF-IDF):** Localización de snacks mediante términos descriptivos y procesamiento de lenguaje natural.
- **Personalización y Compra:** Selección de productos, recepción de recomendaciones personalizadas y gestión de feedback.
- **Salud y Restricciones:** Configuración de perfil con alergias, dietas y seguimiento de metas nutricionales.
- **Lista de Deseos:** Guardado de productos agotados con sistema de notificación de stock.

### B. Funcionalidades del Vendedor
- **Gestión de Productos:** Publicación de snacks y control de inventario en tiempo real.
- **Operaciones Comerciales:** Gestión de descuentos/promociones y módulo de atención al cliente (dudas y reclamos).
- **Logística y Análisis:** Generación de etiquetas de envío y pronóstico de demanda basado en IA.

### C. Funcionalidades del Administrador
- **Gestión de Contenido:** Organización de categorías, etiquetas y moderación de productos/usuarios.
- **Seguridad y Auditoría:** Gestión de roles/permisos (RBAC), auditoría financiera y logs de seguridad.
- **Mantenimiento:** Monitoreo de salud del sistema, configuración de parámetros de IA y gestión de respaldos en PostgreSQL.

### D. Agente IA y Sistemas Externos
- **Agente IA:** Perfilamiento de usuario, optimización de conversión, detección de anomalías y limpieza de Tokens (JWT).
- **Pasarela de Pago:** Procesamiento de transacciones, notificación de estado y gestión de reembolsos.

---

## 2. Resumen de Requerimientos No Funcionales (RNF)

| Categoría | Descripción Clave |
| :--- | :--- |
| **Rendimiento** | Búsquedas TF-IDF < 2s. Recomendaciones < 1.5s. Tareas asíncronas con **Celery**. |
| **Seguridad** | Autenticación JWT y HTTPS/TLS. Control RBAC. Protección contra SQLi/XSS/CSRF. |
| **Disponibilidad** | Mínimo 99% (6 AM – 10 PM). Respaldos manuales y automáticos de DB. |
| **Escalabilidad** | Arquitectura **Docker / Docker Compose**. Cola de tareas con **Redis**. |
| **Mantenibilidad** | Migraciones con **Alembic**. Cobertura de pruebas (unit testing) > 70%. |
| **Compatibilidad** | API REST (DRF) bajo estándar **OpenAPI**. Integración vía callbacks. |

---

## 3. Historias de Usuario (Ejemplos)

| ID | Rol | Título | Objetivo (Para...) |
| :--- | :--- | :--- | :--- |
| **HU-001** | Comprador | Búsqueda inteligente | Encontrar rápidamente snacks de interés sin navegar extensamente. |
| **HU-002** | Comprador | Bloqueo por alergias | Que el sistema impida automáticamente comprar productos no aptos. |
| **HU-004** | Vendedor | Stock en tiempo real | Mantener el inventario actualizado y evitar discrepancias en ventas. |
| **HU-006** | Admin | Respaldo preventivo | Asegurar la integridad de la información crítica antes de mantenimientos. |

---

## Stack Tecnológico Sugerido
- **Backend:** Django / Django REST Framework (DRF)
- **Base de Datos:** PostgreSQL
- **Asincronismo:** Celery + Redis
- **DevOps:** Docker / Docker Compose
- **IA:** Scikit-learn (TF-IDF) / Agentes personalizados
