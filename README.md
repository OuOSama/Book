# React Hooks Cheat Sheet

## 🔹 1. State Management Hooks
ใช้สำหรับจัดการ state ใน Component

### `useState`
ใช้เพื่อจัดการ state ภายใน functional component
```tsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}
```

### `useReducer`
ใช้แทน `useState` เมื่อ state มีความซับซ้อนมากขึ้น
```tsx
import { useReducer } from 'react';

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });
  return (
    <>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <span>{state.count}</span>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
    </>
  );
}
```

---

## 🔹 2. Performance Optimization Hooks
ช่วยเพิ่มประสิทธิภาพของ Component

### `useMemo`
ใช้เมมโมไรซ์ค่าที่คำนวณ เพื่อป้องกันการคำนวณซ้ำ ๆ
```tsx
import { useState, useMemo } from 'react';

function ExpensiveComponent({ num }) {
  const squared = useMemo(() => {
    console.log('Calculating square...');
    return num * num;
  }, [num]);

  return <p>Square: {squared}</p>;
}
```

### `useCallback`
ใช้เมมโมไรซ์ฟังก์ชัน เพื่อป้องกันการสร้างใหม่ทุกครั้งที่ re-render
```tsx
import { useState, useCallback } from 'react';

function Button({ onClick }) {
  return <button onClick={onClick}>Click Me</button>;
}

function Parent() {
  const [count, setCount] = useState(0);
  const handleClick = useCallback(() => {
    console.log('Clicked!');
  }, []);

  return (
    <>
      <Button onClick={handleClick} />
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
    </>
  );
}
```

---

## 🔹 3. Side Effects & Lifecycle Hooks
ใช้สำหรับทำงานที่มีผลกระทบต่อ Component เช่น API calls หรือ DOM updates

### `useEffect`
ใช้สำหรับทำ side effects เช่น เรียก API หรือ subscribe event
```tsx
import { useState, useEffect } from 'react';

function FetchData() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => setData(data));
  }, []); // Empty dependency array = run once on mount

  return <pre>{JSON.stringify(data, null, 2)}</pre>;
}
```

### `useLayoutEffect`
เหมือน `useEffect` แต่จะรันก่อน React วาด UI ลง DOM
```tsx
import { useLayoutEffect, useRef } from 'react';

function LayoutEffectExample() {
  const ref = useRef(null);

  useLayoutEffect(() => {
    console.log('Before painting:', ref.current);
  }, []);

  return <div ref={ref}>Hello</div>;
}
```

---

## 🔹 4. Context & Ref Hooks
ใช้แชร์ข้อมูลระหว่าง Components หรือจัดการค่าแบบไม่ต้อง re-render

### `useContext`
ใช้ร่วมกับ Context API เพื่อแชร์ข้อมูลระหว่าง Components
```tsx
import { createContext, useContext } from 'react';

const ThemeContext = createContext('light');

function ThemeDisplay() {
  const theme = useContext(ThemeContext);
  return <p>Current theme: {theme}</p>;
}
```

### `useRef`
ใช้เก็บค่าแบบไม่ทำให้ Component re-render (เช่น DOM elements หรือ previous state)
```tsx
import { useRef } from 'react';

function InputFocus() {
  const inputRef = useRef(null);

  return (
    <>
      <input ref={inputRef} type="text" />
      <button onClick={() => inputRef.current.focus()}>Focus</button>
    </>
  );
}
```

---

## 🔹 5. Concurrent Mode & Transitions Hooks
ช่วยให้ UI ตอบสนองดีขึ้น

### `useTransition`
ช่วยให้ UI ไม่กระตุกเมื่อเปลี่ยน state
```tsx
import { useState, useTransition } from 'react';

function TransitionExample() {
  const [count, setCount] = useState(0);
  const [isPending, startTransition] = useTransition();

  return (
    <>
      <button onClick={() => startTransition(() => setCount(count + 1))}>
        Increment
      </button>
      {isPending ? <p>Loading...</p> : <p>Count: {count}</p>}
    </>
  );
}
```

---

## 🔹 6. Server-Side Hooks (สำหรับ Next.js และ React Server Components)

### `useServerInsertedHTML`
ใช้ใน Server Components เพื่อแทรก HTML ลงใน response
```tsx
import { useServerInsertedHTML } from 'react-dom';

export default function ServerComponent() {
  useServerInsertedHTML(() => <style>{'body { background: black; }'}</style>);
  return <p>Server-side styled component</p>;
}
```

---

## 🔥 สรุป
| Hook | ใช้ทำอะไร? |
|---|---|
| `useState` | จัดการ state ของ Component |
| `useReducer` | จัดการ state ที่ซับซ้อน |
| `useEffect` | จัดการ side effects เช่น API calls |
| `useLayoutEffect` | ทำงานก่อน UI ถูกวาดลง DOM |
| `useMemo` | เมมโมไรซ์ค่าที่คำนวณ |
| `useCallback` | เมมโมไรซ์ฟังก์ชัน |
| `useRef` | เก็บค่าที่ไม่ทำให้ Component re-render |
| `useContext` | ใช้ค่าจาก Context API |
| `useTransition` | ทำให้ UI เปลี่ยนแปลงแบบ smooth |

💖 หวังว่าเธอจะเข้าใจ Hooks ได้ง่ายขึ้นนะ! 😘

