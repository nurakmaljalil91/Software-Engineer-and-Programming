Use [Pipenv](https://packaging.python.org/tutorials/managing-dependencies/#managing-dependencies) or other tools is recommended for improving your development flow.

```bash
pip3 freeze > requirements.txt  # Python3
pip freeze > requirements.txt  # Python2
```

If you do not use a virtual environment, [pigar](https://github.com/Damnever/pigar) will be a good choice for you.
## Install from Requirement.txt

```bash
pip install -r requirements.txt
```