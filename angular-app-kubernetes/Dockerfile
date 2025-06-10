# Étape 1 : Image avec Node compatible Angular
FROM node:10 AS build

# Définir le répertoire de travail
WORKDIR /app

# Copier les fichiers nécessaires
COPY package*.json ./

# Installer Angular CLI (version compatible)
#RUN npm install -g @angular/cli@16

# Installer les dépendances (avec legacy pour les peerDeps si nécessaire)
RUN npm install --legacy-peer-deps

# Copier le reste du code
COPY . .

# Construire l'application Angular
#RUN npm run build -- --configuration=production --output-path=/app/dist/angular-app
RUN npx ng version
RUN npx ng build --configuration=production --output-path=/app/dist/angular-app || { echo "❌ Build Angular a échoué"; exit 1; }

RUN find /app -type d -name "dist"


# Vérification du contenu du build
#RUN ls -l /app/dist
# Vérification du contenu de /app après le build
RUN ls -l /app

# Vérification du contenu de /app/dist/angular-app
RUN ls -l */dist/*

# Étape 2 : Utiliser une image de Nginx pour héberger l'application construite
FROM nginx:alpine

# Copier l'application Angular construite dans le répertoire de Nginx
COPY --from=build /app/dist/angular-app /usr/share/nginx/html

# Exposer le port 80 pour que Nginx puisse répondre aux requêtes HTTP
EXPOSE 80

# Démarrer Nginx en mode non-démon
CMD ["nginx", "-g", "daemon off;"]
