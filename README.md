# How To Deploy A Django Project To Heroku

You’re going to create a bare-bones Django project and deploy it to the cloud straight from the terminal. By the end, you’ll have a public and shareable link to your first Heroku app.

## Table of Content
1. Create a Virtual ENvironment
2. Install Dependencies
3. Create Django Project
4. Run The Development Server
5. Upload the Django Project to GitHub

### Create A Virtual Environment

It’s a good habit to start every project by creating an isolated virtual environment that won’t be shared with other projects. This can keep your dependencies organized and help avoid package version conflicts. Some dependency managers and packaging tools like Pipenv or poetry automatically create and manage virtual environments for you to follow best practices.

$ mkdir django-heroku
$ cd django-heroku

You’re now in the django-heroku folder, which will be the home for your project. To create a virtual environment here, just run the venv module and provide a path for your new environment. By default, the folder name will become the environment’s name. If you want to, you can instead give it a custom name with the optional --prompt argument:

$ python3 -m venv ./venv --prompt portfolio

A path starting with a leading dot (.) indicates that it’s relative to the current working directory. While not mandatory, this dot clearly shows your intent. Either way, this command should create a venv subdirectory in your portfolio-project root directory:

django-heroku/
│
└── venv/
This new subdirectory contains a copy of the Python interpreter along with a few management scripts. You’re now ready to install project dependencies into it.

### Install Dependencies

Before you can install dependencies for your project, you have to activate your virtual environment that you created earlier

$ source venv/bin/activate

The shell prompt should now display a prefix with your virtual environment’s name to indicate it’s activated. You can double-check which executables the specific commands are pointing to:

(django-heroku) $ which python
/home/ome/django-heroku/venv/bin/python

The above output confirms that running python will execute the corresponding file located in your virtual environment. Now, let’s install the dependencies for your Django project.

Now lets install django. To install Django use the below command. You may have to specify the version of Django you want to install. If the version is not specified, the latest version of Django will be installed

(django-heroku) $ python -m pip install django==3.2.5

This will install the 3.2.5 release of Django. Package names are case insensitive, so it doesn’t matter whether you type django or Django, for example.

Installing Django brings a few additional transitive dependencies, which you can reveal by listing them:

(django-heroku) $ python -m pip list
Package    Version
---------- -------
asgiref    3.4.1
Django     3.2.5
pip        21.1.3
pytz       2021.1
setuptools 56.0.0
sqlparse   0.4.1


Since you want others to be able to download and run your code without problems, you need to ensure repeatable builds. That’s what freezing is for. It outputs roughly the same set of dependencies with their sub-dependencies in a special format and saves it in a requirements.txt file:

(django-heroku) $ python -m pip freeze > requirements.txt

### Create Django Project

To create you Django project, you will have to use the commands below.

(django-heroku) $ django-admin startproject mysite 

Alternatively, you could achieve the same result by calling the django module:

(django-heroku) $ python -m django startproject mysite .

Notice the dot at the end of both commands, which indicates your current working directory, django-heroku, as the destination. Without it, the command would create another parent folder with the same name as your project.

If you’re getting a command not found error or ModuleNotFound exception, then make sure you’ve activated the same virtual environment where you installed Django. Some other common mistakes are naming your project the same as one of the built-in objects or not using a valid Python identifier.

Afterward, you should have this directory layout:

django-heroku/
│
├── mysite/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── venv/
│
├── manage.py
└── requirements.txt


You created a management app named mysite, which contains project-level settings and the main file with URL patterns, among a few other things. You also created the manage.py script that conveniently wraps django-admin and hooks up to your project.

You now have a bare-bones yet runnable Django project. At this point, you would typically start one or more Django apps and define their views and models, but they aren’t necessary for this tutorial.

### Run The Development Server

Before increasing the complexity by throwing Heroku on top of your project, it makes sense to test everything out on a local computer. This may spare you a lot of unnecessary debugging. Fortunately, Django comes with a lightweight web server for development purposes, which requires little to no configuration.

To run the development server, type the following command in your terminal window where you activated the virtual environment before:

(django-heroku) $ python manage.py runserver

It will start the server on localhost port 8000 by default. You can adjust the port number if another application is already using 8000. The server will keep watching for changes in the project source files and automatically reload them when necessary. While the server is still running, navigate to the URL in your web browser:

localhost:8000  or http://127.0.0.1:8000/

### Upload The Django Project To GitHub