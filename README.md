# Containerized Drawing Recognition App

[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-c66648af7eb3fe8bc4f294546bfd86ef473780cde1dea487d3c4ff354943c9ae.svg)](https://classroom.github.com/online_ide?assignment_repo_id=9334554&assignment_repo_type=AssignmentRepo)
![ML Tests](https://github.com/software-students-fall2022/containerized-app-exercise-team4/actions/workflows/ml-tests.yaml/badge.svg)
![Web App Tests](https://github.com/software-students-fall2022/containerized-app-exercise-team4/actions/workflows/web-app-tests.yaml/badge.svg)

A multi-service web application that lets users sketch an object on a canvas and have a Keras CNN classify their drawing in real time. Built as an NYU Software Engineering project to practice container orchestration, microservice design, and shipping a polished, dockerized stack.

## Architecture

Three services, one network, orchestrated by Docker Compose:

| Service | Stack | Role |
|---|---|---|
| `web-app` | Flask + Jinja templates | Auth, leaderboards, score analytics |
| `machine-learning-client` | Flask + Keras (TensorFlow backend) | Serves a CNN trained on Google's QuickDraw doodles |
| `mongodb` | MongoDB 4.0 | Stores users, scores, and per-object stats |

The ML model (`keras10objects.h5`) recognizes 10 QuickDraw categories and grades each prediction by confidence, mapping it to a banded result: `PERFECT` (50 pts), `EXCELLENT` (40), `VERY GOOD` (30), `GOOD` (20), `AVERAGE` (10), `FAILED` (0).

## Quick start

Make sure Docker Desktop is installed ([Mac](https://docs.docker.com/desktop/install/mac-install/) / [Windows](https://docs.docker.com/desktop/install/windows-install/)) and running. From the repo root:

```bash
docker-compose up
```

The web app will be available at <http://127.0.0.1:3000>.

> **Tip:** If Docker is misbehaving with stale images, open Docker Desktop → bug icon (top right) → **Reset to factory defaults**, then re-run `docker-compose up`.
>
> **Note for Apple Silicon users:** One of the ML client's dependencies isn't supported on M1/M2 chips. You'll need to run the ML client inside a virtual machine or x86 emulation layer.

## Running components individually

### Web app (without Docker)

```bash
cd web-app
python -m venv .venv
source .venv/bin/activate         # on Windows: .venv\\Scripts\\activate.bat
pip install -r requirements.txt
export FLASK_APP=app.py            # on Windows: set FLASK_APP=app.py
export FLASK_ENV=development       # on Windows: set FLASK_ENV=development
flask run
```

Flask will print a local address (e.g. `http://127.0.0.1:5000`) — open it in your browser.

### Machine learning client (without Docker)

```bash
cd machine-learning-client
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
flask run
```

## Tests

Each service has its own pytest suite:

```bash
cd web-app && python -m pytest
cd machine-learning-client && python -m pytest
```

## Team

- [Kevin Gong](https://github.com/kxg202)
- [Dixit Timilsina](https://github.com/dt1930)
- [Maaz Ahmed](https://github.com/maazahmedd)
- [Sanjaya Bhatta](https://github.com/itSanjaya)
- [Fatema Nassar](https://github.com/fnassar)
- [Elyazya Al Kobaisi](https://github.com/elyazya)
