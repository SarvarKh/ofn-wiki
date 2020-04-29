After [settings up your docker environment](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/DOCKER.md) you need to know how to go about doing things, here's some help.

### Tips
If you dont want to run docker as root:
https://docs.docker.com/engine/install/linux-postinstall/

EXEC vs RUN
Note that there is a difference between exec/run. Run boots any required containers while exec expects a docker-compose run to happen beforehand. If you execute commands using run you'll end up with trillions of running containers :boom:

### FAQs

To run any of these commands, you need a container running first, so you should do this first:
`sudo docker-compose up --build`
 
- How to have a bash interface inside the container?
`sudo docker-compose exec web bash`

- How to run a command inside the container?
`sudo docker-compose exec web bash -c 'bundle install'`

- How to run karma tests inside the container:
`sudo docker-compose exec web bash -c 'bundle exec rake karma:start'`

- How to access the database that the web container is using?
`docker-compose exec db psql -U ofn -d open_food_network_dev`
