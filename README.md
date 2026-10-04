# Báo cáo: Quản lý nhánh và Giải quyết xung đột (Merge Conflict)

## 1. Mục tiêu
- Tạo mới và di chuyển giữa các nhánh (`main`, `feature-update`).
- Tạo xung đột gộp nhánh (Merge Conflict) trên cùng một dòng ở tệp tin `README.md`.
- Xử lý xung đột thủ công, loại bỏ các ký hiệu đánh dấu (`<<<<<<<`, `=======`, `>>>>>>>`).
- Hoàn thành commit gộp nhánh (3-Way Merge) và kiểm tra đồ thị lịch sử commit qua `git log --graph --oneline`.

---

## 2. Quá trình tạo xung đột & Các bước giải quyết thủ công

### Bước 1: Tạo commit ban đầu trên nhánh `main`
```bash
git init
git checkout -b main
# Tạo README.md ban đầu
git add README.md
git commit -m "Initial commit on main"
```

### Bước 2: Tạo nhánh `feature-update` và chỉnh sửa
```bash
git checkout -b feature-update
# Chỉnh sửa phần Description thành:
# "This project focuses on automated deployments and CI/CD pipeline integration."
git add README.md
git commit -m "Update project description for CI/CD on feature-update"
```

### Bước 3: Quay lại nhánh `main` và chỉnh sửa cùng dòng để gây xung đột
```bash
git checkout main
# Chỉnh sửa phần Description thành:
# "This project focuses on Docker containerization and Kubernetes orchestration."
git add README.md
git commit -m "Update project description for Docker/K8s on main"
```

### Bước 4: Thực hiện gộp nhánh và phát sinh xung đột
```bash
git merge feature-update
```
**Output terminal:**
```plaintext
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

---

## 3. Chi tiết xung đột và Xử lý thủ công

### 3.1. Trạng thái xung đột trong file `README.md`
Khi mở tệp tin `README.md`, Git đánh dấu xung đột bằng các ký hiệu:
```markdown
<<<<<<< HEAD
This project focuses on Docker containerization and Kubernetes orchestration.
=======
This project focuses on automated deployments and CI/CD pipeline integration.
>>>>>>> feature-update
```

### 3.2. Tiến hành xử lý thủ công (Manual Resolution)
- **Phân tích**:
  - Đoạn từ `<<<<<<< HEAD` đến `=======` là nội dung trên nhánh hiện tại (`main`).
  - Đoạn từ `=======` đến `>>>>>>> feature-update` là nội dung từ nhánh được gộp vào (`feature-update`).
- **Cách giải quyết**: Giữ lại và kết hợp cả hai ý của hai nhánh để có mô tả đầy đủ nhất, sau đó xóa sạch các ký hiệu `<<<<<<<`, `=======`, `>>>>>>>`.

**Nội dung sau khi giải quyết xung đột:**
```markdown
# Project Documentation

## Project Name
DevOps Learning Project

## Description
This project focuses on Docker containerization, Kubernetes orchestration, and automated CI/CD pipeline integration.
```

### Bước 5: Hoàn tất commit gộp nhánh (Merge Commit)
```bash
git add README.md
git commit -m "Merge branch 'feature-update' and resolve conflict in README.md"
```

---

## 4. Kết quả kiểm tra lịch sử commit

Chạy lệnh kiểm tra đồ thị phân nhánh:
```bash
git log --graph --oneline
```

**Output:**
```plaintext
*   63c3d78 (HEAD -> main) Merge branch 'feature-update' and resolve conflict in README.md
|\  
| * ba93659 (feature-update) Update project description for CI/CD on feature-update
* | 156b2f6 Update project description for Docker/K8s on main
|/  
* 0df9416 Initial commit on main
```

**Đánh giá kết quả**:
- Nhánh `feature-update` tách ra từ commit gốc `0df9416`.
- Cả 2 nhánh `main` và `feature-update` phát triển song song với 2 commit độc lập (`156b2f6` và `ba93659`).
- Xung đột được giải quyết triệt để và kết hợp tại merge commit có 2 nhánh tổ tiên (`7b52a91`).
