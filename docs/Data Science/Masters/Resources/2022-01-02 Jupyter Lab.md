### Set Password
```bash
jupyter lab password
```

### How to access Jupyter notebook remotely

```bash
jupyter lab --ip 0.0.0.0 --port 8080 
```

```commandline
python -m ipykernel install --user --name=githib_env
```

### Install Extensions
```bash
conda install nodejs
jupyter labextension install jupyterlab_tensorboard
jupyter labextension install jupyterlab_voyager
jupyter labextension install @mflevine/jupyterlab_html
jupyter labextension install @jupyterlab/toc
jupyter labextension install @jupyterlab/google-drive
pip install jupyterlab-drawio
pip install ipympl
pip install jupyterlab_execute_time
pip install jupyter_bokeh
pip install jupyterlab-git
pip install jupyterlab-tabular-data-editor
pip install jupyterlab-spreadsheet-editor
```

#### ipyml Usage
```
%matplotlib widget
```

#### Kite Engine
```
bash -c "$(wget -q -O - https://linux.kite.com/dls/linux/current)"
pip install "jupyterlab-kite>=2.0.2"
```

### Embed Videos in Jupyter Lab

```python
from IPython.display import Video
video_loc = "path_to_video.mp4"
Video(video_loc,embed=True)

## Youtube Video
from IPython.display import HTML
HTML("""<iframe width="560" height="315" src="https://www.youtube.com/embed/MKzkmyiNX0k" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>""")
```
