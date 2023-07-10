# บทที่ 1 ติดตั้งใน Docker

โดย เราจะติดตั้งลงกันใน Docker เพื่อใช้ในการเปิดเซิฟเวอร์ ทั้งนี้ ยังสามารถลงในช่องทางต่างๆได้เช่น APT, YUM, Binary tarballs และ อื่นๆ สามารถดูวิธีติดตั้งของระบบอื่นได้ที่นี่ [LINK](https://docs.percona.com/percona-server/8.0/quickstart-overview.html?_gl=1*17nk6ok*_gcl_au*NTM3NDg1MDAwLjE2ODg5NjI0NDI.)

 เพื่อช่วยให้การลง Percona นั้นง่ายขึ้นขอแนะนำให้เข้าไปดึงบางคำสั่งได้ที่นี่ [LINK](https://registry.hub.docker.com/r/percona/percona-server)

โดยเริ่มจากการพิมคำสั่งลงใน Termainal เพื่อติดตั้ง Percona เวอร์ชั่นที่ต้องการ

``````markdown
docker pull percona/percona-server:8.0
``````

ทั้งนี้สามารถเปลี่ยนเวอร์ชั่นได้ตามต้องการ 
พอรันเสร็จให้เราเช็คว่ามี docker images ไหมเพื่อเช็คความมั่นใจ
```rust
docker images
```
และต่อไปเราจะสร้าว container ของ percona server

```rust
docker run --name container-name -e MYSQL_ROOT_PASSWORD=secret 
-d percona/percona-server:tag
```

โดย container-name คือชื่อ container ที่ต้องการ PASSWORD=secret secret คือรหัสผ่าน และ tag คือ tag ของตัวนั้น
example

```rust
docker run --name percona-server -e MYSQL_ROOT_PASSWORD=root 
-d percona/percona-server:8.0
```

และใช้คำสั่ง 
```rust
docker ps
```

เพื่อเช็คว่าเราสร้าง container สำเร็จแล้ว

ต่อไปเราจะเข้าไปใน docker container ที่เราสร้างไป
```rust
docker exec -it Container-id bash
```
โดย container-id ก็คือ container id ที่เราต้องการ 
และเราจะเข้ามาใน container เรียบร้อย
และต่อไปเราจะใช้ Percona Mysql Server โดยการพิม

```rust
mysql -uroot -proot
```

และทำการเช็คข้อมูล Databases ข้างในโดยการพิม

```rust
show databases;
```

หาก Databases ขึ้นมาแล้วก็นับว่าเราทำการติดตั้งสำเร็จเรียบร้อย