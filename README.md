# Antrags-Analyse-Tool Docker
Setup for running AAT using docker.
## Installation
### Docker
	curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
	sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
	sudo apt-get update
	sudo apt-get install -y docker-ce
### Docker Compose
	sudo pip install docker-compose
### Containers
The setup consists of three containers:
1. **database.** Running a PostgreSQL database supporting PostGIS geospatial extensions.
2. **application.** Running the actual AAT Django application.
3. **webserver.** Running an nginx web server.
#### start the containers
	docker-compose up
#### run the Django migrations
	docker-compose exec application /opt/antrags_analyse_tool/venv/bin/python /opt/antrags_analyse_tool/manage.py migrate
#### confirm the platform is running
Visit http://localhost:8080/api/v1/propositions/proto/?format=json in your browser.
