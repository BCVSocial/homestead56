# Vagrant Box ActiveSocial - BCV FE team

## Vagrant Box with PHP57/Postgres

This is a minimalistic PHP57/Postgres vagrant box meant to be used by the FE, so it might not be fully featured for all the projects requirements. However, it can be twaked to include anything else the team might need that doesn't require a lot of effort. The goal is to jumpstart the development proccess for FE'ends if they strictly need something that the staging server can't provide. *e.g* generating the reset password link to be used locally.

It can only be used in *Mac* right now, but with some work it can work in *Windows* also.

Start by running `bash init.sh` within the main directory after cloning, this creates a new directory in ~/.Homestead56. Setup this box's Homestead file at ~/.Homestead56/Homestead.yaml

*Every change to the Homestead.yaml file must be followed by a `vagrant provision`*

For more information on how to setup homestead official documentation [is located here](https://laravel.com/docs/5.3/homestead).

### Vagrant/Server Steps

1. Install *Virtual Box* and *Vagrant* if not already installed.

2. Git clone `https://github.com/BCVSocial/homestead56.git`

3. `cd homestead56`

4. `vagrant plugin install vagrant-vbguest`

5. edit your `/etc/hosts` file and add the following entry: 

   ```   
       127.0.0.1       activesocial.bcv.dev
   ``` 

6. Check `~/.Homestead56/Homestead.yaml` to see if it matches your local setup of the project.

   ```
       folders:
         - map: ~/BCVSocial
         to: /home/vagrant/BCVSocial
         type: nfs

       sites:
        - map: activesocial.bcv.dev ## Important, this is one of the allowed hostnames inside web/app_dev.php file.
        to: /home/vagrant/BCVSocial/ActiveSocial/web
        type: symfony
   ```

7. `bash init.sh && vagrant up`

8. `vagrant provision & vagrant reload`

9. Finally, if you try and run `vagrant ssh` you should be able to ssh into the virtual machine, to continue with the symfony setup.

10. login to postgres to check where the pg_hba.conf file is:
    * `psql -U homestead -h localhost` password is secret
    * `SHOW hba_file;` Should be `/etc/postgresql/9.4/main/pg_hba.conf`
    
11. Edit the file and look for the following lines: 
    ```
        TYPE  DATABASE        USER            ADDRESS                 METHOD

        host    all             all             127.0.0.1/32            md5

        host    all             PC             127.0.0.1/32             md5

        host    all             all             ::1/128                 md5
    ```
    * Change all ocurrences of `md5` to `trust`
    
 12. Run `sudo service postgresql restart`  
    
### Symfony Steps

1. `cd` into the main project `ActiveSocial` and run `composer install` wait, follow instructions, wait some more.

2. `php app/console doctrine:database:create`

3. `php app/console doctrine:schema:update --force`

4. `php app/console hautelook_alice:doctrine:fixtures:load` 


After following these steps you should have a decent vagrant/symfony setup.

### TODO

Improve performance, it's a little slow, see http://www.whitewashing.de/2013/08/19/speedup_symfony2_on_vagrant_boxes.html

### Improvements ? 

Go ahead =)

## Note: 

### If you encounter: 

```
NFS is reporting that your exports file is invalid. Vagrant does
this check before making any changes to the file. Please correct
the issues below and execute "vagrant reload":
```

### Run: 
```
sudo rm /etc/exports
sudo touch /etc/exports

vagrant halt
vagrant up --provision
```
