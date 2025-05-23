
# 🚩 Jenkins Assignment
---

## 📝 1. What You Need Before Starting

* [x] **Docker Desktop** installed ([Download link here](https://docs.docker.com/get-docker/))
* [x] **Docker Hub account** ([Sign up here](https://hub.docker.com/))
* [x] Your code/project folder on your computer

---

## 🪄 2. Make a Simple Dockerfile

Let’s say your project is a Node.js app with a file called `app.js`.

1. Open your project folder.
2. Make a new file called `Dockerfile` (no extension).
3. Copy and paste this into the Dockerfile:

   ```dockerfile
   # Use Node.js official image
   FROM node:18

   # Set working directory
   WORKDIR /usr/src/app

   # Copy package files (if you have them)
   COPY package*.json ./

   # Install dependencies
   RUN npm install

   # Copy all other files
   COPY . .

   # Open port 3000 (or change to your app's port)
   EXPOSE 3000

   # Start the app
   CMD ["node", "app.js"]
   ```

   *(If your app is Python, Java, etc. — tell me and I’ll give you the right Dockerfile!)*

---

## 🏗️ 3. Build Your Docker Image

Open a terminal in your project folder and run:

```sh
docker build -t yourdockerhubusername/myapp:latest .
```

* Change `yourdockerhubusername` to your Docker Hub username.

---

## ▶️ 4. Run Your App in Docker

```sh
docker run -p 3000:3000 yourdockerhubusername/myapp:latest
```

* If your app uses another port, change `3000` to your app’s port.

Open your browser and go to `http://localhost:3000`
Your app should show up!

---

## ☁️ 5. Push Image to Docker Hub

1. Login to Docker Hub:

   ```sh
   docker login
   ```

   * Enter your Docker Hub username and password.

2. Push your image:

   ```sh
   docker push yourdockerhubusername/myapp:latest
   ```

---

## 🎯 6. (Optional) Try Docker-in-Docker

> **This step is just for practice—don’t worry if it doesn’t work first try!**

* Running Docker inside a Docker container is tricky and often fails for beginners.
* Why? Containers don’t have access to Docker by default.

If you want to try, run this (Linux only):

```sh
docker run --privileged -d --name dind-test docker:dind
```

* Or, to share Docker with the host system (less secure, just for learning):

```sh
docker run -it -v /var/run/docker.sock:/var/run/docker.sock docker
```

> *It’s okay if you don’t do this step!*

---

## 🧑‍💻 7. (Bonus) Jenkins Automation Example

If you are using Jenkins (optional), your Jenkinsfile might look like:

```groovy
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t yourdockerhubusername/myapp:latest .'
      }
    }
    stage('Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
          sh 'docker push yourdockerhubusername/myapp:latest'
        }
      }
    }
  }
}
```

---

## 🔗 Helpful Links

* [Official Docker Getting Started Guide](https://docs.docker.com/get-started/)
* [How to push to Docker Hub](https://docs.docker.com/docker-hub/repos/)
* [Docker-in-Docker explained](https://hub.docker.com/_/docker)

---

## ✅ Done!

If you followed all the steps above, you:

* Made a real Dockerfile ✅
* Built your image ✅
* Ran your app inside Docker ✅
* (Maybe) tried Docker-in-Docker ✅
* Pushed your work to Docker Hub ✅

**Great job!** 🎉
If you get stuck, ask for help—this is how everyone starts!
