SHOPNOW DEVOPS PROJECT – CI/CD DEPLOYMENT DOCUMENTATION
1. Introduction
ShopNow is a web-based e-commerce application deployed using DevOps practices. The project demonstrates how a multi-component application can be containerized using Docker and deployed to an AWS EC2 cloud server.
The application consists of a frontend, admin panel, backend API, and MongoDB database. Docker Compose is used to manage the different application containers.
GitHub is used to store and manage the project source code, while GitHub Actions is used to automate the deployment process. When changes are pushed to the main branch, GitHub Actions connects to the EC2 server through SSH, pulls the latest source code, builds the Docker images, and deploys the updated application.
During the deployment process, several real-world issues were identified and resolved, including EC2 memory limitations, SSH connectivity problems, and incorrect frontend API configuration.
________________________________________
2. Objectives
The main objectives of this project are:
•	To understand the basic DevOps workflow. 
•	To use Git for version control. 
•	To host the project source code on GitHub. 
•	To containerize the application using Docker. 
•	To manage multiple containers using Docker Compose. 
•	To deploy the application on an AWS EC2 instance. 
•	To use Ubuntu as the application server. 
•	To use MongoDB as the application database. 
•	To configure communication between frontend and backend. 
•	To implement automated deployment using GitHub Actions. 
•	To understand SSH-based deployment. 
•	To troubleshoot deployment and server issues. 
•	To successfully deploy the updated application. 
________________________________________
3. Technologies Used
Technology	Purpose
Git	Version control
GitHub	Source code management
GitHub Actions	CI/CD automation
AWS EC2	Cloud hosting
Ubuntu	Server operating system
SSH	Remote server access
Docker	Containerization
Docker Compose	Managing multiple containers
Node.js	Application runtime/build environment
React	Frontend
MongoDB	Database
________________________________________
4. Application Components
The ShopNow application contains four main components.
4.1 Frontend
The frontend is the customer-facing part of the application. It provides the interface through which users can browse and view products.
4.2 Admin
The admin application is used for administrative functionality of the ShopNow application.
4.3 Backend
The backend provides the API required by the frontend and communicates with the MongoDB database.
4.4 MongoDB
MongoDB is used to store the application's data, including product information.
The application therefore follows this basic structure:
Frontend
   |
   ↓
Backend API
   |
   ↓
MongoDB
________________________________________
📸 SCREENSHOT 1 – PROJECT STRUCTURE
<img width="890" height="556" alt="image" src="https://github.com/user-attachments/assets/3438886b-7279-4428-ad56-90408123f17f" />

 

________________________________________
5. Git and GitHub
Git was used to manage changes to the ShopNow source code.
The project was maintained in a GitHub repository. Changes were made locally and then committed and pushed to the main branch.
Some of the Git commands used during the project were:
git status
git add .
git commit -m "message"
git pull origin main
git push origin main
The git status command was used to check the current state of the working directory.
When changes were ready, they were committed and pushed to GitHub.
For example:
git push origin main
The successful push confirmed that the latest local commit had been uploaded to GitHub.
________________________________________
📸 SCREENSHOT 2 – GIT PUSH
<img width="886" height="554" alt="image" src="https://github.com/user-attachments/assets/b1f34376-349a-447f-8350-93b392304c96" />

main -> main

 

________________________________________
6. GitHub Repository
The ShopNow project source code was stored in a GitHub repository.
The repository contains the application source code along with the Docker configuration and GitHub Actions workflow used for deployment.
The GitHub repository acts as the central location from which the deployment process obtains the latest version of the application.
________________________________________
📸 SCREENSHOT 3 – GITHUB REPOSITORY
<img width="936" height="585" alt="image" src="https://github.com/user-attachments/assets/e57d70d3-3d71-40ff-98c2-30ee331387a9" />

 
________________________________________
7. AWS EC2 Deployment Server
AWS EC2 was used as the cloud server for hosting the ShopNow application.
An Ubuntu-based EC2 instance was configured as the deployment server.
The application containers run on this EC2 instance, allowing users to access the deployed ShopNow application over the internet.
The EC2 server was accessed remotely using SSH.
________________________________________
8. SSH Connection
SSH was used to securely connect to the Ubuntu EC2 server from the local Windows computer.
The connection was made using the EC2 private key.
The general SSH command used was:
ssh -i "two-website-key.pem" ubuntu@<EC2-PUBLIC-IP>
After successful authentication, the server provided an Ubuntu shell such as:
ubuntu@ip-172-31-43-63:~$
SSH was also important for the CI/CD pipeline because GitHub Actions uses SSH to execute deployment commands on the EC2 server.
________________________________________
📸 SCREENSHOT 5 – SUCCESSFUL SSH

 <img width="900" height="563" alt="image" src="https://github.com/user-attachments/assets/4597d290-126e-4a8c-b4d4-c2fca5d22f76" />


________________________________________
9. Docker Containerization
Docker was used to containerize the ShopNow application.
Instead of running every component directly on the Ubuntu operating system, the application components were packaged and executed as Docker containers.
The deployment consists of containers for:
•	Frontend 
•	Admin 
•	Backend 
•	MongoDB 
Each component runs in its own container.
This provides isolation between application services and makes the deployment easier to manage.
________________________________________
10. Docker Compose
Docker Compose was used to manage the multiple containers required by the ShopNow application.
Docker Compose allows the frontend, admin, backend, and MongoDB services to be defined and managed together.
The application can be built and started using:
docker compose up -d --build
The running containers can be checked using:
docker ps
The final deployment showed four application containers running.
Frontend
Admin
Backend
MongoDB
________________________________________
📸 SCREENSHOT 6 – DOCKER PS
 <img width="888" height="555" alt="image" src="https://github.com/user-attachments/assets/f9764fe7-4bea-4f57-a0d7-822ac044759d" />


________________________________________
11. MongoDB Database
MongoDB was used as the database for the ShopNow application.
The MongoDB container stores the product information required by the backend.
During troubleshooting, the MongoDB container was checked through its logs.
The MongoDB logs showed normal WiredTiger checkpoint activity, indicating that the MongoDB service was running.
The product data was successfully retrieved from the backend after the API configuration was corrected.
________________________________________
📸 SCREENSHOT 7 – MONGODB CONTAINER
 <img width="898" height="561" alt="image" src="https://github.com/user-attachments/assets/f5f3829d-6cda-4dd5-8447-fda815d50542" />

________________________________________
12. Backend API Verification
The backend API was tested directly from the EC2 server.
The health endpoint was checked using:
curl http://localhost:5000/api/health
The response was:
{
  "status": "OK",
  "message": "ShopNow API is running"
}
This confirmed that the backend service was running correctly.
________________________________________
📸 SCREENSHOT 8 – BACKEND HEALTH CHECK
 <img width="898" height="561" alt="image" src="https://github.com/user-attachments/assets/17847bac-836c-495d-8f48-9ee7c034cc43" />


________________________________________
13. Product API Testing
Initially, the following request was tested:
curl http://localhost:5000/api/products
It returned:
{
  "error": "Route not found"
}
Instead of assuming that the backend was broken, the backend source code was inspected.
The following command was used:
grep -R "router.*product\|products" backend --exclude-dir=node_modules
The source code showed that the product listing endpoint was:
/products
Therefore, the correct endpoint was tested:
curl http://localhost:5000/products
This successfully returned the product data.
The response contained six products, confirming that:
•	Backend was running. 
•	MongoDB was running. 
•	Product data existed. 
•	The product API was working. 
________________________________________
14. Frontend API Configuration
After deployment, the frontend initially displayed:
"No products found matching your criteria."
The backend was working correctly, so the frontend-backend communication was investigated.
The frontend source code was checked using:
grep -R "api/products\|/products" frontend/src --exclude-dir=node_modules
The result showed:
frontend/src/App.js: const response = await fetch(`${API_BASE_URL}/products`);
The API base URL was then checked using:
grep -n "API_BASE_URL" frontend/src/App.js
The configuration contained:
const API_BASE_URL =
  process.env.REACT_APP_API_BASE_URL || 'http://localhost:5000';
The problem was that localhost refers to the machine/container making the request, not the EC2 backend from the user's browser.
The frontend API configuration was corrected so that the deployed frontend could communicate with the backend running on the EC2 server.

________________________________________
15. GitHub Actions CI/CD
GitHub Actions was used to automate the deployment of the ShopNow application.
The workflow is stored inside the GitHub repository under:
.github/workflows/
The purpose of the workflow is to automatically deploy the latest code to the EC2 server after changes are pushed to GitHub.
The basic process is:
Code Change
     ↓
Git Commit
     ↓
Git Push
     ↓
GitHub
     ↓
GitHub Actions
     ↓
SSH to EC2
     ↓
Git Pull
     ↓
Docker Build
     ↓
Docker Compose
     ↓
Updated Application
________________________________________
16. GitHub Actions Deployment Process
The GitHub Actions workflow uses the appleboy/ssh-action to connect to the EC2 server.
During one of the deployments, the workflow successfully connected to the EC2 server and performed a Git update.
The deployment log showed:
From github.com:sivashankarisuperior579/shopnow-devOps-project

7ae9d12..cbe2817 main -> origin/main

Updating 7ae9d12..cbe2817
Fast-forward
This confirms that the EC2 server successfully obtained the latest commit from GitHub.
The deployment then started rebuilding the Docker images.
________________________________________
17. Docker Image Build During CI/CD
After the latest code was pulled, Docker Compose started building the application images.
The deployment log showed Docker building:
backend
frontend
admin
The backend image was successfully created:
shopnow-devops-project-backend:latest
The admin image was also successfully created:
shopnow-devops-project-admin:latest
The frontend image then proceeded through its production build.
This demonstrates that GitHub Actions was not simply copying files; it was actually rebuilding and deploying the updated application.
________________________________________
📸 SCREENSHOT 12 – DOCKER BUILD LOG
 <img width="813" height="508" alt="image" src="https://github.com/user-attachments/assets/d56f6a8f-7ad1-4bda-bc36-6d93b902d888" />



________________________________________
18. EC2 Memory Issue
During one deployment, the GitHub Actions job took significantly longer than the previous deployment.
The deployment eventually failed during the frontend build.
The important error was:
Out of memory: Killed process
The EC2 instance had approximately:
Memory: 908 MiB
Swap: 0 B
The React production build required more memory than was available.
Therefore, the Linux operating system terminated processes because of insufficient memory.
This was identified as the main reason for the deployment failure.
________________________________________
19. EC2 Serial Console Troubleshooting
Because the EC2 instance was temporarily difficult to access through SSH, the EC2 Serial Console was used to inspect the server.
The console showed that the SSH service had started successfully:
Started ssh.service - OpenBSD Secure Shell server.
The console also showed the memory-related error:
Out of memory: Killed process
This helped confirm that the server was experiencing memory pressure rather than simply having an SSH configuration problem.
________________________________________
20. Adding Swap Memory
To provide additional virtual memory, a 2 GB swap file was created on the EC2 server.
The swap file was configured using:
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
The configuration was verified using:
free -h
The result showed:
Swap: 2.0Gi
The swap configuration was also checked using:
sudo swapon --show
The output confirmed:
NAME      TYPE SIZE USED PRIO
/swapfile file   2G   0B   -1
This provided the EC2 server with additional virtual memory and helped prevent the frontend build from failing due to memory exhaustion.
________________________________________
21. Rebuilding and Running the Containers
After resolving the memory issue, the application containers were started again using Docker Compose.
The containers were checked using:
docker ps
The required services were running:
Frontend
Admin
Backend
MongoDB
The backend health endpoint was then tested again to confirm that the application services were functioning.
________________________________________
📸 SCREENSHOT 16 – FINAL RUNNING CONTAINERS	
<img width="1920" height="1200" alt="Screenshot 2026-08-28 115455" src="https://github.com/user-attachments/assets/d3eb0102-57e2-4f6d-8dc7-31743439c150" />

 

________________________________________
22. Final CI/CD Deployment
After correcting the frontend API configuration and resolving the EC2 memory issue, the updated code was committed and pushed to GitHub.
The GitHub Actions workflow was triggered again.
The deployment successfully:
1.	Connected to the EC2 server using SSH. 
2.	Pulled the latest code from GitHub. 
3.	Built the Docker images. 
4.	Started the application containers. 
5.	Deployed the updated application. 
The final GitHub Actions run completed successfully.
________________________________________
📸 SCREENSHOT 17 – SUCCESSFUL DEPLOYMENT ⭐
<img width="929" height="580" alt="image" src="https://github.com/user-attachments/assets/774dca25-709c-4562-9c73-8ba2e61d7dcf" />

 

________________________________________
23. Final Application Verification
After the successful deployment, the ShopNow application was opened in the browser.
The frontend successfully communicated with the backend, and the backend retrieved product information from MongoDB.
The products were displayed correctly on the ShopNow website.
This confirmed that the complete deployment pipeline was working.
________________________________________
📸 SCREENSHOT 18 – FINAL SHOPNOW WEBSITE ⭐⭐⭐
<img width="886" height="554" alt="image" src="https://github.com/user-attachments/assets/7e62102a-5980-4c30-9b99-6b3cbdb09139" />

 
________________________________________
24. Complete Project Workflow
The complete workflow implemented in the project can be summarized as follows:
                    DEVELOPER
                        |
                        ↓
                     VS CODE
                        |
                  Modify Code
                        |
                        ↓
                     Git
                        |
                   git commit
                        |
                        ↓
                git push origin main
                        |
                        ↓
                     GitHub
                        |
                        ↓
               GitHub Actions
                        |
                        ↓
                  SSH to EC2
                        |
                        ↓
                 Ubuntu EC2 Server
                        |
                  Git Pull Latest Code
                        |
                        ↓
                 Docker Compose
                        |
          ┌─────────────┼─────────────┐
          ↓             ↓             ↓
      Frontend        Admin        Backend
                                      |
                                      ↓
                                   MongoDB
                                      |
                                      ↓
                                Product Data
                                      |
                                      ↓
                              ShopNow Website
________________________________________
25. Problems Encountered and Solutions
Problem	Solution
SSH connection was stuck during banner exchange	EC2 server was investigated using the Serial Console and later successfully reconnected
EC2 deployment failed with out-of-memory error	Created a 2 GB swap file
/api/products returned Route not found	Inspected server.js and identified /products as the correct endpoint
Website showed "No products found"	Checked frontend API configuration and corrected the backend URL
Docker containers were not initially running	Restarted the Docker Compose application
Frontend build took longer than previous deployment	Identified memory limitations during React production build
CI/CD deployment initially failed	Resolved memory and API configuration issues and ran deployment again
________________________________________
26. Key DevOps Concepts Learned
Through this project, the following concepts were practically implemented:
Version Control
Git was used to track and manage source-code changes.
Source Code Management
GitHub was used as the central repository for the application.
Containerization
Docker was used to package application components into containers.
Container Management
Docker Compose was used to manage multiple application containers.
Cloud Deployment
AWS EC2 was used to host the application.
Remote Administration
SSH was used to access and manage the Ubuntu server.
CI/CD
GitHub Actions was used to automate the deployment process.
Troubleshooting
Real deployment problems involving memory, SSH connectivity, API routes, and frontend-backend communication were investigated and resolved.
________________________________________
27. Conclusion
The ShopNow project successfully demonstrates a practical DevOps deployment workflow.
The application was containerized using Docker and Docker Compose and deployed on an AWS EC2 Ubuntu server. Git and GitHub were used for source-code management, while GitHub Actions was used to automate the deployment process.
The CI/CD pipeline automatically connects to the EC2 server using SSH, retrieves the latest source code, builds the Docker images, and deploys the application.
During the project, several real-world deployment problems were encountered. An EC2 memory limitation caused the React production build to be terminated, which was resolved by adding 2 GB of swap memory. A frontend API configuration issue was also identified and corrected after testing the backend API directly.
After troubleshooting and corrections, the application was successfully deployed and the product data was displayed correctly.
This project provided practical experience with Git, GitHub, GitHub Actions, Docker, Docker Compose, AWS EC2, Ubuntu, SSH, Node.js, React, MongoDB, CI/CD, and real-world DevOps troubleshooting.


