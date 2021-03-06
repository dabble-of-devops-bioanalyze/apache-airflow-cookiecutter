This is a cookiecutter repo that I use to bootstrap Apache Airflow projects. It uses the [Bitnami Airflow Stack](https://github.com/bitnami/bitnami-docker-airflow) as a base.

The `Makefile` is always the most up to date place to look for commands.

## Quick Start

```
cookiecutter https://github.com/dabble-of-devops-bioanalyze/apache-airflow-cookiecutter
cd my_new_project_dir
make dev/up
```

## Requirements

You must have the Python package [cookiecutter](https://cookiecutter.readthedocs.io/en/1.7.2/first_steps.html) and [Docker Compose](https://docs.docker.com/compose/install/) installed. If you want to use the `Makefile` you'll need to have [make](https://www.gnu.org/software/make/) installed.

## Resources

[Airflow Docs](https://airflow.apache.org/docs/apache-airflow/stable)
[Airflow Plugins](https://airflow.apache.org/docs/apache-airflow/stable/plugins.html?highlight=plugins)
[Airflow as Python Plugins](https://airflow.apache.org/docs/apache-airflow/stable/plugins.html#plugins-as-python-packages)

[Bitnami - Airflow Tags](https://hub.docker.com/r/bitnami/airflow/tags?page=1&ordering=last_updated)
[Bitnami - Airflow Worker Tags](https://hub.docker.com/r/bitnami/airflow-worker/tags?page=1&ordering=last_updated)
[Bitnami - Airflow Scheduler Tags](https://hub.docker.com/r/bitnami/airflow-scheduler/tags?page=1&ordering=last_updated)

[Docker Compose Specs](https://docs.docker.com/compose/compose-file/compose-file-v3/)

## Tags

Bitnami uses [rolling tags](https://docs.bitnami.com/tutorials/understand-rolling-tags-containers/), which means that you are not necessarily getting the same image when you reuse a tag. Most of the time this is fine and will keep your images more secure. Occasionally a bot goes haywire and it's troublesome. Just update the `airflow_tag` in the `cookiecutter.json`. 

## Releases

Releases are matched against the `airflow_tag`.

```
cookiecutter https://github.com/dabble-of-devops-bioanalyze/apache-airflow-cookiecutter/archive/refs/tags/2.1.4-debian-10-r10.zip
```

## Troubleshooting

If you're having trouble starting up the stack it is likely that a configuration value has changed. The best place to see the most up to date variables is the [bitnami docs](https://github.com/bitnami/bitnami-docker-airflow).

Check out [the docker compose](https://github.com/bitnami/bitnami-docker-airflow/blob/master/docker-compose.yml) file and make sure your `.envs/.local/.airflow` file matches.

## Plugins Strategies

While you're developing the simplest way to add your plugins directory to `/opt/bitnami/airflow/plugins`. The dev stack is set to have the airflow webserver restart when anything changes.

When deploying to production I always recommend that you build your image with the plugins and dags copied over into the docker image. You can find out more about this in the [Airflow Docker Images Docs](https://airflow.apache.org/docs/docker-stack/build.html). These docs relate to the the official Airflow images. The images used here are the Bitnami images.

See the [Airflow as Python Plugins](https://airflow.apache.org/docs/apache-airflow/stable/plugins.html#plugins-as-python-packages) docs for more information.

You can also use the [BioAnalyze Apache Airflow Cookiecutter](https://github.com/dabble-of-devops-bioanalyze/apache-airflow-plugin-cookiecutter) with or without github submodules.

Airflow uses [Flask AppBuilder](https://flask-appbuilder.readthedocs.io/en/latest/templates.html) as it's base. This gives you lots of neat functionality.

- [FAB Base Views](https://flask-appbuilder.readthedocs.io/en/latest/views.html)
- [FAB Model Views](https://flask-appbuilder.readthedocs.io/en/latest/quickhowto.html)
- [FAB REST Api](https://flask-appbuilder.readthedocs.io/en/latest/rest_api.html)
- [FAB Templates](https://flask-appbuilder.readthedocs.io/en/latest/templates.html)

## Documenting your project

I didn't include any documentation bundles, but I suggest [jupyter-book](https://pypi.org/project/jupyter-book/) or [sphinx](https://www.sphinx-doc.org/en/master/usage/quickstart.html). Both are installed as a part of the `airflow/requirements/requirements-dev.txt`. Below are examples of how you would bootstrap with either of these.

```python
# pip install sphinx
cd docs
sphinx-quickstart
```

```python
# pip install jupyter-book
cd docs
jupyter-book create
```

Then:

```
livereload --host 0.0.0.0 --port 8082 --wait 5 docs
```


## FAQs

**Do I need to know Docker?**

It's helpful to already know Docker, but you'll be able to get started without any knowledge of Docker. I'm a big proponent of learn by building. If you want to learn by building this is a good project for you. Use the `Makefile` and the `docker-compose.yml` file.
