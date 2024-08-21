## 1. Getting Started

```
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
ansible-galaxy install -r requirements.yml
```

## 2. Generate the test file in the current directory first
In **roles/ssh/tasks/main.yml**, verify this is in place on line 5 and 6 so you don't immediately overwrite your config file:

```
#    dest: ~/.ssh/config
    dest: config_ssh_test
```

## 3. Update variables for your DC locations
In **/roles/ssh/defaults/main.yml** fill in the variables for each DC. Copy the stanzas if you have more locations. 
```
ssh_datacenters:
  - name: This is usually a city name or location name for your data center. [type string]
    slug: Slug is an abbreviation. Example: City is Chicago. Slug could be chi, or ord etc. [type string]
    id: Not needed unless you have an integer representing your data center. Or if you have a correlation between a dc id and an octet in it's mgmt subnet. Example: Chicago, DC=10, mgmt network 10.{{ id }}.0.0/24
```

In **group_vars/all.yml** is where your .com domain name lives. Example www.yourcompany.com. This is used in DNS if your devices have DNS names like "router1-ord.yourcompany.com".

The ssh/config file template can be changed however you need. Located in **roles/ssh/templates/user_config.j2**

## 4. Run the ssh config generator, it will generate a test file in the local directory if you followed the first step in this document
```
ansible-playbook ssh.yml
```

## 5. Look at the file generated "config_ssh_test" in the local directory, make sure it's what you want

```
cat config_ssh_test
```

## 6. If it looks good, backup your existing config file if it has data in it, then comment/uncomment the lines in the first step above.
```
mv ~/.ssh/config ~/.ssh/config.old
```

**roles/ssh/tasks/main.yml**
```
    dest: ~/.ssh/config
#    dest: config_ssh_test
```

## 7. If all is good, you should be able to directly put in the device domain name to SSH to it via the jumpbox.

```
ssh router1-ord.yourcompany.com
```