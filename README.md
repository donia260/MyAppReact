#TP4 :

# 1ere etape :

j'ai cloner le projet dans le powershell avec cette commande : git clone https://github.com/wiem-kb/MyAppReact.git

# 2eme etape dans le server :

j'ai créer un docker file et j'ai mis ce code :FROM node:lts-alpine
WORKDIR /app
COPY package\*.json ./
RUN npm install
COPY . .
ENV MONGO_URI=mongodb://mongodb:27017/mern
EXPOSE 9000
CMD ["npm", "start"]
et aussi un fichier `.dockerignore` exclut `node_modules` pour éviter de copier dans l'image les modules installés sous Windows.

j'ai creer l'image avec : docker build -t mern-server ./Server

# 3eme etape dansla partie client  :
j'ai cree un fichier '.dockerignore'  et un fichier 'dockerfile'
apres j'ai créer une image pour le client nommé mern-client qui est egalement affichée dans le docker .

# reseau docker:
j'ai creer le reseau  et voici le resultat : PS C:\Users\Oussama OUESLATI\MyAppReact\Client> docker network create mern-network
c340bd6c20966c623c66d410fadf990137bc036d52c697205e0e735b9eab8f14

apres j'ai lancer un conteneur appelée 'MongoDB' avec l'instruction 'docker run -d --name mongodb --network mern-network mongo'
l'étape docker run de MongoDB a rencontré une erreur de stockage Docker (système de fichiers en lecture seule), résolue en libérant de l'espace disque, puis que le déploiement a été fait via Docker Compose.
# docker compose:
j'ai créer un fichier 'docker-compose.yml'
apres j'ai lancé le la commande docker compose up --build
apres j'ai vérifier par l'instruction ' docker ps' que les conteneurs `client`, `server` et `mongodb` sont `Up`
j'ai vérifié aussi 'http://localhost:3000' et 'http://localhost:9000/api/health' sont fonctionnel .*
# partie git
j'ai créer un nouveau repository nommé MyAppReact 
j'ai relié le projet avec mon depot crée et j'ai le poussé 
# jenkins 
j'ai créer un jenkinsfile qui contient :
- Checkout : récupération du code depuis GitHub.
- Build images : 'docker compose build' construit les images du serveur et du client.
- Run: 'docker compose down' puis 'docker compose up -d' lancent les 3 conteneurs.
- Test: 'curl -f' vérifie que le serveur ('http://localhost:9000/api/health') et le client ('http://localhost:3000') répondent.

 puis je l'ai pousser dans le git avec cette commande :
git add Jenkinsfile
git commit -m "Ajout du Jenkinsfile"
git push







