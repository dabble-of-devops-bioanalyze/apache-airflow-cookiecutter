This is a cookiecutter repo that I use to bootstrap Apache Airflow projects.

## Requirements

You must have the Python package [cookiecutter](https://cookiecutter.readthedocs.io/en/1.7.2/first_steps.html) and [Docker Compose](https://docs.docker.com/compose/install/) installed. If you want to use the `Makefile` you'll need to have [make](https://www.gnu.org/software/make/) installed.

## Usage

```
cookiecutter https://github.com/dabble-of-devops-bioanalyze/apache-airflow-cookiecutter
cd my_new_project_dir
make dev/up
```

## Resources

[Airflow Docs](https://airflow.apache.org/docs/apache-airflow/stable)

[Airflow Tags](https://hub.docker.com/r/bitnami/airflow/tags?page=1&ordering=last_updated)
[Airflow Worker Tags](https://hub.docker.com/r/bitnami/airflow-worker/tags?page=1&ordering=last_updated)
[Airflow Scheduler Tags](https://hub.docker.com/r/bitnami/airflow-scheduler/tags?page=1&ordering=last_updated)

[Docker Compose Specs](https://docs.docker.com/compose/compose-file/compose-file-v3/)

## Documenting your project

I didn't include any documentation bundles, but I suggest [jupyter-book](https://pypi.org/project/jupyter-book/) or [sphinx](https://www.sphinx-doc.org/en/master/usage/quickstart.html). Both are installed as a part of the `airflow/requirements/requirements-dev.txt`

```python
# pip install sphinx
sphinx-quickstart
```

```python
# pip install jupyter-book
jupyter-book create
```

Then:

```
livereload --host 0.0.0.0 --port 8082 --wait 5 docs
```

## Plugins Strategies

While you're developing the simplest way to add your plugins directory to `/opt/bitnami/airflow/plugins`. The dev stack is set to have the airflow webserver restart when anything changes.

## FAQs

**Do I need to know Docker?**

It's helpful to already know Docker, but you'll be able to get started without any knowledge of Docker. I'm a big proponent of learn by building. If you want to learn by building this is a good project for you. Use the `Makefile` and the `docker-compose.yml` file.