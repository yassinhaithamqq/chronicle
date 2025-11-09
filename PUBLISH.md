# Publishing Chronicle to PyPI

## Option 1: Local Manual Publishing (Easiest)

1. **Install publishing tools:**
   ```bash
   pip install build twine
   ```

2. **Build the package:**
   ```bash
   python -m build
   ```

3. **Upload to PyPI:**
   ```bash
   # First time: Create account at https://pypi.org/account/register/
   # Get API token from https://pypi.org/manage/account/token/
   
   twine upload dist/*
   # Enter username: __token__
   # Enter password: <your-pypi-token>
   ```

## Option 2: GitHub Trusted Publishing

1. **Go to PyPI:**
   - Visit: https://pypi.org/manage/account/publishing/
   - Click "Add a new pending publisher"

2. **Configure publisher:**
   - PyPI Project Name: `chronicle-events`
   - Owner: `dukeblue1994-glitch`
   - Repository: `chronicle`
   - Workflow: `publish.yml`
   - Environment: `pypi`

3. **Create a new release:**
   ```bash
   gh release create v0.1.1 --title "Chronicle v0.1.1" --notes "Release notes"
   ```

## Option 3: Manual Workflow with Token

1. **Get PyPI API token:**
   - Go to https://pypi.org/manage/account/token/
   - Create a token with upload permissions

2. **Add token to GitHub:**
   - Go to: https://github.com/dukeblue1994-glitch/chronicle/settings/secrets/actions
   - Click "New repository secret"
   - Name: `PYPI_API_TOKEN`
   - Value: `<your-token>`

3. **Run workflow:**
   - Go to: https://github.com/dukeblue1994-glitch/chronicle/actions/workflows/publish-manual.yml
   - Click "Run workflow"

## Verifying Publication

After publishing, verify:
```bash
pip install chronicle-events
python -c "import chronicle; print(chronicle.__version__)"
```

Check on PyPI: https://pypi.org/project/chronicle-events/
