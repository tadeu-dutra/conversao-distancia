# Distance Conversion

This repository is forked from the original repository "Conversão de Distâncias" on GitHub: https://github.com/KubeDev/conversao-distancia. This challenge is part of the Cloud & DevOps Immersion program proposed by Fabricio Veronez in November 2024.

This project is a simple application that performs the conversion of different distance units. The goal of this repository is to demonstrate the setup of an environment with Docker and the execution of an application in a container.

## Table of Contents

- [Project Description](#project-description)
- [Environment Setup](#environment-setup)
- [Container Creation and Testing](#container-creation-and-testing)
- [How to Use](#how-to-use)
- [Technologies Used](#technologies-used)
- [Contributions](#contributions)
- [License](#license)

## Project Description

The "Distance Conversion" application allows users to convert distances between different units, such as meters, kilometers, miles, etc. The application is built with [insert language/framework, if applicable].

## Environment Setup

1. **Forking the Repository**
   - Fork the original repository: [KubeDev/conversao-distancia](https://github.com/KubeDev/conversao-distancia).

2. **Cloning the Repository**
   - Clone your new repository:
     ```bash
     git clone https://github.com/your-username/conversao-distancia.git
     cd conversao-distancia
     ```

3. **Creating the Dockerfile**
   - Create a file named `Dockerfile` in the root of the project with the following content:
     ```dockerfile
     # Use an appropriate base image
     FROM python:3.13.0

     # Set the working directory
     WORKDIR /app

     # Copy requirements.txt to working directory
     COPY requirements.txt .

     # Install dependencies
     RUN pip install -r requirements.txt
     
     # Copy all files to /app
     COPY . /app/

     # Expose port 5000
     EXPOSE 5000

     # Command to run the application
     CMD [ "gunicorn", "--bind", "0.0.0.0:5000", "app:app" ]
     ```

## Container Creation and Testing

1. **Building the Docker Image**
   - In the terminal, run the following command to build the image:
     ```bash
     docker build -t conversao-distancia .
     ```

2. **Running the Container**
   - Run the container from the created image:
     ```bash
     docker run -p 8080:5000 conversao-distancia
     ```

3. **Testing the Application**
   - Open your browser and go to `http://localhost:8080` to ensure that the application is running correctly. Perform basic tests for metric conversion.

## How to Use

After following the above steps, the application will be running, and you can interact with it through the web interface. Enter the values and desired units to perform the conversions.

## Uploading to Docker Hub

To upload your Docker image to Docker Hub, follow these steps:

1. **Create a Docker Hub Account**
   - If you don’t already have a Docker Hub account, sign up at [Docker Hub](https://hub.docker.com/).

2. **Log in to Docker Hub**
   - In your terminal, log in to your Docker Hub account:
     ```bash
     docker login
     ```

3. **Tag and Push the Docker Image**
   - Before pushing the image, tag it with your Docker Hub username and repository name.
   - Is a best practice to use an specific version instead of only 'latest' in order to avoid break changes in the future. So it is advisable to have both uploaded to the Docker Registry, the current version as well as the latest. 
     ```bash
     docker push your-username/conversao-distancia:v1
     docker tag your-username/conversao-distancia:v1 tadeu/conversao-distancia:latest
     docker push your-username/conversao-distancia:latest
     ```

4. **Verify the Upload**
   - Go to your Docker Hub account and check if the image appears in your repositories.


## Deploying to a k3d Kubernetes Cluster

To deploy the application to a k3d Kubernetes cluster, follow these steps:

1. **Install k3d**
   - If you haven't installed k3d yet, refer to the [k3d documentation](https://k3d.io/) for installation instructions.

2. **Create a k3d Cluster**
   - Create a new k3d cluster:
     ```bash
     k3d cluster create mycluster
     ```

3. **Apply the Deployment and Service Manifest**
   - Create a file named `deployment.yaml` in a folder called `k8s` and paste the following manifest into it:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: conversao-distancia
     spec:
       replicas: 6
       selector:
         matchLabels:
           app: conversao-distancia
       template:
         metadata:
           labels:
             app: conversao-distancia
         spec:
           containers:
           - name: conversao-distancia
             image: tadeuaugusto/conversao-distancia
             ports:
             - containerPort: 5000
     ---
     apiVersion: v1
     kind: Service
     metadata:
       name: conversao-distancia
     spec:
       selector:
         app: conversao-distancia
       ports:
       - port: 80
         targetPort: 5000
         nodePort: 30000
       type: NodePort
     ```

4. **Deploy the Application**
   - Use `kubectl` to apply the manifest file:
     ```bash
     kubectl apply -f k8s/deployment.yaml
     ```

5. **Access the Application**
   - After deploying, you can access the application using the node port. Open your browser and go to:
     ```
     http://localhost:30000
     ```

## Contributions

Contributions are welcome! Feel free to open issues or submit pull requests.

## License

This project is licensed under the [MIT License](LICENSE).
