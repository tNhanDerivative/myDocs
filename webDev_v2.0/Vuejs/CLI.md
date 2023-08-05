https://viblo.asia/s/bi-kip-luyen-cong-vuejs-rLZDX4AEZk0


## Tổng quan



## Set up project
```
vue create vue-starter-project
```

## CLI file structure

```
project
│       
│
└─── public
│       index.html   
│   
└─── src
│   │   App.vue
│   │   main.js
│   │
│   └─── assets
│	│       A
│   │   
│   │
│   └─── components
			HelloWorld.vue


```

`src/` folder chứa tất cả những component, template và CSS

## Multiple components

![multiple-components](multiple-components.png "multiple components")
mình sử dụng npm (node package manager) . Từng component sẽ tách thành những file riêng lẻ và sử dụng export . Cuối cùng những component nào cần sử dụng chúng ta sẽ import nó vào. và khai báo như kiểu đăng kí cục bộ ở trên.

Để chạy ứng dụng, sử dụng `npm run dev`

### Export component
HelloWorld.vue
```vue
<template>
    ...
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>
```
Đây chính là cách chúng ta tạo ra một file Component và export để sử dụng lại sau.  đơn giản nó chỉ là *export* ra một *Component* có template được định nghĩa ở trong cặp thẻ `<template> </template>` , và có data được khởi tạo với một biến msg là `'Welcome to Your Vue.js App'`.

### Import component
Chúng ta tạo một file có tên Input.vue ở trong folder `src/components`
Input.vue
```vue
<template>
  <div class="input">
    <input type="email" placeholder="Nhập email"/>
  </div>
</template>
<script>
export default {
  name: 'Input'
}
</script>
```
Đơn giản component này chỉ có nhiệm vụ là tạo ra một ô input. Và chúng ta import vào Helloworld.vue
Helloworld.vue
```vue
<template>
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    <Input/> <!-- Sử dụng Component Input  trong Component HelloWorld --> 
    ...
</template>

<script>
import Input from './Input.vue' //dòng này để import từ file Input.vue
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }, 
  components: {Input} //dòng này để khai báo component con của Component Helloworld là Component Input
}
</script>
```

Chúng ta có 2 cái mới được thêm vào đấy là ô input khi hiển thị ngoài trình duyệt và thêm một node `<Input>` vào cây *Component* khi mở Vue Extension* lên
![input.Vue](Input.png "input.vue")


