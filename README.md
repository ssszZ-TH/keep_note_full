# รายละเอียดงาน
project จัดทำขึ้นเพื่อเป็นงานส่งในวิชา cloudcom puting
หัวข้อเรื่อง docker เเละการ interface หลายๆ container เข้าด้วยกัน \
\
จากนั้นก็ เอาลง AWS webservice deploy ลง ec2


![plan ในการทำงาน](./plan.png)

จากรูปเเสดงให้เห็นถึง docker compose ที่ผมสร้าง \
\
# หลักการทำงาน
work dir
```
keep_note_full
├───flask
│   └───templates
├───nginx
└───volume
    └───db
        ├───.mongodb
        │   └───mongosh
        │       └───snippets
        ├───diagnostic.data
        └───journal
```
flask คือ api\
template คือตัวที่ jinja2 จะหยับไป render\
nginx คือ revers proxy\
volume คือ ข้อมูลใน database ซึ่งจะเชื่อมต่อกับ part ใน mongodb container \
compose yaml คือตัวที่ไว้ให้ docker มา compose up เเล้วสามารถใช้งานได้\

# warning

```
mongo-express:
    image: mongo-express
    ports:
      - "8081:8081"
    ...
```

จาก code ส่วนหนึ่งใน yaml file จะเห็นได้ว่า backdoor ของ admin ถูก expose ออกมามาที่เคครื่อง host ใช้สำหรับการ debug\
ถ้าต้องการให้ database ปลอดภัยมาขึ้นควรเอา port ออก

# deploy ไปที่ aws
- สร้าง instance ตามกำลังทรัพย์ ของผมใช้ free tier
- เลือกเป็น ubantu os (ตัวอื่นอาจจะได้ ไม่เคยลอง)
- install docker 
- install git
- create inbound rule ให้กับ instance \
```
port 80:80       nginx
port 8081:8081   backdoor to database 
```

- conmpose up
```
docker compose up -d [argrument อื่นๆ ตามใจ]
```