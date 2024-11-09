# Setting up KVM host and installing terraform on Ubuntu 22.04 

    ./set-up-ubuntu22.04-host.sh
    git clone https://github.com/pasiol/edunetes.git
    cd edunetes
    python3.10 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    ansible-galaxy install -r requirements.yaml
    ansible-playbook playbook-kvm-host-set-up.yaml -K
    sudo reboot
