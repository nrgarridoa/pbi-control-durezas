# Changelog — Control de Durezas

Todos los cambios relevantes de este proyecto se documentan aquí.
Formato basado en [Keep a Changelog](https://keepachangelog.com/es/1.0.0/).

---

## [1.5.0] — 2026-07-23

### Agregado
- `scripts/_extract_shape.py`: extrae la nube de puntos del scatter plot de referencia (fondo blanco, exportado desde Power BI Desktop); clasifica píxeles por HSV, mapea posiciones a coordenadas UTM·Z reales y guarda `_shape_data.json`
- `scripts/_shape_data.json`: 1 964 puntos reales (A=50 Blando, B=747 Mod.Duro, C=1047 Duro, D=120 Muy Duro) con coordenadas UTM·Z y clase de dureza

### Actualizado
- **`scripts/gen_card_thumb.py`** reescrito para leer `_shape_data.json` y renderizar la forma geológica real del tajo (sección X·Z) — reemplaza la aproximación sintética de v1.4.0; corregido el mapeo color→clase según el dashboard real
- **`thumb.png`**: V del tajo real, distribución de colores fiel al dashboard (dominan Duro+Mod.Duro en el fondo, naranja disperso, azul en la cubeta)

### Corrección de paleta
Colores reales del dashboard confirmados por extracción de píxeles:
- Blando → `#E8751A` Naranja Cobre · Mod. Duro → `#137A47` verde oscuro
- Duro → `#C8943C` oro/tan · Muy Duro → `#2E86AB` Azul Acero

---

## [1.4.0] — 2026-07-23

### Actualizado
- **`scripts/gen_card_thumb.py`** reescrito como generación 100 % sintética: ya no depende de `docs/hardness.png`; usa la misma nube de puntos X·Z del social preview pero escalada al frame completo (1200×630), con radio de punto 6 px y más densidad (1 860 puntos vs 560+720)
- **`thumb.png`**: nueva versión con paleta de marca correcta (fondo `#052E1F`, colores de dureza `#2D936C`·`#2E86AB`·`#E8751A`·`#C1292E`), marco interior `PANEL = #031A12`, leyenda centrada al pie

### Eliminado
- Dependencia de numpy y extracción de píxeles HSV de captura — reemplazada por geometría sintética reproducible (`random.seed(42)`)

---

## [1.3.0] — 2026-07-22

### Agregado
- `README.en.md`: traducción completa de la vitrina pública al inglés; mismo nivel de detalle que la versión española (hallazgos, datos, exploración, autor, licencia)
- Selector de idioma activo en ambas versiones del README público (`🇪🇸 / 🇬🇧`)
- Topics de GitHub configurados en ambos repos según estándar del portafolio: base (`power-bi`, `dax`, `power-query`, `data-analytics`, `dashboard`) + dominio (`mining`, `mwd`, `geospatial`, `open-pit-mining`) + técnica (`star-schema`) + solo público (`portfolio`, `data-visualization`)

### Actualizado
- **Paleta de marca aplicada al dashboard**: Verde Mineral `#2D936C` · Azul Acero `#2E86AB` · Naranja Cobre `#E8751A` · Rojo Arcilla `#C1292E` — reemplaza los colores genéricos anteriores en todas las páginas
- **4 capturas**: renombradas a inglés (`summary`, `hardness`, `performance`, `geology`) y regeneradas con la paleta actualizada
- **README privado**: terminología corregida a "Perforación y Voladura (P&V)"; `Dim_Taladro` en todas las referencias (DAX, ETL, diagrama, estructura); diagrama ASCII del modelo con cardinalidades explícitas; colores del semáforo actualizados a paleta de marca; títulos y bio del autor alineados con la versión pública

---

## [1.2.0] — 2026-07-19

### Agregado
- `thumb.png` (1200×630): miniatura para tarjeta de portafolio en `nrgarridoa.github.io`; reproduce la nube de puntos de la sección X-Z sobre fondo oscuro de marca
- `scripts/gen_card_thumb.py`: lee `docs/dureza.png`, extrae los puntos coloreados por clase de dureza y los re-renderiza con Pillow

### Actualizado
- **Estructura de carpetas** reorganizada al estándar `pbi-{{proyecto}}-source`:
  - `.pbix` movido a `pbix/`
  - CSVs movidos a `data/processed/`
  - `screenshots/` renombrado a `docs/`
- **Terminología corregida**: `Dim_Sondaje` → `Dim_Taladro` en modelo Power BI, Power Query, CSV fuente y README (sondaje = exploración; taladro = perforación de producción para voladura)
- **Colores del dashboard** actualizados a paleta de marca (primera iteración, completada en v1.3.0)
- **Link del dashboard en vivo** actualizado tras rediseño de colores en Power BI Service
- **README privado** reescrito desde cero a plantilla v3: estructura numerada por secciones, tabla de limpieza ETL detallada, DAX documentado por carpetas, diagrama del modelo, contexto de operación, tabla de retos y soluciones

---

## [1.1.0] — 2026-06-28

### Agregado
- `scripts/gen_social_preview.py`: genera imagen 1280×640 con la sección X-Z del tajo abierto como gancho visual — nube de puntos con topografía en forma de V, taladros coloreados por clase de dureza, ticks de elevación 3 500–4 000 m; requiere Pillow
- `pbix/Control de durezas - Dashboard.pbix` (9.1 MB): archivo fuente completo del dashboard; solo existe en el repo privado (`.gitignore` en el público lo excluye)

### Actualizado
- **Social preview** (dos iteraciones en la misma sesión):
  1. Primera imagen de marca: título, stack tecnológico, autor y grilla genérica de taladros coloreados por clase de dureza
  2. Segunda versión: grilla reemplazada por la nube de puntos que reproduce la topografía real del pit, con el hueco central del tajo visible

---

## [1.0.0] — 2026-06-28

### Agregado
- Dashboard publicado en Power BI Service con 4 páginas analíticas:
  - **Resumen**: 7 KPIs globales (taladros, registros MWD, dureza promedio, PenRate, eficiencia, metros, horas), distribución por litología, tendencia mensual
  - **Mapa de Durezas**: scatter geoespacial en 3 vistas (planta X-Y, sección norte Y-Z, sección este X-Z); cada punto = 1 registro MWD; 6 segmentadores cruzados; tooltip de 9 campos
  - **Análisis de Rendimiento**: scatter PenRate vs Dureza, comparativa de producción y rendimiento por equipo
  - **Análisis Geológico**: dureza por 7 unidades litológicas, heatmap Roca × Alteración, treemap de distribución
- Modelo estrella: `Fact_Dureza` (97 988 registros MWD) + 5 dimensiones (`Dim_Sondaje` 582, `Dim_Dureza` 4, `Dim_Equipo` 6, `Dim_GrupoLito` 7, `Dim_Calendario` 397)
- 6 CSVs de data en `data/`: `Fact_Dureza.csv`, `Dim_Sondaje.csv`, `Dim_Dureza.csv`, `Dim_Equipo.csv`, `Dim_GrupoLito.csv`, `Dim_Calendario.csv`
- `Tema_ControlDurezas.json`: tema visual importable en Power BI Desktop
- 4 capturas iniciales del dashboard en `screenshots/` (en español; renombradas en v1.2.0)
- `README.md` inicial con descripción del proyecto, estructura de doble repo y guía de reproducción
- `.gitignore` y `LICENSE` (Todos los derechos reservados)
- Estructura de doble repo establecida: `pbi-control-durezas-source` (privado) / `pbi-control-durezas` (público)
