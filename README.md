
Place datasets to ./Ditto/data directory followed by original repo.

export DITTO_PROJECT_NAME="name"
./BUILD-DOCKER-CONTAINER.sh
./RUN-DOCKER-CONTAINER.sh
docker exec -it "name"_ditto_1 /bin/bash


Show demo

cd Ditto/notebook
python3 demo.py
