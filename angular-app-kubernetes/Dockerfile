# Étape 1 : Image avec Node compatible Angular
FROM node:18 AS build

# Définir le répertoire de travail
WORKDIR /app

# Copier les fichiers nécessaires
COPY package*.json ./

# Installer Angular CLI (version compatible)
RUN npm install -g @angular/cli@16

# Installer les dépendances (avec legacy pour les peerDeps si nécessaire)
RUN npm install --legacy-peer-deps

# Copier le reste du code
COPY . .

# Construire l'application Angular
RUN npm run build -- --configuration=production

# Vérification du contenu du build
RUN ls -l /app/dist
