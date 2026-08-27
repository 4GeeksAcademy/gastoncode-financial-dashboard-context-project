# 📊 Project Overview — Financial Metrics Dashboard

> **Última actualización:** 2026-08-26
> **Repositorio:** `4GeeksAcademy/ai-eng-financial-dashboard-context-project`

---

## 🧭 ¿Qué es?

**Financial Metrics Dashboard** es una aplicación web full-stack que visualiza métricas financieras simuladas. Proporciona un panel ejecutivo interactivo con indicadores clave de rendimiento (KPIs), gráficos de ingresos vs gastos, y análisis de márgenes de ganancia.

El proyecto fue desarrollado por estudiantes de **4Geeks Academy** como parte del programa de **AI Engineering**, bajo la autoría de [@marcogonzalo](https://github.com/marcogonzalo) y otros contribuidores.

---

## 🏗️ Arquitectura General

| Componente | Tecnología | Puerto |
|---|---|---|
| **Frontend** | React 19 + TypeScript + Vite + Tailwind CSS 4 | `5173` |
| **Backend** | Python 3.13 + FastAPI + Uvicorn | `8000` |
| **Contenedores** | Docker Compose (2 servicios) | — |

Se ejecuta completamente con `docker compose up --build`.

---

## 🔙 Backend (FastAPI)

**Dependencias principales:** `fastapi`, `uvicorn`, `debugpy`, `pytest`, `httpx`

### Endpoints de la API

| Endpoint | Descripción |
|---|---|
| `GET /health` | Health check |
| `GET /api/metrics` | Lista movimientos financieros con filtros opcionales |
| `GET /api/metrics/facets` | Facetas disponibles (tipos, categorías, rango de fechas) |
| `GET /api/metrics/summary` | Resumen agrupado por day/week/month |
| `GET /api/metrics/categories/top` | Top N categorías por operation_type |
| `GET /api/metrics/comparison` | Comparación entre dos períodos |
| `GET /api/metrics/alerts` | Detección de alertas/anomalías |
| `GET /api/metrics/b2b` | Solo movimientos B2B |
| `GET /api/metrics/b2c` | Solo movimientos B2C |

### Características

- Genera **360 movimientos mock aleatorios** (30 por mes × 12 meses) con semilla fija `seed=42` para resultados deterministas
- Cada movimiento tiene: `create_date`, `amount`, `operation_type`, `category`, `business_type`
- Funciones puras para filtrado, resumen, detección de alertas y comparación

---

## 🖥️ Frontend (React + TypeScript)

**Dependencias principales:** `react`, `recharts`, `lucide-react`, `tailwindcss`, `clsx`, `class-variance-authority`

### Componentes del Dashboard

| Componente | Función |
|---|---|
| `DashboardHeader` | Encabezado con título y badge del período |
| `KPIRow` | Grid de 4 tarjetas KPI |
| `KPICard` | Tarjeta individual con label, valor, descripción e ícono |
| `IncomeOutcomeChart` | Gráfico de líneas (Recharts) ingresos vs gastos mensuales |
| `ProfitPercentChart` | Gráfico de líneas del margen de ganancia porcentual mensual |

### Lógica de negocio (en `lib/`)
- `financial-types.ts` — Interfaces TypeScript
- `financial-utils.ts` — Funciones: `computeKPIs`, `computeMonthlyData`, `formatCurrency`, `formatPercent`
- `mock-data.ts` — Dataset mock estático de 57 movimientos del año 2024
- `utils.ts` — Función utilitaria `cn()` para merging de clases Tailwind

---

## 🐳 Infraestructura Docker

```yaml
services:
  frontend:
    build: ./frontend      # Node 24, puerto 5173
    volumes: ./frontend:/app (con hot-reload)
    depends_on: backend

  backend:
    build: ./backend       # Python 3.13-slim, puertos 8000 y 5678 (debug)
    volumes: ./backend:/app (con hot-reload)
```

---

## 🧪 Tests

| Capa | Framework | Cantidad |
|---|---|---|
| **Backend** | `pytest` + `httpx` (TestClient) | 15 tests |
| **Frontend** | `vitest` | 5 tests (3 describes) |

---

## ✅ Buenas Prácticas Identificadas

1. Separación de lógica de negocio de los endpoints HTTP (funciones puras)
2. Pruebas unitarias en frontend y backend cubriendo edge cases
3. Tipos estrictos con `Literal` que previenen errores en compilación
4. Manejo de estados visuales (loading, empty, error, data)
5. Sistema de theming completo con CSS variables y modo oscuro

## ❌ Riesgos Identificados

| Prioridad | Riesgo | Detalle |
|---|---|---|
| 🔴 Crítica | CORS abierto | `allow_origins=["*"]` en `main.py` |
| 🔴 Crítica | debugpy en producción | Puerto 5678 expuesto con debugger remoto en Dockerfile |
| 🟠 Alta | Datos mockeados sin abstracción | No hay capa repositorio/DAO |
| 🟠 Alta | Sin logging ni trazabilidad | Ninguna ruta tiene logging o manejo de errores |
| 🟡 Media | Volúmenes Docker rompen aislamiento | Código fuente montado como volumen |

---

## 🔄 Estado Actual

- **Rama activa:** `main`
- **Pull request activo:** #6 — `docs: validación de la fase 1 y comprensión del handover`
- **Propósito del PR:** Documentación de validación de Fase 1 y análisis del handover del proyecto
- **Estado:** El proyecto se encuentra en fase de revisión/documentación
