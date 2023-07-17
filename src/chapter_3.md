# บทที่ 3 การใช้ import database
 ***โดยตัวนี้จะเป็นการรีแคพของผู้เขียน***
 โดยวิธีการ import คร่าวๆจะมาจาก [LINK](https://www.digitalocean.com/community/tutorials/how-to-import-and-export-databases-in-mysql-or-mariadb)

 โดยเราจะเข้าไปใน mysql เพื่อสร้าง Database ก่อนในกรณีที่ยังไม่มี


``````markdown
mysql -u root -p
``````
และ create database
``````markdown
CREATE DATABASE new_database;
``````
new_database จะเป็นชื่อที่เราต้องการตั้ง

และเราจะกด **CTRL+D** เพื่อออกมานำเข้าตัว database ที่เราต้องการ

โดยตัว database ที่ผมเอามาจะมีชื่อ init.sql 

แนะนำให้นำไฟล์ไปใส่ไว้ในไฟล์ docker ชื่อ mysql-files เหมือนกัน txt ที่เราเอาไปใส่ใน chapter 2.5

ให้เราเข้าในไฟล์ mysql-files โดยการ 

``````markdown
cd
``````

เข้าไป และพิมคำสั่ง
``````markdown
mysql -u username -p new_database < data-dump.sql
``````
- username คือ username ที่ใช่ในการ login เข้าไปใน database
- new_database คือ ชื่อ database ที่เราพึ่งสร้าง
- data-dump.sql คือ ไฟล์ที่เราต้องการ import เข้าไป

ทีนี้เราก็จะเข้าไปเช็คในระบบว่ามี database ในระบบไหม
โดยเข้าไปใน database โดยการใช้ `USE new_database` และ `show tables;`

แต่ในกรณีของไฟล์ **init.sql** จะเป็นไฟล์ sql ที่เป็นคำสั่งที่สามารถ automate ได้ หรือ สามารถสร้าง database ได้เอง

เราเลยจะเช็คโดยการ
``````markdown
use toy;
``````
เพราะตามคำสั่งจะสร้าง database ชื่อ toy ขึ้นมาแทน
