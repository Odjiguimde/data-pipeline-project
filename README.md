# 🚀 Pipeline Data Engineer Complet

## Architecture
```
[Sources de données] → [ETL Python] → [PostgreSQL + MongoDB] → [API REST FastAPI]
       ↓                    ↓                   ↓                      ↓
  (CSV/API/JSON)     (Nettoyage/Transform)  (Stockage)          (Exposition)
                          ↓
                     [Logs + Monitoring]
```

## Stack Technique
- **Python 3.10+** — ETL, API
- **FastAPI** — API REST
- **PostgreSQL** — données structurées (relationnelles)
- **MongoDB** — données semi-structurées (logs, events)
- **SQLAlchemy** — ORM PostgreSQL
- **Alembic** — migrations DB
- **Loguru** — logging avancé
- **Pandas** — transformation de données
- **Docker + Docker Compose** — orchestration

---

## ⚙️ Installation sur ta machine

### 1. Prérequis
```bash
# Installer Python 3.10+
sudo apt update && sudo apt install python3 python3-pip python3-venv -y

# Installer PostgreSQL
sudo apt install postgresql postgresql-contrib -y
sudo systemctl start postgresql

# Installer MongoDB
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
sudo apt update && sudo apt install -y mongodb-org
sudo systemctl start mongod

# Docker (optionnel mais recommandé)
sudo apt install docker.io docker-compose -y
sudo usermod -aG docker $USER
```

### 2. Environnement Python
```bash
cd data_pipeline
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 3. Configuration PostgreSQL
```bash
sudo -u postgres psql
CREATE USER pipeline_user WITH PASSWORD 'pipeline_pass';
CREATE DATABASE pipeline_db OWNER pipeline_user;
GRANT ALL PRIVILEGES ON DATABASE pipeline_db TO pipeline_user;
\q
```

### 4. Copier et configurer le .env
```bash
cp config/.env.example .env
# Éditer .env avec tes credentials
```

### 5. Migrations de base de données
```bash
alembic upgrade head
```

### 6. Lancer le pipeline
```bash
# ETL complet
python etl/main_pipeline.py

# API REST
uvicorn api.main:app --reload --port 8000
```

### Option Docker Compose (tout en un)
```bash
docker-compose up -d
```

---

## 📁 Structure du Projet
```
data pipeline/
├── README.md
├── requirements.txt
├── docker-compose.yml
├── .env.example
├── alembic.ini
├── config/
│   ├── settings.py
│   └── .env.example
├── etl/
│   ├── extract.py       # Collecte des données
│   ├── transform.py     # Nettoyage & transformation
│   ├── load.py          # Chargement PostgreSQL + MongoDB
│   └── main_pipeline.py # Orchestrateur principal
├── db/
│   ├── postgres.py      # Connexion + modèles SQLAlchemy
│   ├── mongodb.py       # Connexion + opérations MongoDB
│   └── migrations/      # Alembic migrations
├── api/
│   ├── main.py          # App FastAPI
│   ├── routes/
│   │   ├── products.py  # Endpoints produits
│   │   └── pipeline.py  # Endpoints pipeline status
│   └── schemas.py       # Pydantic models
├── logs/
│   └── pipeline.log     # Logs générés automatiquement
├── tests/
│   ├── test_etl.py
│   └── test_api.py
└── scripts/
    ├── seed_data.py     # Générer des fausses données
    └── health_check.py  # Vérifier l'état du système
```
