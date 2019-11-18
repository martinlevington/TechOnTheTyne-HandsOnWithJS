# Part 3 - A Headless CMS Strapi with Docker

We will power our frontend application us a headless CMS, Strapi in this case.

Clone the following repository on your computer. 

    https://github.com/martinlevington/TechOnTheTyne-HandsOnWithJS-Srv

Open a terminal and navigate to the newly cloned repository. 

Note: make sure that you docker is set to run linux containers.

We need to build our docker image. Run the following command:

```
docker image build -t toontakeaway:1.0 .
```


PC:

```
docker run -it  -e DATABASE_NAME=strapi  -e DATABASE_USERNAME=strapi -e DATABASE_PASSWORD=strapi -p 1337:1337  -v C:\Code\headless-cms-toontakeway:/srv/app  toontakeaway:1.0
```

Mac:

```
docker run -it \
-e DATABASE_CLIENT=sqlite \
-e DATABASE_NAME=strapi \
-e DATABASE_USERNAME=strapi \
-e DATABASE_PASSWORD=strapi \
-p 1337:1337 \
-v \`pwd\`/headless-cms-toontakeway:/srv/app \
toontakeaway:1.0
```


visit the admin page:

    http://localhost:1337/admin

Create your first Administrator, enter:

    Username: Administrator
    Password: admin1234
    Email: {your email}