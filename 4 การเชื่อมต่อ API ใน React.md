# 🚀 React API Adventure! การเชื่อมแอปพลิเคชันกับข้อมูลที่ผู้อื่นให้บริการด้วยการใช้ API 

# 1. 📚 ความเข้าใจพื้นฐานเกี่ยวกับ API
## API คืออะไร?
API (Application Programming Interface) คือช่องทางการสื่อสารระหว่างซอฟต์แวร์ที่แตกต่างกัน ในบริบทของเว็บแอปพลิเคชัน API ทำหน้าที่เป็นตัวกลางระหว่าง Frontend (React) และ Backend Server โดยใช้ HTTP Protocol ในการสื่อสาร
## 🤔 คิดง่ายๆ API ก็เหมือน "เป็นกฎของการคุยกัน" ระหว่างลูกค้ากับบริกรร้านอาหารเพื่อสั่งอาหาร!
- ลูกค้า (Client) สั่งอาหาร → React ขอข้อมูลจาก API
- ครัว (Server) ทำอาหาร → API ดึงข้อมูลจากฐานข้อมูล
- บริกรเสิร์ฟอาหาร (Response) → API ส่งข้อมูลกลับมาให้ React แสดงผล
# ประเภทของ API

## REST API
เป็นการคุยกันระหว่าง Client กับ Server เช่น เราใช้ HTTP Method เหมือนเมนูอาหาร 🍽️:
- GET → ขอข้อมูล หรือ ดึงข้อมูลจากเซิร์ฟเวอร์มาอ่าน 🍜
- POST → เพิ่ม หรือ สร้าง ข้อมูลใหม่ใส่ลงไปที่เซิร์ฟเวอร์ 🍔
- PUT / PATCH → อัปเดตข้อมูลที่เก็บในเซิร์ฟเวอร์ 🍕
- DELETE → ลบข้อมูลจากเซิร์ฟเวอร์ออกไป 🍩
- ข้อมูลอยู่ในรูปแบบ JSON หรือ XML
- เป็นมาตรฐานที่นิยมใช้มากที่สุด

นอกเหนือ จาก REST API ก็จะมี อีกหลายมาตรฐานเช่น SOAP API, GraphQL, WebSocket API เป็นต้น
### **สรุป ประเภทของ API ที่นิยมใช้**
1. **REST API** → ใช้ HTTP Methods เช่น **GET, POST, PUT, DELETE**  
2. **GraphQL** → API ที่ให้ Client ระบุโครงสร้างข้อมูลที่ต้องการ  
3. **WebSocket API** → ใช้สำหรับข้อมูลแบบ Real-time เช่นแอปแชท  
## API จะส่งสถานะกลับมา เป็น HTTP Status Codes ที่เป็นรหัสมาตรฐาน มีความหมายดังนี้
### **HTTP Status Codes ที่ควรรู้**
- ✅ **2xx - สำเร็จ** → เช่น **200 OK** (รับข้อมูลสำเร็จ), **201 Created** (สร้างสำเร็จ)  
- ⚠️ **4xx - Client Error** → เช่น **400 Bad Request** (ขอผิดพลาด), **404 Not Found** (ไม่พบข้อมูล)  
- ❌ **5xx - Server Error** → เช่น **500 Internal Server Error** (ข้อผิดพลาดภายในเซิร์ฟเวอร์)  

# 1. ก่อนลงมือทำ ในแบบฝึกปฏิบัตินี้ เราจะใช้ไลบรารีของ JS เพิ่มอีก 5 ตัว
- ✅ **Axios** → ช่วยให้ดึงข้อมูลจาก API ง่ายขึ้น
- ✅ **Tailwind CSS** → จัดสไตล์ให้ดูดี 💅
- ✅ **PostCSS** → เครื่องมือแปลง CSS ให้รองรับเบราว์เซอร์ที่แตกต่างกัน  
- ✅ **Autoprefixer** → ช่วยเติม Prefix ของ CSS อัตโนมัติ เช่น `-webkit-` หรือ `-moz-`
- ✅ **React Query** → บริหารจัดการข้อมูลจาก API  ✅ **@tanstack/react-query** → จัดการการดึงข้อมูลจาก API และ Caching ให้อัตโนมัติ 🚀
  **`@tanstack/react-query` คือ React Query เวอร์ชันล่าสุด** 🎉  
  -- > **React Query ถูกรีแบรนด์เป็น @tanstack/react-query**  
  -- > ดังนั้นการใช้ **`@tanstack/react-query`** ก็คือการใช้ **React Query** นั่นเอง  

      ในบทเรียน **เราใช้ `@tanstack/react-query` จริงๆ** ผ่านโค้ดใน `usePosts.js` และ `App.jsx`:
      1. **ใช้ `useQuery()` ใน `usePosts.js`** → ดึงข้อมูลจาก API  
      2. **ใช้ `QueryClientProvider` ใน `App.jsx`** → จัดการ Cache และ State ของ API  
---

## **🔧 2. การเตรียมโปรเจกต์**
### **2.1 ติดตั้ง Dependencies ที่จำเป็น**
📌 เปิด **Terminal** และรันคำสั่งต่อไปนี้:
```bash
npm install axios @tanstack/react-query tailwindcss postcss autoprefixer
```
📌 **ติดตั้งและตั้งค่า Tailwind CSS**
```bash
npx tailwindcss init -p
```
📌 **แก้ไขไฟล์ `tailwind.config.js`**
```js
module.exports = {
  content: ["./index.html", "./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```
📌 **แก้ไขไฟล์ `src/index.css` เพื่อเพิ่ม Tailwind CSS**
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

## **🛠️ 3. สร้าง API Service Layer**
📌 **สร้างไฟล์ `src/services/api.js`**  
```js
import axios from 'axios';

const api = axios.create({
  baseURL: 'https://jsonplaceholder.typicode.com',
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json'
  }
});

export const PostService = {
  getAllPosts: () => api.get('/posts'),
  getPostById: (id) => api.get(`/posts/${id}`),
  createPost: (data) => api.post('/posts', data),
  updatePost: (id, data) => api.put(`/posts/${id}`, data),
  deletePost: (id) => api.delete(`/posts/${id}`)
};
```

---

## **🧩 4. สร้าง Custom Hook สำหรับการดึงข้อมูล**
📌 **สร้างไฟล์ `src/hooks/usePosts.js`**
```js
import { useQuery } from '@tanstack/react-query';
import { PostService } from '../services/api';

export const usePosts = () => {
  const { data: posts, isLoading, error, refetch } = useQuery({
    queryKey: ['posts'],
    queryFn: async () => {
      const response = await PostService.getAllPosts();
      return response.data;
    }
  });

  return {
    posts,
    loading: isLoading,
    error: error?.message,
    refetch
  };
};
```

---

## **📋 5. สร้างคอมโพเนนต์สำหรับแสดงผลข้อมูล**
📌 **สร้างไฟล์ `src/components/Posts.jsx`**
```jsx
import React from 'react';
import { usePosts } from '../hooks/usePosts';

function Posts() {
  const { posts, loading, error, refetch } = usePosts();

  if (loading) return <div>กำลังโหลดข้อมูล...</div>;
  if (error) return <div>เกิดข้อผิดพลาด: {error}</div>;

  return (
    <div className="posts-container">
      <h2 className="text-2xl font-bold">บทความทั้งหมด</h2>
      <button onClick={refetch} className="mt-4 p-2 bg-blue-600 text-white rounded">รีเฟรช</button>
      <div className="posts-grid">
        {posts.map(post => (
          <div key={post.id} className="post-card">
            <h3 className="text-lg font-semibold">{post.title}</h3>
            <p>{post.body}</p>
          </div>
        ))}
      </div>
    </div>
  );
}

export default Posts;
```

---

## **📝 6. อัปเดต `App.jsx`**
📌 **แก้ไขไฟล์ `src/App.jsx`**
```jsx
import Header from './Header';
import Posts from './components/Posts';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <div className="container mx-auto p-6">
        <Header title="React Workshop" />
        <p className="mb-4">เรียนรู้พื้นฐานของ React ผ่านการปฏิบัติจริง</p>
        <Posts />
      </div>
    </QueryClientProvider>
  );
}

export default App;
```

---

## **🎨 7. ปรับแต่ง CSS สำหรับ UI**
📌 **เพิ่มสไตล์ใน `src/index.css`**
```css
.posts-container {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.posts-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

.post-card {
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: white;
}

.post-card h3 {
  margin-top: 0;
  color: #333;
}

button {
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
```

---

## **🚀 8. การทดสอบ**
1. **เริ่มต้นเซิร์ฟเวอร์**
   ```sh
   npm run dev
   ```
2. **เปิดเบราว์เซอร์ไปที่ `http://localhost:5173/`**
3. **ตรวจสอบว่าแสดงผลข้อมูลจาก API ได้ถูกต้อง**
4. **ลองกดปุ่ม "รีเฟรช" เพื่อโหลดข้อมูลใหม่จาก API**

---

## **✅ 9. สรุป**
- เราใช้ **Axios** และ **React Query** ในการดึงข้อมูลจาก API  
- เราแยก **API Service (`api.js`)** ออกมาเพื่อให้ง่ายต่อการจัดการ  
- เราใช้ **Custom Hook (`usePosts.js`)** เพื่อดึงข้อมูล  
- เราใช้ **Tailwind CSS** ในการตกแต่ง UI  
- เราสร้าง **`Posts.jsx`** เพื่อแสดงข้อมูลจาก API  

---

## **💡 10. แนวทางเพิ่มเติม**
- เพิ่ม **Form สำหรับสร้างโพสต์ใหม่**
- เพิ่ม **Pagination** สำหรับแสดงข้อมูลแบบแบ่งหน้า  
- ใช้ **React Query DevTools** เพื่อตรวจสอบการทำงานของ Query  

---

📌 **ตอนนี้แอปของคุณสามารถดึงข้อมูลจาก API และแสดงผลได้อย่างสมบูรณ์แล้ว!** 🎉
