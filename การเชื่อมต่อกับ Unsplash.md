# 6 ตัวอย่างการเชื่อมต่อกับ Unsplash API
# **React API Adventure: เชื่อมแอปพลิเคชันกับ API อย่างละเอียด**  

**📌 บทเรียนนี้เหมาะสำหรับ**  
- นักศึกษาที่อยากทดลองศึกษาการเชื่อม **React กับ API**  เพิ่มเติมขึ้น
- อยากศึกษา **React Query และ Axios**  อีกหนึ่งตัวอย่าง
- อยากทดลองดึงข้อมูลจาก **Unsplash API** และแสดงผลในแอป  (*นักศึกษาต้องสมัคร Unsplash Developer เพื่อขอ API keys ในการทำแอปนี้*)

---

## **1. ทบทวนความเข้าใจเกี่ยวกับ API**
### **API คืออะไร?**  
API (**Application Programming Interface**) เป็นวิธีที่ช่วยให้แอปพลิเคชันสามารถสื่อสารกันได้ โดย API จะช่วยให้ **React (Frontend)** สามารถดึงข้อมูลจาก **Backend (Server หรือ Database)** ผ่าน **HTTP Protocol**  

### **การทำงานของ API ในแอป**
1. **React (Frontend)** → ขอข้อมูลจาก API (เช่น ขอรูปภาพ)  
2. **API (Backend)** → ส่งคำขอไปยังฐานข้อมูล หรือระบบที่ให้บริการ  
3. **Backend ประมวลผลข้อมูล** → ส่งผลลัพธ์กลับมาผ่าน API  
4. **React ได้รับข้อมูล** → นำข้อมูลมาแสดงผล  

---

## **2. ลงมือทำ: เชื่อม React กับ API**
### **2.1 ติดตั้งไลบรารีที่จำเป็น**
📌 **เปิด Terminal และรันคำสั่งนี้เพื่อดาวน์โหลดไลบรารีที่จำเป็น**  

```bash
npm install axios @tanstack/react-query tailwindcss postcss autoprefixer
```
> **อธิบายแต่ละไลบรารี**  
✅ **Axios** → ใช้สำหรับดึงข้อมูลจาก API  
✅ **React Query** → บริหารจัดการข้อมูล API  
✅ **Tailwind CSS** → ใช้จัดแต่ง UI ให้ดูดี  

---

### **2.2 ตั้งค่า Tailwind CSS**
📌 **สร้างไฟล์ `tailwind.config.js` ในโฟลเดอร์หลักของโปรเจกต์**  
(ไฟล์นี้ใช้กำหนดว่าควรให้ Tailwind CSS ทำงานกับไฟล์ไหนบ้าง)  

```js
module.exports = {
  content: ["./index.html", "./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

📌 **แก้ไขไฟล์ `src/index.css` เพื่อเพิ่ม Tailwind CSS เข้าไป**  

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

### **2.3 สร้างคอมโพเนนต์สำหรับดึงข้อมูล API**
📌 **สร้างไฟล์ใหม่ `src/components/DataFetcher.jsx`**  

```jsx
import React, { useState } from 'react';
import { useQuery } from '@tanstack/react-query';
import axios from 'axios';

// ดึงค่า API Key จากไฟล์ .env
const ACCESS_KEY = import.meta.env.VITE_UNSPLASH_ACCESS_KEY;

// ฟังก์ชันสำหรับดึงข้อมูลรูปภาพจาก Unsplash API
const fetchImages = async (query) => {
  const response = await axios.get(`https://api.unsplash.com/search/photos`, {
    params: { query, per_page: 6 },
    headers: { Authorization: `Client-ID ${ACCESS_KEY}` },
  });
  return response.data.results;
};

const DataFetcher = () => {
  const [query, setQuery] = useState('');
  const [searchQuery, setSearchQuery] = useState('nature');

  const { data, isLoading, error, refetch } = useQuery({
    queryKey: ['images', searchQuery],
    queryFn: () => fetchImages(searchQuery),
    enabled: true,
  });

  return (
    <div className="text-center p-6">
      <h2 className="text-2xl font-bold mb-4">ค้นหารูปภาพจาก Unsplash</h2>

      {/* ช่องกรอกคำค้นหา */}
      <div className="flex justify-center gap-2 mb-4">
        <input
          type="text"
          value={query}
          onChange={(e) => setQuery(e.target.value)}
          placeholder="พิมพ์คำค้นหา เช่น sea, mountain, forest..."
          className="p-2 border rounded text-black w-1/3"
        />
        <button
          onClick={() => {
            if (query.trim()) {
              setSearchQuery(query);
              refetch();
            }
          }}
          className="px-4 py-2 bg-gray-800 text-white rounded hover:bg-gray-700"
        >
          ค้นหา
        </button>
      </div>

      {/* แสดง Loading หรือ Error */}
      {isLoading && <p>กำลังโหลดข้อมูล...</p>}
      {error && <p className="text-red-500">เกิดข้อผิดพลาด: {error.message}</p>}

      {/* แสดงผลรูปภาพ */}
      <div className="grid grid-cols-3 gap-4">
        {data &&
          data.map((image) => (
            <div key={image.id} className="overflow-hidden rounded-lg shadow-md">
              <img src={image.urls.small} alt={image.alt_description} className="w-full h-40 object-cover" />
              <p className="text-sm p-2">{image.alt_description || 'ไม่มีคำอธิบาย'}</p>
            </div>
          ))}
      </div>
    </div>
  );
};

export default DataFetcher;
```

---

### **2.4 เพิ่ม `DataFetcher.jsx` ลงในแอป**
📌 **เปิดไฟล์ `src/App.jsx` และเพิ่มโค้ดนี้**  

```jsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import DataFetcher from './components/DataFetcher';
import './index.css';

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <div className="container mx-auto p-6">
        <h1 className="text-3xl font-bold text-center mb-6">React API Example</h1>
        <DataFetcher />
      </div>
    </QueryClientProvider>
  );
}

export default App;
```

---

### **2.5 ตั้งค่า API Key ให้ปลอดภัย**
📌 **1. สร้างไฟล์ `.env` ในโฟลเดอร์หลักของโปรเจกต์**  
(ใช้เก็บค่าความลับ เช่น API Key โดยที่โค้ดในไฟล์ `.js` ไม่ต้องเผยแพร่ Key นี้)  

📌 **2. เพิ่มบรรทัดนี้ลงไปใน `.env`**  
```env
VITE_UNSPLASH_ACCESS_KEY=your-unsplash-access-key
```

📌 **3. แก้ไข `vite.config.js` เพื่อให้ Vite โหลดค่าจาก `.env`**  
```js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
});
```

📌 **4. Restart Server เพื่อให้ `.env` ทำงาน**
```sh
npm run dev
```

---

## **3. ทดสอบการทำงาน**
1. **เริ่มต้นเซิร์ฟเวอร์**
   ```sh
   npm run dev
   ```
2. **เปิดเบราว์เซอร์ไปที่ `http://localhost:5173/`**
3. **พิมพ์คำค้นหา เช่น `"ocean"` แล้วกดปุ่มค้นหา**
4. **ระบบจะดึงรูปจาก Unsplash มาแสดงผล**

---

## **4. สรุป**
✅ **React สามารถเชื่อมกับ API ได้ผ่าน Axios**  
✅ **ใช้ React Query เพื่อบริหารจัดการข้อมูล API**  
✅ **เก็บ API Key ไว้ใน `.env` เพื่อความปลอดภัย**  
✅ **จัดแต่ง UI ด้วย Tailwind CSS**  

---

📌 **เพิ่มเติม**  
- ลองเปลี่ยนไปใช้ **API อื่นๆ** เช่น NASA API หรือ Weather API  
- เพิ่ม **Pagination** เพื่อเลื่อนดูรูปเพิ่มเติม  
