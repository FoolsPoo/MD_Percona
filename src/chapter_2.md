# บทที่ 2 การใช้ Data Masking 

**โดยเราจะเริ่มจาก Basic Usage กันก่อน**


หลังจากเสร็จบทที่ 1 มาแล้วอย่าพึ่งออกไปไหนเพราะเราจะใช้ต่อกันเลย โดยสามารถดูคำสั่งได้จาก[LINK](https://www.percona.com/blog/data-masking-with-percona-server-for-mysql-an-enterprise-feature-at-a-community-price/)

โดยหลังจากที่เราเข้ามาใน Mysql เรียบร้อยเราจะทำการลง Data Masking กันใน Mysql 

``````markdown
INSTALL PLUGIN data_masking SONAME 'data_masking.so';
``````
แค่นี้เราก็ลง plugin สำเร็จเรียบร้อย

เราจะมาลองใช้เบื้องต้นกัน โดยเริ่มจากการ สร้าง ตารางข้อมูลขึ้นมาก่อน
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

**DATA MASKING SSN numbers**

Social Security Number หรือ SSN คืออะไร หมายเลขรหัสประจำตัวประชาชน เป็นรหัสที่ใช้ในระบบประกันสังคมในสหรัฐอเมริกา

SSN มีความสำคัญอย่างมากในการยืนยันตัวตนและรับสิทธิประโยชน์ต่าง ๆ ในสหรัฐอเมริกา ดังนั้น ควรรักษาความลับของ SSN และไม่ควรเปิดเผยหมายเลขนี้กับบุคคลอื่น จึงเป็นเหตุผลทำไมเราถึงต้อง masking Data

เริ่มด้วยการ สร้าง Table และใส่ข้อมูลลงใน table

``````markdown
create table employee (id int, name char(15), ssn char(11));
``````

``````markdown
insert into employee values (1,"Moe",'123-12-1234'), (2,"Larry",'22-222-2222'),(3,'Curly','99-999-9999');
``````

The MASK_SSN() คือ ก็เหมือนกับคำสั่งก่อนหน้านี้แต่จะแสดงข้อมูลเพียง 4 ตัวสุดท้ายเท่านั้น

``````markdown
select id, 
       name, 
       mask_outer(name,1,1,'#') as 'masked', 
       mask_ssn(ssn) as 'Masked SSN' 
from employee;
``````
``````markdown
+------+-------+--------+-------------+
| id   | name  | masked | Masked SSN  |
+------+-------+--------+-------------+
|    1 | Moe   | #o#    | XXX-XX-1234 |
|    2 | Larry | #arr#  | XXX-XX-2222 |
|    3 | Curly | #url#  | XXX-XX-9999 |
+------+-------+--------+-------------+
``````
**Credit card numbers**

``````markdown
alter table employee add column cc char(16);
``````

``````markdown
update employee set cc = "1234123412341234";
``````

``````markdown
select mask_pan(cc) from employee;
``````

