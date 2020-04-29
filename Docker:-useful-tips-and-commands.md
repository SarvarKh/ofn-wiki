After [settings up your docker environment](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/DOCKER.md) you need to know how to go about doing things, here's some help.

- How to have a bash interface inside the container?
`sudo docker-compose run web bash`

- How to run a command inside the container?
`sudo docker-compose run web bash -c 'bundle install'`

- How to run karma tests inside the container:
`sudo docker-compose run web bash -c 'bundle exec rake karma:start'`

- How to access the database that the web container is using?
`docker-compose exec db psql -U ofn -d open_food_network_dev`
