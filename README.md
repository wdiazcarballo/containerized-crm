# containerized-crm
A toy API project demonstrating how to dockerized the API development.

Here's a step-by-step lab handout to guide your students through enhancing the security of the CRM project with AAA (Authentication, Authorization, and Accounting) and dockerizing the application. This lab is designed to be completed within a 2-hour session.

### Step 1: Setting Up the Environment
1. **Log in to the Shared Droplet**: Connect to the shared DigitalOcean droplet named `brava` using SSH.

2. **On GitHub Create a New Repository**: Create a new repo for the CRM project named **<your username>-containerized-crm**.
   
4. **Clone the repository on brava**:
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
   npm install express mongoose bcrypt jsonwebtoken nodemon
   ```

2. **Set Up Project Files**:
   
   2.1 Create the following files and directory structure
   ### You have to add all files and folder shown in the double asterisks (**).
   
         <your username>-containerized-crm
            .
            ├── **Dockerfile**
            ├── LICENSE
            ├── README.md
            ├── **docker-compose-student-<your number>.yml**
            ├── **index.js**
            ├── node_modules
            ├── package-lock.json
            ├── package.json
            ├── public
            │  └── .
            └── **src**
               ├── **controllers**
               │   ├── **crmController.js**
               │   └── **userController.js**
               ├── **models**
               │   ├── **crmModel.js**
               │   └── **userModels.js**
               └── **routes**
                   ├── **crmRoutes.js**
                   └── **userRoutes.js**
   2.2 Copy the source code from this repository to the corresponding files.

### Step 3: Dockerizing the Application
1. **Create a `Dockerfile`**: Create a `Dockerfile` in the project root with the following content:
   ```Dockerfile
   FROM node:latest
   WORKDIR /app
   COPY package*.json ./
   RUN npm install
   COPY . .
   EXPOSE 3000
   CMD ["node", "index.js"]
   ```

2. **Create a `docker-compose-student-<your number>.yml`**:
   - Replace the placeholder <your number> with the number assigned to you in the class, e.g. if you are number 33 the file name will be **`docker-compose-student-33.yml`**.
   - Create a docker-compose file  with the following content:
     ```yaml
      version: '3.8'
      services:
        api-student-<your number>:
          build: .
          ports:
            - "<your port number>:3000"
          environment:
            DB_URL: mongodb://mongo-student-<your number>:27017/CRMdb-student-<your number>
            PORT: 3000
          depends_on:
            - mongo-student-<your number>
        mongo-student-<your number>:
          image: mongo:latest
          ports:
            - "<your assigned db port>:27017"
          volumes:
            - db-data-student-<your number>:/data/db
      volumes:
        db-data-student-<your number>:
     ```

4. **Build and Run the Docker Containers**:
   ```
   docker-compose -f docker-compose-student-<student_number>.yml up --build
   ```
5. **Clean up after testing**:
   ```
   docker-compose -f docker-compose-student-<student_number>.yml down --rmi all
   ```

### Step 4: Testing the Application
1. **Access the Application**: Access the CRM application in a web browser at `http://152.42.186.38:<your assign server port>`.
2. **Test the API Endpoints**: Use a tool like Postman to test the CRUD operations on the contacts.

### Step 5: Pushing Changes to GitHub
1. **Add Files to Git**:
   ```
   git add .
   git commit -m "Initial CRM project setup with security and Docker"
   ```

2. **Push to GitHub**: Push your changes to a GitHub repository for version control and collaboration.
   You'll need to use a personal access token (PAT) for HTTPS Git operations to allow pushing code to GitHub. Here's a step-by-step guide to help you set this up:

   2.1. **Generate a Personal Access Token (PAT) on GitHub:**
      - Go to your GitHub account settings.
      - Click on "Developer settings" in the left sidebar.
      - Select "Personal access tokens" and then "Generate new token".
      - Give your token a descriptive name, select the appropriate scopes (at least `repo` for full control of private repositories), and click "Generate token".
      - **Important:** Copy the token and save it somewhere secure. You won't be able to see it again after you navigate away from the page.

   2.2 **Configure Git to Use the Personal Access Token:**
      - Open a terminal on your Brava droplet.
      - Navigate to your repository directory (e.g., `cd wdc-containerized-crm`).
      - Use the following command to configure Git to use your PAT for authentication:
        ```bash
        git config credential.helper store
        ```
      - The next time you perform a Git operation that requires authentication (e.g., `git push`), you'll be prompted for your username and password. Enter your GitHub username and use the PAT as the password. Git will store these credentials, so you won't have to enter them again for future operations.

   2.3. **Push Your Changes:**
      - After setting up the PAT, try pushing your changes again with `git push`.
      - You should no longer encounter the authentication error.

   2.4. **Best Practices for Git and GitHub Workflow:**
      - **Commit Often:** Make frequent, small commits to keep a detailed history of your project.
      - **Use Branches:** Create branches for new features or bug fixes to keep your main branch stable.
      - **Write Meaningful Commit Messages:** Clearly describe the changes in each commit.
      - **Pull Before You Push:** Always do a `git pull` before pushing your changes to avoid conflicts.
      - **Review Changes Before Pushing:** Use `git status` and `git diff` to review your changes before committing and pushing.
   
   By following these steps and best practices, you should be able to resolve your authentication issue and have a smoother workflow with Git and GitHub.

### Conclusion
By the end of this lab, students will have a basic understanding of implementing AAA security in a Node.js application and dockerizing the application for deployment. They will also gain experience with version control using Git and GitHub.
