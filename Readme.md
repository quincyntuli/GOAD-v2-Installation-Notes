## LAB Computer Specifications


These notes are based on the installation instructions from [MayFly's repository](https://github.com/mayfly277). Please visit that repository for a more accurate representation of the installation instructions. These are merely my notes and I fully expect that there will be errors and inaccuracies.

The lab setup was attempted on a computer with the following specs

|      |       |
| ----------- | ----------- |
| PROCESSOR  | Intel Core i7-8750H  |
| SYSTEM MEMORY  | 24 GB |
| DRIVE SPACE  | 240 GB (SSD)|



### 01 Install Ubuntu

I use VMware Workstation as my virtual environment. However the setup relies on Vagrant and Virtualbox. Once Ubuntu is up and running inside Vmware, we shall be installting VirtualBox. Later on we shall export the boxes to virtual appliance to run them directly from VMware.

### 02 Update and upgrade repositories
```bash
sudo apt update
sudo apt upgrade
```

### 03 Install Virtualbox 
```bash
sudo apt install virtualbox
```

### 04 Install Vagrant
```bash
wget https://releases.hashicorp.com/vagrant/2.2.19/vagrant_2.2.19_x86_64.deb
sudo apt install ./vagrant_2.2.19_x86_64.deb
vagrant --version
```

### 05 Install Python pip
```bash
sudo apt install python3-pip
pip3 --version
```

### 06 Install Python Virtual Environment 
```bash
sudo apt install python3-venv
```

### 07 Clone  GOAD v2 
```bash
git clone https://github.com/Orange-Cyberdefense/GOAD.git
```

if git is not installed, then install it first

```bash
sudo apt-get install git-all
```

### 08 Create the Virtual Environment
```bash
python3 -m venv venvGOAD
```

### 09 Activate the Virtual Environment
```bash
cd GOAD/ansible
source ~/venvGOAD/bin/activate
```

### 10 Install ansible module
```bash
pip install ansible-core
```

### 11 Install pywinrm
```bash
pip install pywinrm
```

### 12 Install galaxy requirements
```bash
ansible-galaxy install -r requirements.yml
```


### 13 Lets play

First, we let vagrant setup the 5 instances. The ISO will be downloaded and the VMs will be setup. If   a local copy of the .iso already exists then this download part will be skipped and the machine will be imported from the .iso and built.

```bash
vagrant up
```

At this stage the instructions tell us to play the plabook for all playbooks by executing

```bash
ansible-playbook main.yml
```

My recommendation is to play the individuals plays books one by one to give each VM a chance to complete the build process so as to respond properly to the next set of tasks for each playbook.

As copied from the official github, those steps are ...

```bash
ansible-playbook build.yml        # Install stuff and prepare vm
ansible-playbook ad-servers.yml   # create main domains, child domain and enroll servers
ansible-playbook ad-trusts.yml    # create the trust relationships
ansible-playbook ad-data.yml      # import the ad datas : users/groups...
ansible-playbook servers.yml      # Install IIS and MSSQL
ansible-playbook ad-relations.yml # set the rights and the group domains relations
ansible-playbook adcs.yml         # Install ADCS on essos
ansible-playbook ad-acl.yml       # set the ACE/ACL
ansible-playbook security.yml     # Configure some securities (adjust av enable/disable)
ansible-playbook vulnarabilities.yml # Configure some vulnerabilities
```

