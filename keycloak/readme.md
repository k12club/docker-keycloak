# การติดตั้ง Keycloak สำหรับพัฒนาระบบ OIS
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
4. รัน docker-compose.yml
    ```powershell
    docker-compose up
    ```
5. เข้าใช้งาน keycload admin console ที่ http://localhost:8080
    > user: admin\
    > passwaord: admin
6. ในตัวอย่างไม่ได้สร้าง realm ใช้ master realm เลย ซึ่งจะมีผลกับ url ที่ใช้สำหรับเรียกเท่านั้น
7. สร้าง client ชื่อ **yii-oidc**
8. กำหนด **Access** Type เป็น **confidential** แล้วกด save
9. กำหนด Root URL เป็น **http://localhost:8000**
10. กำหนด Valid Redirect URIs เป็น **/site/auth*** (ใส่ * เพื่อบอกว่าอะไรก็ได้ ซึ่งถ้าระบุไม่ตรง keycloak จะบอกว่า page not found)
11. ก็อปปี้ **secret** จากแท็บ "Credentials" ไปใช้ในโปรเจ็ค