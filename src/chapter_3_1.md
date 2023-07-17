# บทที่ 3.1 ลองเล่นระบบทั้งหมด

หลังจากที่เราดีง database มาแล้วเราจะมาลองเล่นกัน

โดยเราจะเล่นกับ **table products**

ให้คิดว่า product_name คือ ชื่อคนนะครับ

โดยเราจะลองนำเอา dictionary name มาแทน product_name

``````markdown
select id, gen_dictionary('name') as user_name from products limit 10;
``````

แล้วถ้ารู้สึกว่าลำบากหากต้องพิมยาวๆงี้ทุกครั้งที่จะเช็ก

เราสามารถใช้ `create view` เพื่อทำให้นำมาใช้ได้สะดวกขึ้น


``````markdown
create view products_view as select id, gen_dictionary('name') as user_name, product_brand, quantity, product_price from products;
``````
เป็นต้น

และพิม
``````markdown
select * from products_view limit 10;
``````
เพื่อเช็ค