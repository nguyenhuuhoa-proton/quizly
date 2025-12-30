# Cấu trúc YAML chi tiết cho Quizly

## 1. Metadata (Thông tin đề thi)

```yaml
metadata:
  title: "Tên đề thi"
  subject: "Môn học"
  grade: "Khối lớp (số hoặc chuỗi)"
  author: "Tên tác giả"
```

**Mô tả:**
- `title`: Tiêu đề bài ôn tập (hiển thị ở màn hình thông tin)
- `subject`: Môn học (Toán, Văn, Anh, v.v.)
- `grade`: Khối học (ví dụ: "10", "11", "12")
- `author`: Người tạo đề thi

---

## 2. Exam (Cấu hình đề thi)

```yaml
exam:
  description: "Mô tả bài thi"
  duration_minutes: 45
  start_time: "2025-01-01T00:00:00"
  end_time: "2025-12-31T23:59:59"
  shuffle_questions: true
  shuffle_answers: true
```

**Mô tả:**
- `description`: Mô tả chi tiết về bài thi
- `duration_minutes`: Thời gian làm bài (phút). Đặt `0` nếu không giới hạn
- `start_time`: Thời gian bắt đầu cho phép làm bài (ISO 8601 format)
- `end_time`: Thời gian kết thúc cho phép làm bài
- `shuffle_questions`: `true/false` - Xáo trộn câu hỏi trong mỗi nhóm (MC, TF, Essay)
- `shuffle_answers`: `true/false` - Xáo trộn đáp án (cho MC) và items (cho TF)

---

## 3. Questions (Danh sách câu hỏi)

### 3.1. Câu hỏi trắc nghiệm ABCD (`multiple_choice`)

```yaml
questions:
  - type: multiple_choice
    question:
      text: "Nội dung câu hỏi (hỗ trợ Markdown)"
      img: "base64_encoded_image"      # Tùy chọn
      img_url: "https://url/image.png" # Tùy chọn (ưu tiên img_url)
    choices:
      A:
        text: "Đáp án A"
        img: "base64"      # Tùy chọn
        img_url: "url"     # Tùy chọn
      B:
        text: "Đáp án B"
      C:
        text: "Đáp án C"
      D:
        text: "Đáp án D"
    correct: "A"  # Key của đáp án đúng
```

**Lưu ý:**
- `choices` phải có các key (A, B, C, D, hoặc tùy chỉnh)
- `correct` phải khớp với một key trong `choices`
- Có thể thêm hình ảnh cho câu hỏi và từng đáp án
- Text hỗ trợ Markdown (in đậm, in nghiêng, danh sách, v.v.)

---

### 3.2. Câu hỏi Đúng/Sai (`true_false_group`)

```yaml
  - type: true_false_group
    question:
      text: "Đề bài chung cho nhóm câu Đúng/Sai"
      img: "base64"       # Tùy chọn
      img_url: "url"      # Tùy chọn
    items:
      a:
        text: "Mệnh đề 1"
        img: "base64"     # Tùy chọn
        img_url: "url"    # Tùy chọn
        correct: true
      b:
        text: "Mệnh đề 2"
        correct: false
      c:
        text: "Mệnh đề 3"
        correct: true
      d:
        text: "Mệnh đề 4"
        correct: false
```

**Lưu ý:**
- Mỗi item có `correct: true` hoặc `correct: false`
- Học sinh phải trả lời ĐÚNG tất cả các items mới được tính là trả lời đúng câu hỏi
- Có thể thêm hình ảnh cho từng item

---

### 3.3. Câu hỏi Tự luận (`essay`)

```yaml
  - type: essay
    question:
      text: "Câu hỏi tự luận"
      img: "base64"       # Tùy chọn
      img_url: "url"      # Tùy chọn
    correct_answer: "Đáp án tham khảo (hỗ trợ Markdown)"
    note: "Ghi chú thêm cho giáo viên chấm"  # Tùy chọn
```

**Lưu ý:**
- Hệ thống sẽ sử dụng AI để chấm điểm tự động (0-100 điểm)
- `correct_answer`: Đáp án mẫu để AI so sánh
- `note`: Thông tin bổ sung giúp AI chấm chính xác hơn
- Câu trả lời bỏ trống sẽ được tính 0 điểm (không gửi AI)

---

## 4. Ví dụ hoàn chỉnh

```yaml
metadata:
  title: "Kiểm tra Toán 10 - Chương 1"
  subject: "Toán"
  grade: "10"
  author: "Nguyễn Văn A"

exam:
  description: "Bài kiểm tra 45 phút về hàm số và đồ thị"
  duration_minutes: 45
  start_time: "2025-01-01T00:00:00"
  end_time: "2025-12-31T23:59:59"
  shuffle_questions: true
  shuffle_answers: true

questions:
  # Trắc nghiệm ABCD
  - type: multiple_choice
    question:
      text: "Tập xác định của hàm số $y = \\frac{1}{x-2}$ là:"
    choices:
      A:
        text: "$\\mathbb{R}$"
      B:
        text: "$\\mathbb{R} \\setminus \\{2\\}$"
      C:
        text: "$\\mathbb{R} \\setminus \\{0\\}$"
      D:
        text: "$(2; +\\infty)$"
    correct: "B"

  # Đúng/Sai
  - type: true_false_group
    question:
      text: "Xét tính đúng sai của các mệnh đề sau:"
    items:
      a:
        text: "Hàm số $y = x^2$ đồng biến trên $\\mathbb{R}$"
        correct: false
      b:
        text: "Đồ thị hàm số $y = |x|$ đối xứng qua trục Oy"
        correct: true
      c:
        text: "Hàm số $y = \\sqrt{x}$ có tập xác định là $[0; +\\infty)$"
        correct: true
      d:
        text: "Hàm số $y = \\frac{1}{x}$ liên tục tại $x = 0$"
        correct: false

  # Tự luận
  - type: essay
    question:
      text: "Cho hàm số $y = f(x) = x^2 - 4x + 3$. Tìm tọa độ đỉnh và vẽ bảng biến thiên."
    correct_answer: |
      **Đỉnh parabol:**
      - Hoành độ: $x_0 = -\\frac{b}{2a} = \\frac{4}{2} = 2$
      - Tung độ: $y_0 = f(2) = 4 - 8 + 3 = -1$
      - Đỉnh: $I(2; -1)$
      
      **Bảng biến thiên:**
      - Hàm số nghịch biến trên $(-\\infty; 2)$
      - Hàm số đồng biến trên $(2; +\\infty)$
      - Giá trị nhỏ nhất: $y_{min} = -1$ tại $x = 2$
    note: "Yêu cầu học sinh nêu được công thức tìm đỉnh và mô tả chiều biến thiên"
```

---

## 5. Quy trình hoạt động

### 5.1. Khi tải bài thi:
1. Kiểm tra thời gian (`start_time`, `end_time`)
2. Hiển thị thông tin đề thi (metadata + exam)
3. Nhấn "Bắt đầu làm bài"

### 5.2. Trong khi làm bài:
1. Hiển thị câu hỏi theo nhóm: **Trắc nghiệm ABCD** → **Đúng/Sai** → **Tự luận**
2. Nếu `shuffle_questions = true`: Xáo trộn trong từng nhóm
3. Nếu `shuffle_answers = true`: Xáo trộn đáp án/items
4. Đếm thời gian (hoặc đếm ngược nếu có `duration_minutes`)
5. Sidebar hiển thị tiến độ (câu đã trả lời)

### 5.3. Khi nộp bài:
1. **Trắc nghiệm**: Chấm ngay lập tức
2. **Tự luận**: 
   - Câu trống → 0 điểm
   - Câu có nội dung → Gửi API AI để chấm (rate limit: 1 request/5.1s)
   - Hiển thị trạng thái "Đang chấm..." với ellipsis animation
3. Tính điểm trung bình tự luận sau khi chấm xong

---

## 6. Hỗ trợ hình ảnh

### Cách 1: Base64
```yaml
question:
  text: "Quan sát hình vẽ sau:"
  img: "iVBORw0KGgoAAAANSUhEUgAAAAUA..."
```

### Cách 2: URL (Khuyến nghị)
```yaml
question:
  text: "Quan sát hình vẽ sau:"
  img_url: "https://example.com/image.png"
```

**Lưu ý:** Nếu cả `img` và `img_url` đều có, hệ thống ưu tiên `img_url`

---

## 7. Hỗ trợ Markdown

Text trong `question.text`, `choices.text`, `items.text`, `correct_answer` đều hỗ trợ Markdown:

```yaml
question:
  text: |
    # Tiêu đề
    
    Đây là **in đậm** và *in nghiêng*.
    
    - Item 1
    - Item 2
    
    ```python
    print("Hello")
    ```
    
    Công thức: $x^2 + y^2 = r^2$
```

---

## 8. Lưu ý quan trọng

1. **Thời gian**: Định dạng ISO 8601 (`YYYY-MM-DDTHH:mm:ss`)
2. **Thứ tự câu hỏi**: Luôn nhóm theo loại (MC → TF → Essay)
3. **Chấm điểm TF**: Phải trả lời đúng TẤT CẢ items mới được tính đúng
4. **AI grading**: 
   - Tối đa 4 lần retry nếu lỗi
   - Delay 5.1s giữa các request
   - Trả về JSON: `{"score": 0-100, "feedback": "..."}`
5. **Responsive**: Hỗ trợ mobile với sidebar toggle

---

## 9. Checklist YAML hợp lệ

- [ ] Có đầy đủ `metadata` (title, subject, grade, author)
- [ ] Có đầy đủ `exam` (description, duration_minutes, start_time, end_time)
- [ ] Mỗi câu hỏi có `type` hợp lệ (multiple_choice, true_false_group, essay)
- [ ] MC: có `choices` và `correct` khớp với key trong choices
- [ ] TF: có `items` và mỗi item có `correct: true/false`
- [ ] Essay: có `correct_answer` (bắt buộc cho AI grading)
- [ ] Thời gian đúng định dạng ISO 8601
- [ ] Nếu dùng hình ảnh: `img_url` hoặc `img` base64 hợp lệ
