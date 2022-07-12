---
title: 'Tổng quan React'
date: 2022-07-12T20:30:00+07:00
draft: false
slug: 'tong-quan-react'
description: 'Giới thiệu tổng quan về thư viện React thông qua việc xây dựng ứng dụng 
hiển thị công việc đơn giản sử dụng React và Javascript thuần.'
summary: 'Giới thiệu tổng quan về thư viện React thông qua việc xây dựng ứng dụng 
hiển thị công việc đơn giản sử dụng React và Javascript thuần.'
tags:
- React
- Tổng quan React
---

> A JavaScript library for building user interfaces

Từ trang chủ của `React`, ta có thể thấy rõ rằng `React` là 1 thư viện
của ngôn ngữ `Javascript` được dùng để xây dựng giao diện người dùng.

Trong bài viết này, ta sẽ cùng nhau xây dựng 1 ứng dụng `todo` đơn giản sử
dụng ngôn ngữ `Javascript` thuần. Và sau đó xây dựng với thư viện `React`
để so sánh và có cái nhìn tổng quan nhất về `React`.

## 1. Web API

// TODO: Viết bài về tổng quan mô hình client - server

// TODO: Viết bài về kiến trúc API

Để đơn giản hóa trong dự án này, chúng ta sẽ sử dụng 1 `API` có sẵn từ
trang web: [https://jsonplaceholder.typicode.com/](https://jsonplaceholder.typicode.com/).

Cụ thể hơn, chúng ta sẽ sử dụng `API` `todos`. `API` này có cách sử dụng
rất đơn giản, được trình bày rõ ở trang web phía trên. Ví dụ, để lấy danh
sách công việc, chúng ta có thể làm như sau:

- Gửi 1 yêu cầu `HTTP` đến `Web API` tại `URL`: [https://jsonplaceholder.typicode.com/todos](https://jsonplaceholder.typicode.com/todos)
- Dữ liệu trả về sẽ bao gồm 1 mảng gồm có 200 đối tượng công việc
- Một đối tượng công việc bao gồm các thuộc tính như dưới đây:

  ```json
  {
    "userId": 1,
    "id": 1,
    "title": "delectus aut autem",
    "completed": false
  }
  ```

## 2. Xây dựng ứng dụng `Todo` với `Javascript` thuần

Bây giờ, sau khi đã có dữ liệu từ `API`, đã đến lúc chúng ta bắt tay vào
xây dựng dự án.

Tiến hành tạo tập tin `index.html` và `script.js` trong cùng 1 `folder`.

Nội dung trong tập tin `index.html` như dưới đây, rất đơn giản:

- Định kiểu nhanh thông qua thư viện `Bootstrap` phiên bản `5`
- Thẻ `div` với `id` `root` là nơi chúng ta dùng `Javascript` để chèn
  nội dung vào
- Bên dưới có thẻ `script` để liên kết đến tập tin `script.js`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0-beta1/dist/css/bootstrap.min.css"
    />
    <title>Todo JavaScript</title>
  </head>
  <body>
    <div id="root"></div>

    <script src="script.js"></script>
  </body>
</html>
```

Về phần tập tin `Javascript`, chúng ta sẽ làm các công việc sau:

- Dùng hàm `fetch` gửi yêu cầu đến địa chỉ `Web API` để lấy về danh
  sách công việc
- // TODO: Viết bài về kĩ thuật AJA
- Tạo nội dung cho trang `web` và chèn vào `DOM`

Đoạn mã hoàn chỉnh như dưới đây:

```js
fetch('https://jsonplaceholder.typicode.com/todos')
  .then((res) => res.json())
  .then((data) => {
    console.log(data);

    const h1 = document.createElement('h1');
    h1.innerHTML = 'Danh sách công việc hôm nay';
    h1.classList.add('my-5', 'text-primary', 'text-center');

    const ul = document.createElement('ul');
    ul.classList.add('list-group');

    for (const todo of data) {
      const li = document.createElement('li');
      li.innerHTML = t.title;
      li.classList.add('list-group-item');

      ul.appendChild(li);
    }

    const div = document.createElement('div');
    div.classList.add('container');

    div.appendChild(h1);
    div.appendChild(ul);

    const root = document.getElementById('root');
    root.appendChild(div);
  });
```

Ở đoạn mã trên, sau khi nhận được danh sách công việc phản hồi từ `API`, chúng ta tiến hành thực hiện các công việc sau:

- Tạo đối tượng `h1` chứa tiêu đề của trang và thêm vào 1 số `class` định
  kiểu có sẵn từ `bootstrap`
- Tạo đối tượng `ul` chứa danh sách công việc
- Với mỗi đối tượng `todo` trong danh sách kết quả trả về, chúng ta tiến
  hành tạo 1 đối tượng `li` tương ứng và chèn vào bên trong đối tượng `ul`
- Khai báo 1 `container` để chứa đối tượng `h1` và `ul`. Sau đó, chọn
  đối tượng `root` và chèn `container` vào.

Như vậy chúng ta đã hiển thị được danh sách công việc sử dụng ngôn ngữ
`Javascript` rất đơn giản.

![Image](/self-study/react/overview/todo-js.png)

## 3. Xây dựng ứng dụng `Todo` với `React`

Cũng với ứng dụng tương tự như phía trên, nhưng lần này, chúng ta sẽ thử
sử dụng thư viện `React` để thực hiện.

Đầu tiên, chúng ta cần cài đặt môi trường cần thiết để phát triển ứng dụng:

- Cài công cụ `NodeJS`, phiên bản được dùng tại bài viết này là `14.15.0`
- Kiểm tra công cụ `npm`, `npx`

// TODO: Viết bài hướng dẫn cách cài đặt môi trường

Sau khi đã có môi trường phát triển, chúng ta có thể nhanh chóng tạo
dự án `React` mới thông qua câu lệnh sau. Trong đó, `todo-react` là
tên của dự án

```sh
npx create-react-app todo-react
```

Ta có thể khởi động dự án `React` nhanh thông qua câu lệnh

```sh
npm start
```

Nội dung của tập tin `index.js` như dưới đây:

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';

class App extends React.Component {
  state = {
    title: 'Danh sách công việc hôm nay',
    todos: [],
  };

  componentDidMount() {
    fetch('https://jsonplaceholder.typicode.com/todos')
      .then((res) => res.json())
      .then((data) => {
        this.setState({
          todos: data,
        });
      });
  }

  renderTodoList() {
    const allLi = [];

    for (const t of this.state.todos) {
      allLi.push(<li className='list-group-item'>{t.title}</li>);
    }

    return allLi;
  }

  render() {
    return (
      <div className='container'>
        <h1 className='text-primary text-center my-5'>{this.state.title}</h1>
        <ul className='list-group'>{this.renderTodoList()}</ul>
      </div>
    );
  }
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

Để định kiểu nhanh với `bootstrap`, ta tiến hành chỉnh sửa tập tin `index.html` trong thư mục `public`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="Web site created using create-react-app"
    />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0-beta1/dist/css/bootstrap.min.css"
    />
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <title>Cong viec Todo</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

Lúc này ta sẽ có được chương trình `todo` giống hệt như khi làm với `Javascript` ở trên.

![Image](/self-study/react/overview/todo-js.png)

Về cái nhìn tổng quan,

- 2 dòng đầu tiên tiến hành chèn `module` `react` vào `module` hiện tại // TODO: Viết bài về module và cú pháp import es
- Khai báo `component` `App` // TODO: Viết bài về component trong react
- Chèn `component` này vào đối tượng `root` trong cây `DOM`

Đi sâu hơn, trong ví dụ phía trên, chúng ta đã tiếp xúc với 1 số khái niệm
cơ bản trong `react`, bao gồm:

- `Component`: `react` giúp chúng ta chia giao diện người dùng thành
  các khối nhỏ gọi là `component`. Thông qua việc chia nhỏ, các `component`
  này có thể được dễ dàng tái sử dụng và tập hợp lại với nhau để tạo nên
  1 giao diện người dùng hoàn chỉnh 1 cách linh hoạt.

- Có 2 cách khai báo `component`:

  - Thông qua `class`: như ví dụ phía trên, ta tạo ra `class` `App` và
    kế thừa từ `class` cha `React.Component`
  - Thông qua hàm hay còn gọi là `Function Component`

- `Component` có thể lưu trữ trạng thái (`state`) hoặc không lưu trữ trạng
  thái. Trong ví dụ phía trên, `component` của chúng ta lưu trữ trạng thái
  bao gồm 2 thuộc tính: tiêu đề trang và danh sách công việc. Mỗi khi trạng
  thái thay đổi, `react` sẽ tự động thực thi phương thức `render` để cập nhật
  thay đổi lên giao diện người dùng.

- Phương thức `render`, đây là phương thức cơ bản nhất mà chúng ta cần
  phải ghi đè (`override`) khi khai báo `Class Component`. Phương thức
  này sẽ được tự động thực thi mỗi khi trạng thái (`state`) của `component`
  thay đổi. Tại phương thức này, chúng ta sử dụng cú pháp `jsx` để xây dựng
  cấu trúc `html`.

- Phương thức `componentDidMount`, đây là 1 trong những phương thức thuộc
  về `react lifecycle methods`. Hiểu đơn giản, phương thức này sẽ được tự động
  thực thi 1 lần duy nhất khi `component` `App` được gắn vào `DOM`. Tại phương
  thức này, ta thực hiện kĩ thuật `AJAX` để gửi yêu cầu đến `Web API`. Đồng
  thời, khi nhận được dữ liệu, ta tiến hành thay đổi trạng thái để cập nhật
  lại giao diện người dùng.

## 4. Đánh giá chủ quan

Như vậy, chúng ta đã xây dựng ứng dụng hiển thị danh sách công việc đơn giản
sử dụng 2 công nghệ:

- `Javascript` thuần
- Thư viện `React`

Giờ là lúc để tổng kết lại những điểm giống nhau và khác nhau giữa 2 công
nghệ này và lí do tại sao nên dùng `react`.

Giống nhau:

- Đều xây dựng được ứng dụng mong muốn
- Sử dụng kĩ thuật `AJAX` để lấy dữ liệu từ `Web API`

Điểm khác nhau cơ bản nhất nằm ở việc tạo các đối tượng `HTML`.

- Khi dùng `Javascript` thuần, chúng ta tiến hành gọi các phương thức:
  `createElement`, `appendChild`, `classList`, ... để tạo đối tượng mới,
  chèn đối tượng này vào đối tượng khác và xử lý việc thêm các lớp định kiểu.
  Khi cấu trúc `HTML` trở nên phức tạp hoặc cần gắn các sự kiện, việc tạo
  thủ công sẽ trở nên mất kiểm soát do số lượng `code` tạo ra quá nhiều.

- `React` cung cấp cú pháp `jsx` giúp đơn giản hóa việc tạo và gắn sự kiện
  trên các đối tượng `HTML`. Với `jsx`, các đối tượng được viết thông qua các
  thẻ, việc chèn biến hoàn toàn dễ dàng thông qua cặp dấu `{}`. Ngoài ra,
  `component` cung cấp 1 cách thức để phân rã giao diện người dùng
  phức tạp thành những thành phần nhỏ và đơn giản.

## 5. Kết luận

Như vậy, bài viết này đã giới thiệu về `React` một cách rất tổng quan. Trong
quá trình này, chúng ta đã cùng nhau xây dựng ứng dụng `todo` đơn giản và
so sánh giữa 2 công nghệ `Javascript` thuần và `React`. Từ đó rút ra những
điểm giống và khác nhau giữa 2 công nghệ.

Hẹn gặp các bạn ở những bài viết sau.
