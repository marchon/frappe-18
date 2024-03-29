


## Install & Setup


[https://github.com/frappe/bench](https://github.com/frappe/bench)


[manual install on ubuntu-16](https://discuss.erpnext.com/t/guide-manual-install-erpnext-on-ubuntu-16-xx-debian-v8-9/22596)

[ubuntu user](https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-ubuntu-16-04)


[mysql reset root password](https://support.rackspace.com/how-to/mysql-resetting-a-lost-mysql-root-password/)

[easy install](https://www.virtual-server.org/blog/how-to-install-erpnext-on-ubuntu-16-04/)

#### drop site and re-install erpnext 

```
bench drop-site site1 --root-password dada && bench new-site site1 --admin-password dada --mariadb-root-password dada --install-app erpnext && bench start

```

#### db restore & migrate

```
bench --site [sitename] --force restore /path/to/SQLFILE
bench --site [sitename] migrate
```

#### pre-requisite packages

```
npm install babel-core less chokidar babel-preset-es2015 babel-preset-es2016 babel-preset-es2017 babel-preset-babili
```


#### get app and create site and install on site

```
$ bench get-app erpnext https://github.com/frappe/erpnext.git
$ bench new-site testsite
$ bench --site testsite install-app schools
```


#### Reset permissions on migrate

```
bench --site all reset-perms
```
#### bench console execute commands

```
bench console
for name in frappe.get_list("Doctype", filters={"module_def": "Poll"}):
    frappe.delete_doc("Doctype", name["name"])
frappe.delete_doc("Module Def", "Poll")
```

### access vbox folder from host

```
access ubuntu folder from host
/etc/samba/smb.conf

```

[setup wizard timeout](https://discuss.erpnext.com/t/setup-wizard-request-timeout-problem-solution/24682)

```
bench config http_timeout 6000
```

#### setup site from bench console if wizard fails

```
[1] # substitute your details in args
[2] args = {
"language":"English (United States)",
"country":"India",
"timezone":"Asia/Kolkata",
"currency":"INR",
"full_name":"Vijay",
"email":"vijaywm@gmail.com",
"password":"ase",
"domain":"Education",
"company_name":"ASE",
"company_abbr":"ASE",
"company_tagline":"Amba School For Excellence",
"bank_account":"ubi",
"fy_start_date":"2017-04-01",
"fy_end_date":"2018-03-31",
"add_sample_data":1,
"program_1":"Program 1",
"course_1":"Courese 1",
"instructor_1":"Instructor 1",
"room_1": "101",
"room_capacity_1":20,
"add_sample_data":1,
"setup_website":0
}

[3] from frappe.desk.page.setup_wizard.setup_wizard import setup_complete
[4] setup_complete(args)

```

#### depends on field

```
eval:doc.video_type==="Satsang"
eval:!in_list(["Services", "Test"], doc.item_group)
```

#### setup mariadb user for access from host

```
  CREATE USER 'frappedbadmin'@`%` IDENTIFIED BY 'frappedbadmin';
  GRANT ALL PRIVILEGES ON *.* TO 'frappedbadmin'@`%`WITH GRANT OPTION;
  FLUSH PRIVILEGES;
  
  sudo service mysql restart
```  


#### /etc/network/interfaces

```
     
     GNU nano 2.5.3                                   File: /etc/network/interfaces

# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto enp0s3
iface enp0s3 inet dhcp

auto enp0s8
iface enp0s8 inet static
        address 192.168.61.100
        netmask 255.255.255.0
        network 192.168.61.1
        bradcast 192.168.61.255
```


#### [install error](https://discuss.erpnext.com/t/unable-to-complete-the-setup-on-vm-virtual-box/27408/2)

```
bench config http_timeout 600 
```