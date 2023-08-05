## Free bootstrap template
bootstrapmade.com
https://themes.getbootstrap.com/
## Layouts
### Container
https://getbootstrap.com/docs/5.3/layout/containers/

https://kb.hostvn.net/container-trong-bootstrap-4_849.html

Containers are the most basic layout element in Bootstrap and are **required when using our default grid system**. 
Trong Bootstrap, container được dùng để đặt lề cho nội dung. Nó chứa các thông tin column của nội dung

Containers thường được sử dụng để bọc nội dung bên trong chúng và có 2 container class:

- `.container` cung cấp một container với chiều rộng tương thích (responsive fixed width container).
- `.container-fluid` cung cấp một container có chiều rộng đầy đủ, trải rộng toàn bộ chiều rộng của khung nhìn (full width container).

`.container` có chiều rộng tối đa reponsive với các màn hình khác nhau như dưới đây

||Extra small<br><br><576px|Small<br><br>≥576px|Medium<br><br>≥768px|Large<br><br>≥992px|X-Large<br><br>≥1200px|XX-Large<br><br>≥1400px|
|---|---|---|---|---|---|---|
|`.container`|100%|540px|720px|960px|1140px|1320px|
|`.container-sm`|100%|540px|720px|960px|1140px|1320px|
|`.container-md`|100%|100%|720px|960px|1140px|1320px|
|`.container-lg`|100%|100%|100%|960px|1140px|1320px|
|`.container-xl`|100%|100%|100%|100%|1140px|1320px|
|`.container-xxl`|100%|100%|100%|100%|100%|1320px|
|`.container-fluid`|100%|100%|100%|100%|100%|100%|




## Grid 
https://niithanoi.edu.vn/hoc-bootstrap.html#28-can-chinh-cac-cot-trong-bootstrap

Làm thế nào để tạo hàng và cột bằng cách sử dụng hệ thống lưới responsive 12 cột của bootstrap
- Lúc đầu hãy tạo một container  bao bọc cho các hàng (row) và cột (column) của bạn bằng cách sử dụng lớp `.container`
- Sau đó tạo các hàng bên trong vùng chứa bằng cách sử dụng lớp `.row` và để tạo các cột bên trong bất kỳ hàng nào bạn có thể sử dụng các lớp `.col-*`, `.col-sm-*`, `.col-md-*`, `.col-lg-*` và `.col-xl-*`.
```html
<div class="container">
    <!-- Hàng với 2 cột bằng nhau -->

    <div class="row">
        <div class="col-md-6">Cột trái</div>
        <div class="col-md-6">Cột phải</div>
    </div>
    
    <!-- Hàng với 2 cột chia tỷ lệ 1:2 -->
    <div class="row">
        <div class="col-md-4">Cột trái</div>
        <div class="col-md-8">Cột phải</div>
    </div>
</div>
```

## Background Image
Căn bản: https://www.youtube.com/watch?v=zHZRFwWQt2w

```css
#hero{
background-color: #064210;
background: url('assets/bg-moutain.jpg') no-repeat center center;
-webkit-background-size: cover;
-moz-background-size: cover;
-o-background-size: cover;
background-size: cover;
}
```
## Image tag
## responsive
`img-fluid` trong thẻ **<img>**. Hình ảnh sẽ reponsive đúng tỷ lệ, **height: auto**. Mặc định, class **.img-fluid** cung cấp **max-width: 100%**.
Do đó ta có thể điều chỉnh kích thước ảnh bằng cách chỉnh thuộc tính max-height trong CSS
```css
#iphone{
max-height: 80vh;
}
```
### aligning (căn vị trí)
class `.float-left` hình ảnh sẽ tự động đẩy về bên trái. Ngược lại
class `.float-right` , hình ảnh sẽ tự động đẩy về bên phải.
Để căn giữa hình ảnh `.mx-auto`  (margin: auto) và `.d-block`(display: block).

## Spacing 

### Horizontal centering ( căn giữa theo chiều ngang)

Additionally, Bootstrap also includes an `.mx-auto` class for horizontally centering fixed-width block level content—that is, content that has `display: block` and a `width` set—by setting the horizontal margins to `auto`.

Giải thích display: trong css : https://viblo.asia/p/su-khac-nhau-giua-display-inline-block-va-inline-block-oOVlYNon58W
 