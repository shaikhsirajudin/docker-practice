[History and Motivation](01-history-and-motivation/README.md)
| [Technology Overview](02-technology-overview/README.md)
| [Installation and Set Up](03-installation-and-set-up/README.md)
| [Using 3rd Party Containers](04-using-3rd-party-containers/README.md)
| [Example Web Application](05-example-web-application/README.md)
| [Building Container Images](06-building-container-images/README.md)
| [Container Registries](07-container-registries/README.md)
| [Running Containers](08-running-containers/README.md)
| [Container Security](09-container-security/README.md)
| [Interacting with Docker Objects](10-interacting-with-docker-objects/README.md)
| [Development Workflows](11-development-workflow/README.md)
| [Deploying Containers](12-deploying-containers/README.md)

---

# DevOps Directive Docker Course

 
 # ![docker-practice](https://www.youtube.com/watch?v=RqTEHSBrYFw)
[Docker: Beginner to Pro](https://courses.devopsdirective.com/docker-beginner-to-pro)
[devops-directive-docker-course](https://github.com/sidpalas/devops-directive-docker-course)

## Sponsor

[![](./readme-assets/shipyard-logo.png)](https://shipyard.build/)

Thank you to [Shipyard](https://shipyard.build/) for sponsoring this course! It is because of their support that I am able to provide it to the community free of charge!

Shipyard is the easiest way to generate on demand ephemeral environments (aka a new environment for every pull request). Sign up today at https://shipyard.social/DevOpsDirectivePromo! The first 300 people to use the promo code "DEVOPSDIRECTIVE" will receive an additional 30 days free on either their startup or business tier plans!

## [01 - History and Motivation](01-history-and-motivation/README.md)

Examines the evolution of virtualization technologies from bare metal, virtual machines, and containers and the tradeoffs between them.

## [02 - Technology Overview](02-technology-overview/README.md)

Explores the three core Linux features that enable containers to function (cgroups, namespaces, and union filesystems), as well as the architecture of the Docker components.

## [03 - Installation and Set Up](03-installation-and-set-up/README.md)

Covers the steps to install and configure Docker Desktop on your system.

## [04 - Using 3rd Party Containers](04-using-3rd-party-containers/README.md)

Before we build our own container images, we can familiarize ourselves with the technology by using publicly available container images. This section covers the nuances of data persistence with containers and then highlights some key use cases for using public container images.

## [05 - Example Web Application](05-example-web-application/README.md)

Learning about containerization is interesting, but without a practical example it isn't very useful. In this section we create a 3 tier web application with a React front end client, two apis (node.js + golang), and a database. The application is as simple as possible while still providing a realistic microservice system to containerize.

## [06 - Building Container Images](06-building-container-images/README.md)

Demonstrates how to write Dockerfiles and build container images for the components of the example web app. Starting with a naive implementation, we then iterate towards a production ready container image.

## [07 - Container Registries](07-container-registries/README.md)

Explains what container registries are and how to use them to share and distribute container images.

## [08 - Running Containers](08-running-containers/README.md)

Using the containerized web application from sections 05 and 06, we craft the necessary commands to run our application with Docker and Docker Compose. We also cover the variety of runtime configuration options and when to use them.

## [09 - Container Security](09-container-security/README.md)

Highlights best practices for container image and container runtime security.

## [10 - Interacting with Docker Objects](10-interacting-with-docker-objects/README.md)

Describes how to use Docker to interact with containers, container images, volumes, and networks.

## [11 - Development Workflows](11-development-workflow/README.md)

Establishes tooling and configuration to enable improved developer experience when working with containers.

## [12 - Deploying Containers](12-deploying-containers/README.md)

Demonstrates deploying container applications to production using three different approaches: railway.app, a single node Docker Swarm, and a Kubernetes cluster.

## General Practice

* To verify docker is install
```
> docker
```
* Run the docker image
finding publicly available image on ![docker hub](https://hub.docker.com/search?badges=official&type=image) on web or locally on left menu
The below command will pull the image form the dockerhub if its not available locally and run a container from it and provided a custom command.

- By default all data created or modified in containers is ephemeral
- # If some data should be present every time a container image is run(e.g. dependency), it should be built into the image itself.
        Container Filesystem
         1) Container layer: R/W
         2) Build Layer 3 "Run npm install" :RO
         3) Build Layer 2 "COPY"            :RO
         4) Build Layer 1 "RUN apt update && apt install nodejs: RO
         5) Base Image   "ubuntu:22.04 :RO

- # If data is generated by the application that needs to be persisted, a Volume should be used to store that outside of the ephemeral container filesystem.
Great question! Let's break it down simply:

---

## 🐳 Docker Volume Mount vs Host File Mount

When you want **persistent storage** or want to **share files** between your **host machine** and a **Docker container**, you can use:

### 1. **Volumes**  
### 2. **Bind Mounts** (aka **Host File Mounts**)

---

## 🔹 1. Docker Volume Mount

**Managed by Docker.**  
Docker stores data in its own internal directories (usually under `/var/lib/docker/volumes/...`).
By default is Volume Mount
### 🔧 Example:
```bash
docker run -v mydata:/var/lib/postgresql/data postgres
```

### ✅ Pros:
- Docker manages location and permissions
- Portable between systems
- Good for data you don’t need to directly edit
- Backups and restores are easier using `docker volume` commands

### ⚠️ Cons:
- Harder to directly access/edit files from your host

---

## 🔸 2. Bind Mount (Host File Mount)

**You specify an exact folder on your host.**  
Docker just maps that folder into the container.

### 🔧 Example:
```bash
docker run -v /path/on/host:/var/lib/postgresql/data postgres
```

### ✅ Pros:
- You can directly edit files on the host
- Great for code, config files, development

### ⚠️ Cons:
- Can have permission issues
- Not as portable across machines
- Less isolated from host OS changes

---

## ⚖️ Summary Table

| Feature                | Volume Mount (`-v myvol:/path`) | Bind Mount (`-v /host/path:/path`) |
|------------------------|-----------------------------|-----------------------------|
| Managed by Docker      | ✅ Yes                      | ❌ No                        |
| Host file access       | ❌ No (unless you inspect volume) | ✅ Yes                       |
| Best for production    | ✅ Yes                      | ⚠️ Maybe (depends)           |
| Best for development   | ⚠️ Sometimes                | ✅ Yes                        |
| Portability            | ✅ High                     | ❌ Low (host path specific)  |

---

```
> docker run docker/welcome-to-docker
```
---

* Run on alpine os  postgress on port 5432
Lets say we needed a copy of postgres 15.1 running on for our application we can just issue a Docker run command and then 
1) set the postgress password, environment variable that is needed. The container wont start if you dont set a password
2) Publish Port 5432, because these Docker containers are running on an isolated Network the only way to connect to it from my host is to publish that port and so that publish command take Port 5432 of my local host my system and connect that to port 5432 inside the container and then specifying the postgres image and specifically the version

```
> docker run --env POSTGRES_PASSWORD=foobarbaz --publish 5432:5432 postgres:15.1-alpine
```
# Connecting in pgAdmin

To register a **Dockerized PostgreSQL database** in **pgAdmin**, follow these steps:

---

### ✅ Prerequisites
- Docker container running PostgreSQL
- pgAdmin installed (on your host or another container)

---

### 🚀 Step-by-Step Instructions

#### **1. Get PostgreSQL Container Details**
First, get the container IP or expose the port.

**Option 1: Expose the port**
When starting the PostgreSQL container, map the default PostgreSQL port (5432) like this:

```bash
docker run --name some-postgres -e POSTGRES_PASSWORD=yourpassword -p 5432:5432 -d postgres
```

**Option 2: Find container IP**
```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' some-postgres
```

---

#### **2. Open pgAdmin and Register a New Server**

1. Open pgAdmin in your browser
2. Right-click on **Servers** > **Create** > **Server**

---

#### **3. Configure the Server Registration**

In the dialog:

##### **General Tab**
- **Name**: Anything you like (e.g., `Docker Postgres`)

##### **Connection Tab**
- **Host name/address**: 
   - If using `-p 5432:5432`, use `localhost`
   - If pgAdmin is in another Docker container, use the **PostgreSQL container name** or **container IP**
- **Port**: `5432` (default)
- **Maintenance database**: `postgres`
- **Username**: `postgres` (or whatever you set with `POSTGRES_USER`)
- **Password**: Your password (set via `POSTGRES_PASSWORD`)

✅ Optional: Check **"Save Password"**

---

#### **4. Click "Save"**
pgAdmin will connect to the Dockerized PostgreSQL and list databases.

---

### 🐳 Tip: Connecting pgAdmin and Postgres via Docker Compose?
Use a shared Docker network and service names as hostnames:

```yaml
services:
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: yourpassword
  pgadmin:
    image: dpage/pgadmin4
    ports:
      - "8080:80"
    environment:
      PGADMIN_DEFAULT_EMAIL: user@example.com
      PGADMIN_DEFAULT_PASSWORD: admin
```

Then in pgAdmin, use `db` as the **host**.

---

run the query
```
Select * from information_schema.tables
```

---

## Example Docker Volume Mount vs Host File Mount?
![04 - Using 3rd Party Containers](04-using-3rd-party-containers/README.md)

1) Installing Dependencies with Application
This step happens during the image build process, and doesn't involve persistent data in the usual sense — but it's about baking the app and its environment into the image.

Lets create the container with the docker image.
The following command will get a running shell within that container
* --rm flag tells Docker that once this container is process exits so exit container it should not store. That stopped container it should actually remove that from our system.
```
> docker run --interactive --tty --rm ubuntu:22.04
```
- lets ping google.com from the shell, it will not find bash command ping.
 ```
 root@efe22e44777e:/# ping google.com -c 1  - bash: ping: command not found

 ```
 - lets install bash command ping, you can do by updating apt package manager. It will fetch meta data associated with the APT package manage 
 ```
apt update
 ```
 - Once we have apt updated package manager we can install IP utils
 ```
apt install iputils-ping
 ```
 Now run the ping

 ```
 ping google.com -c 1

 PING google.com (172.217.5.14) 56(84) bytes of data.
64 bytes from 172.217.5.14 (172.217.5.14): icmp_seq=1 ttl=63 time=36.5 ms
 ```
 now when we exit this container and re-run it we will not find the ping command for that

make changes to command by removing --rm and name the container. so that docker will not remove the container.

```
>docker run --interactive --tty --name my-ubuntu-container ubuntu:22.04
apt update
apt install iputils-ping --yes
ping google.com -c 1
```
once you exit and try to run following command you will still see the container

```
> docker ps -- will list the containers because it stop it doesnt show by default.
> docker ps -a  -- this will show stop container "my-ubuntu-container"
```
Now lets run the container.

```
docker start my-ubuntu-container -- start the container
docker attach my-ubuntu-container -- attached the shell running in that container, you will receive same id.
```
Stop the container
```
docker stop pedantic_panini
```
We generally never want to rly on a container to persist the data, so for a dependency like this, we would want to include it in the image:

# Build a container image with ubuntu image as base and ping installed.
i) Build the docker image with addition update
1)
```bash
docker build -tag my-ubuntu-image -<<EOF
FROM ubuntu:22.04
RUN apt update && apt install iputils-ping --yes
EOF

```
2)
```powershell
@"
FROM ubuntu:22.04
RUN apt-get update && apt-get install iputils-ping --yes
"@ | docker build -t my-ubuntu-image -
```
3) Using a Dockerfile, just create a file named Dockerfile

``` Dockerfile
FROM ubuntu:22.04
RUN apt-get update && apt-get install -y curl

```
Then run:
```cmd
docker build -t my-ubuntu-image 
```
ii) Run the newly build/container based  image "my-ubuntu-image"
```
docker run -it --rm my-ubuntu-image 
ping google.com -c 1
```
-i: interactive mode (keeps STDIN open)
-t: allocates a pseudo-TTY (lets you use the terminal)
---
Now create a file in docker container

```
mkdir my-data
echo "Hello world" > /my-data/hello.text
cat my-data/hello.text
exit

```
now run it once again
```
docker run -it --rm ubuntu:22.04

--out put
Unable to find image 'ubuntu:22.04' locally
22.04: Pulling from library/ubuntu
30a9c22ae099: Already exists
Digest: sha256:d80997daaa3811b175119350d84305e1ec9129e1799bba0bd1e3120da3ff52c3
Status: Downloaded newer image for ubuntu:22.04
root@56fcfa8f8aa5:/# cd my-data
bash: cd: my-data: No such file or directory
```

##  Data persistence in Docker is crucial when you want your **application data to survive even after containers stop, restart, or are deleted**. Let's break this down into the two parts you mentioned:

---

## 🔹 1) **Installing Dependencies with Application**
This step happens **during the image build process**, and doesn't involve persistent data in the usual sense — but it's about **baking the app and its environment into the image**.

### Example: Dockerfile with Dependencies

```Dockerfile
FROM python:3.11-slim

# Install dependencies
RUN pip install flask

# Copy your app code
COPY app.py /app/app.py

# Set the working directory
WORKDIR /app

CMD ["python", "app.py"]
```

Here, the Flask dependency is **installed into the image** using `RUN pip install flask`. Every container created from this image will have Flask ready to go — no persistence needed here, since this is part of the immutable image.

---

## 🔹 2) **Persisting Data Produced by the Application**

Now this is where **volumes and mounts** come into play.

When an application writes data (e.g., logs, user uploads, databases), you don't want that to disappear when the container stops or is removed.

---

## 🔸 Two Main Strategies

### 🔹 a) **Bind Mounts (Host File System)**
- Mounts a **specific path on your host machine** into the container.
- You choose the exact directory on the host.
- Great for development or sharing config files.

#### 🧪 Example:

```bash
docker run -v /path/on/host:/data my-ubuntu-image
```

If your app writes to `/data` inside the container, it actually writes to `/path/on/host` on your machine.

#### 📍 Pros:
- Easy to debug, you can browse the host directory.
- Good for dev, sharing source code or configs.

#### ⚠️ Cons:
- Less portable across environments.
- Security and permission issues may arise.

---

### 🔹 b) **Docker Volumes (Managed by Docker)**
- Docker manages the storage **in a special internal location** (`/var/lib/docker/volumes/...`).
- Easier for portability and backup.
- Better for production.

#### 🧪 Example:

```bash
docker volume create mydata
docker run -v mydata:/data my-ubuntu-image
```

Same idea: your app writes to `/data`, but Docker handles the storage location.

#### 📍 Pros:
- Safer, more isolated from host.
- Easier to backup (`docker volume inspect`, `docker cp`, etc.).
- Clean lifecycle — volumes can live longer than containers.

#### ⚠️ Cons:
- Harder to access from outside Docker (not as transparent).

---

## 🧩 Summary Table

| Feature             | Bind Mount                         | Docker Volume                      |
|---------------------|------------------------------------|-------------------------------------|
| Storage Location     | Host file system                  | Docker-managed storage              |
| Created manually?    | Yes                                | Yes or automatically by Docker     |
| Portability          | Low (host path specific)           | High (Docker handles it)           |
| Ideal Use Case       | Dev, code/config sharing           | App data persistence in prod       |
| Example              | `/home/user/data:/data`           | `my-volume:/data`                  |

---

## 💡 Practical Example: Persisting a SQLite DB

Let's say your app writes to `app.db`.

**Dockerfile:**

```Dockerfile
FROM python:3.11-slim
COPY . /app
WORKDIR /app
RUN pip install flask
CMD ["python", "app.py"]
```

**Host Bind Mount:**

```bash
docker run -v /home/user/sqlite-data:/app/data my-app
```

- `app.py` writes `app.db` to `/app/data/app.db`
- File lives on your host at `/home/user/sqlite-data/app.db`

**Volume Mount:**

```bash
docker volume create sqlite-persist
docker run -v sqlite-persist:/app/data my-app
```

- Same logic, but Docker manages the location.

---

Would you like me to mock up a full project with Flask + SQLite to show this in action?

1. Volumes
Volumes and mounts allow us to specify a location where data should persist beyond the lifecycle of a single container. The data can live in a location managed by docker(volume mount), a location in your host filesystem(bind mount), or in memory (tmpfs mount, not pictured).

```
> docker volume create my-volume
output
my-volume
```
now mount filesystem on that container
```
docker run -it --rm --mount source=my-volume,destination=/my-data/ ubuntu:22.04

output
root@5fef396e66bb:/# 
```
now create the file and exit
```
root@5fef396e66bb:/# echo "Hello World" >hello.txt
root@5fef396e66bb:/# cat hello.txt
Hello World
root@5fef396e66bb:/# exit
exit
```
my-data is default location of root/my-data/

```
>docker run -it --rm --mount source=my-volume,destination=/my-data/ ubuntu:22.04
root@265b53647a1b:/# cd my-data/
root@265b53647a1b:/my-data# cat hello.txt
Hello world
```
To get the shell in docker running virtual machine to see where exactly our files resides.

```
docker run -it --rm --privileged --pid=host justincormack/nsenter1@sha256:5af0be5e42ebd55eea2c593e4622f810065c3f45bb805eaacf43f08f3d06ffd8




```
once you get shell into that machine.
execute this command
```
# cd /var/lib/docker/volumes
# ls
output 
backingFsBlockDev  metadata.db  my-volume
```

```
#cd my-volume
#cd _data
# cat hello.txt
Hello world
```
2. Bind Mounts (aka Host File Mounts)