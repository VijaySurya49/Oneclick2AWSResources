# Oneclick2AWSResources: End to End Pipeline for AWS Resource Creation 
**Oneclick2AWSResources** is a web-based application where users submit details like VPC ID, subnet ID, instance type, PEM key, etc(required parameters for AWS resourcecreation here we took ec2 creation as example). The submission is added to a queue, and after the request is approved by an approver, it moves to an approved queue and generates a JSON response. A Python script running on TeamCity then processes this JSON response, saving the details in a CSV file, which you are using as a database.Within the same Python script, another function reads the CSV file and generates a Terraform variable configuration file, which is then pushed to Bitbucket. Any changes in Bitbucket trigger a code build that automatically creates an EC2 instance. After the instance is successfully created, the instance details, such as the instance name and instance ID, are saved back into the CSV file. Finally, an email is sent to the user with the instance details.

**Data Flow:** The flow starts from the User Interface and goes through Queue → Approver Component → Approved Queue → TeamCity Server → Bitbucket Repository → Code Build System → EC2 Instance Creation → CSV File Update → Email Notification.

**Architecture Diagram:**
![Click2AWSResource](https://github.com/user-attachments/assets/52eeadf6-e839-45bc-97b2-c647843d7025)

**Architecture Diagram Description:**
  
  **1. User Interface (UI):**
    **Input:** Users provide details such as VPC ID, subnet ID, instance type, PEM key, etc.
    **Action:** Submit the request.
  
  **2. Queue:**
    **Purpose:** Store the submitted requests temporarily until they are approved.
    **Flow:** The request moves from the UI to this queue.
  
  **3. Approver Component:**
    **Action:** An approver reviews the requests.
    **Flow:** Upon approval, the request moves to the approved queue.
  
  **4. Approved Queue:**
    **Purpose:** Store the approved requests.
    **Flow:** Approved requests are then picked up by the Python script.
  
  **5. TeamCity Server:**
  
    **Process:** Runs a Python script to handle the approved requests.
      
      **Step 1:** Retrieve the approved requests and save the details in a CSV file (acting as a database).
      
      **Step 2:** Read the CSV file to create a Terraform variable configuration file.
      
      **Step 3:** Push the configuration file to a Bitbucket repository.
  
  **6. Bitbucket Repository:**
    **Action:** Receives the Terraform variable configuration file.
    **Trigger:** Any change in the Bitbucket repository triggers a code build.
  
  **7. Code Build System:**
    **Action:** Executes the build process to create an AWS EC2 instance based on the Terraform configuration.
    **Flow:** Once the EC2 instance is created, the instance details are captured.
  
  **8. EC2 Instance Creation:**
    **Output:** Instance details such as instance name and ID.
    **Flow:** These details are saved back to the CSV file.
  
  **9. CSV File (Database):**
    **Update:** Store instance details after EC2 creation.
  
  **10. Email Notification System:**
    **Action:** Sends an email to the user with the instance details, including instance name and ID.
