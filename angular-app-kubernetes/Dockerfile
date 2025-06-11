# Étape 1 : Image avec Node compatible Angular
FROM node:20 AS build

# Définir le répertoire de travail
WORKDIR /app

# Copier les fichiers nécessaires
COPY package*.json ./

# Installer Angular CLI (version compatible)
RUN npm install -g @angular/cli@16

# Installer les dépendances (avec legacy pour les peerDeps si nécessaire)

RUN rm -rf node_modules package-lock.json && \
    npm install --legacy-peer-deps&& \
    npm install typescript@2.9.2 --save-dev

RUN npx tsc -v


# Copier le reste du code
COPY . .

# Construire l'application Angular
#RUN npm run build -- --configuration=production --output-path=/app/dist/angular-app
RUN npx ng version

ENV NODE_OPTIONS=--openssl-legacy-provider

RUN npx ng build --configuration=production --output-path=/app/dist/angular-app || { echo "❌ Build Angular a échoué"; exit 1; }

RUN find /app -type d -name "dist"

# Vérification du contenu du build
#RUN ls -l /app/dist
# Vérification du contenu de /app après le build
RUN ls -l /app

# Étape 2 : Utiliser une image de Nginx pour héberger l'application construite
FROM nginx:alpine

# Copier l'application Angular construite dans le répertoire de Nginx
COPY --from=build /app/dist/angular-app /usr/share/nginx/html

# Exposer le port 80 pour que Nginx puisse répondre aux requêtes HTTP
EXPOSE 80

# Démarrer Nginx en mode non-démon
CMD ["nginx", "-g", "daemon off;"]
