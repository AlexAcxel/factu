FROM node:22-alpine

WORKDIR /app

# Copie des fichiers de dépendances
COPY package.json package-lock.json* ./

# Installation des dépendances
RUN npm install

# Copie des sources
COPY . .

EXPOSE 5173

# Démarrage du serveur de dev Vite (accessible depuis l'extérieur du container)
CMD ["npm", "run", "dev"]
