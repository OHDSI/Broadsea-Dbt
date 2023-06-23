# Broadsea-Dbt Docker container

An OHDSI Broadsea Docker container for the open source Data Build Tool

The DBT project code and DBT profiles are shared with the container using Docker volumes.
The volume host paths are specified in the .env file.


## Configuration

You can configure the paths to your DBT project and profiles by editing the `.env` file:

```env
DBT_PROJECT_PATH=./project
DBT_PROFILE_PATH=./profiles
```

The paths are relative to the current directory and will be mounted as volumes in the broadsea-dbt container.

## Usage

You can run DBT commands using Docker Compose:

```bash
docker compose run --rm dbt <dbt-command>
```

Replace `<command>` with your desired DBT command.

For example, to show the DBT help infot, use the following command:

```bash
docker compose run --rm dbt run --help
```

## demo

First, start the broadsea-atlasdb demo database:

```bash
docker compose --profile atlasdb up -d
```

Important: Wait until the atlasdb container shows as 'healthy' state when you run 'docker ps'.

Run the example dbt project using the broadsea-atlasdb demo omop cdm postgreSQL database
that contains a single dbt model example_dbt_project/models/example/person_id_model.sql
which reads the first 100 person_ids from the demo_cdm schema person table and creates
a new table in the public schema called person_id_model:
  
```bash
docker compose run --rm dbt clean
cd example_dbt_project 
docker compose run --rm dbt run
```

Connect to the atlasdb database using a postgreSQL client to see the new public.person_id_model table.

Shutdown the atlasdb database container when you are finished with the demo:
```bash
docker compose --profile atlasdb down
```


## Build Instructions

Note. The OHDSI/Broadsea-Dbt Docker image is in Docker Hub, so building the image is optional.

First, clone the repository and navigate into the project directory:

```bash
git clone https://github.com/OHDSI/Broadsea-Dbt.git
cd Broadsea-Dbt
```

Next, build the Docker image using Docker Compose:

```bash
docker compose build dbt
```

This will build the broadsea-dbt Docker image based on the Dockerfile in the project directory.
The resulting image will be tagged as `ohdsi/broadsea-dbt`.
