# Develop GenAI Apps with Gemini and Streamlit: Challenge Lab

## Task 2: Write Streamlit Framework and Prompt Python Code to Complete `chef.py` 

This guide explains how to complete **Task 2** in the **Challenge Lab** for the course **Develop GenAI Apps with Gemini and Streamlit**.

You will modify and test the `chef.py` Streamlit app to integrate with the **Gemini API** and generate meal recipes based on user inputs. 

**Chef App**!  
This project demonstrates how to build, test, and deploy a **Generative AI application** using **Gemini Pro**, **Streamlit**, and **Cloud Run**.

---

## What You Will Do 

- Download and update the `chef.py` application.
- Implement options for **Red**, **White**, and **None** (presumably for wine pairings).
- Modify the **Gemini prompt template**.
- Test the application locally using **Streamlit**.

---

## Project Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/GoogleCloudPlatform/generative-ai.git
cd generative-ai/gemini/sample-apps/gemini-streamlit-cloudrun
```

---

### 2. Install Python Dependencies

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
pip install google-cloud-aiplatform google-cloud-logging
```
---

### 3. Setup Environment Variables

```bash
export PROJECT=<your-project-id>
export REGION=us-central1
```

---

### 4. Download and Modify `chef.py`

```bash
gsutil cp gs://spls/gsp517/chef.py .
```

- Modify `chef.py`:
  - Include options for **Red**, **White**, and **None**.
  - Update the **Gemini prompt template**.

- Save changes and upload the file:

```bash
gcloud storage cp chef.py gs://<your-bucket-name>/
```

---

### 5. Test Locally with Streamlit

Run the app locally:

```bash
streamlit run chef.py --browser.serverAddress=localhost --server.enableXsrfProtection=false --server.enableCORS=false --server.port=8080
```

---

### 6. Prepare Dockerfile

Edit `Dockerfile`:

```bash
vi Dockerfile
```

- Change `app.py` to `chef.py`.

---

### 7. Build and Push Docker Image

Set variables:

```bash
AR_REPO='chef-repo'
SERVICE_NAME='chef-streamlit-app'
```

Create Artifact Registry:

```bash
gcloud artifacts repositories create "$AR_REPO" --location="$REGION" --repository-format=Docker
```

Build and push the Docker image:

```bash
gcloud builds submit --tag "$REGION-docker.pkg.dev/$PROJECT/$AR_REPO/$SERVICE_NAME"
```

---

### 8. Deploy to Cloud Run

Deploy your container to Cloud Run:

```bash
gcloud run deploy "$SERVICE_NAME" --port=8080 --image="$REGION-docker.pkg.dev/$PROJECT/$AR_REPO/$SERVICE_NAME" --allow-unauthenticated --region="$REGION" --platform=managed --project="$PROJECT" --set-env-vars=PROJECT=$PROJECT,REGION=$REGION
```

---

You should now see a working Chef app that takes user preferences and generates recipes using the **Gemini** model!
