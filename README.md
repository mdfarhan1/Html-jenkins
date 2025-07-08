# Html-Jenkins 🚀

This project demonstrates a simple HTML website deployment using **Docker** and **Jenkins CI/CD pipeline**.

## 🌐 Project Overview

This repo contains a single HTML file that is styled with embedded CSS. When the Jenkins pipeline is triggered, it:

1. Clones this repository
2. Builds a Docker image using `nginx:alpine`
3. Copies the HTML file into the Docker container
4. Runs the container and exposes it on port **8082**

## 🧾 File Structure

Html-jenkins/
├── simple.html # The main HTML file


---

## ⚙️ Jenkins Pipeline Script

Here's a summary of the Jenkins pipeline used to deploy this project:

```groovy
pipeline {
    agent any

    triggers {
        githubPush() // Optional: auto-trigger on push (requires GitHub webhook)
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/mdfarhan1/Html-jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    writeFile file: 'Dockerfile', text: '''
                    FROM nginx:alpine
                    COPY simple.html /usr/share/nginx/html/index.html
                    '''
                    sh 'docker build -t farhan-html .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker rm -f html-container || true'
                    sh 'docker run -d -p 8082:80 --name html-container farhan-html'
                }
            }
        }
    }
}
```
🔗 Access the Web Page
After the pipeline completes, you can access the running webpage at:
http://--your-server-ip--:8082 OR if running locally: http://localhost:8082

## 📬 Contributing

Feel free to fork this repo and make it your own. Pull requests are welcome!

## 📄 License

This project is licensed under the MIT License.
