# lf-documentation
Public documentation for LightFi sensors, data etc.

## Creating docs from markdown files

### Mkdocs

#### Initial setup
- See [Mkdocs getting-started](https://squidfunk.github.io/mkdocs-material/getting-started/)
- Create venv `python -m venv venv` & source virtual environment
- Install mkdocs `pip install mkdocs-material`
- Bootstrap mkdocs `mkdocs new .`
- Edit `mkdocs.yml` to add theme etc. (see getting-started above)
- Can edit documentation in `./docs`

#### Viewing docs while writing
- `mkdocs serve -a localhost:8001` (on port 8001)

#### GitHub pages publishing
- See [mkdocs documentation](https://squidfunk.github.io/mkdocs-material/publishing-your-site/) 
- Create github action to publish to `gh-pages` branch (edit from template)
- ...
