Vagrant.configure(2) do |config|

  config.vm.box = "ebrc/wdk-base"
  config.vm.box_url = "http://software.apidb.org/vagrant/wdk-base.json"

  config.vm.network :forwarded_port, guest: 9380, host: 9380, auto_correct: true

  config.vm.provision :ansible do |ansible|
    ansible.playbook = "playbook.yml"
  end

end
