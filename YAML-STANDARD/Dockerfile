FROM python:3.9-slim

# On définit le dossier de travail dans le conteneur. Toutes les commandes vont s'exectuer dans /app
WORKDIR /app 

# Copier les librairies
COPY requirements.txt .

# Installer les librairies
RUN pip install --no-cache-dir -r requirements.txt

# Copier le code source depuis la machine vers /app dans le conteneur
COPY . .

# l'application écoute sur le port 5000
EXPOSE 5000

# Commande de démarrage
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "5000"]
