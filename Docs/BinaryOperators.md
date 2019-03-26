# Toán tử trong Prometheus.

## Toán tử toán học

| Toán tử |  Ý nghĩa |
|:-------:|:--------:|
|    +    | Cộng     |
|    -    | Trừ      |
|    *    | Nhân     |
|    /    | Chia     |
|    %    | Lấy dư   |
|    ^    | Lũy thừa |

## Toán tử so sánh

| Toán tử |      Ý nghĩa      |
|:-------:|:-----------------:|
|    ==   | So sánh bằng      |
|    !=   | Không bằng        |
|    >    | Lớn hơn           |
|    <    | Nhỏ hơn           |
|    >=   | Lớn hơn hoặc bằng |
|    <=   | Nhỏ hơn hoặc bằng |

## Toán tử Logic

| Toán tử | Ý nghĩa |
|:-------:|:-------:|
|   and   | Và      |
|    or   | Hoặc    |
|  unless | Trừ     |

Trong đó với _unless_ cho ví vụ:
	A = {1,2,3,4,5,6,7}
	B = {5,6,7,8,9}

_unless_ tương đương với toán từ trử. Những phần tử thuộc A nhưng không thuộc B.
Khi đó: A unless B = {1,2,3,4}

## Vector matching

### One-to-one vector matches
**One-to-one**

### Many-to-one and one-to-many vector matches
**Many-to-one** và **one-to-many**


## Toán tử kết hợp.

|    Toán tử   |                    Ý nghĩa                   |
|:------------:|:--------------------------------------------:|
|      sum     | Tính tổng                                    |
|      min     | Tìm giá trị nhỏ nhất                         |
|      max     | Tìm giá trị lớn nhất                         |
|      avg     | Trung bình cộng                              |
|    stddev    | Độ lệch chuẩn                                |
|    stdvar    | Phương sai                                   |
|     count    | Đếm số phần tử trong vector                  |
| count_values | Đếm số phần tử trong vector cùng giá trị     |
|    bottomk   | Những phân tử k nhỏ nhất theo giá trị sample |
|     topk     | Những phân tử k lớn nhất theo giá trị sample |
|   quantile   | Tính toán fi(0 ≤ fi ≤ 1)                     |

Các toán tử trên được sử dụng để tổng hợp hoặc duy trì dimensions bởi toán tử _without_ hoặc _by_.
```sh
<aggr-op>([parameter,] <vector expression>) [without|by (<label list>)]
```
Cho ví dụ:
Nếu metric http_requests_total_ có time series xuất hiệ theo label _application, instance_ và _group_. Chúng ta có thể tính tổng số các HTTP request trên giây của mỗi ứng dụng và nhóm trong tất cả các trường hợp. 
```sh
sum(http_requests_total) without (instance)
```

Điều này tương đương với:
```sh
sum(http_requests_total) by (application, group)
```

Nếu chúng ta chỉ quan tâm đến tổng số HTTP request mà chúng ta thấy trong tất cả các ứng dụng. Chúng ta có thể viết đơn giản như sau:
```sh
sum(http_requests_total)
```

Để đếm só phiên bản đang chạy xây dựng, chúng ta có thể viết:
```sh
count_values("version", build_version)
```

Để lấy 5 lượng HTTP request lớn nhất trong tất cả các trường hợp. Ta viết như sau:
```sh
topk(5, http_requests_total)
```

## Độ ưu tiên của các toán tử
1. ^
2. *, /, %
3. +, -
4. ==, !=, <=, <, >=, >
5. and, unless
6. or
