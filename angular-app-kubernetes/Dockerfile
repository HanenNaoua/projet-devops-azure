# Étape 1 : Utiliser une image de Node.js pour construire l'application
#FROM node:20 AS build
FROM node:16-alpine AS builder


# Fixer l'option OPENSSL_legacy_provider pour résoudre l'erreur OpenSSL
ENV NODE_OPTIONS=--openssl-legacy-provider

# Définir le répertoire de travail
WORKDIR /app

# Copier le package.json et package-lock.json pour installer les dépendances
COPY package.json package-lock.json ./

# Installer TypeScript à une version compatible
# Installer les dépendances en ignorant les peer dependencies
#RUN npm install --legacy-peer-deps
#RUN npm install typescript@5.2.2 --save-dev

RUN rm -rf node_modules

RUN npm install --legacy-peer-deps && npm install typescript@5.2.2 --save-dev

# Vérifier les packages installés (débogage)
RUN npm list --depth=0

# Copier tout le reste du code source dans le conteneur
COPY . .

# Construire l'application Angular pour la production avec plus de détails
#RUN npm run build -- --configuration=production --verbose

RUN npm run build --configuration=production --verbose

# Vérification du contenu de /app après le build
RUN ls -l /app

# Vérification du contenu de /app/dist/angular-app
RUN ls -l /app/dist/angular-app

# Étape 2 : Utiliser une image de Nginx pour héberger l'application construite
FROM nginx:alpine
#FROM nginx:1.25-alpine
#FROM docker.io/library/nginx:alpine



# Copier l'application Angular construite dans le répertoire de Nginx
COPY --from=build /app/dist/angular-app /usr/share/nginx/html

# Exposer le port 80 pour que Nginx puisse répondre aux requêtes HTTP
EXPOSE 80

# Démarrer Nginx en mode non-démon
CMD ["nginx", "-g", "daemon off;"]
