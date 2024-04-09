# containerized-crm
A toy API project demonstrating how to dockerized the API development.

Here's a step-by-step lab handout to guide your students through enhancing the security of the CRM project with AAA (Authentication, Authorization, and Accounting) and dockerizing the application. This lab is designed to be completed within a 2-hour session.

### Step 1: Setting Up the Environment
1. **Log in to the Shared Droplet**: Connect to the shared DigitalOcean droplet named `brava` using SSH.

2. **On GitHub Create a New Repository**: Create a new repo for the CRM project.
   
3. **Clone the repository on brava**:
   3.1. Configure Git with Your Username and Email:
   In the terminal, type the following commands, replacing Your Name and your_email@example.com with your actual name and email address you use for Git:
  ```
  git config --global user.name "Your Name"
  git config --global user.email "your_email@example.com"
  ```
  These commands set your name and email address globally for all repositories you work with on your computer. If you want to set the username and email address for just the current repository, remove the --global flag.
  3.2. Verify Configuration:
   To ensure the configuration was successful, you can check the configuration using the following commands:
   ```
   git config user.name
   git config user.email
   ```
   After configuring your Git username and email, you should be able to make commits.
  3.3. Clone the repository to your working directory on the brava machine.
  

5. **Create a New Node.js Project**: Initialize a new Node.js project with `npm`.
   ```
   npm init -y
   ```

### Step 2: Setting Up the CRM Application
1. **Install Dependencies**: Install the necessary dependencies for the CRM project.
   ```
   npm install express mongoose 
   ```

2. **Set Up Project Files**: Create the project files (`crmModel.js`, `crmController.js`, `crmRoutes.js`, `index.js`) based on the provided code.

3. **Add Security Features**:
   - Implement basic authentication using middleware in `index.js`.
   - Use environment variables to store sensitive information like database credentials.

### Step 3: Dockerizing the Application
1. **Create a `Dockerfile`**: Create a `Dockerfile` in the project root with the following content:
   ```Dockerfile
   FROM node:14
   WORKDIR /usr/src/app
   COPY package*.json ./
   RUN npm install
   COPY . .
   EXPOSE 3000
   CMD ["node", "index.js"]
   ```

2. **Introduce Docker Compose**:
   - Explain the concept of Docker Compose and its advantages.
   - Create a `docker-compose.yml` file with the following content:
     ```yaml
     version: '3'
     services:
       crm_app:
         build: .
         ports:
           - "3000:3000"
         environment:
           - DB_URL=mongodb://mongo:27017/CRMdb
         depends_on:
           - mongo
       mongo:
         image: mongo
         ports:
           - "27017:27017"
     ```

3. **Build and Run the Docker Containers**:
   ```
   docker-compose up --build
   ```

### Step 4: Testing the Application
1. **Access the Application**: Access the CRM application in a web browser at `http://brava:3000`.
2. **Test the API Endpoints**: Use a tool like Postman to test the CRUD operations on the contacts.

### Step 5: Pushing Changes to GitHub
1. **Add Files to Git**:
   ```
   git add .
   git commit -m "Initial CRM project setup with security and Docker"
   ```

2. **Push to GitHub**: Push your changes to a GitHub repository for version control and collaboration.

### Conclusion
By the end of this lab, students will have a basic understanding of implementing AAA security in a Node.js application and dockerizing the application for deployment. They will also gain experience with version control using Git and GitHub.
