                                              #-#-#-#-#-#-#-#-#- #-#-#-#-#-#-#-#-#- Docker notes #-#-#-#-#-#-#-#-#- #-#-#-#-#-#-#-#-#- #-#-#-#-#-#-#-#-#-

🪞Docker Image
A Docker image is like a blueprint — it has everything your app needs: Code, Libraries, Dependencies ,OS setup

📦 Docker Container
A Docker container is a running instance of an image — like launching the blueprint.
You can run many containers from the same image.

🔁 Analogy:
Image = recipe 📖
Container = dish 🍽️ made from the recipe
                                      
Using in VM or in any linux based EC2 instance. 
You need to first clone the files from git " git clone https://... " from the respective repository.

Then go inside that folder and check all the necessary files " code.py and dockerfile "

 Use VIM to edit docker file. 
💕Sample could be like this:

GNU nano 7.2                    
FROM ubuntu:latest      ##👉 Uses the latest Ubuntu image as the base OS.


# Set working directory   ## 👉 Sets /app as the working directory inside the container (like cd /app).
WORKDIR /app

# Copy local files to the container  ##👉 Copies all files from your local folder to the container's /app folder.
COPY . /app

#Install the necessary packages      ## 👉 Installs Python3 and pip so your Python code can run.
RUN apt-get update && apt-get install -y python3 python3-pip

#Set Environment Variables  ##👉 Sets an environment variable NAME=World (can be used in your Python script).
ENV NAME World

# Set default command to run the script  ##👉 Tells Docker to run calculator.py using Python when the container starts.
CMD ["python3", "calculator.py"]

---------------------------------------------------------------DOCKER IMAGE BUILDING AND RUNNING------------------------------------------------------------------------------------------------------

After confirming run these command :

1.) 🛠️ ""docker build -t username/your_image_name . ""
👉 Builds a Docker image from your Dockerfile.

-t = tag your image (naming it)

username/your_image_name = name (good if you're pushing to Docker Hub)

"." = use current directory as context (where your Dockerfile is)

🧠 Use case: Prepares the image to be run or pushed.

2.) 🚀 "" docker run -it your_image_name ""
👉 Runs a container based on your image.

"-i"  = interactive (keeps STDIN open)

"-t"= allocates a terminal (TTY)

your_image_name = name of the image you want to run

🧠 Use case: Starts your Python app or script inside a container.

---------------------------------------------------------------DOCKER IMAGE PUSHING------------------------------------------------------------------------------------------------------
3.) 📤 docker push username/your_image_name
👉 Pushes your image to Docker Hub (your online Docker registry).

🔐 Requirements:
You must be logged in to Docker:
docker login
Your image must be tagged correctly with your Docker Hub username.

4.) 📥 docker pull username/image_name
👉 Downloads (pulls) an image from Docker Hub to your local system.

---------------------------------------------------------------DOCKER IMAGE CLEANING------------------------------------------------------------------------------------------------------
🚀 Clean up your Docker environment like a pro! 🌟
First, take a look at the mess — like exploring your cluttered room! 🔍

Run: docker images
You’ll see a list of images that have piled up over time.

Now, let’s declutter by removing the extra images you don’t need! 🧹

🚀 Delete a single image: docker rmi image_name_or_id
Replace image_name_or_id with the actual name or ID you want to delete.

🚀 For a deep clean, remove multiple at once: docker rmi image1 image2 image3
(Don’t worry, you’ll be back to a tidier workspace! 😉)

Want to get rid of all unused images? Try this quick fix to prune them like an expert! 💡
docker image prune
It will ask if you’re sure — say YES and free up some space!

Feeling brave? Clean everything! 🔥
docker rmi $(docker images -q)
Warning: This will delete ALL local Docker images! 

---------------------------------------------------------------DOCKER MULTISTAGE IMAGES ------------------------------------------------------------------------------------------------------

❤️❤️❤️ The main purpose of choosing a golang based applciation to demostrate this example is golang is a statically-typed programming language that does not require a runtime in the traditional sense. Unlike dynamically-typed languages like Python, Ruby, and JavaScript, which rely on a runtime environment to execute their code, Go compiles directly to machine code, which can then be executed directly by the operating system.

So the real advantage of multi stage docker build and distro less images can be understand with a drastic decrease in the Image size.

---------------------------------------------------------------DOCKER BIND MOUNTS AND VOLUMES ------------------------------------------------------------------------------------------------------


A container is called ephemeral because:

🔁 It’s temporary by design:
Containers are meant to start fast, do their job, and shut down.

Once stopped or deleted, everything inside the container that's not saved externally is lost.

🧠 Think of it like:
Imagine a food truck (the container). It:

Rolls in (starts up),

Cooks food (runs the app),

Packs up and leaves (stops/deletes). If you didn’t save the recipe (data) somewhere else, it’s gone.

🧱 Why this design?
Scalability: Spin up 100 containers quickly.

Consistency: Always start from the same image → no surprises.

Isolation: Each container runs independently, cleanly.

Statelessness: Encourages storing data in volumes/databases instead.

🔐 So how to persist data?
Use volumes (bind mounts or Docker volumes).

Store data in external storage or services (like AWS S3, RDS, etc.).

---

### **1. The Technical Truth: Why Ephemeral?**  
- **Filesystem Layers**: Containers use a **temporary writable layer** (copy-on-write). When the container dies, that layer is wiped.  
- **Process-Based**: No `systemd` or init systems (by default). The container **lives only as long as its main process**.  
- **Orchestration Reality**: Tools like Kubernetes **kill and recreate containers constantly** (upgrades, scaling, failures).  

---

### **2. What Actually Disappears?**  
| **Lost When Container Dies**          | **Persists If Configured**         |  
|---------------------------------------|-------------------------------------|  
| Temp files (`/tmp`, `/var/log`)       | Data in **volumes** (`-v my_volume:/data`) |  
| Runtime app state (e.g., memcached)   | **Bind mounts** (`-v /host/path:/data`) |  
| Modifications to the image (e.g., `apt install`) | **External databases** (Postgres, S3) |  

---

### **3. Pro Tips to Tame Ephemerality**  
#### ✅ **For Data Persistence**  
```sh
# Named volume (Docker-managed)  
docker run -v db_data:/var/lib/mysql mysql  

# Bind mount (host directory)  
docker run -v $(pwd)/app:/app nginx  
```  

#### ✅ **For Surviving Restarts**  
- Use `--restart=unless-stopped` to let Docker respawn dead containers.  
- **But**: Still assume any single container can die anytime (design for failure).  

#### ✅ **For Debugging**  
```sh
# Inspect a dead container's files (before deletion)  
docker cp <dead-container-id>:/var/log/app ./logs  
```  

#### ⚠️ **Anti-Patterns**  
- **Don’t** SSH into containers to "fix" things (rebuild the image instead).  
- **Don’t** rely on container-internal storage for databases (use volumes).  

---

### **4. Why This Matters in Production**  
- **Stateless apps scale horizontally** (just add more containers).  
- **Stateful apps** (e.g., databases) **must** use volumes + backups.  
- **CI/CD pipelines** rely on ephemerality to ensure clean deploys.  




