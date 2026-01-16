1. Cấu Trúc Thiết Kế & Giao Diện (UI/UX)
Giao diện được chia thành hai khu vực chính theo bố cục ngang (Landscape), sử dụng phong cách Glassmorphism (hiệu ứng kính mờ) hiện đại và tông màu lễ hội (Vàng Gold, Đỏ Tết).

Hình Nền & Không Gian:
Sử dụng hình nền lễ hội chung của toàn bộ ứng dụng.
Có lớp phủ mờ (overlay) để làm nổi bật nội dung.
Hiệu ứng mây trôi (clouds-container) tạo chiều sâu cho không gian.
Khu Vực Điều Khiển (Left Panel - Controls Panel):
Nằm cố định bên trái màn hình.
Chứa tiêu đề "ĐUA NGỰA" và nút quay về Menu chính.
Thẻ Giải Thưởng: Hiển thị tên giải thưởng đang chọn và số lượng còn lại.
Form Cài Đặt: Cho phép chọn loại giải thưởng, số lượng ngựa tham gia (2-15), và thời gian đua dự kiến.
Đồng Hồ: Hiển thị thời gian thực của cuộc đua.
Nút Thao Tác: Hai nút lớn "BẮT ĐẦU" và "LÀM MỚI" được đặt gọn gàng phía dưới.
Sân Khấu Trò Chơi (Right Panel - Game Stage):
Chiếm phần lớn diện tích màn hình bên phải.
Là nơi diễn ra cuộc đua, hiển thị đường đua, ngựa, và các chướng ngại vật.
Bảng Xếp Hạng: Nằm góc trên bên phải, hiển thị Top 5 ngựa dẫn đầu theo thời gian thực (chỉ hiện tên/ID, không hiện số mét).
Cảnh Báo: Các thông báo nổi (như "Hố Tử Thần") xuất hiện giữa màn hình khi có sự kiện.
Màn Hình Chiến Thắng: Một lớp phủ (overlay) xuất hiện khi có ngựa về đích, hiển thị cúp, tên người thắng và nút chơi lại.
2. Logic Vận Hành (Core Logic)
Trò chơi sử dụng thư viện vật lý Matter.js để mô phỏng chuyển động và va chạm, kết hợp với logic game tùy chỉnh.

A. Khởi Tạo & Dữ Liệu
Dữ Liệu Người Chơi: Được tải từ localStorage (key: lucky_draw_participants). Danh sách này được xáo trộn ngẫu nhiên trước khi gán vào ngựa.
Dữ Liệu Giải Thưởng: Tải từ localStorage (key: lucky_draw_prizes) để hiển thị và trừ số lượng khi có người thắng.
Ngựa: Mỗi con ngựa là một vật thể vật lý hình tròn (để xử lý va chạm mượt hơn) nhưng hiển thị bằng hình ảnh động (GIF).
Trên lưng ngựa hiển thị Tên Nhân Viên (nếu có) hoặc Mã NV/Số thứ tự.
Màu sắc nài ngựa (Jockey) được gán ngẫu nhiên từ bảng màu quy định.
B. Cơ Chế Đua (Game Loop)
Camera: Camera tự động bám theo con ngựa dẫn đầu (Leader), tạo cảm giác đua kịch tính.
Di Chuyển:
Ngựa chạy từ trái sang phải.
Tốc độ cơ bản được cộng thêm các yếu tố ngẫu nhiên (tăng tốc đột ngột, giảm tốc).
Cơ chế đuổi bắt (Catch-up): Ngựa ở xa top đầu sẽ được tăng nhẹ tốc độ để cuộc đua luôn sát nút.
AI Ngựa:
Ngựa có thể tự động thay đổi làn chạy (di chuyển lên/xuống) ngẫu nhiên để tránh chướng ngại vật hoặc vượt mặt.
Có chỉ số "Nitro" (tăng tốc) và "Stun" (bị choáng).
C. Hệ Thống Chướng Ngại Vật & Sự Kiện
Đường đua được sinh ra ngẫu nhiên với các loại chướng ngại vật:

Vũng Bùn (MUD): Làm chậm tốc độ di chuyển.
Bẫy Gai (SPIKE): Làm ngựa bị choáng, dừng lại trong một khoảng thời gian ngắn và nảy lên.
Tăng Tốc (NITRO): Một vùng hình chữ nhật, khi ngựa chạy qua sẽ được kích hoạt tốc độ cao trong thời gian ngắn.
Hố Tử Thần (HOLE):
Là loại bẫy nguy hiểm nhất.
Xuất hiện ngẫu nhiên phía trước ngựa dẫn đầu sau một khoảng thời gian.
Nếu ngựa rơi vào hố sẽ bị LOẠI NGAY LẬP TỨC khỏi cuộc đua.
D. Điều Kiện Thắng & Kết Thúc
Vạch Đích: Không cố định từ đầu. Vạch đích chỉ được sinh ra khi thời gian đua vượt quá thời gian cài đặt (ví dụ: sau 30 giây). Điều này đảm bảo cuộc đua luôn kéo dài đúng ý muốn bất kể tốc độ ngựa.
Xác Định Người Thắng:
Con ngựa đầu tiên chạm vạch đích và không bị loại sẽ là Nhà Vô Địch.
Hệ thống tự động lưu thông tin người thắng vào lịch sử (lucky_draw_winners) và giảm số lượng giải thưởng tương ứng.
Hiệu ứng pháo giấy (Confetti) và nhạc chiến thắng được kích hoạt.
3. Công Nghệ Sử Dụng
HTML5 Canvas: Để vẽ đường đua, nền, và các hiệu ứng hạt (bụi, pháo hoa).
Matter.js: Engine vật lý 2D xử lý va chạm và chuyển động.
DOM Elements: Dùng để hiển thị ngựa (thẻ div chứa img và text) đè lên Canvas để dễ dàng tùy biến CSS (như hiển thị tên trên lưng).
LocalStorage: Lưu trữ cơ sở dữ liệu cục bộ (người chơi, giải thưởng, lịch sử).