## **🔹 ส่วนที่ 2: วิธีสร้างและใช้งาน React Components**

### **1. การสร้าง Component แรก**
React Components คือส่วนประกอบที่ใช้สร้าง UI โดยแต่ละ Component จะเป็นอิสระและสามารถนำมาใช้ซ้ำได้ (Reusable) เราจะเริ่มต้นด้วยการสร้าง Component ง่ายๆ และนำมาใช้งานในโปรเจกต์ React ของเรา

#### **ขั้นตอนที่ 1: สร้าง Component ในไฟล์ `App.jsx`**
1. เปิดไฟล์ `src/App.jsx` ซึ่งเป็นไฟล์หลักของโปรเจกต์ React
2. แก้ไขโค้ดให้เป็นดังนี้:

```jsx
function App() {
  return (
    <div>
      <h1>สวัสดี React!</h1>
      <p>ยินดีต้อนรับสู่การพัฒนาเว็บแอปด้วย React</p>
    </div>
  );
}
export default App;
```

- **คำอธิบาย:**
  - `App` คือ Component หลักของแอปพลิเคชัน
  - `return` ใช้สำหรับแสดงผล JSX (JavaScript XML) ซึ่งเป็นรูปแบบของ HTML ใน React
  - `export default App` ใช้เพื่อส่งออก Component `App` เพื่อให้สามารถนำไปใช้ในไฟล์อื่นได้

#### **ขั้นตอนที่ 2: สร้าง Component แยกในไฟล์ `Header.jsx`**
1. สร้างไฟล์ใหม่ในโฟลเดอร์ `src` ชื่อ `Header.jsx`
2. เขียนโค้ดดังนี้:

```jsx
function Header({ title }) {
  return <h1>{title}</h1>;
}
export default Header;
```

- **คำอธิบาย:**
  - `Header` เป็น Component ที่รับ `props` ชื่อ `title` เพื่อแสดงผลเป็นหัวข้อ
  - `{ title }` คือการ Destructuring Props เพื่อดึงค่า `title` จาก Props ที่ส่งเข้ามา
  - `export default Header` ส่งออก Component `Header` เพื่อให้สามารถนำไปใช้ในไฟล์อื่นได้

#### **ขั้นตอนที่ 3: นำ `Header` ไปใช้ใน `App.jsx`**
1. เปิดไฟล์ `App.jsx` และแก้ไขโค้ดดังนี้:

```jsx
import Header from './Header';

function App() {
  return (
    <div>
      <Header title="React Workshop" />
      <p>เรียนรู้พื้นฐานของ React ผ่านการปฏิบัติจริง</p>
    </div>
  );
}
export default App;
```

- **คำอธิบาย:**
  - `import Header from './Header';` นำเข้า Component `Header` จากไฟล์ `Header.jsx`
  - `<Header title="React Workshop" />` ใช้ Component `Header` และส่ง Props ชื่อ `title` เข้าไป

---

### **2. การสร้างและใช้งาน Component ที่ซับซ้อนขึ้น**
เราสามารถสร้าง Component ที่ซับซ้อนขึ้นได้โดยการแบ่งส่วน UI ออกเป็น Component ย่อยๆ และนำมาใช้งานร่วมกัน

#### **ตัวอย่าง: สร้าง Component `Button`**
1. สร้างไฟล์ใหม่ในโฟลเดอร์ `src` ชื่อ `Button.jsx`
2. เขียนโค้ดดังนี้:

```jsx
function Button({ label, onClick }) {
  return (
    <button onClick={onClick}>
      {label}
    </button>
  );
}
export default Button;
```

- **คำอธิบาย:**
  - `Button` เป็น Component ที่รับ `props` ชื่อ `label` และ `onClick`
  - `onClick` เป็นฟังก์ชันที่ทำงานเมื่อผู้ใช้คลิกปุ่ม

#### **นำ `Button` ไปใช้ใน `App.jsx`**
1. เปิดไฟล์ `App.jsx` และแก้ไขโค้ดดังนี้:

```jsx
import Header from './Header';
import Button from './Button';

function App() {
  const handleClick = () => {
    alert('ปุ่มถูกคลิ้กแล้ว!');
  };

  return (
    <div>
      <Header title="React Workshop" />
      <p>เรียนรู้พื้นฐานของ React ผ่านการปฏิบัติจริง</p>
      <Button label="คลิกตรงนี้" onClick={handleClick} />
    </div>
  );
}
export default App;
```

- **คำอธิบาย:**
  - `handleClick` เป็นฟังก์ชันที่แสดง Alert เมื่อผู้ใช้คลิกปุ่ม
  - `<Button label="คลิกตรงนี้" onClick={handleClick} />` ใช้ Component `Button` และส่ง Props ชื่อ `label` และ `onClick` เข้าไป

---

### **3. การใช้ CSS ใน React Components**
เราสามารถเพิ่มสไตล์ให้กับ Component ได้โดยการเขียน CSS ในไฟล์แยกหรือใช้ Inline Styles

#### **ตัวอย่าง: เพิ่ม CSS ใน `Header.jsx`**
1. สร้างไฟล์ใหม่ในโฟลเดอร์ `src` ชื่อ `Header.css`
2. เขียนโค้ด CSS ดังนี้:

```css
.header {
  color: blue;
  font-size: 2rem;
  text-align: center;
}
```

3. ดึง CSS เข้ามาใช้ใน `Header.jsx`:

```jsx
import './Header.css';

function Header({ title }) {
  return <h1 className="header">{title}</h1>;
}
export default Header;
```

- **คำอธิบาย:**
  - `import './Header.css';` เป็นการดึงไฟล์ CSS มาใช้
  - `className="header"` ใช้คลาส CSS ที่กำหนดในไฟล์ `Header.css`

---

### **4. การส่งข้อมูลระหว่าง Components ด้วย Props**
Props คือข้อมูลที่ส่งจาก Component หนึ่งไปยังอีก Component หนึ่ง เราสามารถส่งข้อมูลได้หลายรูปแบบ เช่น String, Number, Function, หรือ Object

#### **ตัวอย่าง: ส่ง Object เป็น Props**
1. แก้ไข `App.jsx` ดังนี้:

```jsx
import Header from './Header';
import Button from './Button';

function App() {
  const user = {
    name: 'John Doe',
    age: 25,
  };

  return (
    <div>
      <Header title="React Workshop" />
      <p>ชื่อ: {user.name}, อายุ: {user.age}</p>
      <Button label="ตกลง" onClick={() => alert('สวัสดี ' + user.name)} />
    </div>
  );
}
export default App;
```

- **คำอธิบาย:**
  - `user` เป็น Object ที่เก็บข้อมูลชื่อและอายุ
  - `{user.name}` และ `{user.age}` แสดงข้อมูลจาก Object `user`

---

### **สรุป**
- **Component** คือส่วนประกอบที่ใช้สร้าง UI ใน React
- **Props** ใช้สำหรับส่งข้อมูลระหว่าง Components
- **JSX** คือรูปแบบของ HTML ใน React
- **CSS** สามารถใช้ได้ทั้งแบบไฟล์แยกและ Inline Styles

## **แหล่งข้อมูลเกี่ยวกับ React Component**

### **1. React Official ]**
- **React Official Documentation**  
  [https://react.dev/](https://react.dev/)  
  เป็นแหล่งข้อมูลที่ครบถ้วนที่สุดเกี่ยวกับ React Components, Props, State, และอื่นๆ

---

### **2. React Components Libraries**
#### **Material-UI (MUI)**  
  [https://mui.com/](https://mui.com/)  
  Library ที่มี Components สำเร็จรูปมากมาย เช่น Button, Card, Modal, และอื่นๆ พร้อมตัวอย่างการใช้งาน

#### **Ant Design**  
  [https://ant.design/](https://ant.design/)  
  Library ที่มี Components สวยงามและใช้งานง่าย เช่น Table, Form, Dropdown, และอื่นๆ

#### **Chakra UI**  
  [https://chakra-ui.com/](https://chakra-ui.com/)  
  Library ที่เน้นการ Customize Components ได้ง่าย และมี Accessibility ในตัว

#### **Tailwind UI (React)**  
  [https://tailwindui.com/components](https://tailwindui.com/components)  
  Components ที่ใช้ร่วมกับ Tailwind CSS มีตัวอย่าง Components ให้เลือกมากมาย

---

### **3. เว็บไซต์ตัวอย่าง Components**
#### **React Bootstrap**  
  [https://react-bootstrap.github.io/](https://react-bootstrap.github.io/)  
  Components ที่สร้างจาก Bootstrap เช่น Navbar, Button, Card, และอื่นๆ

#### **React Suite**  
  [https://rsuitejs.com/](https://rsuitejs.com/)  
  Library ที่มี Components หลากหลาย เช่น DatePicker, Table, และ Form

#### **Blueprint**  
  [https://blueprintjs.com/](https://blueprintjs.com/)  
  Library ที่เน้น Components สำหรับเว็บแอปพลิเคชันที่ซับซ้อน เช่น Table, Form, และ Popover

---

### **4. เว็บไซต์สำหรับฝึกสร้าง Components**
#### **Storybook**  
  [https://storybook.js.org/](https://storybook.js.org/)  
  เครื่องมือสำหรับสร้างและทดสอบ Components แยกจากแอปพลิเคชันหลัก

#### **CodeSandbox**  
  [https://codesandbox.io/](https://codesandbox.io/)  
  แพลตฟอร์มสำหรับสร้างและแชร์ React Components ออนไลน์

#### **StackBlitz**  
  [https://stackblitz.com/](https://stackblitz.com/)  
  อีกหนึ่งเครื่องมือสำหรับสร้างและทดสอบ React Components ออนไลน์

---

### **5. เว็บไซต์ตัวอย่างโค้ด**
#### **React Examples**  
  [https://reactjsexample.com/](https://reactjsexample.com/)  
  เว็บไซต์ที่รวมตัวอย่างโค้ด React Components มากมาย

#### **Awesome React Components**  
  [https://github.com/brillout/awesome-react-components](https://github.com/brillout/awesome-react-components)  
  รายการ Components และ Libraries ที่น่าสนใจบน GitHub

---

### **6. เว็บไซต์สำหรับฝึกสร้าง Components ด้วยตัวเอง**
#### **FreeCodeCamp**  
  [https://www.freecodecamp.org/](https://www.freecodecamp.org/)  
  มีบทเรียนและแบบฝึกหัดเกี่ยวกับ React Components ฟรี

#### **Frontend Mentor**  
  [https://www.frontendmentor.io/](https://www.frontendmentor.io/)  
  แบบฝึกหัดสร้าง Components จาก Design ที่ให้มา

---

### **7. เว็บไซต์สำหรับดู UI Components**
#### **UIverse**  
  [https://uiverse.io/](https://uiverse.io/)  
  รวม Components สวยๆ จากชุมชน เช่น Button, Card, และ Loader

#### **Codepen**  
  [https://codepen.io/](https://codepen.io/)  
  แพลตฟอร์มสำหรับแชร์และดูตัวอย่างโค้ด React Components


