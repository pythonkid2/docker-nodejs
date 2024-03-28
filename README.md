# Docker Assignment

## Objective
The objective of this assignment is to demonstrate basic proficiency in working with Docker by containerizing a simple Node.js/Express REST API.

## Assignment Files
The assignment includes two files:
- `main.js`: Simple Node/Express REST API that returns a "Hello, World!" response on port 3000.
- `package.json`: Dependency management file.

## Steps to Complete the Assignment

### Step 1: Download the Assignment Files

```
# Download the files

wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=12gZcH1Jr1xWWRzyH3yMqJ9DV_3rlHsnK' -O assignment_files.zip

# Unzip the files

apt install unzip

unzip assignment_files.zip

# Go into the directory

cd assignment_files

```

### Step 2: Check and Install Node.js

### Check if Node.js is installed

```
node -v
```

### If Node.js is not installed, install it

```
apt install nodejs
apt install npm
```

### Step 3: Install Dependencies

```
npm install
```

### Step 4: Run the Node.js Application

```
node main.js
```

# Step 5: Dockerize the Application
- Create a `Dockerfile` in the same directory as `main.js` and `package.json` with the provided content.

```
# Use the official Node.js image as base
FROM node:latest

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application
COPY . .

# Expose the port the app runs on
EXPOSE 3000

# Command to run the application
CMD ["node", "main.js"]

```
### Build the Docker image:
  
  ```
  docker build -t node-app .
  ```

### Step 6: Run the Docker Container

```
docker run -d -p 3000:3000 node-app
```

### Step 7: Access the API

- Open your web browser and go to http://localhost:3000 in AWS IP:3000
- Alternatively, use curl:

  ```
  curl http://localhost:3000
  ```

## Bonus Task: Docker Compose
- Create a `docker-compose.yml` file with the provided content.
```
version: '3'

services:
  node:
    build: .
    ports:
      - "3000:3000"

```
- Run the following command:

  ```
  docker-compose up -d
  ```

## Author
 # Mathew Joseph
