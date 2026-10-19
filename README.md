# Física Computacional — Instituto Balseiro

Material público de la materia.

- **Sitio:** <https://flavio.colavecchia.net/fisica-computacional-ib/>
- **Rama de la cursada:** `2026`

## Ver el sitio localmente

```bash
uv sync
uv run quarto preview
```

Cada push a `2026` publica el sitio con GitHub Actions. Las páginas `.qmd` con
`execute: enabled: true` ejecutan su código al renderizar, con el entorno de
`pyproject.toml`; el resto del sitio no ejecuta código.
