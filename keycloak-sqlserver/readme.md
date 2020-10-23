# การติดตั้ง Keycloak และ import json เข้า keycloak container และเชื่อมต่อ sql server
1. ติดตั้ง docker (ถ้าติดตั้งบน windows ให้ติดตั้ง hyper-v ด้วย)
   > https://www.docker.com/products/docker-desktop
2. ตรวจสอบว่า docker พร้อมใช้งานหรือไม่
    ```powershell
    docker --version
    ```
    หากถูกต้องจะแสดงเวอร์ชั่นออกมา
3. ดาวน์โหลด keycloak image
    ```powershell
    docker pull jboss/keycloak
    ```
4. ดาวน์โหลด mssql และ mssql-tools image
5. สร้าง keycloak database สามารถสร้างได้หลายวิธี ในตัวอย่างจะสร้างด้วย docker
6. รัน docker-compose.yml
    ```powershell
    docker pull mcr.microsoft.com/mssql/server
    docker pull mcr.microsoft.com/mssql-tools
    ```
7. รันคำสั่ง
    ```powershell
    docker run --name mssql -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Password!23' mcr.microsoft.com/mssql/server
    ```
8. รันคำสั่ง
    ```powershell
    docker run -d --name mssql-scripts mcr.microsoft.com/mssql-tools /bin/bash -c 'until /opt/mssql-tools/bin/sqlcmd -S mssql -U sa -P "Password!23" -Q "create database Keycloak"; do sleep 5; done'
    ```
9. กด ctrl+C 
10. เข้าใช้งาน keycload admin console ที่ http://localhost:8080
    > user: admin\
    > passwaord: admin
11. ในตัวอย่างไม่ได้สร้าง realm ใช้ master realm เลย ซึ่งจะมีผลกับ url ที่ใช้สำหรับเรียกเท่านั้น
12. สร้าง client ชื่อ **yii-oidc**
13. กำหนด **Access** Type เป็น **confidential** แล้วกด save
14. กำหนด Root URL เป็น **http://localhost:8000**
15. กำหนด Valid Redirect URIs เป็น **/site/auth*** (ใส่ * เพื่อบอกว่าอะไรก็ได้ ซึ่งถ้าระบุไม่ตรง keycloak จะบอกว่า page not found)
16. ก็อปปี้ **secret** จากแท็บ "Credentials" ไปใช้ในโปรเจ็ค