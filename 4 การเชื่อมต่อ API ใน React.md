# 4 🚀 React API Adventure! การเชื่อมแอปพลิเคชันกับข้อมูลที่ผู้อื่นให้บริการด้วยการใช้ API 

# 📚 ความเข้าใจพื้นฐานเกี่ยวกับ API
## API คืออะไร?
API (Application Programming Interface) คือช่องทางการสื่อสารระหว่างซอฟต์แวร์ที่แตกต่างกัน ในบริบทของเว็บแอปพลิเคชัน API ทำหน้าที่เป็นตัวกลางระหว่าง Frontend (React) และ Backend Server โดยใช้ HTTP Protocol ในการสื่อสาร
## 🤔 คิดง่ายๆ API ก็เหมือน "เป็นกฎของการคุยกัน" ระหว่างลูกค้ากับบริกรร้านอาหารเพื่อสั่งอาหาร!
- ลูกค้า (Client) สั่งอาหาร → React ขอข้อมูลจาก API
- ครัว (Server) ทำอาหาร → API ดึงข้อมูลจากฐานข้อมูล
- บริกรเสิร์ฟอาหาร (Response) → API ส่งข้อมูลกลับมาให้ React แสดงผล
# ประเภทของ API

## REST API
เป็นการคุยกันระหว่าง Client กับ Server เช่น เราใช้ HTTP Method เหมือนเมนูอาหาร 🍽️:
- GET → ขอข้อมูล หรือ ดึงข้อมูลมาอ่าน 🍜
- POST → เพิ่ม หรือ สร้าง ข้อมูลใหม่ 🍔
- PUT / PATCH → อัปเดตข้อมูล 🍕
- DELETE → ลบข้อมูล 🍩
- ข้อมูลอยู่ในรูปแบบ JSON หรือ XML
- เป็นมาตรฐานที่นิยมใช้มากที่สุด

นอกเหนือ จาก REST API ก็จะมี อีกหลายมาตรฐานเช่น SOAP API, GraphQL, WebSocket API เป็นต้น
## API จะส่งสถานะกลับมา เป็น HTTP Status Codes ที่เป็นรหัสมาตรฐาน มีความหมายดังนี้
- 2xx: สำเร็จ (200 OK, 201 Created)
- 4xx: Client Error (400 Bad Request, 404 Not Found)
- 5xx: Server Error (500 Internal Server Error)

# ลงมือทำ
## Step 1: ติดตั้งเครื่องมือก่อนเริ่มงาน!
📌 ติดตั้งไลบรารีที่เราจะใช้:
```bash
npm install axios @tanstack/react-query clsx tailwindcss @heroicons/react
```
- ✅ Axios → ช่วยให้ดึงข้อมูลจาก API ง่ายขึ้น
- ✅ React Query → บริหารจัดการข้อมูลจาก API 
- ✅ Tailwind CSS → จัดสไตล์ให้ดูดี 💅

  ## Step 2: สร้างคอมโพเน้นต์ใหม่ ชื่อ DataFetcher.jsx
  📌 สร้างไฟล์ใหม่ที่ src/components/DataFetcher.jsx
  ```jsx
    import React from 'react';
    import { useQuery } from '@tanstack/react-query';
    import axios from 'axios';
    
    const DataFetcher = () => {
      const { data, isLoading, error } = useQuery({
        queryKey: ['posts'],
        queryFn: async () => {
          const response = await axios.get('https://jsonplaceholder.typicode.com/posts');
          return response.data.slice(0, 5);
        },
      });
    
      if (isLoading) return <p>กำลังโหลดข้อมูล... 🚀</p>;
      if (error) return <p>❌ เกิดข้อผิดพลาด: {error.message}</p>;
    
      return (
        <div>
          <h2>🔥 ข้อมูลจาก API</h2>
          <ul>
            {data.map((post) => (
              <li key={post.id}>
                <strong>{post.title}</strong>
                <p>{post.body}</p>
              </li>
            ))}
          </ul>
        </div>
      );
    };
    
    export default DataFetcher;    
  ```

  
  
