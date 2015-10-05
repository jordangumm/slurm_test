#Define the list of machines
slurm_cluster = {
    :controller => {
        :hostname => "a-head",
        :ipaddress => "10.10.10.5"
    },
    :server => {                                                              
        :hostname => "compute-0",
        :ipaddress => "10.10.10.6"
    },
    :server => {
        :hostname => "compute-1",
        :ipaddress => "10.10.10.7"
    },
    :server => {                                                              
        :hostname => "compute-2",
        :ipaddress => "10.10.10.8"
    },
    :server => {
        :hostname => "compute-3",
        :ipaddress => "10.10.10.9"
    },

}

#Provisioning inline script
$script = <<SCRIPT
apt-get update
apt-get install -y -q vim slurm-llnl
echo "10.10.10.3    a-head" >> /etc/hosts
echo "10.10.10.4    compute-0" >> /etc/hosts
echo "10.10.10.5    compute-1" >> /etc/hosts
echo "10.10.10.6    compute-2" >> /etc/hosts
echo "10.10.10.7    compute-3" >> /etc/hosts
wget https://raw.github.com/guillermo-carrasco/mussolblog/master/setting_up_a_testing_SLURM_cluster/slurm.conf
cp slurm.conf /etc/slurm-llnl/
SCRIPT

Vagrant.configure("2") do |global_config|
    slurm_cluster.each_pair do |name, options|
        global_config.vm.define name do |config|
            #VM configurations
            config.vm.box = "precise64"
            config.vm.hostname = "#{name}"
            config.vm.network :private_network, ip: options[:ipaddress]

            #VM specifications
            config.vm.provider :virtualbox do |v|
                v.customize ["modifyvm", :id, "--memory", "512"]
            end

            #VM provisioning
            config.vm.provision :shell,
                :inline => $script
        end
    end
end
