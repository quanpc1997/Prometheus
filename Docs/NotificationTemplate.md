# NOTIFICATION TEMPLATE REFERENCE

Prometheus tạo và gửi các cảnh báo đến Alertmanager, sau đó Alertmanager gửi thông báo ra ngoài cho những nền tảng nhận tin nhắn khác nhau dựa vào các label. 
Thông báo đã được gửi đến trình nhận tin nhắn được xây dựng thông qua các templates. Alertmanager đi kèm với những mẫu mặc định nhưng chúng cũng có thể được tùy chỉnh. 

## Data structures
### Data
Data là một cấu trúc được truyền thông qua các template thông báo và đẩy vào webhook.

| Tên               | Kiểu   | Chức năng                                                                                                      |
|-------------------|--------|----------------------------------------------------------------------------------------------------------------|
| Receiver          | string | Xác định tên của trình tin nhắn mà thông báo sẽ được gửi đến có thể là gmail, telegram, teams,...              |
| Status            | string | Xác định trạng thái firing nếu như một cảnh báo được bắn ra và ngược lại nếu được xử lý                        |
| Alerts            | Alert  | Danh sách alert object                                                                                         |
| GroupLabels       | KV     | Những nhãn các cảnh báo đã được nhóm.                                                                          |
| CommonLabels      | KV     | Các nhãn chung cho tất cả các cảnh báo                                                                         |
| CommonAnnotations | KV     | Đặt chú thích chung cho tất cả các cảnh báo. Được sử dụng cho các chuỗi thông tin bổ sung dài hơn về cảnh báo. |
| ExternalURL       | string | Backlink đến Alertmanager đã gửi thông báo.                                                                    |

### Alert

| Tên               | Kiểu     | Chức năng                                                                                                      |
|-------------------|----------|----------------------------------------------------------------------------------------------------------------|
| Status            | string   | Xác định cảnh báo được giải quyết hay đang bắn													                |
| Labels            | KV       | Một bộ nhãn được gắn vào cảnh báo.            										                            |
| Annotations       | KV       | Một chú thích được gán cho cảnh báo.                                                                           |
| StartsAt          | time.Time| Thời điểm cảnh báo bắt đầu được bắn. Nếu được bỏ qua thời gian hiện tại được gán cho Alertmanager.             |
| EndsAt            | time.Time| Chỉ đặt nếu thời gian kết thúc của cảnh báo được biết. 														|
| GeneratorURL      | string   | Một backlink xác định thực thể gây ra cảnh báo này.															|


### KV
KV là một tập hợp các cặp chuỗi key/value được sử dụng để thể hiện nhãn và chú thích.
```sh
type KV map[string]string
```

Ví dụ: Chú thích ở đây được bao gồm 2 chú thích nhỏ:
```sh
{
  summary: "alert summary",
  description: "alert description",
}
```

Ngoài việc truy cập trực tiếp dữ liệu (Labels và Annotations) được lưu dưới dạng KV, còn có các phương thức cho việc sắp xếp, xóa và xem các Nhãn:

**Phương thức trong KV**

| Tên         | Tham số  | Trả về                             | Ý nghĩa                                                      |
|-------------|----------|------------------------------------|--------------------------------------------------------------|
| SortedPairs | _        | Danh sách các cặp chuỗi key/values | Trả về danh sách được sắp xếp của key/value                  |
| Remove      | []string | KV                                 | Trả về một bản sao của key/value mà không cần các key đã cho |
| Names       | _        | []string                           | Trả về tên của các Label names trong LabelSet                |
| Values      | _        | []string                           | Trả về một danh sách các value của LabelSet                  |

### Functions
Lưu ý các chức năng mặc định cũng được cung cấp bởi Go templating.

| Tên          | Tham số                    | Kết quả                                                                                                                                                                                                    |
|--------------|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| title        | string                     | strings.Title, Viết hoa kí tự đầu tiên mỗi từ.                                                                                                                                                             |
| toUpper      | string                     | strings.ToUpper, Đổi tất cả sang dạng chữ hoa.                                                                                                                                                             |
| toLower      | string                     | strings.ToLower, Đổi tất cả sang dạng chữ thường.                                                                                                                                                          |
| match        | pattern, string            | Regexp.MatchString. Sử dụng chuỗi theo định dạng Regexp.                                                                                                                                                   |
| reReplaceAll | pattern, replacement, text | Regexp.ReplaceAllString Regexp substitution, unanchored.                                                                                                                                                   |
| join         | sep string, s []string     | strings.Join, nối các phần tử của s để tạo một chuỗi đơn. The separator string sep is placed between elements in the resulting string. (note: argument order inverted for easier pipelining in templates.) |
| safeHtml     | text string                | html/template.HTML, Đánh dấu chuỗi là HTML không yêu cầu tự động thoát
