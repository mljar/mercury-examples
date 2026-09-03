# Pivot Table

A Mercury app that accepts a CSV upload and opens it in an interactive
drag-and-drop pivot table.

Install and run from the repository root:

```bash
pip install -r pivot-table/requirements.txt
mercury --working-dir pivot-table
```

The notebook uses Mercury's current `UploadFile` API: the uploaded filename is
available as `name`, while `value` contains the file bytes.

Docs: [UploadFile example](https://runmercury.com/examples/utility-apps/summarize-dataset-app/)
