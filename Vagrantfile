# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    # /*=====================================
    # =            FREE VERSION!            =
    # =====================================*/
    # This is the free (still awesome) version of Scotch Box.
    # Please go Pro to support the project and get more features.
    # Check out https://box.scotch.io to learn more. Thanks

	
	config.vm.provider "virtualbox" do |v|
		v.memory = 2048
    #    v.cpus = 2
    end
	
	
    config.vm.box = "scotch/box"
    config.vm.network "private_network", ip: "192.168.33.10"
    config.vm.hostname = "scotchbox"
    config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]

    # Optional NFS. Make sure to remove other synced_folder line too
    #config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777","fmode=666"] }


	config.vm.provision "shell", inline: <<-SHELL

        ## Only thing you probably really care about is right here
        DOMAINS=("site1.local" "phpmvc.loc")

        ## Loop through all sites
        for ((i=0; i < ${#DOMAINS[@]}; i++)); do

            ## Current Domain
            DOMAIN=${DOMAINS[$i]}

            echo "Creating directory for $DOMAIN..."
            mkdir -p /var/www/$DOMAIN/public

            echo "Creating vhost config for $DOMAIN..."
			
			cat >/etc/apache2/sites-available/$DOMAIN.conf <<EOL
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	ServerName ${DOMAIN}
	# ServerAlias www.${DOMAIN}
	DocumentRoot /var/www/${DOMAIN}/public
	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
	<Directory "/var/www/${DOMAIN}/public">
		Options Indexes FollowSymLinks
		AllowOverride All
		Require all granted
	</Directory>
</VirtualHost>
EOL

            echo "Enabling $DOMAIN. Will probably tell you to restart Apache..."
            sudo a2ensite $DOMAIN.conf

            echo "So let's restart apache..."
            sudo service apache2 restart

        done

    SHELL


end
