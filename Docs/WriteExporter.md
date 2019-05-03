# Write Exporter

## The Counter
Counter là một kiểu metrics mà có lễ chúng ta sẽ thường sử dụng. Counters theo dõi số lượng hoặc kích thước cửa các events. Chúng chủ yếu được sử dụng để theo dõi tần suất một đường dẫn mã cụ thể được thực thi. 

Ví dụ 3-3 dưới đây để thêm một metric nhằm tính số lần Hellow World được yêu cầu:
```sh
from prometheus_client import Counter
REQUESTS = Counter('hello_worlds_total',
 	'Hello Worlds requested.')
class MyHandler(http.server.BaseHTTPRequestHandler):
 	def do_GET(self):
 	REQUESTS.inc()
 	self.send_response(200)
 	self.end_headers()
 	self.wfile.write(b"Hello World")

```
Chương trình trên có 3 phần gồm: Import thư viện, định nghĩa metrics và đo đạc.

**Import thư viện**
Python yêu cầu chúng ta import những hàm và class từ những modules khác nhau mà chương trình sử dụng. Do đó, chúng ta phải import Counter class từ thư viện _prometheus_client_ ngay đầu chương trình.

**Định nghĩa metric**
Prometheus metrics phải được định nghĩa trước khi chúng ta sử dung. Ở đây tôi định nghĩa một counter được gọi là _hello_worlds_total_. Chuỗi _Hello Worlds requested_ sau đó sẽ xuất hiện trong _/metric_ page để giúp chúng ta hiểu ý nghĩa của metrics đó.

Metrics được tự động đăng kí với thư viện client trong _default registry_. Bạn không cần pull metric trở lại _start_http_server_. Lưu ý rằng, metric phải có tên duy nhất.

**Đo đạc(Instrumentation)**
Bây giờ chúng ta đã có những metric đã được định nghĩa. Chúng ta có thể sử dụng nó. Phương thức _inc_ tăng giá trị cho counter lên một đơn vị.

Khi bạn chạy một chương trình, metrics mới sẽ được xuất hiện trong _/metrics_. Nó sẽ bắt đầu từ 0 và tăng lên một đơn vị sau mỗi lần mà truy cập vào URL của ứng dụng. 

Counter rất hữu ích để theo dõi số lần xảy ra các tình huống bất ngờ xảy ra. 
