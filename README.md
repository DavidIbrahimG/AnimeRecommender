
# 🎌 Anime Recommender System

## 📖 Overview

The **Anime Recommender System** is a machine learning project designed to suggest anime titles to users based on both content features and collaborative filtering. It leverages data from MyAnimeList (MAL), containing millions of user interactions and detailed anime metadata. 

This end-to-end solution integrates data ingestion, preprocessing, model training, and deployment, and was deployed using **Jenkins**, **Google Container Registry (GCR)**, and **Google Kubernetes Engine (GKE)**.

---

## 🗂️ Datasets

Three key CSV datasets were used and **ingested from a Google Cloud Storage (GCS) bucket**:

### 1. `anime.csv`

Metadata about each anime:

| Column | Description |
|--------|-------------|
| `MAL_ID` | Unique anime identifier |
| `Name`, `English name`, `Japanese name` | Titles |
| `Score` | Average MAL score |
| `Genres` | List of genres |
| `Type`, `Episodes`, `Duration` | Format and length |
| `Aired`, `Premiered`, `Rating` | Schedule and audience |
| `Studios`, `Producers`, `Licensors` | Production details |
| `Ranked`, `Popularity`, `Members`, `Favorites` | Community metrics |
| `Score-10` to `Score-1` | User rating distribution |

**Example:**
```csv
1,Cowboy Bebop,8.78,"Action, Adventure, Comedy, Drama, Sci-Fi, Space",Cowboy Bebop,カウボーイビバップ,TV,26,...
```

---

### 2. `anime_with_synopsis.csv`

Adds **long-form plot summaries** to each anime.

| Column | Description |
|--------|-------------|
| `MAL_ID`, `Name`, `Score`, `Genres` | Same as above |
| `Synopsis` | Detailed plot description |

**Example:**

> "In the year 2071, humanity has colonized several planets... The ragtag team aboard the spaceship Bebop..."

---

### 3. `animelist.csv`

User-anime interaction data.

| Column | Description |
|--------|-------------|
| `user_id` | Unique user ID |
| `anime_id` | Corresponds to `MAL_ID` |
| `rating` | User rating (1–10) |
| `watching_status` | e.g., watching, completed, dropped |
| `watched_episodes` | Number of episodes watched |

The dataset was filtered to **5 million rows** to balance scale and performance.

**Example:**
```csv
0,67,9,1,1
0,6702,7,1,4
```

---

## ☁️ Cloud Integration and Data Ingestion

All datasets were stored in a **GCP bucket** and downloaded programmatically:

- ✅ `gcs_downloader.py` authenticated using a service account key  
- ✅ Datasets were read using **Pandas** for cleaning and merging  
- ✅ Large `animelist.csv` file was **filtered down to 5 million interactions**

---

## 🧠 Model Overview

The system combines:

### ✅ Collaborative Filtering
- Matrix Factorization based on user ratings  
- Captures user taste patterns

### ✅ Content-Based Filtering
- Uses genres, synopsis, and studio info  
- Applies TF-IDF or text embeddings on synopses

---

## ⚙️ ML Pipeline Workflow

1. Data Ingestion from GCS  
2. Data Merging (`anime.csv` + `anime_with_synopsis.csv` + `animelist.csv`)  
3. Exploratory Data Analysis (EDA)  
4. Model Training  
5. Evaluation & Metrics  
6. Image Packaging with Docker  
7. Push to GCR  
8. Deploy to GKE via Jenkins

---

## 🚀 DevOps & Deployment

### 🐳 Docker
- Dockerized the training and serving components  
- Included `Dockerfile` and health checks

### 🔧 Jenkins
CI/CD pipeline configured with:
- `gcloud auth activate-service-account`  
- `docker build -t gcr.io/<project>/ml-project:latest`  
- `docker push` to Google Container Registry  
- `kubectl apply -f deployment.yaml` to Google Kubernetes Engine

### ☸️ Google Kubernetes Engine (GKE)
- Deployed the app to GKE via Jenkins  
- Autoscaling and load balancing managed through Kubernetes YAML files

---

## 🧪 Sample Insight

> Users who watched **"Cowboy Bebop" (TV)** were recommended **"Cowboy Bebop: Tengoku no Tobira" (Movie)** due to shared genres, storyline continuation, and high collaborative filtering score.

---
