# 🛰️ ResQ-Net — Sistema de Gestión de Recursos Humanitarios

> Caso 10 · Ayuda Humanitaria | Introducción a la Ingeniería 2026-I  
> Departamento de Ingeniería de Sistemas

---

## ¿Qué es ResQ-Net?

ResQ-Net es un sistema de coordinación de respuesta humanitaria diseñado para operar **sin conexión a internet central**. Después de un desastre —terremoto, inundación, deslizamiento— la infraestructura de telecomunicaciones colapsa justo cuando más se necesita. ResQ-Net usa redes Mesh y computación en el borde (fog computing) para que brigadas de rescate compartan información y coordinen recursos en tiempo real, incluso sin red.

El MVP es una aplicación web interactiva que simula el sistema completo corriendo en el navegador, sin ningún servidor externo.

---

## 🚀 Cómo correr el MVP

No requiere instalación.

```bash
# Opción 1: abrir directamente el archivo
open index.html   # macOS
start index.html  # Windows

# Opción 2: servidor local
python -m http.server 8080
# Luego abrir http://localhost:8080
```

El MVP funciona completamente offline (excepto los tiles del mapa, que requieren internet para cargarse la primera vez).

---

## 🗺️ Funcionalidades del MVP

| Función | Descripción |
|---------|-------------|
| Mapa interactivo | Leaflet.js con OpenStreetMap. Muestra incidentes y brigadas en tiempo real. |
| Simulación automática | Genera incidentes aleatorios cada 4 segundos para demostrar el sistema bajo carga. |
| Motor de triaje | Algoritmo de priorización con 4 criterios: severidad, afectados, antigüedad, estado. |
| Asignación manual | Asigna la brigada más cercana disponible con un clic. |
| Panel de recursos | Inventario de suministros médicos, transporte y equipamiento. |
| Bitácora auditada | Registro completo de todas las decisiones con timestamp. |
| Modo offline | Sin dependencias de servidor; corre en Raspberry Pi con Chromium. |

---

## 🏗️ Arquitectura

```
┌─────────────────────────────────────────────────────┐
│                  CAPA NUBE (opcional)                │
│           Sincronización diferida · HTTPS            │
└─────────────────────┬───────────────────────────────┘
                      │ Internet cuando está disponible
┌─────────────────────┼───────────────────────────────┐
│              CAPA FOG / EDGE                         │
│   Nodo A ←──Mesh──→ Nodo B ←──Mesh──→ Nodo C        │
│   (Raspberry Pi / laptop de campo)                   │
└─────────────────────┬───────────────────────────────┘
                      │ Wi-Fi Direct / BT LE
┌─────────────────────┼───────────────────────────────┐
│              CAPA DE CAMPO                           │
│   📱 Brigadas   🚁 Drones   📦 Centros de suministro  │
└─────────────────────────────────────────────────────┘
```

---

## ⚖️ Algoritmo de Triaje

La puntuación de cada incidente se calcula así:

```
score = (severidad × 40) + (personas × 0.4) + (antigüedad × 0.5) + (bono_no_asignado × 5)
```

| Criterio | Peso | Justificación |
|----------|------|---------------|
| Severidad médica | 40 pts | Criterio clínico principal |
| Personas afectadas | hasta 40 pts | Maximiza vidas |
| Tiempo sin atención | hasta 15 pts | Penaliza retrasos |
| Bono sin asignar | 5 pts | Evita inanición de incidentes |

**Importante:** el algoritmo produce recomendaciones, no órdenes. Todo coordinador humano puede sobreescribir cualquier decisión. La transparencia y la auditoría son requisitos de diseño, no características opcionales.

---

## 🧰 Stack tecnológico

- **HTML5 / CSS3 / JavaScript ES2022** — sin frameworks, corre en cualquier navegador
- **Leaflet.js 1.9** — mapas interactivos open-source
- **OpenStreetMap** — cartografía libre sin costo de API
- **LaTeX / Overleaf** — documentación técnica
- **Git / GitHub** — control de versiones

---

## 📁 Estructura del repositorio

```
resq-net/
├── index.html          ← MVP completo (una sola página)
├── main.tex            ← Documento técnico en LaTeX
├── main.pdf            ← PDF compilado del documento
└── README.md           ← Este archivo
```

---

## 👥 Equipo

| Integrante | Rol |
|-----------|-----|
| Jose Daniel castro Villadiego | Arquitectura y backend |
| Santiago Andres García Acevedo | Frontend / MVP |
| Santiago de Jesús Trejos Zuluaga | Algoritmos de triaje |
| Yeiner Jesús Hernández bustos | Documentación y ética |
| Yordi Romario Jiménez López | Integración y pruebas |

**Docente:** Miguel Alberto Caro Álvarez  
**Asignatura:** Introducción a la Ingeniería · 2026-I  
**Caso:** 10 — Ayuda Humanitaria

---

## 📜 Nota ética

ResQ-Net fue diseñado siguiendo el Principio 1 del Código de Ética ACM/IEEE: *actuar consistentemente con el interés público*. El sistema nunca toma decisiones autónomas finales sobre quién recibe atención. Esas decisiones las toma siempre un ser humano.
