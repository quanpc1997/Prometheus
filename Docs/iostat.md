# IOSTAT

Tham khao: https://www.robustperception.io/mapping-iostat-to-the-node-exporters-node_disk_-metrics

Prometheus metric đặt tên có xu hướng lên quan đến các loại dữ liệu thô. Vì vậy _node_disk_reads_completed_total_ là số bytes được ghi vào ổ đĩa nhất định và được cung cấp bởi trường đầu tiên của _/proc/diskstats_. **iostat** có đầu ra như sau:

```sh
Device:  rrqm/s  wrqm/s  r/s   w/s     rkB/s    wkB/s     avgrq-sz  avgqu-sz    await   r_await    w_await    svctm    %util
sda       0.00   8.00    0.00  15.00   0.00     342.00    45.60     0.04        2.40    0.00       2.40       0.40     0.60
```

Tại đây _r/s_ là số lần đọc mỗi giây được tính từ lần đo trước đó được thực hiện (hoặc kể từ khi khởi động cho lần đầu tiên). Tương đương với lệnh _rate(node_disk_reads_completed_total[5m])_ trong PromQL. Tương tự:
| iostat | promethueus                                | Ý nghĩa                  |
|--------|--------------------------------------------|--------------------------|
| r/s    | rate(node_disk_reads_completed_total[5m])  | Số lần đọc trên một giây |
| w/s    | rate(node_disk_writes_completed_total[5m]) | Số lần ghi trên một giây |
| rrqm/s | rate(node_disk_reads_merged_total[5m])     |                          |
| wrqm/s | rate(node_disk_writes_merged_total[5m])    |                          |

Với băng thông, iostat sẽ sử dụng đơn vị là KB là mặt định. Còn Prometheus thì lại sử dụng bytes.

| iostat   | promethueus                                | Ý nghĩa                  |
|----------|--------------------------------------------|--------------------------|
| rkB/s    | rate(node_disk_read_bytes_total[5m])       | Số lần đọc trên một giây |
| wkB/s    | rate(node_disk_written_bytes_total[5m])    | Số lần ghi trên một giây |

_avgrq-sz_ là dung lượng trung bình của mỗi request được tính bằng bytes, bao gồm cả đọc và ghi. Nó được tính toán bởi iostat được tính bằng cách chia bytes cho các operations. Vì vậy:
_(rate(node_disk_read_bytes_total[5m]) + rate(node_disk_written_bytes_total[5m])) / (rate(node_disk_reads_completed_total[5m]) + rate(node_disk_writes_completed_total[5m]))_ 


_avgqu-sz_ is simpler, the average queue length. This is based on field 11, which gives us _rate(node_disk_io_time_weighted_seconds_total[5m])_

_r_await_ and w_await are how long read and write requests took on average, so for reads that's _rate(node_disk_read_time_seconds_total[5m]) / rate(node_disk_reads_completed_total[5m])_ and similarly for writes. await is both combined, so you can add and then divide if you want it.

%util is utilisation as a percentage, _rate(node_disk_io_time_seconds_total[5m])_ will produce the same as a ratio which is more standard in Prometheus. svctm is deprecated, but it'd be the IO time divided by the sum of the reads and writes completed.

There one other notable metric which iostat doesn't expose which is field 9, _node_disk_io_now_ the number of IOs in progress. Newer kernels will also expose discard stats, useful for SSDs.
