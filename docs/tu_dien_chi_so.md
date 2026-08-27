# Từ điển chỉ số — Game mobile / Live-ops

Mỗi chỉ số: định nghĩa, công thức, bẫy thường gặp, và sự kiện tối thiểu cần thu thập để tính được nó.

## DAU (Daily Active Users)

- **Định nghĩa:** số người dùng duy nhất mở/hoạt động trong game trong một ngày.
- **Công thức:** đếm `user_id` duy nhất có sự kiện hoạt động trong ngày đó.
- **Bẫy thường gặp:** đếm theo lượt phiên (session) thay vì người dùng duy nhất sẽ đếm trùng một người mở app nhiều lần trong ngày.
- **Sự kiện cần thu thập:** `session_start` (hoặc `app_open`) có `user_id` và dấu thời gian (timestamp).

## MAU (Monthly Active Users)

- **Định nghĩa:** số người dùng duy nhất hoạt động trong 30 ngày gần nhất (cửa sổ trượt, không phải tháng lịch).
- **Công thức:** đếm `user_id` duy nhất có sự kiện hoạt động trong khoảng 30 ngày.
- **Bẫy thường gặp:** dùng "tháng lịch" (1-30/31 hàng tháng) thay vì cửa sổ trượt 30 ngày khiến số liệu nhảy bất thường vào đầu/cuối tháng, không phản ánh đúng xu hướng thực.
- **Sự kiện cần thu thập:** giống DAU (`session_start` có `user_id`, timestamp), chỉ khác cách gộp theo cửa sổ thời gian.

## Tỷ lệ dính chân (Stickiness = DAU/MAU)

- **Định nghĩa:** tỷ lệ người dùng hoạt động hàng tháng mà quay lại chơi mỗi ngày — đo mức độ game trở thành thói quen.
- **Công thức:** `DAU trung bình trong tháng / MAU`.
- **Bẫy thường gặp:** so sánh DAU/MAU giữa hai game có chu kỳ chơi khác nhau (game hàng ngày như match-3 vs game phiên dài như chiến thuật theo mùa) là so sánh khập khiễng — bản chất game quyết định mức DAU/MAU "tốt", không có một ngưỡng chung cho mọi thể loại.
- **Sự kiện cần thu thập:** không cần thêm gì ngoài dữ liệu đã dùng cho DAU và MAU.

## Retention D1 / D7 / D30

- **Định nghĩa:** tỷ lệ người cài đặt (hoặc tham gia thử nghiệm) vào ngày 0 mà còn quay lại hoạt động đúng vào ngày thứ 1 / 7 / 30 sau đó.
- **Công thức:** `số người có hoạt động vào đúng ngày N sau cài đặt / tổng số người cài đặt ở cohort đó`.
- **Bẫy thường gặp:** so retention D7 với D1 rồi kết luận "giảm mạnh" — retention theo mốc xa hơn luôn thấp hơn mốc gần, đó là chuyện đương nhiên của mọi game, không phải phát hiện. So sánh có ý nghĩa là chênh lệch giữa hai nhóm/phiên bản **ở cùng một mốc**.
- **Sự kiện cần thu thập:** `install_date` (hoặc ngày tham gia thử nghiệm) ở cấp người dùng, và `session_start` có timestamp để biết chính xác ngày N có hoạt động hay không.

## Churn (tỷ lệ rời bỏ)

- **Định nghĩa:** tỷ lệ người dùng từng hoạt động nhưng ngừng hoạt động trong một khoảng thời gian xác định — về bản chất là phần bù của retention (`churn = 1 - retention`).
- **Công thức:** `số người không quay lại trong khoảng N ngày / tổng số người ở cohort đó`.
- **Bẫy thường gặp:** định nghĩa "ngừng hoạt động" mơ hồ (bao lâu không mở app thì tính là churn?) — cần chốt ngưỡng cụ thể và nói rõ ngưỡng đó khi báo cáo, vì churn 7 ngày và churn 30 ngày là hai con số rất khác nhau.
- **Sự kiện cần thu thập:** giống retention — cần `install_date`/ngày bắt đầu theo dõi và lịch sử `session_start` đầy đủ.

## Session length (độ dài phiên chơi)

- **Định nghĩa:** khoảng thời gian một người chơi liên tục trong một lần mở app, từ lúc mở đến lúc thoát/hết hoạt động.
- **Công thức:** `session_end_time - session_start_time` cho mỗi phiên, thường báo cáo bằng trung vị (không phải trung bình) vì phân bố lệch phải.
- **Bẫy thường gặp:** dùng trung bình thay vì trung vị — vài phiên treo máy hoặc để app chạy nền (không tương tác thật) có thể kéo trung bình lên rất cao, không phản ánh trải nghiệm thật.
- **Sự kiện cần thu thập:** `session_start` và `session_end` (hoặc heartbeat định kỳ để suy ra thời điểm kết thúc khi không có sự kiện đóng app rõ ràng).

## Tỷ lệ chuyển đổi người trả tiền (Conversion to Payer)

- **Định nghĩa:** tỷ lệ người chơi từng thực hiện ít nhất một giao dịch mua trong game trên tổng số người chơi.
- **Công thức:** `số người có ít nhất 1 giao dịch IAP / tổng số người chơi (hoặc MAU) trong kỳ`.
- **Bẫy thường gặp:** tính trên tổng số người cài đặt từ trước tới nay thay vì trên cohort/kỳ cụ thể — làm tỷ lệ trông thấp giả tạo vì gộp cả người dùng cũ đã rời bỏ từ lâu.
- **Sự kiện cần thu thập:** sự kiện giao dịch (`purchase`/`iap_transaction`) có `user_id`, giá trị, thời điểm.

## ARPU (Average Revenue Per User)

- **Định nghĩa:** doanh thu trung bình trên mỗi người dùng (tính cả người không trả tiền).
- **Công thức:** `tổng doanh thu trong kỳ / tổng số người dùng hoạt động trong kỳ`.
- **Bẫy thường gặp:** một vài người chi rất mạnh (whale) kéo lệch trung bình, khiến ARPU trông "khoẻ" hơn thực tế trải nghiệm của đa số người chơi — nên báo kèm phân vị (ví dụ chi tiêu trung vị, hoặc ARPU sau khi loại top 1% chi tiêu) để có góc nhìn cân bằng hơn.
- **Sự kiện cần thu thập:** giống conversion — sự kiện giao dịch có giá trị tiền, cộng với số người dùng hoạt động (mẫu số).

## ARPPU (Average Revenue Per Paying User)

- **Định nghĩa:** doanh thu trung bình chỉ tính trên nhóm người **đã từng trả tiền** (không tính người chơi miễn phí).
- **Công thức:** `tổng doanh thu trong kỳ / số người trả tiền trong kỳ`.
- **Bẫy thường gặp:** nhầm ARPU với ARPPU khi trình bày — ARPPU luôn cao hơn ARPU nhiều lần vì mẫu số nhỏ hơn hẳn, so sánh chéo hai chỉ số này giữa các báo cáo khác nhau mà không ghi rõ loại nào dễ gây hiểu lầm về sức khoẻ doanh thu thực tế.
- **Sự kiện cần thu thập:** giống ARPU, chỉ khác mẫu số (chỉ đếm người có giao dịch).

## LTV (Lifetime Value)

- **Định nghĩa:** tổng doanh thu dự kiến một người chơi sẽ tạo ra trong suốt vòng đời sử dụng game.
- **Công thức:** ước lượng phổ biến `LTV ≈ ARPU × thời gian gắn bó trung bình dự kiến` (nhiều biến thể phức tạp hơn dùng đường cong retention để ngoại suy).
- **Bẫy thường gặp:** ngoại suy LTV từ dữ liệu quá ngắn (ví dụ chỉ có 14 ngày dữ liệu như bộ Cookie Cats) — mọi con số LTV tính từ cửa sổ ngắn đều mang tính giả định mạnh về hành vi dài hạn, cần nêu rõ đây là ước lượng, không phải đo lường trực tiếp.
- **Sự kiện cần thu thập:** kết hợp toàn bộ sự kiện retention + giao dịch theo thời gian, cộng với `install_date` để dựng đường cong theo cohort.

## IAP và IAA

- **Định nghĩa:** hai nguồn doanh thu chính của game free-to-play. **IAP** (In-App Purchase) là doanh thu từ giao dịch mua trong game (vật phẩm, gói VIP...). **IAA** (In-App Advertising) là doanh thu từ hiển thị quảng cáo cho người chơi.
- **Công thức:** IAP = tổng giá trị giao dịch mua; IAA = tổng doanh thu quảng cáo (thường tính qua eCPM × số lượt hiển thị/1000).
- **Bẫy thường gặp:** chỉ tối ưu IAP hoặc chỉ tối ưu IAA mà bỏ qua tác động chéo — nhồi nhét quảng cáo quá mức (tăng IAA ngắn hạn) có thể làm giảm retention và từ đó giảm cả IAP dài hạn.
- **Sự kiện cần thu thập:** sự kiện giao dịch (cho IAP) và sự kiện hiển thị/click quảng cáo kèm doanh thu quy đổi (cho IAA).

## eCPM (effective Cost Per Mille)

- **Định nghĩa:** doanh thu quảng cáo quy đổi trên mỗi 1.000 lượt hiển thị — chỉ số chuẩn hoá để so sánh hiệu quả quảng cáo giữa các kênh/vị trí khác nhau.
- **Công thức:** `(tổng doanh thu quảng cáo / tổng số lượt hiển thị) × 1.000`.
- **Bẫy thường gặp:** so sánh eCPM giữa các định dạng quảng cáo khác nhau (banner, interstitial, rewarded video) mà không tách riêng — mỗi định dạng có eCPM đặc thù rất khác nhau, gộp chung sẽ làm mờ vấn đề thực sự nằm ở đâu.
- **Sự kiện cần thu thập:** sự kiện hiển thị quảng cáo (`ad_impression`) kèm định dạng, vị trí, và doanh thu quy đổi từ mạng quảng cáo.

## Chi phí kéo người dùng (CAC/CPI) và thời gian hoàn vốn

- **Định nghĩa:** **CAC/CPI** (Cost per Acquisition/Install) là chi phí trung bình để có một người dùng mới thông qua quảng cáo/marketing. **Thời gian hoàn vốn (payback period)** là số ngày cần để LTV cộng dồn của một cohort vượt qua CAC đã bỏ ra cho cohort đó.
- **Công thức:** `CAC = tổng chi phí marketing / số lượt cài đặt mới`; thời gian hoàn vốn là ngày N nhỏ nhất mà `LTV cộng dồn đến ngày N ≥ CAC`.
- **Bẫy thường gặp:** so CAC trung bình toàn kênh mà không tách theo kênh/chiến dịch — các kênh có CAC và chất lượng người dùng (retention, chi tiêu) rất khác nhau, gộp chung dễ che mất kênh đang lỗ.
- **Sự kiện cần thu thập:** dữ liệu chi phí từ nền tảng quảng cáo (theo kênh, chiến dịch), gắn với `install_date`/`user_id` qua attribution, cộng toàn bộ sự kiện doanh thu theo thời gian để dựng đường cong LTV cộng dồn.

---

## Bẫy khi đọc số — rút từ kinh nghiệm thực tế

- **Tài khoản test/nội bộ lẫn trong dữ liệu người chơi thật.** Một số tài khoản QA/dev có hành vi cực đoan (chơi liên tục, test cả hai nhóm A/B, giao dịch giả) làm lệch cả tử số lẫn mẫu số của mọi chỉ số ở trên — nếu số tài khoản test phân bố không đều giữa các nhóm so sánh, sai lệch này có hướng chứ không tự triệt tiêu (bài học thực tế: 414 đơn hàng test lẫn trong dữ liệu vận hành TMĐT từng làm sai lệch kết luận về tỷ lệ huỷ đơn).
- **Right-censoring khi so cohort mới với cohort cũ.** Cohort cài đặt gần đây chưa có đủ thời gian để đạt các mốc retention xa (D30, D90) — so trực tiếp retention D30 của cohort mới (chưa đủ 30 ngày dữ liệu) với cohort cũ sẽ luôn cho kết quả thấp giả tạo cho cohort mới, không phản ánh đúng bản chất.
- **ARPU trung bình bị vài người chi lớn kéo lệch.** Giống hệt vấn đề trung bình/trung vị đã gặp với `sum_gamerounds` trong dự án này — một vài "cá voi" (whale) chi tiêu rất mạnh có thể khiến ARPU trung bình "đẹp" trong khi trải nghiệm trả phí của đa số người chơi lại rất khác, luôn nên báo kèm trung vị hoặc phân vị.
