# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  USERNAME = "vagrant"
  
  config.vm.provider "docker" do |docker|
    # Create ssh keys unless they are already created
    %x(ssh-keygen -f #{USERNAME}.key -t ed25519 -N "") unless File.file?("#{USERNAME}.key")

    docker.build_dir = "."
    docker.build_args = [
      # Make default user inside the VM have the same UID and GID as the user running the vagrant command
      # This makes it a lot easier working with shared code locations, because the user will own the files
      "--build-arg", "USERNAME=#{USERNAME}",
      "--build-arg", "UID=#{Process.uid}",
      "--build-arg", "GID=#{Process.gid}",
      "--build-arg", "KEYFILE=#{USERNAME}.key.pub"
    ]
    docker.has_ssh = true
    docker.remains_running = true
    # Use project's directory name as name for the container.
    # (Note that this will fail if such a container already exists.)
    docker.name = File.dirname(__FILE__).split("/").last
    # Use container name as hostname inside the container, as is the default behavior for other providers.
    docker.create_args = ["--hostname", docker.name]
  end

  config.ssh.username=USERNAME
  config.ssh.private_key_path="#{USERNAME}.key"

  # Install jekyll, as per https://jekyllrb.com/docs/installation/ubuntu/
  # Except we do a per-machine install here, not a per-user one as suggested there.
  config.vm.provision "shell", name: "Install jekyll", inline: <<-SHELL
    sudo apt-get update && sudo apt-get install -y ruby-full build-essential zlib1g-dev

    sudo gem install jekyll bundler
  SHELL

  # Expose port 4000 as the default port for jekyll
  config.vm.network :forwarded_port, guest: 4000, host: 4000

  GEM_HOME = "$HOME/gems"
  # Set the GEM_HOME environment variable to be set when logging into bash
  # This is so that running bundle install from the login will work as expected
  config.vm.provision "shell", name: "Set gem install path", env: { "GEM_HOME" => GEM_HOME }, privileged: false, inline: <<-SHELL
    if ! grep --quiet GEM_HOME ~/.bashrc; then
      echo "Setting gem install path to $GEM_HOME"
      echo export GEM_HOME="$GEM_HOME" >> ~/.bashrc
      echo export PATH="$GEM_HOME/bin:$PATH" >> ~/.bashrc
    else
      echo "gem install path was already set, skipping"
    fi
  SHELL

  # Install jekyll plugins to the correct gem install path
  # Note that since the provisioner doesn't run bashrc, we have to path in the GEM_HOME again
  config.vm.provision "shell", name: "Install jekyll plugins", env: { "GEM_HOME" => GEM_HOME }, privileged: false, inline: <<-SHELL
    cd /vagrant
    bundle install
  SHELL

  # Install convenience script so we can just serve with "serve" instead of having to "bundle exec jekyll serve --host"
  config.vm.provision "shell", name: "Install convenience script", inline: <<-SHELL
    mkdir -p /home/vagrant/.local/bin
    echo 'bundle exec jekyll serve --host 0.0.0.0 --incremental --watch --force_polling $@' > /home/vagrant/.local/bin/serve  && chmod +x /home/vagrant/.local/bin/serve
    echo 'bundle exec jekyll build $@' > /home/vagrant/.local/bin/build  && chmod +x /home/vagrant/.local/bin/build
  SHELL

  # Use SSH forwarding to allow git to use the host's private key from inside the VM
  config.ssh.forward_agent = true
  # Forward X11
  config.ssh.forward_x11 = true
  # cd to the /vagrant directory upon login
  config.ssh.extra_args = ["-t", "cd /vagrant; bash --login"]
end
