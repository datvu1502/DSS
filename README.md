# PHÂN LOẠI THƯ RÁC SỬ DỤNG THUẬT TOÁN NAIVE BAYES
Trong thời đại kỹ thuật số, ứng dụng của tin nhắn ngày càng nhiều. Kéo theo đó là tình trạng các tin nhắn rác (SMS spam) ngày càng tăng. Các tin nhắn rác không chỉ gây phiền hà cho người nhận, mà còn có thể công cụ lừa đảo. Do đó, cần có một công cụ, thuật toán để đánh dấu tin nhắn có phải là tin nhắn rác hay không.

Bài báo cáo này trình bày các bước sử dụng thuật toán Naive Bayes để phân loại thư rác (SMS spam).

# Tổng quan về bộ dữ liệu
## Bộ dữ liệu

SMS Spam Collection v.1 là một tập hợp các tin nhắn được gắn thẻ SMS đã được thu thập để nghiên cứu Spam SMS. Tập dữ liệu chứa các tin nhắn SMS bằng tiếng Anh gồm 5.572 tin nhắn, được gắn thẻ là ham (hợp pháp) hoặc spam. Bộ dữ liệu được lấy trên https://archive.ics.uci.edu/ml/datasets/sms+spam+collection. Được từ nhiều nguồn trên Internet, cụ thể là:

•	Gồm 425 tin nhắn rác SMS được trích xuất thủ công từ trang web Grumbletext. Đây là một diễn đàn của Vương quốc Anh, trong đó người dùng điện thoại di động tuyên bố công khai về tin nhắn rác SMS, hầu hết trong số họ không báo cáo về tin nhắn rác đã nhận được. Trang web Grumbletext là: http://www.grumbletext.co.uk/ .

•	Một tập hợp con gồm 3.375 tin nhắn SMS của NUS SMS Corpus (NSC), là tập hợp của khoảng 10.000 tin nhắn hợp pháp được thu thập để nghiên cứu tại Khoa Khoa học Máy tính của Đại học Quốc gia Singapore. http:
//www.comp.nus.edu.sg/ rpnlpir/downloads/corpora/smsCorpus/

•	Danh sách 450 tin nhắn ham SMS được thu thập từ Luận án Tiến sĩ của
Caroline Tag có tại http://etheses.bham.ac.uk/253/1/Tagg09PhD.pdf.

•	Số lượng 1.002 tin nhắn ham SMS và 322 tin nhắn rác được trích xuất từ SMS Spam Corpus v.0.1 Big do José María Gómez Hidalgo tạo ra và được công khai tại: http://www.esp.uem.es/jmgomez/smsspamcorp

## Thống kê dữ liệu
 
Tập dữ liệu có có tổng số 4.825 tin nhắn SMS hợp pháp (86,59%) và tổng số 747 (13,41%) tin nhắn rác.

![tkdl](https://github.com/datvu1502/DSS/assets/118582440/fa1815fd-41b8-4077-aaaa-63aebd984bdc)


## Định dạng
Tập dữ liệu chứa một thông báo trên mỗi dòng. Mỗi dòng bao gồm hai cột: một cột label chứa nhãn (ham hoặc spam) và một cột message chứa văn bản nội dung tin nhắn. Trong đó tin nhắn không được sắp xếp theo thứ tự thời gian.

![data](https://github.com/datvu1502/DSS/assets/118582440/23dd61c2-7f60-49b2-8771-6f82f588d754)


## Extract dữ liệu
Đầu tiên, ta trích xuất các thông tin sau:
•	Tổng số ký tự
•	Tổng số từ
•	Tổng số câu

![sktht](https://github.com/datvu1502/DSS/assets/118582440/47426d35-f3b5-4463-8f4d-7f379e050ade)

 
## Trực quan hóa dữ liệu ham và spam theo số lượng ký tự, từ, câu

Kết quả hiển thị ra mà hình:

![tqhpng](https://github.com/datvu1502/DSS/assets/118582440/9a5482af-4f3e-46fe-875d-263f21abc1dc)

# Tiền xử lý dữ liệu
## Cài đặt các thư viện xử lý văn bản

![image](https://github.com/datvu1502/DSS/assets/118582440/fcfc6e0d-f4e5-4daa-8307-f142c956e7e7)

## Xử lý dữ liệu văn bản
Trước khi tiến hành phân loại, việc tiền xử lý dữ liệu đầu vào là rất quan
trọng để đảm bảo kết quả phân loại chính xác. Với dữ liệu dạng chuỗi, việc xử lý
trước là cực kỳ cần thiết để phục vụ cho việc trích xuất các đặc trưng và phân
loại.

Có nhiều phương pháp khác nhau để xử lý dữ liệu chuỗi, tùy thuộc vào từng
bộ dữ liệu và mục đích phân tích. Dưới đây là một số công đoạn xử lý dữ liệu
em lựa chọn trong đề tài này:

• Loại bỏ các liên kết web

• Loại bỏ email

• Loại bỏ chuỗi số

Tạo các hàm loại bỏ:

![image](https://github.com/datvu1502/DSS/assets/118582440/1e845bcd-3c45-4a75-aae2-5190eeb26a3a)

![image](https://github.com/datvu1502/DSS/assets/118582440/c90984d4-560e-4b25-9d2b-3759988c3520)

![image](https://github.com/datvu1502/DSS/assets/118582440/b20a5c96-f7fc-4936-8c24-7a0c664a4190)

• Chuyển về chữ thường: Bước này bao gồm chuyển toàn bộ văn bản về
dạng chữ thường. Việc này là cần thiết để đảm bảo tính đồng nhất trong
dữ liệu văn bản. Ví dụ, từ "Hello" và "hello" sẽ được xem như cùng một từ. 

• Phân đoạn thành từng từ: Phân đoạn (tokenization) là quá trình chia
nhỏ văn bản thành từng từ hoặc token riêng lẻ. Điều này giúp chia nhỏ văn
bản thành các đơn vị có nghĩa. Ví dụ, câu "I love dogs" sẽ được phân đoạn
thành ba token riêng biệt: "I", "love" và "dogs".

• Loại bỏ các ký tự đặc biệt: Các ký tự đặc biệt như dấu câu, ký hiệu
hoặc bất kỳ ký tự không phải chữ hoặc số nào thường được loại bỏ khỏi
văn bản. Bước này được thực hiện để loại bỏ nhiễu và tập trung vào nội
dung văn bản chính.

• Loại bỏ các từ dừng(stopwords): Các từ dừng (stop words) là các từ
thông thường như "the", "is" hoặc "and" không mang ý nghĩa đáng kể trong
bối cảnh phân tích văn bản. Việc loại bỏ các từ dừng giúp giảm số chiều
của dữ liệu và cải thiện hiệu suất tính toán. Ngoài ra, việc loại bỏ các dấu
câu giúp làm sạch văn bản hơn và loại bỏ bất kỳ ký tự không cần thiết nào.

• Rút gọn từ: Rút gọn từ (stemming) là quá trình giảm từ về dạng gốc
hoặc hình thức cơ bản nhất của nó. Nó bao gồm việc loại bỏ các hậu tố và
biến đổi để thu được ý nghĩa cốt lõi của từ. Ví dụ, việc rút gọn từ sẽ chuyển
đổi các từ như "running", "runs" và "ran" về dạng cơ bản chung của chúng,
là "run". Bước này giúp chuẩn hóa các từ và giảm kích thước từ vựng.

Xây dựng hàm xử lý dữ liệu:

![image](https://github.com/datvu1502/DSS/assets/118582440/7b58e4b9-fedf-431a-8423-e2c9d52f55cd)

Một số kết quả hiến thị sau khi xử lý:

![hienthixl](https://github.com/datvu1502/DSS/assets/118582440/e34da172-19aa-43f6-9a2f-03c82582c6f1)

## Sửa nhãn từ ’spam’ và ’ham’ thành 1 và 0

![nhiphan](https://github.com/datvu1502/DSS/assets/118582440/25c9408d-f8b5-49f1-b806-5d77105177fb)

## Trực quan dữ liệu sau khi xử lý bằng Word cloud
Sử dụng 'đám mây từ' để xem những từ phổ biến.

• Ham:

![ham](https://github.com/datvu1502/DSS/assets/118582440/df89376d-95ad-4ed0-b20e-1c7f99a6cc70)

•	Spam:

![spam](https://github.com/datvu1502/DSS/assets/118582440/2fa21d41-60c5-4020-b33f-c01c5c2607e2)

## So sánh tổng số ký tự, từ trong tin nhắn spam và ham (hoặc không phải spam)

•	So sánh ký tự

![charecter](https://github.com/datvu1502/DSS/assets/118582440/6f88617e-4509-4ad6-af4f-0c2dfbe29ab4)

• So sánh từ

![word](https://github.com/datvu1502/DSS/assets/118582440/6d49350f-2d3d-4007-98bc-9449af272797)

# Ứng dụng thuật Naive Bayes phân loại thư rác
## Xây dựng mô hình
### Phương pháp Bag of Words

Phương pháp Bag of Words (BoW) sẽ chuyển các từ, các câu, đoạn văn ở
dạng text của văn bản về một vector mà mỗi phần tử là một số. BoW học được
một bộ từ vựng từ tất cả các văn bản rồi mô hình các văn bản bằng cách đếm
số lần xuất hiện của mỗi từ trong văn bản đó. Bag of Words không quan tâm
đến thứ tự từ trong câu và cũng không quan tâm đến ngữ nghĩa của từ. Ở đây,
ta sử dụng thư viện có sẵn ’sklearn’ để thực hiện phương pháp này.

![image](https://github.com/datvu1502/DSS/assets/118582440/6d36b699-5e21-4331-ae66-cbca9dd0a12b)

### Định dạng input và output
![image](https://github.com/datvu1502/DSS/assets/118582440/cb08de4b-1252-4170-821f-fe02ac7b6264)

### Xây dựng mô hình bằng hàm MultinomialNB() của thư viện ’sklearn.naive_bayes’:

![image](https://github.com/datvu1502/DSS/assets/118582440/1c66e144-3bcc-423c-b4a7-e5fa07ae65c2)

## Đánh giá mô hình

![image](https://github.com/datvu1502/DSS/assets/118582440/ef28daba-a502-4576-82de-9331ba5121a8)

Mô hình phân loại thư rác sử dụng thuật toán Naive Bayes trên có độ chính xác
là 96.95%.

![matran](https://github.com/datvu1502/DSS/assets/118582440/e932f35b-e977-471b-9233-91f2a01920da)

Với dữ liệu tập test:

• Mô hình dự đoán đúng 957/957 tin nhắn không phải Spam.

• Mô hình dự đoán đúng 124/158 tin nhắn Spam.


