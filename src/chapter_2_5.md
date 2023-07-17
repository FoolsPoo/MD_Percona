# บทที่ 2 Use dictionaries to generate random terms

สำหรับการจะดึงข้อมูลจาก Dictionary นั้นจะแตกต่างและยุ่งยากกว่านิดหน่อยเพราะเราจะต้องส่งข้อมูลเข้ามาใน เซฟเวอร์
โดยเราจะสร้าง ไฟล์ TXT จากภายนอกแล้วจึงนำเข้าไปในตัวเซฟเวอร์ 


**gen_blacklist**

คือการแปลง terms นึง ให้เป็นอีก terms นึง
``````markdown
gen_blacklist(str, dictionary_name, replacement_dictionary_name)
``````
ก่อนที่เราจะใช้ได้จะต้องนำ Dictionary เข้ามาก่อน

โดย การเช็ก PATH ของ ***secure-file-priv*** ซึ่งจะใช้ในการ load ตัว Dictionary ขึ้นมา

``````markdown
show variables like "secure_file_priv"; 
``````
หรือ
``````markdown
SELECT @@global.secure_file_priv;
``````

เราจะได้ Path ของตัว ***secure-file-priv*** มา

และเราจะสร้างไฟล์ TXT เพื่อใช้ในการทดลอง ชื่อ fruit.txt และ nut.txt โดยสามารถใช้ Notepad ได้เพื่อความสะดวก

***fruit***
``````markdown
apple
berry
pieapple
``````

***nut***
``````markdown
wallnut
peanut
almond
``````
และนำไปวางไว้ข้างใน Path ที่เราหามา

และเราจะ load เข้าโดยใช้คำสั่ง
``````markdown
SELECT gen_dictionary_load('/var/lib/mysql-files/mydict','mydict');
``````

โดย **mydict** แรกคือชื่อไฟล์ที่ใส่เข้าไป และ **mydict** หลังคือชื่อ dictionary ที่เราจะตั้ง

เช่น
``````markdown
SELECT gen_dictionary_load('/var/lib/mysql-files/fruit.txt','fruit');
``````
หากขึ้น
``````markdown
+---------------------------------------------------------------+
| gen_dictionary_load('/var/lib/mysql-files/fruit.txt','fruit') |
+---------------------------------------------------------------+
| Dictionary load success                                       |
+---------------------------------------------------------------+
``````
ก็ถือว่าสำเร็จแล้ว 

**อย่าลืม load ของ nut.txt ด้วย**

โดยเราจะกลับไปเล่น คำสั่ง gen_blacklist

``````markdown
SELECT gen_blacklist('apple', 'fruit', 'nut');
``````

โดย apple คือ str ที่อยู่ใน fruit โดยจะถูกแทนที่ด้วย nut แบบสุ่ม 
