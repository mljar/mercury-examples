# PyDeck in Jupyter and Mercury

A Mercury app demonstrating GeoJSON, arc, and hexagon PyDeck layers selected
from the app sidebar.

Install and run from the repository root:

```bash
pip install -r pydeck/requirements.txt
mercury --working-dir pydeck
```

PyDeck maps are display-only in the Mercury app; changing the Mercury selector
reruns the notebook and renders the selected map.

Docs: [Mercury Select widget](https://runmercury.com/docs/input/select/)
