1) อธิบายว่า Deployment, replicas, selector, template คืออะไร

Deployment: ออบเจกต์สำหรับจัดการวงจรชีวิตของ Pod แบบ declarative เช่น สร้าง/อัปเดตเวอร์ชัน/rollout/rollback ให้มั่นใจว่าจำนวน Pod ตามที่กำหนดยังคงพร้อมใช้งาน

replicas: จำนวนสำเนา Pod ที่ต้องการให้รันพร้อมกัน ถ้า Pod ตาย/ไม่พร้อมใช้งาน Controller จะพยายามสร้างใหม่ให้เท่าค่านี้เสมอ

selector: เงื่อนไข (ส่วนใหญ่ใช้ labels) ที่ Deployment ใช้เพื่อ “จับคู่” กับ Pod ที่มันต้องดูแล ต้องตรงกับ template.metadata.labels

template: แม่แบบสำหรับสร้าง Pod ใหม่ ระบุ metadata (labels) และสเปกคอนเทนเนอร์ (image, ports, env ฯลฯ)

2) อธิบายว่า Service, port, targetPort ทำงานยังไง

Service: เป็นเสมือนชื่อโดเมน + Load Balancer ภายในคลัสเตอร์ ผูกชื่อเสมือนเข้ากับกลุ่ม Pod (เลือกจาก labels) ทำให้การเข้าถึง Pod มีความคงที่ แม้ Pod จะถูกสร้าง/ลบ/เปลี่ยน IP

port: พอร์ตที่ Service เปิดให้บริการภายในคลัสเตอร์ (Client ภายในเรียก serviceName:port)

targetPort: พอร์ต “ปลายทาง” ภายในคอนเทนเนอร์ของ Pod ที่ Service จะยิงทราฟฟิกไปหา (ควรตรงกับ containerPort)

(ในกรณี NodePort) จะมี nodePort เพิ่มมา คือพอร์ตบนเครื่อง Node ที่เปิดให้เข้าจากภายนอกคลัสเตอร์ → รูปแบบการเข้าถึง: http://<NodeIP>:<nodePort>

3) อธิบายความสัมพันธ์ระหว่าง Deployment และ Service

Deployment สร้างและดูแล กลุ่ม Pod ตาม template/replicas

Service ค้นหาและกระจายทราฟฟิก ไปยัง Pod ที่ตรงกับ selector (labels)

เมื่อใช้ label เดียวกัน (เช่น app: nginx) Service จะรู้ว่าต้องส่งทราฟฟิกไปยัง Pod ที่ถูกสร้าง/ดูแลโดย Deployment นั้นๆ โดยอัตโนมัติ
