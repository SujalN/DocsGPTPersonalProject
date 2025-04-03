<h1 align="center">
  DocsGPT
</h1>

## Project Structure

- Application - Flask app (main application).

- Scripts - Script that creates similarity search index for other libraries.

- Frontend - Frontend uses <a href="https://vitejs.dev/">Vite</a> and <a href="https://react.dev/">React</a>.

## QuickStart

> [!Note]
> Make sure you have [Docker](https://docs.docker.com/engine/install/) installed

On Mac OS or Linux, write:

`./setup.sh`

It will install all the dependencies and allow you to download the local model, use OpenAI or use our LLM API.

Otherwise, refer to this Guide for Windows:

1. Create a `.env` file in your root directory and set the env variables and `VITE_API_STREAMING` to true or false, depending on whether you want streaming answers or not.
   It should look like this inside:

   ```
   LLM_NAME=[docsgpt or openai or others] 
   VITE_API_STREAMING=true
   API_KEY=[if LLM_NAME is openai]
   ```

   See optional environment variables in the [/.env-template] and [/application/.env_sample] files.

2. Run ./run-with-docker-compose.sh.

3. Navigate to http://localhost:5173/.

To stop, just run `Ctrl + C`.

## Development Environments

### Spin up Mongo and Redis

For development, only two containers are used from [docker-compose.yaml](https://github.com/arc53/DocsGPT/blob/main/docker-compose.yaml) (by deleting all services except for Redis and Mongo).
See file [docker-compose-dev.yaml](./docker-compose-dev.yaml).

Run

```
docker compose -f docker-compose-dev.yaml build
docker compose -f docker-compose-dev.yaml up -d
```

### Run the Backend

> [!Note]
> Make sure you have Python 3.10 or 3.11 installed.

1. Export required environment variables or prepare a `.env` file in the project folder:
   - Copy [.env_sample](https://github.com/arc53/DocsGPT/blob/main/application/.env_sample) and create `.env`.

(check out [`application/core/settings.py`](application/core/settings.py) if you want to see more config options.)

2. (optional) Create a Python virtual environment:
   You can follow the [Python official documentation](https://docs.python.org/3/tutorial/venv.html) for virtual environments.

a) On Mac OS and Linux

```commandline
python -m venv venv
. venv/bin/activate
```

b) On Windows

```commandline
python -m venv venv
 venv/Scripts/activate
```

3. Download embedding model and save it in the `model/` folder:
You can use the script below, or download it manually from [here](https://d3dg1063dc54p9.cloudfront.net/models/embeddings/mpnet-base-v2.zip), unzip it and save it in the `model/` folder.

```commandline
wget https://d3dg1063dc54p9.cloudfront.net/models/embeddings/mpnet-base-v2.zip
unzip mpnet-base-v2.zip -d model
rm mpnet-base-v2.zip
```

4. Install dependencies for the backend:

```commandline
pip install -r application/requirements.txt
```

5. Run the app using `flask --app application/app.py run --host=0.0.0.0 --port=7091`.
6. Start worker with `celery -A application.app.celery worker -l INFO`.

### Start Frontend

> [!Note]
> Make sure you have Node version 16 or higher.

1. Navigate to the [/frontend](https://github.com/arc53/DocsGPT/tree/main/frontend) folder.
2. Install the required packages `husky` and `vite` (ignore if already installed).

```commandline
npm install husky -g
npm install vite -g
```

3. Install dependencies by running `npm install --include=dev`.
4. Run the app using `npm run dev`
