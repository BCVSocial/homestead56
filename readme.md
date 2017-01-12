# Vagrant Box ActiveSocial - BCV FE team

## Vagrant Box with PHP57/Postgres

This is a minimalistic PHP57/Postgres vagrant box meant to be used by the FE, so it might not be fully featured for all the projects requirements. However, it can be twaked to include anything else the team might need that doesn't require a lot of effort. The goal is to jumpstart the development proccess for FE'ends if they strictly need something that the staging server can't provide. *e.g* generating the reset password link to be used locally.

It can only be used in *Mac* right now, but with some work it can work in *Windows* also.

Start by running `bash init.sh` within the main directory after cloning, this creates a new directory in ~/.Homestead56. Setup this box's Homestead file at ~/.Homestead56/Homestead.yaml

For more information on how to setup homestead official documentation [is located here](https://laravel.com/docs/5.3/homestead).

### Steps

1. Install *Virtual Box* and *Vagrant* if not already installed.

2. Git clone `https://github.com/BCVSocial/homestead56.git`

3. `cd homestead56`

4. `vagrant plugin install vagrant-vbguest`

5. `vagrant init`

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
         
         
7. `vagrant provision & vagrant reload`

8. Finally, if you try and run `vagrant ssh` you should ssh into the virtual machine.

### What's next ?

After following these steps you should have a decent vagrant setup should continue with steps outlined [is located here](https://github.com/BCVSocial/ActiveSocial)

### Improvements ? 

Go ahead =)
