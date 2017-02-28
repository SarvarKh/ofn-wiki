I am working on setting OFN Greece. Here is how I deployed openfoodnetwork on hekoru from ubuntu. I am ruby/rails newbie, these instructions may contain mistakes.

1. Install first locally on ubuntu (on your machine/virtual machine) according to the instructions : https://github.com/openfoodfoundation/openfoodnetwork/blob/master/README.md 
2. Open a heroku account

3. Download again a clone of this git, unpack
4. add this version to heroku : 
> git init
> git add .
> git commit -m "example"
> heroku create
> heroku addons:create heroku-postgresql
> git push heroku master

5. You may stumble on the same error than I did, this is how I solved them
  remote: Value assigned to config.time_zone not recognized.Run "rake -D time" for a list of tasks for finding appropriate time zone names.
