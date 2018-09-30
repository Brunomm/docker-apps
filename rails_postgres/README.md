# Rails + PostgreSQL

### Faça o clone deste repositório:
```bash
cd ~/projects
git clone git@github.com:Brunomm/docker-apps.git
cd docker-apps/rails_postgres
```
### Crie uma pasta para seu projeto e copie os arquivos desta pasta para lá
```bash
mkdir ~/projects/my-project
cp -a ./ ~/projects/my-project
```

### Vá até a pasta do novo projeto e inicialize o git:
```bash
cd ~/projects/my-project
git init
git add -A
git commit -m "Init git with Docker"
```

### Crie o projeto Rails
```bash
docker-compose run --rm app rails new . --database=postgresql
```

### Altere o proprietário do diretório, pois o Docker roda os comandos como root:
```bash
sudo chown -R $USER:$USER .
```

### Ajuste o arquivo `config/database.yml`
```yml
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  host: <%= ENV["POSTGRES_HOST"] %>
  username: postgres
  password: <%= ENV['POSTGRES_PASSWORD'] %>
```

### Crie o database:
```bash
docker-compose run --rm app rails db:create
```
### Agora já pode gerar os scaffolds e testar se está tudo ok
```bash
docker-compose run --rm app rails g scaffold Book title:string description:text
docker-compose run --rm app rails db:migrate
docker-compose up
# http:localhost:3000/books
```
