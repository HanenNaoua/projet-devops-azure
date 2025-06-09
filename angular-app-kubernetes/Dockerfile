# Étape 1 : Utiliser une image de Node.js pour construire l'application
FROM node:10 AS build

# Fixer l'option OPENSSL_legacy_provider pour résoudre l'erreur OpenSSL
#ENV NODE_OPTIONS=--openssl-legacy-provider

# Définir le répertoire de travail
WORKDIR /app

RUN npm install -g @angular/cli@6.0.8
COPY package*.json ./
RUN npm install rxjs@6.2.2 typescript@2.9.2 --save && npm install



# Copier le package.json et package-lock.json pour installer les dépendances
#COPY package.json package-lock.json ./

# Installer les dépendances
#RUN npm install  # Installe les dépendances
#RUN npm install --legacy-peer-deps


# Copier tout le reste du code source dans le conteneur
COPY . .

# Construire l'application Angular pour la production avec plus de détails
#RUN npm run build -- --configuration=production --verbose
RUN ng build --configuration=production --verbose

# Vérification du contenu de /app après le build
RUN ls -l /app

# Vérification du contenu de /app/dist/angular-app
RUN ls -l /app/dist/angular-app
