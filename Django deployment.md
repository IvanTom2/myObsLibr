Инструкция по разворачиванию сайта на сервере на стеке nginx-gunicorn-django.

```
Step 1. Install all requirements in system
sudo apt install nginx, python3, git

Step 2. Get git repository with project
1. You can make bare repo in server and use it like git server
git init --bare %gitname
2. You should clone this git to need location
# if init bare in server
git clone /home/{user}/gits/{gitname} /home/{user}/{sitedir}/source

Step 3. Configure project settings
Here you should choose dirs for database/statics/media
Here you should rewrite DEBUG = FALSE
Here you should add ALLOWED_HOSTS = ['{host}']

Step 4. In basedir execute collectstatic
python3 -m manage collectstatic

Step 5. Configure nginx: 
sudo nvim /etc/nginx/sites-availabe/{projectname}

>> 
server {
	listen 80;
	server_name {domen};

	location /static/ {
		root /home/{user}/{dirname}; # if /{dirname}/static
	}

	location / {
		proxy_pass http://unix:/tmp/{projectname}.socket;
	}
}
<<

Step 6. Create systemd for gunicorn
sudo nvim /etc/systemd/system/{projectname}.socket

>>
[Unit]
Description=Gunicorn server for {SITENAME}

[Service]
Restart=on-failure
User={user}
WorkingDirectory=/home/{user}/{dirname}/source
ExecStart=/home/{user}/{dirname}/venv/bin/gunicorn \
	--bind unix:/tmp/{projectname}.socket \
	{projectname}.wsgi:application

[Install]
WantedBy=multi-user.target
<<

Step 7. Run new socket
sudo systemctl daemon-reload
sudo systemctl enable gunicorn.django_tdd.service
sudo systemctl start gunicorn.django_tdd.service
```

___
Relations: [[Django Python]] 
Tags: #instruction #code 
References: 
Query: Как развернуть задеплоить Django на nginx и gunicorn