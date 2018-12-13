
Chef Basics
===========

### Prepare the environment

mkdir ~/chef ; cd ~/chef

### Download the docker compose file:

**macOS:**
curl -o docker-compose.yml https://raw.githubusercontent.com/learn-chef/chef/master/docker-compose.yml

**Windows (PowerShell):**
Invoke-WebRequest -useb -o docker-compose.yml https://raw.githubusercontent.com/learn-chef/chef/master/docker-compose.yml

### Retrieve the latest Chef workstation images
docker-compose pull

## Run the payload

### Start the containers
docker-compose up -d

### Stop the containers
docker-compose down --rmi all

### Connect to the workstation container
docker exec -it workstation bash

### Use the file resource to create a Hello World file
chef-run web1 file hello.txt content='Hello World!'

### Check the contents of the file
ssh web1 cat /hello.txt

### ...and remove it
chef-run web1 file hello.txt action=delete

###  basic recipe.rb contents:
```
apt_update
package 'figlet'
directory '/tmp'
execute 'write_hello_world' do
    command 'figlet Hello World! > /tmp/hello.txt'
    not_if { File.exist?('/tmp/hello.txt') }
end
```

### Folder structure:
```
tree webserver
webserver/
|-- README.md
|-- metadata.rb
|-- recipes
|   `-- default.rb
`-- templates
    `-- index.html.erb
```

### File types
metadata.rb - version control & metadata.
default.rb - default recipe to run if no other recipe is specified.

### Basic recipe 2:
```
#
# Cookbook:: webserver
# Recipe:: default
#
# Copyright:: 2018, The Authors, All Rights Reserved.
  
apt_update

package 'apache2'

template '/var/www/html/index.html' do
  source 'index.html.erb'
end

service 'apache2' do
  action [:enable, :start]
end
```
### Template example:
```
cat webserver/templates/index.html.erb
<html>
  <head>
    <title>Learn Chef Demo</title>
  </head>
  <body>
    <h1>Hello Learn Chef</h1>
    <p>This is < % =node['hostname']% ></p>
  </body>
</html>
```
