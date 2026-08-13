# Mở đầu: Dự án Kho Dữ liệu & Học máy Toàn diện

Khí hậu Việt Nam vô cùng đa dạng và ngày càng khó lường. Với tư cách là những sinh viên đam mê Kỹ thuật Dữ liệu (Data Engineering), chúng tôi muốn vượt ra khỏi những bảng tính đơn giản để xây dựng một kiến trúc mạnh mẽ, có khả năng mở rộng nhằm xử lý và phân tích 12 năm dữ liệu khí tượng trên khắp 40 tỉnh thành. 

Kho lưu trữ (repository) này ghi lại toàn bộ hành trình của chúng tôi: từ việc thiết kế một Kho dữ liệu (Data Warehouse) tập trung và viết các truy vấn đa chiều phức tạp, đến việc xây dựng các dashboard tương tác và huấn luyện các mô hình học máy để dự báo lượng mưa.

## Thách thức
Dữ liệu khí tượng tự nhiên thường có nhiều nhiễu, mang tính mùa vụ cao và phức tạp về mặt không gian. Việc làm việc với tập dữ liệu thô (hơn 181.000 bản ghi hàng ngày từ 2009 đến 2021) đã đặt ra một số thách thức:
* **Sự phân mảnh:** Dữ liệu cần được làm sạch kỹ lưỡng, loại bỏ trùng lặp và tái cấu trúc sang một định dạng tối ưu cho các truy vấn phân tích tốc độ cao.
* **Tính đa chiều:** Việc theo dõi các biến đổi khí hậu đòi hỏi phải "cắt lát" dữ liệu theo thời gian (Năm/Quý/Tháng) và địa lý (Miền/Tiểu vùng/Tỉnh) cùng một lúc.
* **Biến động phi tuyến tính:** Lượng mưa cực kỳ khó dự đoán nếu chỉ dùng các phương pháp tuyến tính đơn giản do sự tương tác phức tạp giữa độ ẩm, áp suất và gió.

## Giải quyết bài toán

### 1. Xây dựng Nguồn Sự thật Duy nhất (ETL & Kho Dữ liệu)
Thay vì truy vấn trên các file phẳng (flat files), chúng tôi đã thiết kế một Kho dữ liệu theo **Mô hình Hình sao (Star Schema)** trên SQL Server. Chúng tôi xây dựng một luồng ETL tự động bằng **SSIS** để trích xuất các file CSV thô, áp dụng các phép biến đổi (dùng Derived Column để tách thời gian, Sort để loại bỏ trùng lặp), và nạp dữ liệu sạch vào bảng trung tâm `FACT_WEATHER` được bao quanh bởi 4 bảng chiều (Dimension).

### 2. Cắt lát và Xoay chiều Dữ liệu (OLAP)
Để trả lời các câu hỏi nghiệp vụ phức tạp một cách nhanh chóng, chúng tôi đã xây dựng một khối OLAP Cube bằng **SSAS**. Bằng cách định nghĩa các phân cấp (ví dụ: Miền -> Tiểu vùng -> Tỉnh) và viết 15 câu **truy vấn MDX** nâng cao, chúng tôi có thể dễ dàng theo dõi các điểm bất thường, chẳng hạn như những tỉnh nào có số ngày mưa giảm từ năm 2020 đến 2021, hay hướng gió chủ đạo trong những mùa mưa bão đỉnh điểm.

### 3. Kể chuyện bằng Dữ liệu (Business Intelligence)
Chúng tôi kết nối khối OLAP với **Power BI** và **Looker Studio** để "thổi hồn" vào những con số. Các dashboard của chúng tôi cung cấp những góc nhìn trực quan về:
* Phân bố lượng mưa theo miền bằng biểu đồ Sankey (làm nổi bật miền Trung là khu vực mưa nhiều nhất).
* Mối tương quan nghịch biến giữa nhiệt độ và độ ẩm thông qua biểu đồ tán xạ (scatter plots) và bản đồ nhiệt (heatmaps).
* Xu hướng biến động nhiệt độ lịch sử kéo dài hơn một thập kỷ.

### 4. Dự báo những điều khó lường (Học máy - Machine Learning)
Chúng tôi không dừng lại ở việc báo cáo lịch sử. Dữ liệu sau khi xử lý được đưa vào Python để dự báo tổng lượng mưa hàng tháng. Sau khi xử lý phân phối lệch phải (right-skewed) và tạo thêm các đặc trưng độ trễ thời gian (`rain_lag12`), chúng tôi đã thử nghiệm một số mô hình học máy dạng cây (tree-based ensemble models). 

**Hiệu suất mô hình trên Tập kiểm tra (Test Set):**

| Thuật toán | Điểm R² | RMSE |
| :--- | :--- | :--- |
| Random Forest | 68.22% | 2,839.32 |
| LightGBM | 67.54% | 2,869.92 |
| **CatBoost (Tốt nhất)** | **69.91%** | **2,762.88** |

*CatBoost đã chứng minh là mô hình bền bỉ nhất trong việc nắm bắt bản chất phi tuyến tính và mang tính mùa vụ cao của lượng mưa tại Việt Nam.*

## Cấu trúc Thư mục (Repository)
Để dễ dàng điều hướng, dự án được tổ chức thành các giai đoạn theo dạng module:
* `0_Dataset/`: Điểm bắt đầu (dữ liệu khí tượng CSV thô).
* `1_SSIS_ETL/`: Solution Visual Studio chứa các tác vụ Data Flow và trình quản lý kết nối.
* `2_SSAS_Cube/`: Thiết kế khối OLAP cube, các chiều phân cấp và các độ đo tính toán.
* `3_MDX_and_Excel/`: Thư mục chuyên dụng chứa 15 kịch bản truy vấn `.mdx` nâng cao và các file `.xlsx` thể hiện quá trình phân tích đa chiều qua PivotTables và PivotCharts.
* `4_Dashboards/`: Các file báo cáo trực quan và dashboard (file `.pbix`, các tài liệu tham khảo Looker Studio và các bản xuất PDF).
* `5_Data_Mining/`: Jupyter notebooks trình bày chi tiết quá trình Khám phá dữ liệu (EDA), kỹ thuật trích xuất đặc trưng (feature engineering) và huấn luyện mô hình học máy.

## Công cụ và Công nghệ
* **Lưu trữ & Xử lý:** SQL Server, SQL Server Integration Services (SSIS), SQL Server Analysis Services (SSAS).
* **Phân tích & Trực quan hóa:** MDX, Power BI, Looker Studio.
* **Khoa học Dữ liệu:** Python, Pandas, Scikit-learn, LightGBM, CatBoost.

## Team
* **Võ Hồ Trung Quân** - Data Engineer / Data Analyst
* **Trần Đình Trung Hiếu** - Data Engineer / Data Analyst

*Dự án này được thực hiện như một đồ án tổng hợp cho môn học Kho Dữ liệu và OLAP tại Trường Đại học Công nghệ Thông tin (UIT).*
