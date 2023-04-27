# How to securely set environment variables from a .env file in Docker Compose

Nowadays, working with code repos is a reality for everyone. Docker is another technology that is highly prevalent in the daily lives of programmers. It has become common practice to use a docker-compose file to easily run the environment. However, issues arise when these environments require sensitive configuration information. Often, developers resort to hardcoding this information, which can lead to accidentally committing it to the repository. While it is possible to remove this information from remote repos, it is still a significant headache. Prevention is the best solution.


## The solution

1. First we will simulate an application using environment variables. For this we will create a simple Dockerfile that will print the environment variables.

**Dockerfile**
```dockerfile
FROM alpine:latest
CMD echo "My name is $NAME, I am from $COUNTRY and I am a $JOB."
```


2. Now we will create our docker-compose. The docker-compose will be the entry point for our application. We will use the.

**docker-compose**
```yaml
version: '3.9'

services:
  demo:
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      - NAME=${NAME}
      - COUNTRY=${COUNTRY}
      - JOB=${JOB}
```
> **Note:** We are using the `${}` syntax to reference the environment variables from the `.env` file.


3. Now we will create our `.env` file. This file will contain the environment variables that we want to use in our application.

**.env**
```conf
NAME=Nelson
COUNTRY=Portugal
JOB=Developer
```

4. Let's run our environment.
**Run**
```bash
docker-compose up --build
```

If everything went well, you should see the following output:
```bash
demo_1  | My name is DSKT-NelsonBN, I am from Portugal and I am a Developer.
```

5. To finish, we will add the `.env` file to the `.gitignore` file.
```bash
echo ".env" >> .gitignore
```
> **Note:** Now you can see that your `.env` file is no longer visible in the git status.

> **Note:** In my demo the file is versioned, so we can see all the configuration. However, in your case, the file should not be versioned.

6. You can test using the command:
```bash
git status
```