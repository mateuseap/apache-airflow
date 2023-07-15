# Apache Airflow

Learning the fundamental concepts of Apache Airflow

    .
    ├── materials
    │   └── *
    ├── .gitignore
    └── README.md

➡️ Course: [Apache Airflow: The Hands-On Guide](https://www.udemy.com/course/the-ultimate-hands-on-course-to-master-apache-airflow/)

## The basics of Apache Airflow

### What is Apache Airflow?
**Apache Airflow** is an open-source platform to programmatically author, schedule, and monitor workflows.

Pros:

- It is **dynamic**, everything is done in Python.
- It is **scalable**.
- Has a **user interface**.
- It is **extensible**, so you can add plugins to it on your own.

### Core Components
- **Web server** 
    - Flask server with Gunicorn serving the UI.
- **Scheduler**
    - Daemon in charge of scheduling workflows.
- **Metastore**
    - Database where metadata is stored.
- **Executor**
    - Class defining **how** your tasks should be executed.
- **Worker**
    - Process/subprocess **executing** your task.

### What is a DAG?

**DAG** stands for **directed acyclic graph**, in other words, it is a directed graph with no loops. Here is an example:

![Directed Acyclic Graph](./assets/DAG.png)

Basically, a DAG in Apache Airflow represents a **data pipeline**. The **vertices** of a DAG represent the **tasks**, and the **directed edges** represent dependencies between the tasks.

### Operators

**Operators** are the building blocks of Airflow DAGs. They contain the logic of how data is processed in a pipeline. Each task in a DAG is defined by instantiating an operator. There are many different types of operators available in Airflow.

- **Action Operators**
    - Operators in charge of executing something.
- **Transfer Operators**
    - Operators that allow transfer data from a source to a destination.
- **Sensor Operators**
    - Operators that wait for something to happen before moving forward.

### Task instances

Apache Airflow **task instances** are defined as a representation for a specific run of a task and categorized by a collection of a DAG, a task, and a point in time.

### Workflow

A **workflow** in Apache Airflow is the combination of all the concepts we've learned about before in this README file.

### What Apache Airflow is not?

It is not a data streaming solution, nor a data processing framework. You should not process TB or GB of data in Airflow. Apache Airflow is a way to trigger external tools; it's an excellent orchestrator.

## How Apache Airflow works?

### One Node Architecture

![One Node Architecture](./assets/One%20Node%20Architecture.png)

### Multi Nodes Architecture (Celery)

![Multi Nodes Architecture (Celery)](./assets/Multi%20Nodes%20Architecture%20(Celery).png)

## Apache Airflow dependencies

### Extra dependencies

The ```apache-airflow``` PyPI basic package only installs what’s needed to get started. Additional packages can be installed depending on what will be useful in your environment. For instance, if you don’t need connectivity with **Postgres**, you won’t have to go through the trouble of installing the ```postgres-devel``` yum package, or whatever equivalent applies on the distribution you are using.

Most of the **extra dependencies** are linked to a corresponding **provider package**. For example “amazon” extra has a corresponding ```apache-airflow-providers-amazon``` provider package to be installed. When you install Airflow with such extras, the necessary provider packages are installed automatically (latest versions from PyPI for those packages). However, you can freely upgrade and install provider packages independently from the main Airflow installation.

For the list of the extras and what they enable, check out the Airflow documentation: [Reference for package extras](https://airflow.apache.org/docs/apache-airflow/stable/extra-packages-ref.html)

### Provider packages

The Airflow 2.0 is delivered in multiple, separate, but connected **provider packages**. The core of Airflow scheduling system is delivered as ```apache-airflow``` package and there are around 60 provider packages which can be installed separately as so called **Airflow provider packages**. The default Airflow installation doesn’t have many integrations and you have to install them yourself.

For more informations about it, check out the Airflow documentation: [Provider packages](https://airflow.apache.org/docs/apache-airflow-providers/index.html)

### Differences between extras and providers

**Extras** and **providers** are different things, though many extras are leading to installing providers. Extras are standard Python setuptools feature that allows to add additional set of dependencies as optional features to “core” Apache Airflow, while providers packages are just one of the type of such optional features, but not all optional features of Apache Airflow have corresponding providers.

## Apache Airflow files

After installing the Apache Airflow, you'll get the following files inside your ```airflow``` folder:

    .
    ├── airflow.cfg
    ├── airflow.db
    ├── logs
    │   └── *
    └── webserver_config.py

- ```airflow.cfg```
    - Stores all configuration settings of Apache Airflow.
- ```airflow.db```
    - Corresponds to the SQLite database of Apache Airflow.
- ```logs```
    - Folder that stores the logs of the ```scheduler``` and the ```tasks```.
- ```webserver_config.py```
    - File used to configure the web server, more specifically, used to configure the way the users are authenticated in Apache Airflow **user interface**.

## Running Apache Airflow locally using Docker

The fastest way is by using a **docker image** of Apache Airflow, you can use the **Dockerfile** that is inside [materials/section-2/](/materials/section-2/Dockerfile) folder to build your first Apache Airflow docker image. After getting the Dockerfile, run the below commands:

```bash
# Build a docker image from the Dockerfile in the current directory and name it 'airflow-basic'
docker build -t airflow-basic .

# Create a docker container named 'airflow' using the 'airflow-basic' docker image, also binds the container port 8080 with our local port 8080. This docker container will run in the background because of the '-d' param
docker run --name airflow -d -p 8080:8080 airflow-basic
```

After running the above commands and waiting a little bit of time, you can view the Apache Airflow **user interface** by openning [http://localhost:8080/](http://localhost:8080/) in your web browser:

![Apache Airflow UI](./assets/Apache%20Airflow%20UI.png)

You'll be able to login using the below credentials:

- Username: ```admin```
- Password: ```admin```

There is a lot of useful commands that you can view in [materials/section-2/docs/cli_commands.txt](/materials/section-2/docs/cli_commands.txt) file, I recommend you take a look in it! Also, I strongly recommend to learn about Docker ([Docker reference documentation](https://docs.docker.com/reference/)), it's really important.

## Apache Airflow CLI

There are some operations that you can't do by using the friendly Apache Airflow user interface, you'll need to use the **Apache Airflow CLI (command language interface)**. The first thing you have to know is that the commands in Apache Airflow CLI are **grouped**, most of the commands are separated in different groups according to the **resources** they interact to. Below is listed some useful commands:

- ```airflow -h```
    - List all available groups and commands.
- ```airflow db init```
    - Initialize the **metadabase** and generate the files and folders needed by Apache Airflow.
- ```airflow db reset```
    - Delete all the **metadata** in the Apache Airflow **metadatabase**.
- ```airflow db upgrade```
    - Upgrade the schemas that are in the Apache Airflow **metadatabase**.
- ```airflow webserver```
    - Start the **web server** and the **user interface** of Apache Airflow.
- ```airflow scheduler```
    - Start the **scheduler**.
- ```airflow celery worker```
    - Says that the machine is a Apache Airflow **worker**, so you can run **tasks** on it.
- ```airflow dags list```
    - List all the **dags**.
- ```airflow dags trigger [dag_id] -e [execution_date]```
    - Trigger an especific **dag** and set it's **execution date**.
- ```airflows dags list-runs -d [dag_id]```
    - List all **runs** of an specific **dag**.
- ```airflow dags backfill -s [start_date] -e [end_date] [dag_id] --reset-dagruns```
    - Retry already past triggered **dag runs** of an specific **dag** between two dates (ex: ```[start_date] -> 2021-01-01``` and ```[end_date] -> 2021-01-05```).
- ```airflow tasks list [dag_id]```
    - List all the **tasks** of a given **dag**.
- ```airflow tasks test [dag_id] [task_id] [execution_date]```
    - Check if an specific **task** of a given **dag** works. 