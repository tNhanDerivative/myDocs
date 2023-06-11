## Khởi tạo Component

Component là thứ được nhắc đến ở đây. Là một trong những tính năng mạnh mẽ nhất của VueJS bởi khả năng tái sử dụng. Khi component chạy, Hook khởi tạo sẽ thiết lập những gì liên quan đến component, trước khi đưa vào DOM. Chúng ta không thể truy xuất DOM và phần tử đã được hook khởi tạo mounting (`this.$el`), em sẽ nói thêm ở phần Mounting.
![lifecycle](lifecycle.png "lifecycle hook")


### beforeCreate()
Thường dùng để khởi tạo Store (vuex), authentication
```javascript
  beforeCreate() {
    this.$store.commit('initializeStore')
    const token = this.$store.state.token

    if (token) {
      axios.defaults.headers.common['Authorization'] = "Token " + token
    } else {
      axios.defaults.headers.common['Authorization'] = ''
    }

  }
```
Đọc Document của djoser để hiểu call API này
https://djoser.readthedocs.io/en/latest/sample_usage.html
```javascript
      axios.defaults.headers.common['Authorization'] = "Token " + token
```

Đặc biệt beforeCreate() sẽ được gọi đồng bộ ngay sau khi Vue được khởi tạo. Các data (dữ liệu) và event (sự kiện) chưa được thiết lập. 
Do đó ta thay đổi các data object ở đây vì nó sẽ ảnh hưởng đến việc render html
Vue does not allow dynamically adding new root-level reactive properties to an already created instance. However, it’s possible to add reactive properties to a nested object
```javascript
data: () {
    return {
        someObject:{
            cars: true,
    }
}
```
and add the property with the [`vm.$set`] method:
```javascript
methods: {
        click_me: function () {
            this.$set(this.someObject, 'planes', true)
            //Vue.set(vm.someObject, 'planes', true)
        }
    }
```

### created()

-   Được gọi ngay khi object vue được tạo ra. Ở trạng thái này, thể hiện vue đã hoàn thành, bao gồm việc tính dữ liệu data, các thuộc tính tính toán trong computed, các phương thức viết trong **methods**, các event/watch. Ta có thể truy cập các thuộc tính này.
- Ta thường dùng để gọi API, AJAX để lấy dữ liệu gán vào các biến 
- 
### beforeMount()
- ít dùng
- 
### mounted()
Trong mounted hook, chúng ta có thể hoàn toàn truy cập đến component, template và DOM
Thường dùng để thay đổi chèn DOM hay tích hợp các thư viện khác như jQuery

### Các hook khác
https://viblo.asia/p/vuejs-life-cycle-hooks-OeVKBRXYKkW#_mounted-7