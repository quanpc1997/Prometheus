# Storage

Prometheus cung cấp 2 giải pháp lưu trữ những giá trị dựa trên chuỗi thời gian(time series) là local storage và remote storage.

## Local storage
Dữ liệu được lưu trữ theo một định dạng tùy chỉnh trên disk. 

### On-disk layout
Các samples được gom trong 2h để nhóm thành một block. Mỗi block chứa một hoặc nhiều tệp chunk. Những tệp chunk này chứa tất cả các dữ liệu trên chuỗi thời gian, cũng như một tệp metadata và tệp index(trong đó những tên indexes metric và labels sẽ theo chuỗi thời gian trong các tệp chunk). Khi series bị xóa thông qua API, các bản ghi sẽ được lưu trữ trong các tệp tombstone riêng biệt thay vì xóa dữ liệu ngay lập tức khỏi các tệp chunk)


Block cho phép các samples đang đến được giữ trong RAM. Điều này chống lại việc một bản ghi(WAL) nào đó có thể được gửi lại khi máy chủ Prometheus gặp sự cố phải khởi động lại. Các tệp Write-ahead log(WAL) được lưu trữ trong thư mục _wal_ trong 128MB segment. Các tệp chứa dữ liệu thô vì chưa được nén nên chúng có dung lượng đáng kể so với thông thường. Prometheus sẽ giữ tối thiểu ba bản ghi nhật kí trước. Tuy nhiên các máy chủ lưu lượng cao có thể thấy nhiều hơn ba tệp WAL vì nó cần dữ liệu thô ít nhất 2h.

Cấu trúc thư mục của một Prometheus server's data sẽ trông giống như sau:

```sh
./data/01BKGV7JBM69T2G1BGBGM6KB12
./data/01BKGV7JBM69T2G1BGBGM6KB12/meta.json
./data/01BKGTZQ1SYQJTR4PB43C8PD98
./data/01BKGTZQ1SYQJTR4PB43C8PD98/meta.json
./data/01BKGTZQ1SYQJTR4PB43C8PD98/index
./data/01BKGTZQ1SYQJTR4PB43C8PD98/chunks
./data/01BKGTZQ1SYQJTR4PB43C8PD98/chunks/000001
./data/01BKGTZQ1SYQJTR4PB43C8PD98/tombstones
./data/01BKGTZQ1HHWHV8FBJXW1Y3W0K
./data/01BKGTZQ1HHWHV8FBJXW1Y3W0K/meta.json
./data/01BKGV7JC0RY8A6MACW02A2PJD
./data/01BKGV7JC0RY8A6MACW02A2PJD/meta.json
./data/01BKGV7JC0RY8A6MACW02A2PJD/index
./data/01BKGV7JC0RY8A6MACW02A2PJD/chunks
./data/01BKGV7JC0RY8A6MACW02A2PJD/chunks/000001
./data/01BKGV7JC0RY8A6MACW02A2PJD/tombstones
./data/wal/00000000
./data/wal/00000001
./data/wal/00000002
```
Các block ban đầu cuối cùng sẽ bị nén thành các khối dài hơn.


Chú ý local storage có hạn chế rằng bộ nhớ khó thể nhóm lại hay mở rộng. Do đó, nó không thể tùy ý mở rộng hoặc bảo toàn với các sự cố ngừng hoạt động của disk hoặc node. Tuy nhiên, nếu yêu cầu về độ bền của bạn không nghiêm ngặt, bạn vẫn có thể thành công trong việc lưu trữ tới nhiều năm dữ liệu trong bộ nhớ cục bộ.

Để biết thêm chi tiết về định dạng tệp, xem thêm [Định dạng TSDB](https://github.com/prometheus/tsdb/blob/master/docs/format/README.md)

## Operational aspects
Prometheus có nhiều flags chấp nhận cấu hình trên local storage. Một số tham số quan trọng là:

	* _--storage.tsdb.path_: Quy định vị trí lưu dữ liệu của Promethes. Mặc định là _data/_.
	
	* _--storage.tsdb.retention.time_: Quy định thời gian dữ liệu được lưu trữ. Mặc định là 15 ngày.
	
	* _--storage.tsdb.retention.size_: Quy định dung lượng tối đa của block(Điều này không bao gồm dung lượng của WAL). Dữ liệu cũ nhất sẽ được xóa đi đầu tiên. Mặc định tùy chọn này bị vô hiệu hóa. Cờ này chỉ mang tính chất trải nhiệm và có thể bị thay đổi trong tương lai. Các đơn vị hỗ trợ: KB, MB, GB, PB.

Trung binh, Prometheus chỉ sử dụng 1-2 bytes cho mỗi sample. Do vậy, để lập kế hoạch dung lượng của máy chủ Prometheus, bạn có thể sử dụng công thức sau:
```sh
# Dung lượng đĩa cần = Thời gian lưu trữ dữ liệu * số gói được tích hợp mối giây * số bytes cho mỗi sample
needed_disk_space = retention_time_seconds * ingested_samples_per_second * bytes_per_sample
```

Nếu Local storage bị hỏng vì bất kỳ lý do gì, cách tốt nhất là tắt Prometheus đi và xóa toàn bộ thư mục lưu trữ. Các tệp hệ thống không tuân thủ POSIX sẽ  không được hỗ trợ bởi local storage của Prometheus, các lỗi có thể xảy ra và không có khả năng để phục hồi. NFS chỉ có khả năng POSIX, Hầy hết các triển khai đều không có. Bạn có thể thử xóa các thư mục riêng lẻ để giải quyết vấn đề.

Nếu cả chính sách duy trì thời gian và kích thước được chỉ định, thì bất kì chính sách trigger nào sẽ được sử dụng ngay lúc đó.

## Remote storage integrations
Lưu trữ cục bộ của Prometheus bị giới hạn bởi các nút đơn lẻ về khả năng mở rộng và độ bền. Thay vì cố gắng giải quyết lưu trữ phân cụm trong chính Prometheus, Prometheus có một bộ giao diện cho phép tích hợp với các hệ thống lưu trữ từ xa.

### Tổng quan
Tích hợp Prometheus cùng với hệ thống remote systems bằng một trong 2 con đường sau:

	* Prometheus có thể ghi những samples mà nó thu nhập từ memote URL theo định dạng chuẩn.
	
	* Prometheus có thể đọc dữ liệu sample từ một remote URL theo một chuẩn định sẵn.

![Tích hợp Prometheus cùng với hệ thống remote systems](./Images/remote_integrations.png)

Cả 2 giao thức đọc và ghi đều sử dụng giao thức mã hóa bộ đệm được nén chặt chẽ qua HTTP. Các giao thức chưa được coi là API ổn định và có thể thay đổi để sử dụng gRPC qua HTTP/2 trong tương lai, khi tất cả các bước nhảy giữa Prometheus và remote storage có thể được coi là hỗ trợ HTTP/2 một cách an toàn.

Để xem chi tiết cấu hình remote storage tích hợp trong Prometheus, xem [remote write](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#remote_write) và [remote read]https://prometheus.io/docs/prometheus/latest/configuration/configuration/#remote_read) 

Để xem chi tiết request và response message, xem [remote storage protocol buffer definitions](https://github.com/prometheus/prometheus/blob/master/prompb/remote.proto)

Lưu ý rằng trên read path, Prometheus chỉ tìm nạp dữ liệu chuỗi thô cho một label selectors và time ranges từ đầu cuối. Tất cả các đánh giá PromQL trên dữ liệu thô vẫn xảy ra trong chính Prometheus. Điều này có nghĩa là các truy vấn đọc từ xa có một số giới hạn về khả năng mở rộng, vì tất cả dữ liệu cần thiết cần được tải vào máy chủ Prometheus truy vấn trước rồi xử lý ở đó. Tuy nhiên, việc hỗ trợ đánh giá phân phối đầy đủ về PromQL được coi là không khả thi trong thời điểm hiện tại.

## Existing integrations
Để học nhiều hơn về những tích hợp hiện có cùng với hệ thống remote storage. Xem thêm [Tích hợp](https://prometheus.io/docs/operating/integrations/#remote-endpoints-and-storage)
