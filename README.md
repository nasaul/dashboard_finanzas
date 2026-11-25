# Dashboard Finanzas

Dashboard para visualizar acciones financieras. Los datos se descargan con `yfinance`, se guardan en DuckDB y se muestran con Streamlit.

> Estado: estructura inicial, todavía sin funcionalidad.

## Requisitos

- [uv](https://docs.astral.sh/uv/)

## Instalación

```bash
uv sync
```

## Desarrollo

```bash
uv run pytest
uv run ruff check .
uv run ruff format .
```

Las convenciones de branches, commits y PRs están en [AGENTS.md](AGENTS.md). Los cambios se registran en [CHANGELOG.md](CHANGELOG.md).
