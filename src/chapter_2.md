# บทที่ 2 การใช้ Data Masking Basic usage

หลังจากเสร็จบทที่ 1 มาแล้วอย่าพึ่งออกไปไหนเพราะเราจะใช้ต่อกันเลย

โดยสามารถดูคำสั่งได้จาก[LINK](https://www.percona.com/blog/data-masking-with-percona-server-for-mysql-an-enterprise-feature-at-a-community-price/)

โดยหลังจากที่เราเข้ามาใน Mysql เรียบร้อยเราจะทำการลง Data Masking กันใน Mysql 

``````markdown
INSTALL PLUGIN data_masking SONAME 'data_masking.so';
``````
แค่นี้เราก็ลง plugin สำเร็จเรียบร้อย

เราจะมาลองใช้เบื้องต้นกัน

โดยเริ่มจากการ สร้าง ตารางข้อมูลขึ้นมาก่อน
``````markdown
create table sensative_data (id int, hushhush bigint);
``````

ใส่ข้อมูลตัวอย่างลงไป

``````markdown
insert into sensative_data values (1,1234567890),(2,0987654321);
``````

ต่อไปคือการทำ Data masking 
``````markdown
SELECT id, 
       hushhush as 'Original', 
       MASK_INNER(convert(hushhush using binary),2,3) as 'Inner', 
       MASK_OUTER(convert(hushhush using binary),3,3) as 'Outer' 
FROM sensative_data;
``````
MASK_INNER คือการเปลี่ยนคำด้านในตามกำหนด โดยค่าพื้นฐานจะถูกแทนด้วย (X) และเอามาแทนตามลำดับ 2,3 คือ เซนเซอร์ หลังจากตัวอักษรที่ 2 จนถึง ตัวอักษรที่ 3 นับจากหลังสุด

MASK_OUTER คือการเปลี่ยนคำโดยเริ่มจากข้างนอกแทนที่ข้างใน

``````markdown
+------+------------+------------+------------+
| id   | Original   | Inner      | Outer      |
+------+------------+------------+------------+
|    1 | 1234567890 | 12XXXXX890 | XXX4567XXX |
|    2 | 987654321  | 98XXXX321  | XXX654XXX  |
+------+------------+------------+------------+
``````