# Ghi chú chất lượng dữ liệu — Buổi 1

Nguồn: `notebooks/01_kham_pha.ipynb`. Bộ dữ liệu Cookie Cats (`data/raw/cookie_cats.csv`), 90.189 người chơi.

## Người chơi 0 vòng (`sum_gamerounds == 0`)

- Số lượng: 3.994 người (4,43% tổng số), phân bố đều giữa hai nhóm (gate_30: 4,33%, gate_40: 4,52%).

**Quyết định:** giữ người chơi 0 vòng khi tính retention.

**Lý do:** loại đi tức là bỏ luôn 87+29−5 = 111 người có tín hiệu retention thật (87 người `retention_1=True`, 29 người `retention_7=True`, 5 người trùng cả hai), trong khi không có bằng chứng cụ thể nào nói đó là lỗi, chỉ là nghi ngờ.

**Giới hạn:** không có cách nào từ dữ liệu hiện có để biết `sum_gamerounds=0` là do thật sự chưa mở lại app, hay do sự kiện chơi và sự kiện mở app được ghi nhận từ hai nguồn khác nhau.

## Giá trị cực đại (`sum_gamerounds` = 49.854)

Giá trị cực đại khả năng cao do bot vì `sum_gamerounds` của case này gấp 16,84 lần so với case lớn thứ 2 (2.961 vòng) — trong khi case thứ 2 gần tương đương case thứ 3 và các case còn lại trong top 10 (đều trong khoảng 2.000–3.000 vòng).

Ảnh hưởng tới trung bình: kéo trung bình lên khoảng 1% — trung bình toàn bộ (90.189 người) là 51,87 vòng, nếu bỏ riêng ca này thì còn 51,32 vòng, chênh lệch 0,55 (≈1%).

**Giới hạn:** dữ liệu hiện có (`userid`, `version`, `sum_gamerounds`, `retention_1/7`) không có thông tin nào khác (như tần suất phiên chơi, thiết bị, IP...) để xác nhận chắc chắn đây là bot; đây chỉ là suy luận dựa trên độ lớn bất thường của một con số, chưa phải bằng chứng trực tiếp.

## Các kiểm tra khác đã thực hiện

- `userid`: không trùng lặp (90.189 giá trị unique = 90.189 dòng).
- Cân bằng nhóm: gate_30 = 44.700, gate_40 = 45.489. Kiểm định chi-square cho SRM: statistic=6.90, p=0.0086 — có ý nghĩa thống kê nhưng độ lệch tuyệt đối nhỏ (~0,87 điểm %), chưa đủ để nghi ngờ cơ chế random hoá bị hỏng nghiêm trọng.
