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
Warning: This will delete ALL local Docker images! But hey, sometimes it’s worth it to start fresh, right?


