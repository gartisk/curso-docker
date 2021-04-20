docker-compose up -d

docker-compose ps

docker-compose exec db psql -U postgres -c '\l'

docker-compose logs -f -t

docker-compose exec db psql -U postgres -d email_sender -c 'select * from emails'

Para escalar: Ex: Cria 3 instancias de worker
docker-compose up -d --scale worker=3

docker-compose logs -f -t worker