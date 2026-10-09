# KIỂM THỬ API BẰNG POSTMAN


## 1. Mục tiêu

- Làm quen với giao diện Postman và cách tổ chức các request.
- Gửi yêu cầu HTTP bằng các phương thức GET, POST, PUT và DELETE.
- Thiết lập URL, headers và dữ liệu JSON trong request body.
- Đọc mã trạng thái, nội dung và thời gian phản hồi.
- Viết test tự động để kiểm tra phản hồi và phát hiện sai kiểu dữ liệu.
- Sử dụng biến để tái sử dụng địa chỉ API.
- Vận dụng cách gửi GET vào API thời tiết.

## 2. Cơ sở lý thuyết

Postman hỗ trợ tạo và gửi yêu cầu đến API, quan sát phản hồi và viết các kiểm tra tự động. Một request thường gồm phương thức HTTP, URL, tham số, headers và body tùy chức năng của API.

| Thành phần | Vai trò trong bài thực hành |
| --- | --- |
| GET | Đọc dữ liệu khóa học hoặc bài viết |
| POST | Tạo một bài viết |
| PUT | Cập nhật/thay thế dữ liệu bài viết theo ID |
| DELETE | Xóa bài viết theo ID |
| Header `Content-Type: application/json` | Khai báo body gửi lên có định dạng JSON |
| Response body | Dữ liệu hoặc thông báo do API trả về |
| Status code | Kết quả xử lý ở mức HTTP, ví dụ `200 OK`, `201 Created` |
| Collection | Nhóm các request liên quan để quản lý và tái sử dụng |
| Variable | Lưu giá trị dùng chung, được tham chiếu bằng `{{ten_bien}}` |

Mã trạng thái thành công chưa đủ để kết luận API đáp ứng mọi yêu cầu. Cần kiểm tra thêm nội dung, kiểu dữ liệu và trạng thái dữ liệu sau thao tác.

## 3. Môi trường và dữ liệu tham khảo

### 3.1. Các thành phần quan sát được

| Thành phần | Giá trị trong ảnh nguồn |
| --- | --- |
| Công cụ | Postman trên máy tính |
| Địa chỉ API cục bộ | `http://localhost:3000` |
| Tài nguyên | `/courses`, `/posts`, `/posts/1` |
| Trường dữ liệu khóa học | `id`, `name`, `description` |
| Trường dữ liệu bài viết | `id`, `title`, `views` |
| Biến dùng lại URL | `url = http://localhost:3000/posts` |
| API mở rộng | API thời tiết hiện tại của WeatherAPI |

Repository được kiểm tra chỉ có `README.md`; chưa có mã nguồn API, tệp dữ liệu hay Postman Collection để tái chạy trực tiếp. Vì vậy, muốn thực hành lại cần chuẩn bị dịch vụ API tương ứng đang chạy trên cổng `3000`. `localhost` là máy đang chạy dịch vụ của người thực hành.

### 3.2. Chuẩn bị trên Postman

1. Mở Postman và tạo một collection để lưu bài thực hành.
2. Tạo các request tương ứng với từng trường hợp ở mục 4.
3. Với POST và PUT, chọn **Body → raw → JSON** và kiểm tra header `Content-Type`.
4. Nhập dữ liệu, chọn **Send**, sau đó quan sát status và response body.
5. Lưu request, thêm script kiểm tra phù hợp và chụp ảnh kết quả.

## 4. Nội dung thực hành và kết quả trên ảnh nguồn

### 4.1. GET — Đọc dữ liệu khóa học

**Request:**

```http
GET http://localhost:3000/courses
```

Không cần request body trong ví dụ này. Ảnh nguồn cho thấy API trả về `200 OK` và một đối tượng JSON:

```json
{
  "id": 1,
  "name": "Kiến thức cơ bản",
  "description": "Kiến thức cơ bản cho anh iem"
}
```

**Nhận xét:** Request đọc dữ liệu đã nhận được phản hồi thành công. Body trong ảnh là một đối tượng, vì vậy không nên mặc định endpoint này trả về danh sách.

![GET courses trả về 200 OK và dữ liệu khóa học](https://github.com/HgTrong/BAOCAO/assets/124127247/e33ee91c-8701-4788-9afc-efbb357919f2)

*Hình 1. Kết quả GET trong repository tham khảo.*

### 4.2. POST — Thêm bài viết

**Request:**

```http
POST http://localhost:3000/posts
Content-Type: application/json
```

**Body theo ảnh nguồn:**

```json
{
  "id": "3",
  "title": "Kiến thức nâng cao",
  "views": "Kiến thức nâng cao cho anh iem"
}
```

**Kết quả quan sát:** Postman hiển thị `201 Created`, response chứa dữ liệu vừa gửi. Ảnh kiểm tra `/posts` sau đó có bản ghi với ID `3`.

![POST posts trả về 201 Created](https://github.com/HgTrong/BAOCAO/assets/124127247/c442dd71-d202-43bf-b3ce-1511dfe61464)

*Hình 2. Gửi POST và nhận phản hồi tạo mới.*

![Danh sách posts có thêm bản ghi ID 3](https://github.com/HgTrong/BAOCAO/assets/124127247/912fe504-bccc-46f9-b47d-b3e4442f2684)

*Hình 3. Dữ liệu sau thao tác thêm, theo ảnh nguồn.*

**Nhận xét:** API chấp nhận dữ liệu không đồng nghĩa dữ liệu đúng về nghiệp vụ. Nếu `views` biểu diễn số lượt xem thì giá trị văn bản trong ví dụ không phù hợp; cần kiểm tra kiểu số nguyên không âm.

### 4.3. PUT — Cập nhật bài viết

**Request:**

```http
PUT http://localhost:3000/posts/1
Content-Type: application/json
```

**Body theo ảnh nguồn:**

```json
{
  "id": "1",
  "title": "hihihi",
  "views": "1000"
}
```

**Kết quả quan sát:** Phản hồi hiển thị `200 OK`. Ảnh danh sách sau cập nhật cho thấy bản ghi ID `1` có `title` là `hihihi` và `views` là chuỗi `"1000"`.

![PUT cập nhật bài viết ID 1](https://github.com/HgTrong/BAOCAO/assets/124127247/095bc474-4771-4aff-87ce-e0fe25523c3d)

*Hình 4. Request PUT và response.*

![Dữ liệu sau cập nhật bài viết](https://github.com/HgTrong/BAOCAO/assets/124127247/8dff73f0-a1a1-4972-a7c5-bc6e38c8cf2d)

*Hình 5. Kiểm tra lại dữ liệu sau PUT.*

**Nhận xét:** `"1000"` là chuỗi, còn `1000` là số trong JSON. Đây là khác biệt cần kiểm tra bằng script.

### 4.4. DELETE — Xóa bài viết

**Request:**

```http
DELETE http://localhost:3000/posts/1
```

Ảnh nguồn vẫn để dữ liệu trong body. Khi thực hành lại, cần tuân theo hợp đồng API; đối với thao tác xóa theo ID trong URL, không nên tự thêm body nếu API không yêu cầu.

**Kết quả quan sát:** Ảnh Postman hiển thị `200 OK` cùng đối tượng ID `1`; ảnh danh sách `/posts` sau thao tác không còn bản ghi này, chỉ còn các bản ghi ID `2` và `3`.

![DELETE bài viết ID 1](https://github.com/HgTrong/BAOCAO/assets/124127247/d7cb3aa9-b36a-4a21-a8be-20b63009a816)

*Hình 6. Request DELETE trong tài liệu nguồn.*

![Danh sách bài viết không còn ID 1](https://github.com/HgTrong/BAOCAO/assets/124127247/9448c3c8-4ba8-48bd-a7f7-bd0650a6d4aa)

*Hình 7. Kiểm tra dữ liệu sau xóa.*

**Nhận xét:** Nên xác nhận thao tác xóa bằng GET sau DELETE, không chỉ dựa vào mã trạng thái.

## 5. Viết test tự động và phân tích lỗi

Trong giao diện Postman hiện tại, script kiểm thử phản hồi được nhập ở **Scripts → Post-response**, kết quả xem tại **Test Results**. Một số phiên bản cũ dùng tên tab **Tests**. Cú pháp ví dụ bên dưới sử dụng `pm.test`, `pm.response` và `pm.expect` theo tài liệu Postman [3].

### 5.1. Kết quả test POST trong repository

![Kết quả test POST đạt 4 trên 5 với lỗi kiểu dữ liệu views](https://github.com/HgTrong/BAOCAO/assets/124127247/2f958738-5a56-4c4c-b87b-ddd9e31b0bd4)

*Hình 8. Bốn test PASS, một test FAIL trong ảnh nguồn.*

| Nội dung kiểm tra | Kết quả hiển thị |
| --- | --- |
| Status code bằng `201` | PASS |
| Có các trường `id`, `title`, `views` | PASS |
| `id` là chuỗi không rỗng | PASS |
| `title` là chuỗi không rỗng | PASS |
| `views` là số nguyên không âm | FAIL |

Thông báo lỗi trong ảnh có nội dung `expected '1000' to be a number`. Điều này cho thấy phản hồi chứa chuỗi `"1000"`, không đáp ứng điều kiện kiểu số. Ảnh test thuộc một lần gửi có dữ liệu khác ví dụ POST ở mục 4.2; không nên coi hai ảnh là cùng một lần chạy.

### 5.2. Đề xuất sửa dữ liệu và script POST

Nếu yêu cầu nghiệp vụ xác định `views` là số lượt xem, sửa body thành số, đồng thời dùng ID chưa tồn tại trong dữ liệu thực hành:

```json
{
  "id": "4",
  "title": "Thực hành kiểm thử Postman",
  "views": 1000
}
```

Script đề xuất cho POST:

```javascript
pm.test("POST trả về 201", function () {
    pm.response.to.have.status(201);
});

pm.test("Có đủ id, title và views", function () {
    const data = pm.response.json();
    pm.expect(data).to.be.an("object");
    pm.expect(data).to.have.property("id");
    pm.expect(data).to.have.property("title");
    pm.expect(data).to.have.property("views");
});

pm.test("id là chuỗi không rỗng", function () {
    const data = pm.response.json();
    pm.expect(data.id).to.be.a("string");
    pm.expect(data.id.trim().length).to.be.above(0);
});

pm.test("title là chuỗi không rỗng", function () {
    const data = pm.response.json();
    pm.expect(data.title).to.be.a("string");
    pm.expect(data.title.trim().length).to.be.above(0);
});

pm.test("views là số nguyên không âm", function () {
    const data = pm.response.json();
    pm.expect(data.views).to.be.a("number");
    pm.expect(Number.isInteger(data.views)).to.equal(true);
    pm.expect(data.views).to.be.at.least(0);
});
```

Điều kiện ID dạng chuỗi phù hợp với tài nguyên `/posts` trên ảnh nguồn; cần điều chỉnh nếu API thực tế quy định ID là số. Không chuyển `views` sang số ngay trong phép kiểm tra vì như vậy có thể che mất lỗi kiểu dữ liệu của phản hồi.

**Trạng thái:** Đây là phương án sửa đề xuất, chưa có kết quả chạy lại để xác nhận 5/5 PASS.

## 6. Sử dụng biến lưu URL

Ảnh nguồn khai báo biến global:

| Tên biến | Giá trị |
| --- | --- |
| `url` | `http://localhost:3000/posts` |

Sau khi lưu biến, request GET tham chiếu bằng:

```http
GET {{url}}
```

![Khai báo biến url trong Globals](https://github.com/HgTrong/BAOCAO/assets/124127247/0501cd37-c387-4774-b791-03247c892390)

*Hình 9. Khai báo URL dùng chung.*

![GET dùng biến url trả về 200 nhưng bộ test hiển thị 0 trên 5](https://github.com/HgTrong/BAOCAO/assets/124127247/e56bffc9-64c6-4964-a7da-157c867db090)

*Hình 10. GET dùng biến: HTTP 200, Test Results 0/5.*

**Phân tích:** Request nhận được dữ liệu, nhưng phần script vẫn kiểm tra status `201` và các trường trên một đối tượng như POST. Trong khi đó, GET `/posts` trên ảnh trả về `200` và một mảng. Đây là sự không phù hợp giữa script và request. Ảnh không mở chi tiết từng lỗi, nên chưa thể kết luận nguyên nhân riêng của cả năm test.

Script thay thế đề xuất cho GET `/posts`:

```javascript
pm.test("GET danh sách trả về 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Phản hồi là mảng bài viết", function () {
    pm.expect(pm.response.json()).to.be.an("array");
});

pm.test("Mỗi bài viết có đủ các trường", function () {
    const posts = pm.response.json();
    pm.expect(posts).to.be.an("array");
    posts.forEach(function (post) {
        pm.expect(post).to.have.property("id");
        pm.expect(post).to.have.property("title");
        pm.expect(post).to.have.property("views");
    });
});
```

Biến giúp thay đổi địa chỉ dùng chung mà không phải sửa từng request. Có thể tổ chức thêm biến `baseUrl = http://localhost:3000` và dùng `{{baseUrl}}/posts`, `{{baseUrl}}/courses`; đây là đề xuất mở rộng, không phải cấu hình đã có trong ảnh.

## 7. Thực hành mở rộng: API thời tiết

Ảnh cuối trong repository minh họa GET endpoint `current.json` của WeatherAPI với địa điểm `London`. Ảnh cho thấy `200 OK`, body có hai nhóm dữ liệu `location` và `current`, nhưng **Test Results hiển thị 0/5**. Script trên ảnh vẫn kiểm tra status `201` và các trường bài viết, nên không phù hợp với API thời tiết.

Ví dụ URL để cấu hình lại bằng biến riêng, sử dụng khóa của người thực hành:

```text
https://api.weatherapi.com/v1/current.json?key={{weather_api_key}}&q=London&aqi=no
```

Script đề xuất:

```javascript
pm.test("API thời tiết trả về 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Có thông tin địa điểm và thời tiết", function () {
    const data = pm.response.json();
    pm.expect(data).to.have.property("location");
    pm.expect(data).to.have.property("current");
    pm.expect(data.location).to.have.property("name");
    pm.expect(data.current).to.have.property("temp_c");
    pm.expect(data.current.temp_c).to.be.a("number");
});
```

Ảnh nguồn chứa khóa API trực tiếp trong URL, nên không được nhúng lại trong báo cáo này. Khi tự thực hành, cần che khóa trước khi chụp và thêm ảnh kết quả vào đây. Chưa có ảnh chạy lại hoặc kết quả xác nhận cho script đề xuất.

## 8. Tổng hợp kết quả

| STT | Nội dung | Kết quả quan sát từ nguồn | Đánh giá |
| --- | --- | --- | --- |
| 1 | GET `/courses` | `200 OK`, có dữ liệu khóa học | Đọc dữ liệu thành công trên ảnh |
| 2 | POST `/posts` | `201 Created`, bản ghi xuất hiện trong danh sách | Tạo dữ liệu thành công; `views` cần kiểm tra kiểu |
| 3 | PUT `/posts/1` | `200 OK`, dữ liệu ID `1` thay đổi | Cập nhật thành công; `views` vẫn là chuỗi |
| 4 | DELETE `/posts/1` | `200 OK`, danh sách sau xóa không có ID `1` | Có minh họa kiểm tra sau xóa |
| 5 | Test tự động cho POST | 4/5 PASS | Một lỗi kiểu dữ liệu `views` |
| 6 | GET qua biến `url` | `200 OK`, 0/5 test | Request có phản hồi; cần sửa bộ test |
| 7 | GET thời tiết | `200 OK`, 0/5 test | Có dữ liệu thời tiết; cần bộ test riêng |

Không cộng các hàng này thành tỷ lệ PASS chung vì chúng kết hợp thao tác HTTP, kiểm tra dữ liệu và nhiều bộ test ở những lần chạy khác nhau.

## 9. Bài học và kết luận

Qua việc phân tích nội dung thực hành trong repository, có thể hệ thống hóa quy trình kiểm thử API: chuẩn bị request, gửi yêu cầu, đọc phản hồi, kiểm tra dữ liệu và xác minh trạng thái sau thay đổi.

Các điểm rút ra:

- Phải phân biệt HTTP thành công với việc thỏa mãn toàn bộ test.
- Kiểu dữ liệu JSON ảnh hưởng trực tiếp đến kết quả kiểm thử.
- GET danh sách, POST tạo mới và API thời tiết cần các điều kiện kiểm tra khác nhau.
- Sau POST, PUT hoặc DELETE, cần đọc lại dữ liệu để kiểm chứng tác động.
- Ảnh minh họa phải thể hiện rõ method, URL, status, response và Test Results liên quan.

Báo cáo nguồn minh họa được các thao tác cơ bản của Postman, đồng thời thể hiện những lỗi cần xử lý. Để hoàn tất một lần thực hành cá nhân, cần chạy lại các request trong môi trường của mình, áp dụng bộ test phù hợp và ghi nhận kết quả thực tế sau sửa lỗi.
