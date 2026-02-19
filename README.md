# 🔹 Flask App with MySQL Docker Setup

A simple Flask app that interacts with a MySQL database. The app allows users to submit messages, which are then stored in the database and displayed on the frontend.

## 🔹 Prerequisites

Before you begin, make sure you have the following installed:

- Docker
- Git (optional, for cloning the repository)

## 🔹 Setup

##### 1️⃣ Clone this repository (if you haven't already)

```bash
git clone https://github.com/ankittripathe/networking-two-tier-flaskapp.git
```

##### 2️⃣ Navigate to the project directory

```bash
cd networking-two-tier-flaskapp
```

##### 3️⃣ MySQL Container Setup

```bash
This is used when you run the MySQL Docker container.

docker run -d \
 --name mysql-docker \ (docker customName)
-e MYSQL_ROOT_PASSWORD=root \
 -e MYSQL_DATABASE=devops \
 -p 3306:3306 \
 mysql:8

🔹 These variables are for MySQL image only.
```

| Variable              | Purpose                          |
| --------------------- | -------------------------------- |
| `MYSQL_ROOT_PASSWORD` | Sets root password               |
| `MYSQL_DATABASE`      | Creates database automatically   |
| `MYSQL_USER`          | (optional) create new user       |
| `MYSQL_PASSWORD`      | (optional) password for new user |

##### 4️⃣ MySQL configuration For Backend Container (Flask / Node / Java)

```bash
-e MYSQL_HOST=mysql-conatinerName
-e MYSQL_USER=your_username (example:- root)
-e MYSQL_PASSWORD=your_password (example:- root)
-e MYSQL_DB=your_database (example:- devops)
```

| Where Used           | Variables                                                               |
| -------------------- | ----------------------------------------------------------------------- |
| 🐬 MySQL Container   | `MYSQL_ROOT_PASSWORD`, `MYSQL_DATABASE`, `MYSQL_USER`, `MYSQL_PASSWORD` |
| 🚀 Backend Container | `MYSQL_HOST`, `MYSQL_USER`, `MYSQL_PASSWORD`, `MYSQL_DB`                |

## 🔹 To run this two-tier application using without docker-compose

##### 1️⃣ First create a docker image from Dockerfile

```bash
docker build -t flaskapp .
```

##### 2️⃣ Now, make sure that you have created a network using following command

```bash
docker network create custom-NetworkName

# Example
  docker network create two-tier

# Check network list
  docker network ls

# Remove network
 docker network rm network-Name
```

##### 3️⃣ Attach both the containers in the same network, so that they can communicate with each other

(1) MySQL container

```bash
docker run -d \
    --name mysql-docker \
    --network=two-tier \
    -e MYSQL_ROOT_PASSWORD=root \
    -e MYSQL_DATABASE=devops \
    -p 3306:3306 \
    mysql:latest
```

(2) flaskapp (Backend container)

```bash
docker run -d \
    --name flaskapp-docker \
    --network=two-tier \
    -e MYSQL_HOST=mysql-docker \ (This should be the container name of mysql)
    -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=root \
    -e MYSQL_DB=devops \
    -p 5000:5000 \
    flaskapp:latest
```

## 🔹 Notes

- Make sure to replace placeholders.
  `your_username`
  `your_password`
  `your_database`
  with your actual MySQL configuration.

- If you encounter issues, check Docker logs and error messages for troubleshooting.

```

```
