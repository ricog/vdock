# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

ENV['VAGRANT_DEFAULT_PROVIDER'] ||= 'docker'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "mysql" do |mysql|
    mysql.vm.provider 'docker' do |d|
      d.image  = 'mysql'
      d.name   = 'vdock_mysql'
      d.remains_running = false
      d.env    = {
        'MYSQL_ROOT_PASSWORD' => 'password'
      }
      d.ports  = ['3330:3306']
      d.vagrant_vagrantfile = "DockerVagrantfile"
    end
  end

  config.vm.define "memcached" do |mysql|
    mysql.vm.provider 'docker' do |d|
      d.image  = 'ricog/memcached'
      d.name   = 'vdock_memcached'
      d.remains_running = false
      d.ports  = ['11211:11211']
      d.vagrant_vagrantfile = "DockerVagrantfile"
    end
  end

  config.vm.define "php" do |web|
    web.vm.provider 'docker' do |d|
      d.image           = 'ricog/php-fpm'
      d.name            = 'vdock_php'
      d.remains_running = false
      d.volumes         = ['/var/www']

      d.link('vdock_mysql:mysql')
      d.link('vdock_memcached:memcached')
      d.vagrant_vagrantfile = "DockerVagrantfile"
    end

    web.vm.synced_folder ".", "/var/www", owner: 'web', group: 'web'
  end

  config.vm.define "web" do |web|
    web.vm.provider 'docker' do |d|
      d.image           = 'ricog/nginx'
      d.name            = 'vdock_web'
      d.remains_running = false
      d.ports           = ['8030:80']
      d.create_args     = ['--volumes-from', 'vdock_php']
      d.link('vdock_php:fpm')
      d.vagrant_vagrantfile = "DockerVagrantfile"
    end
  end
end

