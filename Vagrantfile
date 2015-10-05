#Define the list of machines
slurm_cluster = {
    :head => {
        :hostname => "head",
        :ipaddress => "10.10.10.3"
    },
    :compute0 => {                                                              
        :hostname => "compute0",
        :ipaddress => "10.10.10.4"
    },
    :compute1 => {
        :hostname => "compute1",
        :ipaddress => "10.10.10.5"
    },
    :compute2 => {                                                              
        :hostname => "compute2",
        :ipaddress => "10.10.10.6"
    },
    :compute3 => {
        :hostname => "compute3",
        :ipaddress => "10.10.10.7"
    },

}

#Provisioning inline script
$script = <<SCRIPT
apt-get update
apt-get install -y -q vim slurm-llnl
echo "10.10.10.3    head" >> /etc/hosts
echo "10.10.10.4    compute0" >> /etc/hosts
echo "10.10.10.5    compute1" >> /etc/hosts
echo "10.10.10.6    compute2" >> /etc/hosts
echo "10.10.10.7    compute3" >> /etc/hosts
wget https://raw.github.com/jordangumm/slurm_test/master/slurm.conf
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
