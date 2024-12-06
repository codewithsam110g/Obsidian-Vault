## Setup:
1. Install Python
2. Install the Django and pillow using pip
## Project Creation
1. use django-admin command in terminal to Create a Django **Project**
``` bash
django-admin startproject project_name
```
2. Now in that folder we can create the Django **Apps*
``` bash
python3 main.py startapp app_name
```
### Django Project  vs Apps:
1. Project the entire project, the entire website or web-app
2. It has the settings, database configurations etc
3. On the other hand the App is more of a individual part or module adding a Particular Function
4. Apps are reusable in different Django projects
5. For eg: given a good Authentication App module, we can reuse that same code by copying and changing the a few settings to say we are using that module/app

### Django Architechture
1. Django Follows a MVT (Model - View- Template) arch a derivative of MVC (Model View Controller)
2. There are many ways we can use django for a website or webapp
3. For frontend we can use the templates and static files or we can use JS or other frontends and make the django an API server, serving data and auth
4. The MVT arch has 3 things the models, which handle the database in python (No SQL Needed, Still can connect to a SQL or NoSQL DB)
5. View, this will process the requests and give appropriate response either a JSON Resp for API Mode or a HTML Response from the Request and Appropriate Template and Static file
6. Templates, This is the HTML Code for the website, in addition to that this also contains Django Template Code similar to jinja code
7. The Django Code is similar to python but we write that in html to reuse parts of the html code based on the data to generate a dynamic html code using the data in the request

### Note: We will use templates (HTML, CSS and JS) as the frontend in this tutorial, later we will see how we can use others

### Configuration:
1. First we need to create 2 folders in the project root, named templates and static
2. As the names suggest they will be used for storing the template files and static files 
3. now we change the settings.py in the project folder inside the project root folder. like todo_app > todo_app > settings.py
4. import os in the top of the imports section
5. Firstly we add all the apps we have created. For this eg we will implement a Todo App so, the apps are **tasks** and **auth_dj**, if not created them create in the project root using the startapp cmd
6. now that we have created them we add them into the installed_apps list
7. now in the settings.py templates section, add **BASE_DIR/templates** to the DIRS list
8. to make the static files usable we have to set the static files dir, so scroll to the bottom and add these lines
``` python

STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static'),
]

```

With this we have successfully setup the django project, now go the project root folder and open a terminal and run this cmd to run a development server
``` bash
python3 manage.py runserver
```

this is will run a test server at the addr http://localhost:8000 in your computer, now open this addr in your browser and you should see the welcome page of the django server

In Next chapter we will start the templates to show our website