# lf-documentation
Public documentation for LightFi sensors, data etc. View online at:

**[https://docs.lightfi.io](https://docs.lightfi.io)**

## Creating docs from markdown files

### Mkdocs

#### Initial setup
- See [Mkdocs getting-started](https://squidfunk.github.io/mkdocs-material/getting-started/)
- Create venv `python -m venv venv` & source virtual environment e.g. `source venv/bin/activate`
- Install mkdocs `pip install mkdocs-material`
- Bootstrap mkdocs `mkdocs new .`
- Edit `mkdocs.yml` to add theme etc. (see getting-started above)
- Can edit documentation in `./docs`, see [Writing your docs](https://www.mkdocs.org/user-guide/writing-your-docs/) 

#### Viewing docs while writing
- `mkdocs serve -a localhost:8001` (on port 8001)

#### GitHub pages publishing
- See [mkdocs documentation](https://squidfunk.github.io/mkdocs-material/publishing-your-site/) 
- Create github action to publish to `gh-pages` branch (edit from template)
- Ensure repository setup is set to use `gh-pages` branch root
- Should be available on [https://lightfilimited.github.io/lf-documentation/](https://lightfilimited.github.io/lf-documentation/)
- Add custom domain to Organisation Settings (will need to add TXT record to DNS)
- Add DNS CNAME record pointing `docs.lightfi.io` to `lightfilimited.github.io`
- Wait a bit for DNS things to propagate
- Add custom pages domain on github repository settings
- Add `CNAME` file to `docs/`
