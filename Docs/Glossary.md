# Glossary - Bảng chú giải

## Alert
Một Alert là một cảnh báo trong Prometheus. Alert được gửi từ Prometheus đến Alertmanager.

## Alertmanager
Alertmanager nhận những cảnh báo, ttongr hợp chúng thành các nhóm, xử lý và gửi những thông báo đến email, Slack, Telegram,...

## Sample
Một Sample là một giá trị tại một thời điểm được lấy trong một chuỗi thời gian.
Trong Prometheus, mỗi sample bao gồm một giá trị float64 và mộtdấu thười gian được xác định đến milisecond.

## Bridge
Một Bridge là một thành phần mà lấy samples từ một client library và exposes(biểu diễn) chúng đến một nền tảng monitor system không phải Prometheus. Cho ví dụ, Python, Go, và Java clients có thể export metrics đến Graphite.

## Client library
một Client Library là một thư viện hỗ trợ một vài ngôn ngữ _(Go, Java, Python, Ruby,...)_ giúp việc lập trình lấy dữ liệu một cách dễ dàng.

## Collector
Một Collector là một phần của exporter mà đại diện cho một bộ các metrics. Nó có thể là một metrics nếu nó là một phần của direct instrumentation, hoặc nhiều metrics nếu nó được pull metrics từ một hệ thông khác.

## Direct instrumentation
Direct instrumentation là một instrumentation được thêm vào source code của chương trình.

## Endpoint
Là nơi lấy các dữ liệu metric.

## Exporter
Là một chương trình được sử dụng để thu nhập dữ liệu và chuyển đổi sang dạng mà Prometheus có thể hiểu được.

## Instance
Một instance là một label mà định danh một job, đối tượng duy nhất.

## Job
Job là một tập hợp các targets có cùng mục đích. Cho ví dụ việc monitoring một group của processes replicated for scalability or reliability, is called a job.

## Notification
Một Notification đại diện một nhóm của các cảnh báo và nó được gửi bởi Alertmanager đến email, Slack, Telegram,...

## Promdash
Promdash là một bảng quản lí động được xay dựng cho Prometheus. Nó đã không đùng nữa và được thay thế bởi Grafana.

## Prometheus 
Prometheus thường đề cập đến nhị phân cốt lõi của hệ thống Prometheus. Nó cũng có thể đề cập đến toàn bộ hệ thống giám sát Prometheus.

##PromQL
PromQL là Prometheus Query Language. Nó cho phép cho phép cá hoạt động bao gồm: aggregation, slicing and dicing, prediction and joins.

## Pushgateway
Pushgateway là một dịch vụ trung gian mà cho phép bạn đẩy các metrics từ các jobs mà không thể bị loại bỏ. Chỉ nên sử dụng Pushgateway trong một số trường hợp nhất định. 
Nó được sử dụng để hỗ trợ các job có thời gian ngắn. Việc các job này không tồn tại lâu dẫn đến dữ liệu chỉ cần dẩy vể Pushgateway rồi đẩy về Prometheus Server.

## Remote Read
Remote read  là một tính năng Prometheus cho phép đọc chuỗi thời gian trong suốt từ các hệ thống khác (chẳng hạn như lưu trữ dài hạn) như một phần của truy vấn.

## Remote Read Adapter
Không phải tất cả các hệ thống đều trực tiếp hỗ trợ remote read. Một remote read adapter nằm giữa Prometheus và một hệ thống khác. Chuyển đổi time series requests và responses giữa chúng.

## Remote Write
Remote write là một tính năng của Prometheus mà cho phép gửi các samples đến một hệ thống khác như là lưu trữ dài hạn.

## Remote Write Adapter
Không phải tất cả các hệ thống đều trực tiếp hỗ trợ remote write. Một remote write nằm giữa Prometheus và hệ thống khác. Chuyển đổi các samples trong remote write vào trong một định dạng mà hệ thống khác có thể hiểu được.

## Remote Write Endpoint
Một remote write endpoint là cái mà Prometheus nói từ khi đang remote write.

## Silence
Một Silence trong Alertmanager ngăn chặn cảnh báo, với những labels phù hợp với silence thì không được đưa ra thông báo.

## Target 
Một target được hiểu là một mục tiêu để lấy dữ liệu _(scrape)_. Cho ví dụ những labels được chấp nhận, bất kì xác thực yêu cầu để kết nối.

## Time-series
Là một chuỗi các dữ liệu được đo liên tục trong một khoảng thời gian liền nhau theo một tần xuất xác định.