



## 1. CHECK IF MYSQL/MARIADB IS RUNNING

#### Mariabdb:
```
sudo systemctl status mariadb
```

#### MySQL:
```
sudo systemctl status mysql
```

### LOGIN TO MYSQL/MARIADB
##### (ROOT USER)

```root user
mysql -u root -p
```

##### (FRAPPE USER)
``` frappe user
mysql -u frappe -p
```

##### (ERPNEXT USER)
``` erpnext user
mysql -u erpnext -p
```





#### 4. FIND ERPNext DATABASE NAME
``` SQL
SHOW DATABASES;
```

### 5. CONNECT TO ERPNext DATABASE
```
USE `_91dfdfs23dfd3200f1b`;
```
`Database changed`


#### 6. LIST ALL TABLES
```
SHOW TABLES;
```

### 8. EXIT MYSQL
```
exit;
```

### 9
```
SELECT name, customer_name FROM `tabCustomer` LIMIT 10;
```





















