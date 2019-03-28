# Các hàm trong Prometheus

Tham khảo: https://prometheus.io/docs/prometheus/latest/querying/functions/

## abs()
abs(v instant-vector)
Biến tất cả các đầu vào thành trị tuyệt đối của nó.

## absent()
absent(v instant-vector)
_(*)_

## ceil()
ceil(v instant-vector)
làm tròn các giá trị samples được truyền vào.

## floor()
floor(v instant-vector)
Làm tròn xuống.

## changes()
changes(v range-vector)
Đầu vào là time series.
Trả về số lần giá trị của nó đã thay đổi trong phạm vi thời gian.

## clamp_max()
clamp_max(v instant-vector, max scalar)
Giữ giá trị sample của tất cả các thành phần trong v để có giới hạn trên tối đa.

## clamp_min()
clamp_min(v instant-vector, min scalar)
	- Giữ giá trị sample của tất cả các thành phần trong v để có giới hạn trên tối thiểu.

## day_of_month()
day_of_month(v=vector(time()) instant-vector)
Trả về ngày trong tháng. Giá trị trong khoảng 1 - 31 tùy tháng.

## day_of_week()
day_of_week(v=vector(time()) instant-vector)
Trả về thứ trong tuần. Giá trị từ 0 đến 6. Trong đó 0 là chủ nhật.

## days_in_month()
days_in_month(v=vector(time()) instant-vector)
Trả về số ngày của một tháng. Giá trị từ 28 đến 31.

## minute()
minute(v=vector(time()) instant-vector)
Trả vê số phút của giờ. Giá trị nằm trong khoảng từ 0-59

## month()
month(v=vector(time()) instant-vector)
Trả về tháng của năm. Giá trị từ 1 đến 12.

## time()
Trả về số giây từ ngày 01/01/1970.

## hour()
hour(v=vector(time()) instant-vector) 
Trả về giờ của ngày. Giá trị thuộc khoảng 0 đến 23.

## delta()
delta(v range-vector)
Tính toán sự chênh lệch giữa giá trị đầu và cuối của mỗi thành phần time series trong range-vector v. Trả về một Instance vector với các vector đã cho và các nhãn tương đương. Delta được ngoại suy để bao trùm phạm vi toàn thời gian như được chỉ định trong bộ chọn vector phạm vi, do đó có thể nhận được kết quả không nguyên ngay cả khi các giá trị sample là tất cả các số nguyên.

Sau đây là ví dị trả về sự chênh lệch nhiệt độ CPU trong khoảng thời gian 2h trước:
```sh
delta(cpu_temp_celsius{host="zeus"}[2h])
```
delta chỉ lên sử dụng với đồng hồ đo.

## deriv()
deriv(v range-vector)
Tính đạo hàm mỗi giây của time series trong một phạm vi vector v, sử dụng hồi quy tuyến tính đơn giản.

## exp()
exp(v instant-vector)
Tính toán hàm số mũ cho tất cả các tham số truyền vào v. Các trường hợp đặc biệt là:
	* Exp(+Inf) = +Inf
	* Exp(NaN) = NaN

## histogram_quantile()
histogram_quantile(φ float, b instant-vector)
Tính toán fi (0 ≤ fi ≤ 1) từ các buckets b của biểu đồ. Các samples trong b là số lượng quan sát trong mỗi nhóm. Mỗi sample phải có nhãn le trong đó giá trị nhãn biểu thị giới hạn trên bao gồm của buckets. (Các samples không có nhãn như vậy sẽ bị bỏ qua trong âm thầm.) Loại số liệu biểu đồ tự động cung cấp chuỗi thời gian với hậu tố _bucket và nhãn phù hợp.
_(*)_

## holt_winters()
holt_winters(v range-vector, sf scalar, tf scalar)
Tạo ra một giá trị được làm mịn cho chuỗi thời gian dựa trên phạm vi trong v. Hệ số làm mịn sf càng thấp, tầm quan trọng càng cao đối với dữ liệu cũ. Hệ số xu hướng tf càng cao, càng có nhiều xu hướng trong dữ liệu được xem xét. Cả sf và tf phải nằm trong khoảng từ 0 đến 1.
holt_winters nên sử dụng với đồng hồ đo.

## idelta()
idelta(v range-vector)
Tính toán sự chênh lệch giữa 2 samples trong ranger vector v.
Trả về một instant vector với các delta đã cho và các nhãn tương đương.

## increase()
increase(v range-vector)
Đếm mức tăng của time serial trong ranger vector. 
Cho ví dụ sau trả về số lượng yêu cầu HTTP được đo trong 5 phút. Mỗi chuỗi thời gian là một ranger vector.

```sh
increase(http_requests_total{job="api-server"}[5m])
```

## irate()
irate(v range-vector)
Tính toán increase môi giây của time series trong range vector.

Cho ví dụ sau trả về số request HTTP mỗi giây tính toán trong 5 phút.
```sh
irate(http_requests_total{job="api-server"}[5m])
```
irate chỉ nên được sử dụng khi vẽ đồ thị bộ đếm biến động, di chuyển nhanh. Sử dụng _rate_ cho cảnh báo và slow-moving counters,
Chú ý rằng: Khi kết hợp _irate()_ với một toán tử tổng hợp như _sum()_ hoặc bất kì hàm nào khác, luôn luôn sử dụng một _irate()_ đầu tiên, sau đó tổng hợp. Ngoài ra _irate()_ không thể phát hiện counter restart khi target restart.

## label_join()
label_join(v instant-vector, dst_label string, separator string, src_label_1 string, src_label_2 string, ...)
jouns tất cả các giá trị của tất cả src_labels sử dụng separtor và trả về timeseries cùng label dst_label chứa những giá trị đã joined. Có thể có bất kỳ số src_labels nào trong hàm này.

Ví dụ sau sẽ trả về một vector với mỗi timeseres có một _foo_ label cùng với giá trị a,b,c được thêm vào:
```sh
label_join(up{job="api-server",src1="a",src2="b",src3="c"}, "foo", ",", "src1", "src2", "src3")
```

## label_replace()
	(*)


## ln()
ln(v instant-vector) tính toán hàm logarit cho tất cả các giá trị trong v.
	* ln(+Inf) = +Inf
	* ln(0) = -Inf
	* ln(x < 0) = NaN
	* ln(NaN) = NaN

## log2(), log10()
giống như ln chỉ khác cơ số.

## predict_linear()
predict_linear(v range-vector, t scalar)
Dự đoán giá trị của timeseries t giây kể từ bây giờ, dựa trên vectơ phạm vi v, sử dụng hồi quy tuyến tính đơn giản.

## rate()
rate(v range-vector)
Tính toán tốc độ trung bình của increase trong timeseries trong range vector.
Cho ví dụ trả về tốc độ CPU đo đạc được sau 5 phút:
Khi có _(rate()_:
```sh
rate(node_cpu_seconds_total{instance=~"172.27.100.154:9100",job=~"lab04",mode="user"}[5m]) 

{cpu="0",instance="172.27.100.154:9100",job="lab04",mode="user"}	0.02101754385965474
{cpu="1",instance="172.27.100.154:9100",job="lab04",mode="user"}	0.021263157894741435
{cpu="2",instance="172.27.100.154:9100",job="lab04",mode="user"}	0.014350877192982968
{cpu="3",instance="172.27.100.154:9100",job="lab04",mode="user"}	0.011438596491228836
```
Khi không có _rate()_:
```sh
node_cpu_seconds_total{cpu="0",instance="172.27.100.154:9100",job="lab04",mode="user"}
17930.15 @1553766703.144
17930.44 @1553766718.144
17930.78 @1553766733.144
17931.17 @1553766748.144
17931.48 @1553766763.144
17931.72 @1553766778.144
.....
```
Rõ ràng khi có rate dữ liệu sẽ được xử lí. Ngược lại thì dữ liệu ở dạng thuần rất khó đọc. Rate giúp ta tính toán tốc độ của một số thành phần.

_rate_ chỉ lên sử dụng cùng counter. Nó phù hợp nhất cho việc cảnh báo và đồ thị của slow-moving counters.

Khi kết hợp _rate()_ cùng với một toán tử tông hợp thì luôn luôn _rate()_ sẽ nằm ở vị trí đầu tiên sau đó là các toán tử khác. Ngoài ra không thể phát hiện counter restart khi target restart.

## resets()
resets(v range-vector)
Trả về số counter rester không cung cấp trong một khoảng thời gian. Bất kì sự giảm trong giá trị chênh lệnh giữa 2 samples liên tiếp được hiểu là counter reset.
_resets_ chỉ nên sử dụng cùng counters.

## round()
round(v instant-vector, to_nearest=1 scalar)
làm tròn các giá trị sample của các thành phần trong v đến giá trị nguyên gần nhất. Đối số to_nearest cho phép chỉ định chỉ số làm tròn.

## scalar()
_(*)_

## sort()
sort(v instant-vector)
Sắp xếp theo giá trị tăng dần.

## sort_desc()
Sắp xếp theo giá trị giảm dần

## sqrt()
sqrt(v instant-vector) - lấy căn bậc 2

## vector()
vector(s scalar) trả về một scalar s không có nhãn.

## year()
year(v=vector(time()) instant-vector) trả về năm.

## <aggregation> _over_time()

The following functions allow aggregating each series of a given range vector over time and return an instant vector with per-series aggregation results:
	* avg_over_time(range-vector): the average value of all points in the specified interval.
	* min_over_time(range-vector): the minimum value of all points in the specified interval.
	* max_over_time(range-vector): the maximum value of all points in the specified interval.
	* sum_over_time(range-vector): the sum of all values in the specified interval.
	* count_over_time(range-vector): the count of all values in the specified interval.
	* quantile_over_time(scalar, range-vector): the φ-quantile (0 ≤ φ ≤ 1) of the values in the specified interval.
	* stddev_over_time(range-vector): the population standard deviation of the values in the specified interval.
	* stdvar_over_time(range-vector): the population standard variance of the values in the specified interval.
Note that all values in the specified interval have the same weight in the aggregation even if the values are not equally spaced throughout the interval.
