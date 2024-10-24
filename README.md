# azure-documentation

##How to spin up a Azure Virtual Machine with a Java SpringBoot backend and a React frontend with hibernate.
## Prerequisites
- You’ve set up a VM in Azure.
- You’ve configured the network and opened the necessary ports.
- You’re able to SSH into your VM using the command (adjust for your project):
  ```bash
  ssh -i azure-tf-server azureadmin@<VM_IP_ADDRESS>
  ```

Note your working directory in the VM (e.g., /home/azureadmin).
Ensure Java is installed on the VM:

```bash
java --version
```

If Java is not installed, install it.
Setting Up the Spring Boot Backend
Verify Spring Boot Application:
Ensure that your Java Spring Boot application is functional using a tool like Insomnia or a React frontend with all required API requests. Don’t forget to configure CORS in the controllers.

Build the Spring Boot Application:
Navigate to your Spring Boot project and run the following command to build your project:

```bash
./gradlew build
```
This should generate a .jar file of your application.
Transfer the JAR File to the VM:
Use scp to securely copy the JAR file to your VM:

```bash
scp -i azure-tf-server <path_to_jarfile.jar> azureadmin@<VM_IP_ADDRESS>:/home/azureadmin
```
Once the file is transferred, SSH into your VM:

```bash
ssh -i azure-tf-server azureadmin@<VM_IP_ADDRESS>
```
Run the JAR File:
Navigate to the folder where the JAR file was copied (use ls to find it) and run it:

```bash
java -jar jarfile.jar
```
Optionally, you can run it in the background:

```bash
java -jar jarfile.jar &
```
Test the Endpoints:
Test your API endpoints (e.g., using Postman or a frontend). You should get valid responses, and the backend should be running without interruption.

##Setting Up the React Frontend

Verify Backend Availability:
Ensure that your Spring Boot backend is up and running. Use java -jar jarfile.jar & to keep it running in the background.

Configure React Project:
Ensure your React project points to the correct backend endpoint (adjust base URL as needed):
ex: const baseUrl = "http://<YOUR_VM_IP_ADDRESS>:5000";

Test ir locally first:
Run the React project locally to verify everything works as expected:

```bash
npm run dev
```

Your React app should now be accessible at the provided localhost and 100% functional with the bakcend in the VM.

Build React Application:
Once everything looks correct, build the production version of the React app:

```bash
npm run build
```
Transfer Build to VM:
Securely copy the entire dist folder (generated after the build) to your VM using recursive scp:

```bash
scp -r -i azure-tf-server <path_to_dist_folder> azureadmin@<VM_IP_ADDRESS>:/home/azureadmin
```

SSH into the VM:
SSH back into your VM and ensure that the dist folder has been transferred using ls.

Navigate to dist Folder:
Go to the dist folder where your React build files are located:

```bash
cd dist
```
Install Node.js and npm:
Inside the dist folder, install node and npm (if they aren’t installed already):
Verify the installations:

```bash
node --version
npm --version
````

Install serve:
Install serve globally to serve your React app:

```bash
sudo npm install -g serve
```
Verify the isntallation.
Serve the React static files using the installed serve:

Explaination of sudo serve -s . -p 80 &
Following these steps should result in the backend listening on port 80 by default. This means that by running the following command, you will serve a static file (React build), with all files indicated by the "." on the specified port 80 wich connects the frontend to the backend:
Now run this:

```bash
sudo serve -s . -p 80
````
Optionally, run it in the background:

```bash
sudo serve -s . -p 80 &
```

Verify React Frontend:
Go to your public VM IP address in a browser (e.g., http://<YOUR_VM_IP_ADDRESS>) and verify that the React app is being served.



