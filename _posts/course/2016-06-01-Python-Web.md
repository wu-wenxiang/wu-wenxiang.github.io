---
layout:         post
title:          Python-Web
category:       course
description:    本教程假设您已经掌握了Python基础，将介绍在Python Web开发过程中需要用到的Fullstack开发技术集，并将通过一个简单案例介绍Python-Web开发中需要用到的若干知识点，包括：HTML，CSS, WSGI, MVC框架(Model/View/Controller), Form表单，WebService，JavaScript，Session，Cookie，以及一个Python-MVC-Web微框架Flask。
---

## Fullstack Python Web
- Python
	- 2.7
	- 3.5
	- import this
- MVC Web Framework
	- URL Routine
	- Database Interaction
	- Authentication
	- Output Template
	- Code Organization
	- Django: Batteries included
	- Flask: Extensible
- WSGI: Web Server Gateway Interface
	- PEP 333 & 3333
		
		![wsgi-interface.png](http://www.mattmakai.com/source/static/img/presentations/2014-full-stack-python/wsgi-interface.png)
		
	- WSGI Server: 
		- Green Unicorn
		- mod_wsgi
		- uWSGI
		- CherryPy
- Production Environment

	![web-wsgi.png](http://www.mattmakai.com/source/static/img/presentations/2014-full-stack-python/web-wsgi.png)

	- Platform(Server)
		- Bare metal
		- Virtualized
		- IaaS
		- PaaS
	- OS 
		- Linux
	- Web Server

		![web-browser-server-wsgi.png](http://www.mattmakai.com/source/static/img/presentations/2014-full-stack-python/web-browser-server-wsgi.png)

		- Apache
		- Nginx
- Source Control

	![app-source-control.png](http://www.mattmakai.com/source/static/img/presentations/2014-full-stack-python/app-source-control.png)

	- Git
	- Murcurial
- Dependence
	- Requirement: `pip install -r requirements.txt`

		![pd-git-repo.png](http://www.mattmakai.com/source/static/img/presentations/2014-full-stack-python/pd-git-repo.png)
		![requirements-txt.png](http://www.mattmakai.com/source/static/img/presentations/2014-full-stack-python/requirements-txt.png)

	- VirtualEnv

		![app-source-control-empty-virtualenv.png](http://www.mattmakai.com/source/static/img/presentations/2014-full-stack-python/app-source-control-empty-virtualenv.png)

	- PyPI

		![app-source-control-virtualenv-pypi.png](http://www.mattmakai.com/source/static/img/presentations/2014-full-stack-python/app-source-control-virtualenv-pypi.png)

- Data
	- Relational

		![server-setup-with-db.png](http://www.mattmakai.com/source/static/img/presentations/2014-full-stack-python/server-setup-with-db.png)
		
		这张图不恰当，Database Server应该和WSGI Application/Framework相关联

		- PostgreSQL
		- MySQL

- Config/Deploy Management

	![server-db-config-mgmt.png](http://www.mattmakai.com/source/static/img/presentations/2014-full-stack-python/server-db-config-mgmt.png)

	- Chef
	- Puppet
	- SaltStack
	- Ansible
	- Fabric
- Web analytics

	![2014-full-stack-python/ga-fsp.png](http://www.mattmakai.com/source/static/img/presentations/2014-full-stack-python/ga-fsp.png)
	
- Log & Monitoring

	![logging-analytics-monitoring.png](http://www.mattmakai.com/source/static/img/presentations/2014-full-stack-python/logging-analytics-monitoring.png)

- Task Queue

	![task-queues.png](http://www.mattmakai.com/source/static/img/presentations/2014-full-stack-python/task-queues.png)

	- Celery
	- Taskmaster
	- RQ

- Even More
	- CSS & JavaScript
	- APIs
	- Web app security
	- NoSQL data stores
	- IaaS, PaaS
	- Static content

- Reference
	- [Full Stack Python](http://www.fullstackpython.com/)
	- [Driving Demand for Full Stack Developers](http://programming.oreilly.com/2014/05/driving-demand-for-full-stack-developers.html)
	- [Full Stack Python GitHub repo](https://github.com/makaimc/fullstackpython.github.com)
	- [Full Stack Python](http://www.mattmakai.com/presentations/2014-full-stack-python-dc.html), [github](https://github.com/makaimc/mattmakai.github.io/blob/gh-pages/static/presentations/2014-full-stack-python-dc.html)

## Basic Console

## TK

## WSGI

## Flask & MVC

## Form

## Cookie & Session