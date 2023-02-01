---
layout: post
title: To Do List
description: >
  [참조] 노마드 코더 ReactJS로 영화 웹 서비스 만들기
sitemap: true
hide_last_modified: true
categories:
  - web
  - react
---

# To Do List

* toc
{:toc .large-only}

## To Do List 입력창

**App.js**

```js
function App() {
  const [toDo, setTodo] = useState("");
  const [toDos, setToDos] = useState("");
  const onChange = (event) => setTodo(event.target.value);
  const onSubmit = (event) => {
    event.preventDefault();
    if (toDo === "") {
      return;
    }
    setToDos((currentArray) => [toDo, ...currentArray]);
    setTodo("");
  };
  console.log(toDos);
  return (
    <div>
      <h1>My To Dos ({toDos.length})</h1>
      <form onSubmit={onSubmit}>
        <input
          onChange={onChange}
          value={toDo}
          type="text"
          placeholder="Write your to do..."
        />
        <button>Add To Do</button>
      </form>
    </div>
  );
}
```

- `setToDos((currentArray) => [toDo, ...currentArray])`: `currentArray`의 원소만 꺼내서 배열에 추가됨



<span style="font-size:70%">[참조] 노마드 코더 ReactJS로 영화 웹 서비스 만들기</span>

끝!