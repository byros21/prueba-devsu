# 🏦 Banca por Internet BP - Propuesta Arquitectónica

**Arquitecto de Soluciones:** [Tu Nombre]  
**Fecha:** Marzo 2026  
**Versión:** 2.0 (Fase 1 + Fase 2 + Migración)

---

## 📋 Resumen del Ejercicio

Diseñar un sistema de banca por internet para la entidad BP que permita:

- ✅ Consultar histórico de movimientos  
- ✅ Realizar transferencias entre cuentas propias e interbancarias  
- ✅ Notificaciones mínimas en 2 canales (SMS/Push + Email)  
- ✅ Onboarding con reconocimiento facial  
- ✅ Autenticación OAuth2.0  
- ✅ Auditoría inmutable de todas las acciones  

**Entregables:** Documento PDF + Diagramas C4 + Repositorio público.

---

## 🔍 Enfoque de Análisis

### 1. Descomposición de Requerimientos

*(Detalle de análisis funcional y no funcional aquí)*

---

### 2. Evaluación de Alternativas

| Decisión            | Opción A        | Opción B                  | Selección  | Justificación |
|--------------------|----------------|--------------------------|------------|--------------|
| Frontend Web       | Angular        | React                    | ✅ React   | Virtual DOM + ecosistema maduro = mejor performance para dashboards |
| Móvil              | React Native   | Flutter                  | ✅ Flutter | Compilación nativa esencial para reconocimiento facial sin lag |
| Auth Flow          | Implicit       | Auth Code + PKCE         | ✅ PKCE    | Estándar seguro para clientes públicos (móvil/SPA) |
| Patrón Auditoría   | Síncrono       | Observer + Kafka         | ✅ Observer| Desacoplamiento + consistencia eventual sin bloquear transacciones |
| Cache              | Write-Through  | Cache-Aside              | ✅ Cache-Aside | Menor latencia para lecturas frecuentes + protección de DB |

---

## 🎯 ¿Por Qué Dos Fases?

### Fase 1: MVP Hardened (Monolito Modular en PaaS)

**Justificación teórica:**

- **Principio YAGNI (You Aren't Gonna Need It):** No sobre-ingenierizar para volumen futuro incierto.  
- Un monolito modular bien estructurado escala hasta ~100K usuarios sin problemas.  
- **Costo de Oportunidad:** Permite validar hipótesis de negocio rápidamente.

---

### Fase 2: Enterprise (Microservicios + Kubernetes)

**Justificación teórica:**

- **Ley de Conway + Escalabilidad de Equipos:** Microservicios permiten autonomía cuando hay múltiples equipos.  
- **Economías de Escala:**  
  - +67% costo total  
  - −83% costo unitario  
  - ROI positivo en 12–18 meses  

---

## 🔄 Plan de Migración: Fase 1 → Fase 2

**Patrón:** Strangler Fig (Higuera Estranguladora)

---

### 🗺️ Roadmap de 6 Meses

| Mes | Servicio a Migrar        | Actividad Clave                    | Criterio de Éxito |
|-----|--------------------------|-----------------------------------|------------------|
| 1   | Foundation               | Setup EKS, Istio, GitOps          | Cluster operativo + pipelines |
| 2   | Auth Service             | Extraer + OAuth2 + PKCE           | 100% auth en microservicio |
| 3   | Customer + Account       | Database per Service + Saga       | Transacciones ACID distribuidas |
| 4   | Transfer Service         | Kafka + Event-Driven              | Latencia < 500ms p95 |
| 5   | Movement + Notification  | CQRS + Multi-channel              | Queries < 200ms + notifs < 5s |
| 6   | Audit + Onboarding + DR  | WORM + Facial + Chaos Testing     | SLA 99.95% + RTO < 1h |

---

## ⚙️ Reglas de Migración Segura

- API Gateway como router (feature flags)
- Contratos estables (OpenAPI / Swagger)
- Doble escritura temporal
- Rollback automático (>10% degradación)
- Monitoreo paralelo en tiempo real

---

