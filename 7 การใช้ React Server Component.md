# **🚀 การใช้งาน React Server Components และการปรับปรุงประสิทธิภาพแอป**

**📌 ปฏิบัติการนี้มีเป้าหมายเพื่อ:**  
- แนะนำ **React Server Components (RSCs)**  
- ใช้เพื่อต้องการปรับปรุงประสิทธิภาพของ **React App**  
- ทำความเข้าใจความแตกต่างระหว่าง **Client Components กับ Server Components**  
- นักศึกษาจะต้องสร้าง github project ใหม่ เพื่อทำปฏิบัติการนี้
---

## **🌎 1. React Server Components คืออะไร?**  
**React Server Components (RSCs)** คือ **ฟีเจอร์ใหม่ของ React** ที่ช่วยให้เราสามารถ **เรนเดอร์บางส่วนของแอปบนเซิร์ฟเวอร์** ก่อนที่จะส่งไปยังเบราว์เซอร์  

### **🔗 เปรียบเทียบ Client Components vs. Server Components**
| คุณสมบัติ | Client Components (CSR) | Server Components (RSCs) |
|-----------|--------------------------|---------------------------|
| เรนเดอร์ที่ไหน? | เบราว์เซอร์ (Client) | เซิร์ฟเวอร์ (Server) |
| โหลดข้อมูล (Data Fetching) | ทำที่ฝั่ง Client | ทำที่ฝั่ง Server |
| ขนาด Bundle | ใหญ่ขึ้น (ต้องโหลดทุกโค้ด JS) | เล็กลง (โหลดเฉพาะที่จำเป็น) |
| รองรับ Interactive UI | ✅ ใช่ | ❌ ไม่สามารถใช้ useState หรือ event handlers |
| เหมาะสำหรับ | UI ที่ต้องมีปฏิสัมพันธ์ เช่น ปุ่ม, ฟอร์ม | ข้อมูลแบบ Static หรือที่ดึงมาจากฐานข้อมูล |

### **🚀 ข้อดีของ React Server Components**
✅ **ลดขนาด Bundle ของ JavaScript** → โหลดหน้าเว็บเร็วขึ้น  
✅ **ดึงข้อมูลจาก API หรือฐานข้อมูลบนเซิร์ฟเวอร์โดยตรง**  
✅ **ลดการใช้ Client-side JavaScript** → ทำให้แอปเบราว์เซอร์ทำงานเบาขึ้น  

---

## **🛠️ 2. เริ่มต้นใช้งาน React Server Components (RSCs)**
> 🔥 **React Server Components ต้องใช้กับ Next.js 13+ หรือ React 18+ (ผ่าน Framework เช่น Remix หรือ Hydrogen)**  

### **🔧 2.1 สร้างโปรเจกต์ Next.js**
📌 **ติดตั้ง Next.js (ใช้ App Router)**
```bash
npx create-next-app@latest my-rsc-app --experimental-app
cd my-rsc-app
npm install
```

### **📂 โครงสร้างไฟล์**
```
my-rsc-app/
│── app/
│   ├── page.js        # Server Component (default)
│   ├── layout.js      # Layout Component
│   ├── components/
│   │   ├── PostList.jsx  # Server Component
│   │   ├── Button.jsx    # Client Component
│   ├── api/
│   │   ├── posts.js      # API Fetching
│── package.json
│── next.config.js
│── .gitignore
```

---

## **📦 3. การใช้งาน React Server Components**
### **🌍 3.1 สร้าง Server Component ดึงข้อมูลจาก API**
📌 **สร้างไฟล์ `app/components/PostList.jsx`**
```jsx
import React from 'react';

async function fetchPosts() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts');
  return res.json();
}

// ✅ Server Component
export default async function PostList() {
  const posts = await fetchPosts();

  return (
    <div>
      <h2 className="text-xl font-bold">📜 รายการโพสต์</h2>
      <ul>
        {posts.slice(0, 5).map((post) => (
          <li key={post.id} className="border-b p-2">
            <h3 className="text-lg font-semibold">{post.title}</h3>
            <p>{post.body}</p>
          </li>
        ))}
      </ul>
    </div>
  );
}
```
> ✅ **เหตุผลที่ใช้ `async function`** → เพราะ Server Components สามารถใช้ `async/await` ได้โดยตรง  

---

### **🖥️ 3.2 เรียกใช้ Server Component ในหน้าเพจ**
📌 **แก้ไขไฟล์ `app/page.js`**
```jsx
import PostList from './components/PostList';

export default function Home() {
  return (
    <main className="p-6">
      <h1 className="text-2xl font-bold">🚀 React Server Components</h1>
      <PostList />
    </main>
  );
}
```

---

### **🖱️ 3.3 สร้าง Client Component เพื่อใช้งาน Interactivity**
📌 **สร้างไฟล์ `app/components/Button.jsx`**
```jsx
'use client'; // บอกว่าเป็น Client Component

import { useState } from 'react';

export default function Button() {
  const [count, setCount] = useState(0);

  return (
    <button
      onClick={() => setCount(count + 1)}
      className="px-4 py-2 bg-blue-500 text-white rounded"
    >
      คลิก {count} ครั้ง
    </button>
  );
}
```
> 🔥 **React Server Components ไม่มี `useState` หรือ Event Handlers** → ต้องใช้ `"use client";` เพื่อทำให้เป็น **Client Component**  

📌 **แก้ไข `app/page.js` เพื่อใช้ปุ่ม**
```jsx
import PostList from './components/PostList';
import Button from './components/Button';

export default function Home() {
  return (
    <main className="p-6">
      <h1 className="text-2xl font-bold">🚀 React Server Components</h1>
      <PostList />
      <Button />
    </main>
  );
}
```

---

## **🚀 4. วิธีปรับปรุงประสิทธิภาพของ React App**
### **📌 4.1 ใช้ Server Components ดึงข้อมูล**
❌ **เดิม**: ใช้ `useEffect()` หรือ `useState()` ดึงข้อมูลจาก API บนฝั่ง Client  
✅ **ใหม่**: ใช้ `fetch()` ภายใน Server Component  

### **📌 4.2 แยก Client และ Server Components ให้เหมาะสม**
- **ใช้ Server Components สำหรับ**: Static Data เช่น รายชื่อโพสต์, ค่าที่ไม่เปลี่ยนบ่อย  
- **ใช้ Client Components สำหรับ**: การโต้ตอบ เช่น ฟอร์ม, ปุ่ม, Animation  

### **📌 4.3 ใช้ React Suspense สำหรับการโหลดข้อมูลแบบ Asynchronous**
📌 **ตัวอย่าง `app/components/SuspenseWrapper.jsx`**
```jsx
import { Suspense } from 'react';
import PostList from './PostList';

export default function SuspenseWrapper() {
  return (
    <Suspense fallback={<p>⏳ กำลังโหลดโพสต์...</p>}>
      <PostList />
    </Suspense>
  );
}
```
📌 **แก้ไข `app/page.js`**
```jsx
import SuspenseWrapper from './components/SuspenseWrapper';

export default function Home() {
  return (
    <main className="p-6">
      <h1 className="text-2xl font-bold">🚀 React Server Components</h1>
      <SuspenseWrapper />
    </main>
  );
}
```
> 🔥 **Suspense ช่วยให้ UI ไม่กระตุก ขณะโหลดข้อมูลจาก API**  

---

## **✅ 5. สรุป**
- **React Server Components** ช่วยลดขนาดไฟล์ JavaScript ที่โหลดบน Client  
- **ใช้ Server Components** สำหรับโหลดข้อมูลแบบ Static  
- **ใช้ Client Components** สำหรับ Interactive UI  
- **แยก Components ตามความเหมาะสม** เพื่อให้แอปทำงานได้เร็วขึ้น  

---

## **📌 6. แหล่งข้อมูลเพิ่มเติม**
- [🔗 React Server Components Docs](https://react.dev/learn/passing-data-to-components#server-components)  
- [🔗 Next.js Server Components](https://nextjs.org/docs/app/building-your-application/rendering/server-components)  

---

📌 **ตอนนี้คุณสามารถใช้ React Server Components ได้แล้ว!** 🚀✨
