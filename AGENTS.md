# AGENTS.md

Guía para personas y agentes de IA que trabajan en este repo.

## Proyecto

Dashboard para visualizar acciones financieras, escrito en Python.

| Pieza | Herramienta |
|---|---|
| Datos | `yfinance` |
| Almacenamiento | DuckDB (`data/finanzas.duckdb`, fuera de git) |
| Dashboard | Streamlit |
| Dependencias | `uv` |
| Tests | `pytest` |
| Lint y formato | `ruff` |

### Arquitectura

```text
yfinance ──► ingest ──► data/finanzas.duckdb ──► app (Streamlit)
```

- La ingesta es la única que llama a la API y escribe en DuckDB.
- El dashboard solo lee DuckDB; nunca llama a la API directamente.

## Estructura

```text
src/dashboard_finanzas/   # código del paquete
tests/                    # tests con pytest, espejo de src/
.github/workflows/        # CI
data/                     # base de datos local (ignorada)
```

## Comandos

```bash
uv sync                      # instala dependencias
uv run pytest                # tests
uv run ruff check .          # lint
uv run ruff format .         # formato
```

CI corre `ruff check`, `ruff format --check` y `pytest`. Todo debe pasar antes de abrir un PR.

## Flujo de trabajo

Nada se sube directo a `main`: todo cambio entra por PR.

### Branches

Formato: `<tipo>/<descripcion-corta-en-kebab-case>`

| Tipo | Uso |
|---|---|
| `feat/` | Funcionalidad nueva |
| `fix/` | Corrección de bug |
| `docs/` | Solo documentación |
| `test/` | Solo tests |
| `refactor/` | Cambio interno sin cambiar comportamiento |
| `chore/` | Configuración, dependencias, CI |

Ejemplos: `feat/ingesta-precios-diarios`, `fix/fechas-zona-horaria`.

### Commits

[Conventional Commits](https://www.conventionalcommits.org/) con los mismos tipos que las branches:

```text
feat: agrega ingesta de precios diarios
fix: corrige zona horaria en fechas de cierre
```

### Pull requests

- Título con formato de Conventional Commit.
- La descripción tiene exactamente dos secciones y nada más:

```markdown
## Por qué
<el problema que resuelve el PR, en una o dos oraciones>

## Qué cambió
- <un cambio por línea>
```

- Un PR = un cambio coherente. Si se puede partir, pártelo.
- Todo PR que cambie comportamiento agrega una línea en `CHANGELOG.md` bajo `## [Unreleased]`.

## Tests

- Los archivos viven en `tests/` y se llaman `test_<modulo>.py`.
- Fixtures compartidas en `tests/conftest.py` (créalo cuando haga falta).
- Los tests no llaman a la red: simula `yfinance` y usa DuckDB en memoria (`duckdb.connect()`).
