![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Power Query](https://img.shields.io/badge/Power_Query-2E86AB?style=for-the-badge&logo=microsoftexcel&logoColor=white)
![Star Schema](https://img.shields.io/badge/Star_Schema-1A1D24?style=for-the-badge&logo=databricks&logoColor=white)
![MWD](https://img.shields.io/badge/MWD_Data-E8751A?style=for-the-badge&logo=scipy&logoColor=white)

#### ¿Te resulta útil? Apóyalo con una [![Star](https://img.shields.io/github/stars/nrgarridoa/pbi-control-durezas?style=social)](https://github.com/nrgarridoa/pbi-control-durezas/stargazers) en el repositorio.

---

# Control de Durezas — Dashboard de Perforación y Voladura · Tajo Abierto

> **En perforación y voladura, no conocer la dureza del terreno antes de perforar cuesta dinero: brocas que se rompen, equipos subutilizados, voladuras ineficientes y retrasos en producción. Este dashboard transforma 97 988 registros MWD en inteligencia operativa — la diferencia entre planificar a ciegas y decidir con datos.**

Dashboard interactivo que convierte datos crudos del sistema MWD (Measurement While Drilling) en un mapa geoespacial de dureza del terreno, análisis de rendimiento por equipo y patrones geológicos — todo filtrable en tiempo real para superintendentes, jefes de perforación y geólogos.

[![Ver Dashboard en Vivo](https://img.shields.io/badge/Ver_Dashboard_en_Vivo-Power_BI_Service-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://app.powerbi.com/view?r=eyJrIjoiZTlkZTNjMjctOWNkMC00MzE0LWEwZWEtMDIwM2NiNjI4MDU4IiwidCI6ImY3YWNmODc2LWU3ZTgtNDQ0Yy05NWFlLWY5NTQ4YWNmZTMyZiIsImMiOjR9)

<sub>🇪🇸 Español · [🇬🇧 English](README.en.md)</sub>

---

## Vista previa

<div align="center">
  <img src="docs/summary.png" alt="Resumen — KPIs y distribución de dureza" width="90%" />
  <br/><br/>
  <img src="docs/hardness.png" alt="Mapa de Durezas — Sección Este X-Z" width="32%" />
  <img src="docs/performance.png" alt="Análisis de Rendimiento — PenRate vs Dureza" width="32%" />
  <img src="docs/geology.png" alt="Análisis Geológico — Dureza por litología" width="32%" />
</div>

---

## Qué problema resuelve

| Sin este dashboard | Con este dashboard |
|---|---|
| La dureza se conoce **después** de perforar | Mapa geoespacial de dureza para planificar **antes** |
| No se sabe qué equipo rinde mejor en cada zona | Comparativa de PenRate por equipo y tipo de roca |
| Geólogos y perforistas trabajan con datos separados | Una sola fuente de verdad con filtros cruzados |
| Los reportes son estáticos y tardíos | Dashboard interactivo actualizable en tiempo real |
| Errores de sensor pasan desapercibidos | Data limpiada y validada (98% de calidad) |

---

## Hallazgos clave

**1. La dureza controla el rendimiento**
- Roca blanda: **120.7 m/hr** de velocidad de penetración
- Roca muy dura: **34.4 m/hr** — casi 4× más lento
- Impacto directo en el costo por metro perforado y en el consumo de brocas

**2. La alteración importa tanto como la litología**
- Alteración fílica y potásica producen roca **~40% más dura** que la argílica
- Un taladro en zona fílica necesita más tiempo y desgasta más la broca

**3. No todos los equipos rinden igual por zona**
- DM-M3: 85 m/hr promedio — mayor producción acumulada (551 K metros)
- Pit Viper 351: 77 m/hr — menor producción, pero con menos horas operativas
- Dato clave para asignar equipos según la dureza esperada de cada zona

**4. Eficiencia del 95.2%**
- La profundidad real vs. planificada muestra alto cumplimiento operativo
- Ene y Dic son meses parciales (16 y 26 taladros), ajustar comparaciones YoY

---

## Cómo explorar el dashboard en vivo

En el [dashboard interactivo](https://app.powerbi.com/view?r=eyJrIjoiZTlkZTNjMjctOWNkMC00MzE0LWEwZWEtMDIwM2NiNjI4MDU4IiwidCI6ImY3YWNmODc2LWU3ZTgtNDQ0Yy05NWFlLWY5NTQ4YWNmZTMyZiIsImMiOjR9):

- **Segmentadores:** filtra por zona, alteración, roca, clase de dureza, litología o equipo — todos los visuales responden simultáneamente.
- **Filtrado cruzado:** clic en cualquier barra, punto o porción y el resto se ajusta.
- **Mapa geoespacial:** usa los bookmarks (Vista en Planta / Sección Norte / Sección Este) para cambiar de perspectiva del tajo.
- **Tooltips:** pasa el cursor sobre cualquier punto del mapa para ver 9 campos: coordenadas, zona, equipo, dureza, PenRate, profundidad y más.

---

## Los datos

El modelo está construido sobre 6 tablas en esquema estrella:

| Tabla | Contenido principal | Registros | Tipo |
|---|---|:---:|---|
| Fact_Dureza | Dureza, PenRate, coordenadas UTM, metros, horas por pasada | 97 988 | Hechos |
| Dim_Taladro | Taladros: zona, equipo, profundidad plan/real | 582 | Dimensión |
| Dim_Dureza | Clase: Blando / Mod. Duro / Duro / Muy Duro | 4 | Dimensión |
| Dim_Equipo | Modelo, fabricante, pulldown, peso | 6 | Dimensión |
| Dim_GrupoLito | Clasificación ISRM R0–R6 | 7 | Dimensión |
| Dim_Calendario | Jerarquía de fechas Ene 2024–Ene 2025 | 397 | Dimensión |

> **Origen:** Data sintética generada con distribuciones ajustadas a parámetros reales de operaciones de pórfido de cobre, altiplano peruano. Ningún dato se usa tal cual — todo pasa por un proceso de limpieza y ETL documentado en el repositorio privado.

---

## Páginas del dashboard

- **Resumen** — 7 KPIs globales: taladros, registros MWD, dureza promedio, PenRate, eficiencia, metros y horas. Distribución de dureza por litología y tendencia mensual.
- **Mapa de Durezas por Taladro** — Visualización geoespacial en 3 vistas (planta X-Y, sección norte Y-Z, sección este X-Z). Cada punto = 1 registro MWD, coloreado por clase de dureza. 6 segmentadores.
- **Análisis de Rendimiento** — Scatter PenRate vs Dureza (relación inversa). Comparativa de producción y rendimiento por equipo.
- **Análisis Geológico** — Dureza por las 7 unidades litológicas. Heatmap Roca × Alteración. Treemap de distribución litológica.

---

## Medidas y cálculos

Las medidas DAX se organizan en la tabla `_Medidas`, agrupadas por carpetas:

| Carpeta | Qué resuelve | Ejemplos |
|---|---|---|
| KPIs | Indicadores globales del panel resumen | Total Taladros, PenRate Promedio, Eficiencia Perforación |
| Producción | Volumen acumulado por periodo o equipo | Metros Totales, Horas Totales, Metros por Taladro |
| Rendimiento | Comparativas de velocidad por clase de dureza | PenRate por Clase, Variación PenRate |

> El código DAX completo y su lógica están documentados en el repositorio privado.

---

<details>
<summary><strong>Enfoque técnico — clic para expandir</strong></summary>

### Modelo de datos

Esquema estrella: 1 tabla de hechos central + 5 dimensiones.

```
                    Dim_Calendario (397)
                          │ 1
                          │ N
Dim_Equipo (6) ──1        │        1── Dim_Dureza (4)
             │  N         ▼       N │
             └──── Dim_Taladro ──── Fact_Dureza (97 988) ────N──1── Dim_GrupoLito (7)
                    (582)  1──N ──►
```

| Tabla | Filas | Se relaciona con | Cardinalidad |
|---|:---:|---|:---:|
| Fact_Dureza | 97 988 | tabla central de hechos | — |
| Dim_Taladro | 582 | Fact_Dureza | 1 : N |
| Dim_Dureza | 4 | Fact_Dureza | 1 : N |
| Dim_Calendario | 397 | Dim_Taladro | 1 : N |
| Dim_GrupoLito | 7 | Fact_Dureza | 1 : N |
| Dim_Equipo | 6 | Dim_Taladro | 1 : N |

### Decisiones de diseño clave

- **Sin mapa base:** los datos de coordenadas son confidenciales en operación real; se usó un scatter plot personalizado en UTM locales con bookmarks para cambio de perspectiva, logrando el mismo efecto sin exponer la ubicación exacta.
- **Tooltip de 9 campos:** diseñado para que el perforista o geólogo obtenga toda la información de un punto sin salir del visual.
- **Paleta de dureza con colores de marca:** Verde Mineral `#2D936C` · Azul Acero `#2E86AB` · Naranja Cobre `#E8751A` · Rojo Arcilla `#C1292E` — consistente en todas las páginas para que el patrón sea inmediatamente reconocible.

### Stack

| Herramienta | Uso |
|---|---|
| Power BI Desktop | Desarrollo del modelo y dashboard |
| Power Query (M) | ETL, limpieza y tipado de datos |
| DAX | Medidas, KPIs y columnas calculadas |
| Python / Pillow | Generación de assets de marca (social preview, card thumb) |
| Git / GitHub | Versionamiento y publicación |
| Power BI Service | Publicación y embed web |

> El detalle reproducible completo (`.pbix`, data, DAX documentado, scripts) está en el repositorio privado — disponible bajo solicitud.

</details>

---

## Estructura

### Repositorio público (este repositorio)

```
pbi-control-durezas/
├── docs/
│   ├── summary.png
│   ├── hardness.png
│   ├── performance.png
│   └── geology.png
├── social-preview.png
├── thumb.png
├── LICENSE
└── README.md
```

### Proyecto completo (repositorio privado — acceso bajo solicitud)

```
pbi-control-durezas-source/
├── pbix/
│   └── Control de durezas - Dashboard.pbix
├── data/
│   └── processed/
│       ├── Fact_Dureza.csv
│       ├── Dim_Taladro.csv
│       ├── Dim_Dureza.csv
│       ├── Dim_Equipo.csv
│       ├── Dim_GrupoLito.csv
│       └── Dim_Calendario.csv
├── docs/
├── scripts/
│   ├── gen_social_preview.py
│   └── gen_card_thumb.py
├── Tema_ControlDurezas.json
├── social-preview.png
├── thumb.png
├── CHANGELOG.md
├── LICENSE
└── README.md
```

---

## Autor

### Nilson Rolando Garrido Asenjo
**Mining Engineer · Industrial Manager · Power BI Developer · Python Developer · Data Analyst**

[![Portfolio](https://img.shields.io/badge/Portfolio-nrgarridoa.github.io-0068FF?style=for-the-badge&logo=github&logoColor=white)](https://nrgarridoa.github.io)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-nrgarridoa-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/nrgarridoa)
[![YouTube](https://img.shields.io/badge/YouTube-nrgarridoa-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/@nrgarridoa)
[![Email](https://img.shields.io/badge/Email-nrgarridoa@gmail.com-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:nrgarridoa@gmail.com)
[![GitHub](https://img.shields.io/badge/GitHub-nrgarridoa-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/nrgarridoa)

Ingeniero de Minas y Administrador Industrial con +10 años de experiencia; trayectoria operativa en Newmont Yanacocha, Gold Fields y Silver Mountain; Project Manager en CODEa UNI y consultor para Chinalco y Antamina. Piloto acreditado de drones con operaciones en superficie (fotogrametría, volumetría) y subterránea (LiDAR con Elios 3 para Flyability). Docente de Power BI y Python aplicado a minería.

> ¿Quieres ver el proyecto completo (`.pbix`, modelo, DAX documentado)? Doy acceso guiado al repositorio privado bajo solicitud.

---

## Licencia

Contenido bajo **[CC BY-NC-ND 4.0](LICENSE)** — puedes verlo y compartirlo dando crédito, sin uso comercial ni obras derivadas. © 2026 Nilson Rolando Garrido Asenjo.
