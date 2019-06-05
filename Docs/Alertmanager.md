# Alertmanager

Tham khảo: https://pracucci.com/prometheus-understanding-the-delays-on-alerting.html

## Các tham số cần chú ý trong Alertmanager
_Cấu hình trong file alertmanager.yml_
**scrape_interval**: Là chu kì quét dữ liệu vật lý. Mặc định là 1 phút
**evaluation_interval**: Là thời gian thay đổi từ trạng thái này sang trạng thái khác của một Alert.

_Cấu hình trong file prometheus.yml_
**group_by**: Nhóm các cảnh báo lại với nhau.
**group_wait** Thời gian chờ để gửi thông báo sau khi nhóm các cảnh báo.
**group_interval**: Thời gian đợi để gửi thông báo tiếp theo sau khi thông báo đầu tiên được gửi.
**repeat_interval**: Thời gian để gửi thông báo lặp lại nếu alert vẫn chưa được giải quyết

_Cấu hình trong file qRule.rules.yml_
**for**: Thời gian chờ để kiểm tra xem alert được kích hoạt có phải chỉ tức thời hay không. Mặc định là 0 phút

## Các trạng thái và vòng đời của một Alert
Alert có 3 trạng thái đó là: _inactive, pending, firing_
* **inactive**: Trạng thái Alert chưa được kích hoạt.
* **pending**: Trạng thái Alert đã được kích hoạt nhưng còn đợi kiểm tra có phải cảnh báo tức thời hay không.
* **firing**: Trạng thái mà một Alert đã được bắn cho người dùng.

Thời gian một alert từ lúc kích hoạt đến lúc được bắn là:
	**Time = Chu kì quét vật lý + thời gian chuyển trạng thái _(pending -> firing)_ + thời gian kiểm tra kích hoạt tức thời.**



