## 🎥 Project Demonstration

A complete walkthrough of the Dockerization process is available in the release assets.

**Demo Video:**  
https://drive.google.com/file/d/14QhalLl5LZPrjRnot4KiW7kaVSRN_Uc7/view?usp=sharing 

# Flask Notes Application - Dockerized

## Overview

This repository contains a Dockerized version of the Flask Notes Application completed as part of the **AlgoAnalytics Cloud Internship Assignment**.

The application allows users to:

- Create an account
- Log in securely
- Create personal notes
- View saved notes
- Delete notes
- Log out

The application uses:

- Flask
- Flask-SQLAlchemy
- Flask-Login
- SQLite
- Docker

---

# Assignment Objective

The following objectives were completed successfully:

- Forked an existing open-source Flask application.
- Understood the application architecture.
- Identified project dependencies.
- Created a Dockerfile.
- Built the Docker image.
- Executed the application inside a Docker container.
- Verified application functionality.
- Documented the complete process.
- Identified and fixed bugs discovered during testing.

---

# Steps Followed to Complete the Assignment

## 1. Forked Repository

Forked the original Flask Notes application into my GitHub account.

---

## 2. Analysed the Project

Studied the project structure and identified:

- After forking the repository, my first task included analyzing the project and understanding that how its built, where I asked this questions to myself:
  1. Which language/framework is used to build this application?
  2. Which type of dependency file our application uses?
  3. How are those dependencies installed?
  4. Does it require build?
  5. On which port our application listens?
  6. What commands are used to start our application?

- After researching and answering above questions, writing Dockerfile for application became easier.

---

## 3. Identified Database

While analysing the application, I was observed that the project uses SQLite through SQLAlchemy and automatically creates the database during application startup.

The application invokes:

```python
create_database(app)
```

which internally checks whether the database exists and creates it if necessary using SQLAlchemy.

Because SQLite is a file-based database and is initialized automatically by the application, no manual database initialization or setup scripts were required.

So **single Docker container** was sufficient to run the complete application. Unlike client-server databases such as MySQL or PostgreSQL. 

---

## 4. Created Dockerfile

Created a Dockerfile to:

- Use an official Python image
- Copy application files
- Install dependencies
- Expose the application port
- Start the Flask application

---

## 5. Built Docker Image

```bash
docker build -t flask-notes .
```

---

## 6. Verified Docker Image

```bash
docker images
```

---

## 7. Started Container

```bash
docker run -d \
--name flask-notes \
-p 8080:5000 \
flask-notes
```

---

## 8. Verified Running Container

```bash
docker ps
```

---

## 9. Accessed Application

```
<AWS_PUBLIC_IP>:8080
```

Verified:

- User Registration
- Login
- Notes Creation
- Notes Deletion
- Logout

# Acknowledgements

This project is based on the following open-source repository:

**Original Repository:**
> <https://github.com/SHOCKWAVE07/fullStackFlaskNotes.git>

All credits for the original application belong to its respective author(s).

This repository is a fork created solely for completing the AlgoAnalytics Cloud Internship Dockerization Assignment.

---

# Project Structure

```
.
├── flask/
├── website/
│   ├── auth.py
│   ├── models.py
│   ├── views.py
│   ├── static/
│   └── templates/
├── tests/
├── main.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── README.md
└── .gitignore
```

### Important Files

| File | Description |
|-------|-------------|
| `main.py` | Application entry point |
| `website/auth.py` | Authentication routes |
| `website/models.py` | Database models |
| `requirements.txt` | Python dependencies |
| `Dockerfile` | Docker image instructions |
| `README.md` | Project documentation |

---

# Application Architecture

```
Browser
      │
      ▼
Flask Application
      │
Flask-SQLAlchemy
      │
SQLite Database (database.db)
```

The application uses SQLite as its database, therefore no separate database server or additional Docker container is required.

---

# Docker Commands Used

### Build Image

```bash
docker build -t <IMAGE_NAME> <destination_file>
```

### List Images

```bash
docker images
```

### Run Container

```bash
docker run -d \
--name <CONTAINER_NAME> \
-p <HOST_MACHINE_PORT>:<CONTAINER_PORT> \
<IMAGE_NAME>
```

### View Running Containers

```bash
docker ps
```

### View Logs

```bash
docker logs <CONTAINER_ID/NAME>
```

### Stop Container

```bash
docker stop <CONTAINER_ID/NAME>
```

### Remove Container

```bash
docker rm <CONTAINER_ID/NAME>
```

### Remove Image

```bash
docker rmi <IMAGE_ID/NAME>
```

---

# Problems Encountered

## Issue 1

### Signup Failure

**Error**

```
AttributeError:
'NoneType' object has no attribute 'is_active'
```

<img width="1913" height="967" alt="Screenshot 2026-07-29 144531" src="https://github.com/user-attachments/assets/74c53399-38cc-45a2-981e-a3415f002bf9" />


### Cause

After creating a new user, the application attempted to log in the variable `user`, which was `None`, instead of the newly created user object.

### Resolution

Changed:

```python
login_user(user, remember=True)
```

to

```python
login_user(new_user, remember=True)
```

---

## Issue 2

### Logout Not Working

### Cause

The logout route incorrectly called:

```python
login_user(current_user)
```

instead of logging the user out.

### Resolution

Changed:

```python
login_user(current_user)
```

to

```python
logout_user()
```

---

## Issue 3

### Flask Application Not Accessible Outside the Container

### Cause

The Flask development server was started using:

```python
app.run(debug=True)
```

By default, Flask binds to `127.0.0.1`, which only accepts connections from inside the container. As a result, the application could not be accessed through Docker port mapping.

### Resolution

Updated the application to listen on all network interfaces:

```python
app.run(host="0.0.0.0", debug=True)
```

This allows the application to be accessed through:

```
http://localhost:8080
```

after mapping container port `5000` to host port `8080`

---

# Improvements Made

Besides Dockerizing the application, the following improvements were made:

- Fixed user signup authentication bug.
- Fixed logout functionality.
- Verified complete application workflow.
- Added complete project documentation.

---

# Technologies Used

- Python
- Flask
- Flask-SQLAlchemy
- Flask-Login
- SQLite
- Docker

---

# Assignment Outcome

Successfully completed the Cloud Internship Dockerization Assignment by:

- Understanding an existing Flask application.
- Containerizing the application using Docker.
- Verifying functionality inside a Docker container.
- Documenting the deployment process.
- Identifying and fixing application bugs encountered during testing.

---

# Author

**Srijit Nitin Shirsat**

GitHub: https://github.com/Srijit-Shirsat
