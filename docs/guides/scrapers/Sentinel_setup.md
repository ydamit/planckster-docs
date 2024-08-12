---
sidebar_label: Local Sentinel Setup 
sidebar_position: 2
---

# Sentinel Scraper Setup Guide

## Description
This guide details how to set up a local instance of the Sentinel Image Scraper, which uses the SentinelHub API to scrape images for a given location and date.

## Prerequisites
 ### 1. Setting Up Docker

Before using Docker, you need to have it installed on your system. Follow these steps to ensure Docker is installed:

### For Windows and Mac:

1. **Download Docker Desktop**:
   - Visit the [Docker Desktop download page](https://www.docker.com/products/docker-desktop) and download the installer for your operating system.

2. **Install Docker Desktop**:
   - Run the downloaded installer and follow the on-screen instructions to complete the installation.

3. **Start Docker Desktop**:
   - After installation, start Docker Desktop from your applications menu. Docker will start running in the background.

4. **Verify Installation**:
   - Open a terminal or command prompt and run the following command to check if Docker is installed correctly:
     ```bash
     docker --version
     ```
   - You should see the Docker version information if it is installed correctly.

### For Linux:

1. **Update Your Package Index**:
   - Open a terminal and run:
     ```bash
     sudo apt-get update
     ```

2. **Install Required Packages**:
   - Run:
     ```bash
     sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
     ```

3. **Add Docker’s Official GPG Key**:
   - Run:
     ```bash
     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
     ```

4. **Add Docker Repository**:
   - Run:
     ```bash
     sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
     ```

5. **Install Docker Engine**:
   - Run:
     ```bash
     sudo apt-get update
     sudo apt-get install docker-ce
     ```

6. **Start Docker**:
   - Run:
     ```bash
     sudo systemctl start docker
     ```

7. **Verify Installation**:
   - Run:
     ```bash
     docker --version
     ```
   - You should see the Docker version information if it is installed correctly.

By following these steps, you will ensure that Docker is installed and running on your system.
**Ensure docker is installed and running succesfully before proceeding with the next steps**

### 2. Ensure you have a running instance of Kernel-Planckster, see [Guides](https://dream-aim-deliver.github.io/planckster-docs/docs/category/guides) -> [Kernel-planckster](https://dream-aim-deliver.github.io/planckster-docs/docs/category/kernel-planckster) for help.

### 3. Obtain a client ID and client secret from [Sentinel Hub](https://www.sentinel-hub.com/).

**Optional:** Create a Virtual environment to avoid any package version conflicts
```bash
python3 -m venv .venv
source .venv/bin/activate
```
- `python3 -m venv`: This command uses the venv module in Python to create a virtual environment.
- `.venv`: This is the name of the directory where the virtual environment will be created. You can name it anything, but .venv is a common convention.

## Setup Instructions

### 1. Clone the Repository
```bash
git clone https://github.com/dream-aim-deliver/mpi-sda-sentinel.git
cd mpi-sda-sentinel
```
- `git clone https://github.com/dream-aim-deliver/mpi-sda-sentinel.git`:
    - `git clone`: This command is used to create a local copy of a remote repository.
    
    - https://github.com/dream-aim-deliver/mpi-sda-sentinel.git: This is the URL of the remote Git repository you want to clone. It points to the repository hosted on GitHub.
    
    - When you run this command, Git downloads the entire repository (including its history and all files) to the directory on your local machine from which you ran the clone command. The new directory will have the same name as the repository (mpi-sda-sentinel).

- `cd mpi-sda-sentinel`:
    - `cd`: This command is used to change the current directory in your terminal or command prompt.
    - mpi-sda-sentinel: This should be the directory name you want to navigate into. Verify the correct directory name after cloning.

In summary, these commands clone a GitHub repository to your local machine and then change into the directory of the cloned repository.

### 2. Prepare Environment variables
Copy the `env.template` file to create a `.env` file:
```bash
cp .env.template .env
```
Fill in the environment variables in the `.env` file:
```bash
- sh_client_id = {ENTER THE CLIENT ID FROM Sentinel Hub}
- sh_client_secret = {ENTER CLIENT SECRET FROM Sentinel Hub}
- HOST = {THE HOSTNAME OF THE FASTAPI APP}
- PORT = {THE PORT OF THE FASTAPI APP}
```

### 3. Run the Docker Container
To build and run the Docker Container, execute the following script:
```bash
./run.sh
```
**troubleshoot common error:** The `run.sh` file has the following configurations
```bash
docker run --name mpi-satellite \
    --rm \
    -e "HOST=0.0.0.0" \
    -e "PORT=8000" \
    -e "sh_client_id= CLIENT_ID" \
    -e "sh_client_secret= CLIENT_SECRET" \
    -p "8000:8000" \
    mpi-satellite
```
change the `PORT` to any other available port(e.g. 8001) and the `-p` flag correspondingly ("8001:8001") , if any port conflict occurs.

### 4. Run the Demo
To test the setup , you can run the `demo.sh` file, ensure all the below fields are filled correctly before running the script-
- Open the `demo.sh` file:

```bash
python sentinel_scraper.py --start_date=2023-8-8 --end_date=2023-8-30 \
 --long_left=-156.708984 --lat_up=20.759645 --long_right=-156.299744 --lat_down=20.955027 --log-level="INFO" \
 --kp_auth_token test123 --kp_host localhost --kp_port 8000 --kp_scheme http \
 --sentinel_client_id YOUR CLIENT-ID --sentinel_client_secret YOUR CLIENT SECRET  \
```
Now run the file using:
```bash
./demo.sh
```

If the above script runs successfully this means your local environment for the Sentinel Image Scraper is set up and running.