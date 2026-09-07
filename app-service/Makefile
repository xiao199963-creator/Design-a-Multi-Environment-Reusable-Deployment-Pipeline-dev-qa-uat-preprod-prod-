TAG ?= latest
ENV ?= dev

build:
	python3 -m pip install -r requirements.txt
	python3 -m compileall -q app.py

test: build
	python3 -c "from app import app; c = app.test_client(); r = c.get('/healthz'); assert r.status_code == 200"

image:
	docker build -t app-service:$(TAG) .

deploy: image
	echo "Deploying app-service:$(TAG) to $(ENV)"