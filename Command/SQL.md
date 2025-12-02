



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


### LOGIN TO MYSQL/MARIADB
##### (FRAPPE USER)
``` frappe user
mysql -u frappe -p
```

``` erpnext user
mysql -u erpnext -p
```

#### 4. FIND ERPNext DATABASE NAME
``` site1.local
cat ~/frappe-bench/sites/site1.local/site_config.json
```

### 5. CONNECT TO ERPNext DATABASE
```sql
USE site1.local;
```

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





















