# BÀI NỘP LAB5 — Track 3: Du lịch · Nhóm BayAnToan
**Lớp:** L3A · **Bàn / zone:** Zone 2 · **Track:** T3 (Du lịch)

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Nguyễn Văn A | HV001 | Trưởng nhóm: Phụ trách cấu trúc lát cắt §1, lập luận mức tự chủ §4, demo ca khó. |
| Trần Thị B | HV002 | UX Designer & Copy: Soạn kịch bản rủi ro §2, viết câu chữ AI và ánh xạ HAX/PAIR §3, thiết kế giao diện §5. |
| Lê Hoàng C | HV003 | Prototype Engineer: Dựng mã HTML/CSS/JS tương tác cho prototype §5, xây dựng bộ tiêu chí kiểm thử §6, xử lý ca lạ khi demo §7. |

---

## §1. Lát cắt & Phạm vi (Chặng 1)

### 1.1 Trả lời các câu hỏi cốt lõi
1. **Ai đang làm việc gì, AI ra quyết định gì?**
   * *Ai:* Hành khách (`Nguyễn Minh An`, 34 tuổi) đi cùng con nhỏ (`Nguyễn Gia Bình`, 5 tuổi).
   * *Làm việc gì:* Vừa nhận tin chuyến bay `BM 213` (19:00, 12/11) bị huỷ do bão, đang hoang mang nhắn vào mục "Đặt chỗ của tôi" để tìm chuyến bay thay thế.
   * *AI quyết định gì:* AI quyết định **tìm kiếm và tạm giữ chỗ chuyến bay thay thế phù hợp**, hỏi lại khi thông tin chưa rõ, hướng dẫn xác nhận đổi vé, hoặc chuyển nhân viên/mở luồng hoàn vé; tuyệt đối không tự ý chốt đổi vé làm mất quyền lợi của khách.
2. **Sản phẩm thật hôm nay thế nào? AI được thêm quyền gì?**
   * *Hôm nay:* Khách phải tự mò vào *Đặt chỗ của tôi $\rightarrow$ Hoàn tiền & Đổi lịch $\rightarrow$ Đổi lịch $\rightarrow$ Tìm chuyến mới $\rightarrow$ Xem phí $\rightarrow$ Thanh toán*. Quy trình qua gần 10 màn hình, rất dễ hết chỗ tốt trong lúc khách đang cuống cuồng.
   * *AI được thêm quyền:* AI được quyền **đọc thông tin đặt chỗ** và **chủ động gọi tool `giu_cho_chuyen_moi` (tạm giữ 2 vé trong 20 phút)** thay cho khách ngay trong lúc trò chuyện — việc mà trước đây người dùng không thể làm được trên app thông thường.
3. **Nếu AI sai, ai chịu gì? Hướng nào đắt hơn?**
   * *Nếu AI tự ý đổi vé sai:* Khách mất lần đổi miễn phí duy nhất (chính sách hãng Bay Mẫu: chỉ đổi miễn phí 1 lần trong 48h). Đổi nhầm giờ hoặc tách vé khiến trẻ 5 tuổi không bay được thì khách mất tiền triệu mua vé mới hoặc lỡ việc quan trọng.
   * *Nếu AI tự ý gửi hoàn vé:* Khách mất toàn bộ vé, tiền bị giam tới 90 ngày, không thể rút lại yêu cầu.
   * *Hướng đắt hơn:* **Tự ý gửi hoàn vé** và **tự ý đổi vé sai khi chưa rõ thông tin** đắt hơn rất nhiều so với hướng "bắt khách bấm xác nhận" hoặc "tạm giữ chỗ".
4. **Ba việc tính năng KHÔNG làm (kèm lý do):**
   * *Việc 1:* **KHÔNG tự ý gửi yêu cầu hoàn vé (`gui_yeu_cau_hoan_ve`)**: Vì Traveloka quy định yêu cầu hoàn đã gửi thì *không thể huỷ*, khách mất vé ngay lập tức và tiền hoàn mất tới 90 ngày mới về tài khoản.
   * *Việc 2:* **KHÔNG tự xử lý đổi tên hành khách hoặc tách vé cho trẻ em bay một mình**: Bé Nguyễn Gia Bình mới 5 tuổi. Luật hàng không quy định trẻ dưới 12 tuổi bắt buộc phải có người lớn đi kèm; AI không có thẩm quyền thẩm định giấy tờ bảo trợ hay quy định an toàn này.
   * *Việc 3:* **KHÔNG tự tính toán hoặc cam kết bồi thường tiền mặt**: Chuyến bay huỷ do **bão (thời tiết - bất khả kháng)**, chính sách hãng Bay Mẫu và luật hàng không quy định rõ: *Miễn trừ nghĩa vụ bồi thường ứng trước bằng tiền mặt*. AI không được hứa bồi thường gây hiểu nhầm.

### 1.2 Đọc bảng tool như người thiết kế
* **Tool chỉ đọc:** `tra_cuu_dat_cho` (Đọc mã đặt chỗ `TRV-7Q2K`, số lượng khách, các chuyến bay còn chỗ).
* **Tool thay đổi trạng thái nhưng đảo ngược được:** `giu_cho_chuyen_moi` (Giữ chỗ 20 phút, hết hạn tự nhả, không mất phí); `chuyen_nhan_vien` (Chuyển cuộc chat, tốn vài phút của nhân viên).
* **Tool KHÔNG đảo ngược được (Nguy hiểm nhất):** `doi_chuyen_bay` (Dùng mất lần đổi miễn phí duy nhất); `gui_yeu_cau_hoan_ve` (Gửi rồi là không rút lại được).
* **Nhu cầu không có tool đáp ứng:** Khách đòi *"đổi tên người bay sang bạn tôi"* hoặc *"bồi thường tiền mặt 2 triệu"*. AI phải giải thích quy định từ chối và hướng dẫn gặp quầy dịch vụ tại sân bay.

### Tuyên bố Lát cắt (Một câu chuẩn):
> **Trong mục Đặt chỗ của tôi (Traveloka), khi hành khách vừa nhận tin chuyến bay bị huỷ do bão nhắn hỏi, AI tự động tra cứu, đề xuất và giữ chỗ chuyến thay thế, hỏi lại để xác nhận hành khách và giờ bay, hướng dẫn đổi vé hoặc chuyển nhân viên, để khách tới nơi kịp mà không bị mất vé hay giam tiền vì thao tác không thể đảo ngược.**

## §4. Mức tự chủ (Chặng 4 — Mốc M2)

### 4.1 Bảng phân bổ Mức tự chủ cho 5 Tools theo Cost-of-Error

| Tên Tool | Mức tự chủ | Ai duyệt / Thấy gì / Quá hạn thì sao | Lý do theo Cost-of-Error (Cái giá khi sai) | Phương án đã cân nhắc & Vì sao loại |
|---|---|---|---|---|
| `tra_cuu_dat_cho` | **TỰ LÀM** | AI tự động chạy khi mở màn hình chat. | **Hành động chỉ đọc (Read-only)**. Sai thì chỉ cần tải lại dữ liệu, không ảnh hưởng tới trạng thái đặt vé hay tiền của khách. | *Cân nhắc:* Bắt khách nhập mã xác nhận mới cho tra cứu. *Loại vì:* Quá rườm rà, khách đã đăng nhập app và đang xem đúng vé đó. |
| `giu_cho_chuyen_moi` | **TỰ LÀM** | AI tự động chạy khi tìm thấy chuyến phù hợp. Đồng hồ đếm ngược 20:00 hiển thị trên màn hình. Hết 20 phút tự nhả vé về hệ thống. | **Hoàn toàn đảo ngược được**. Nếu bắt chờ khách duyệt mới giữ chỗ thì trong 5 phút chat, khách khác sẽ mua mất vé. Giữ nhầm thì vé tự nhả, không ai mất tiền hay quyền lợi. | *Cân nhắc:* Hỏi khách "Bạn có muốn giữ chỗ không?" rồi mới giữ. *Loại vì:* Chậm trễ, rủi ro mất chỗ trong đêm bão là cực lớn. |
| `doi_chuyen_bay` | **CẦN DUYỆT** | **Khách hàng tự duyệt**. Thấy: Tên chuyến mới, giờ cất cánh, tên 2 hành khách, chi phí 0đ, cảnh báo *mất lượt đổi miễn phí*. Quá 20 phút không duyệt thì vé giữ tự nhả. | **Bất khả đảo ngược / Chi phí cực cao**. Đổi vé thành công là mất luôn lượt đổi miễn phí duy nhất. Bắt khách bấm 1 nút xác nhận chặn đứng 100% rủi ro AI đoán nhầm chuyến. | *Cân nhắc:* AI tự đổi luôn cho khách (Tự làm). *Loại vì:* Khách nói "sáng mai" rất mơ hồ; tự đổi có thể làm khách lỡ việc hoặc kẹt chuyến bay không mong muốn. |
| `gui_yeu_cau_hoan_ve` | **CẦN DUYỆT (2 lớp)** | **Khách tự duyệt qua Modal Popup**. Thấy: Cảnh báo mất vé, thời gian tiền về (tới 90 ngày), không có bồi thường bão. Phải tự tay tích checkbox xác nhận mới bấm được nút. | **Bất khả đảo ngược / Thiệt hại nghiêm trọng**. Gửi rồi là không thể huỷ trên Traveloka. Khách mất chỗ trên các chuyến bay giải toả và bị giam tiền. Bắt buộc tạo ma sát cao để khách cân nhắc. | *Cân nhắc:* Cấm tiệt AI làm việc này, bắt gọi tổng đài. *Loại vì:* Đêm bão tổng đài sẽ nghẽn 100%, khách có nhu cầu hoàn vé thật sẽ bị bỏ rơi gây bức xúc cực độ. |
| `chuyen_nhan_vien` | **TỰ LÀM** | AI tự kích hoạt khi phát hiện ca vượt thẩm quyền (đổi tên, trẻ bay 1 mình, khách gắt gỏng hoặc từ chối đề xuất). | **Chuyển thừa chỉ tốn vài phút công nhân viên**, nhưng chuyển thiếu thì khách bị kẹt lại sân bay nguy hiểm đến tính mạng trong đêm bão. | *Cân nhắc:* Cần quản lý duyệt mới cho chuyển nhân viên. *Loại vì:* Tạo thêm nút thắt cổ chai vô nghĩa khi khách đang khẩn cấp. |

### 4.2 Tool khó xếp nhất và Cách giải quyết
* **Tool khó xếp nhất:** `gui_yeu_cau_hoan_ve` (Gửi yêu cầu hoàn vé).
* **Bất đồng ban đầu trong nhóm:**
  * Thành viên A: Đòi để **CẤM** (Bắt buộc gọi tổng đài vì thao tác này gửi đi là mất vé vĩnh viễn, rủi ro khách bấm nhầm quá lớn).
  * Thành viên C: Đòi để **CẦN DUYỆT ĐƠN GIẢN** (Khách bấm 1 nút là gửi, vì đêm bão khách cần hoàn tiền gấp để bắt xe khách hoặc thuê khách sạn).
* **Cách nhóm chốt:** Thiết kế cơ chế **"Cần duyệt 2 lớp có rào cản ma sát cao (High-friction Confirmation)"**:
  * Không cấm (để tránh làm nghẽn tổng đài lúc bão).
  * Nhưng không cho duyệt bằng 1 click chat đơn giản: Bắt buộc mở Modal Popup chuyên biệt + giải thích 3 cái mất (mất vé, mất quyền đổi, chờ tiền 90 ngày) + **Checkbox bắt buộc tích xác nhận**. Nhờ vậy chặn đứng hoàn toàn việc gửi nhầm ở K2.

### 4.3 Thử nghiệm mức tự chủ trên 3 kịch bản
1. **Thử trên K1 (Đòi đi chuyến 21:00 chỉ còn 1 chỗ):**
   * *Kết quả:* Cái sai bị chặn đứng hoàn toàn. AI không có quyền tự gọi `doi_chuyen_bay`. AI chỉ dùng quyền `giu_cho_chuyen_moi` trên chuyến 07:00 sáng mai và hiện nút cho khách duyệt. Trẻ em không bị bỏ rơi, vé không bị đổi nhầm.
2. **Thử trên K2 (Đòi hoàn tiền + đền tiền bão):**
   * *Kết quả:* Cái sai bị chặn. AI không thể tự gửi yêu cầu hoàn vé. Modal bật lên cảnh báo rõ "Không có tiền bồi thường mặt do bão", khách thấy kẹt tiền 90 ngày nên suy nghĩ lại và quay sang chọn đổi chuyến.
3. **Thử trên K-Normal (Khách nhắn: "Đổi giúp tôi sang chuyến 7h sáng mai BM 201 cho cả 2 người"):**
   * *Kết quả:* Khách bình thường không bị chờ vô ích. AI tự động giữ chỗ ngay lập tức trong 1 giây, đưa ra thẻ xác nhận đúng chuyến `BM 201`, khách chỉ cần bấm đúng 1 nút "Xác nhận đổi" là xong trong vòng 10 giây.

### 4.4 Mức tự chủ đã đổi lời hứa với người dùng như thế nào?
* **Trước đây (Nếu chọn Tự làm):** Lời hứa là *"AI sẽ tự giải quyết tất cả cho bạn trong chớp mắt"* $\rightarrow$ Lời hứa này nguy hiểm vì tạo ảo tưởng AI hiểu hết mọi việc, dẫn tới khách mất vé oan.
* **Sau khi thiết kế lại:** Lời hứa trở thành: **"AI giữ chỗ an toàn ngay lập tức cho bạn; bạn là người quyết định cuối cùng bằng một chạm."** Lời hứa này xây dựng niềm tin vững chắc (*Calibrated Trust* theo PAIR), khách an tâm không sợ mất chỗ mà vẫn nắm quyền kiểm soát vé của mình.

