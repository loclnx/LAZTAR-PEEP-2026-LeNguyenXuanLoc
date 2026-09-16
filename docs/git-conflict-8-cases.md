
# Git Conflict: 8 trường hợp thường gặp và cách xử lý

## 1. Conflict khi có thay đổi chưa commit (`git stash`)

Dùng khi đang sửa code nhưng chưa commit và cần lấy code mới từ remote.

### Các bước

```bash
git stash
git pull
git stash pop
```

Nếu `git stash pop` bị conflict:

```bash
git status
```

Mở file bị conflict và sửa thủ công:

```text
<<<<<<< HEAD
code hiện tại
=======
code từ stash
>>>>>>> ...
```

Giữ hoặc kết hợp phần code phù hợp, sau đó xóa các dòng conflict marker.

Tiếp tục:

```bash
git add .
git commit -m "Resolve stash conflict"
git push
```

---

## 2. Conflict khi đã commit nhưng chưa push (`git pull --rebase`)

Dùng khi local đã có commit nhưng remote cũng có commit mới.

### Các bước

```bash
git pull --rebase
```

Nếu xảy ra conflict:

```bash
git status
```

Mở file bị conflict và sửa thủ công.

Sau đó:

```bash
git add .
git rebase --continue
```

Nếu tiếp tục gặp conflict, lặp lại:

```bash
# sửa conflict
git add .
git rebase --continue
```

Khi rebase hoàn tất:

```bash
git push
```

Nếu muốn hủy rebase:

```bash
git rebase --abort
```

---

## 3. Conflict khi dùng `git merge`

Dùng khi gộp một branch vào branch hiện tại.

Ví dụ:

```bash
git switch main
git merge loclnx
```

Nếu xảy ra conflict:

### Bước 1: Kiểm tra

```bash
git status
```

### Bước 2: Mở file conflict

Ví dụ:

```text
<<<<<<< HEAD
code của main
=======
code của loclnx
>>>>>>> loclnx
```

Sửa thủ công và xóa các conflict marker.

### Bước 3: Add file

```bash
git add .
```

### Bước 4: Hoàn tất merge

```bash
git commit
```

### Bước 5: Push

```bash
git push
```

Nếu muốn hủy merge:

```bash
git merge --abort
```

---

## 4. Conflict khi dùng `git cherry-pick`

`git cherry-pick` dùng để áp dụng một commit cụ thể vào branch hiện tại.

Ví dụ:

```bash
git cherry-pick abc123
```

Nếu conflict:

### Bước 1: Kiểm tra

```bash
git status
```

### Bước 2: Sửa file conflict

Chọn hoặc kết hợp code phù hợp rồi xóa:

```text
<<<<<<<
=======
>>>>>>>
```

### Bước 3: Add

```bash
git add .
```

### Bước 4: Tiếp tục cherry-pick

```bash
git cherry-pick --continue
```

Nếu có conflict tiếp theo, lặp lại các bước trên.

Nếu muốn hủy:

```bash
git cherry-pick --abort
```

---

## 5. Conflict khi dùng `git rebase`

Dùng khi muốn đặt các commit của branch hiện tại lên trên một branch khác.

Ví dụ:

```bash
git switch loclnx
git rebase main
```

Nếu conflict:

### Bước 1: Kiểm tra

```bash
git status
```

### Bước 2: Sửa file conflict

Mở file và sửa thủ công:

```text
<<<<<<< HEAD
code từ main
=======
code từ loclnx
>>>>>>> ...
```

Xóa các conflict marker sau khi xử lý.

### Bước 3: Add

```bash
git add .
```

### Bước 4: Tiếp tục rebase

```bash
git rebase --continue
```

Nếu lại conflict, tiếp tục:

```bash
# sửa conflict
git add .
git rebase --continue
```

Nếu muốn hủy toàn bộ rebase:

```bash
git rebase --abort
```

---

## 6. Conflict khi dùng `git revert`

`git revert` tạo một commit mới để hoàn tác một commit cũ.

Ví dụ:

```bash
git revert abc123
```

Nếu xảy ra conflict:

### Bước 1: Kiểm tra

```bash
git status
```

### Bước 2: Sửa file conflict

Chọn hoặc kết hợp code phù hợp và xóa conflict marker.

### Bước 3: Add

```bash
git add .
```

### Bước 4: Tiếp tục revert

```bash
git revert --continue
```

### Bước 5: Push

```bash
git push
```

Nếu muốn hủy:

```bash
git revert --abort
```

---

## 7. Conflict khi dùng `git stash pop`

Đây là trường hợp xảy ra khi thay đổi trong stash đụng với code hiện tại.

### Bước 1: Xem stash

```bash
git stash list
```

### Bước 2: Lấy stash ra

```bash
git stash pop
```

Nếu conflict:

### Bước 3: Kiểm tra

```bash
git status
```

### Bước 4: Sửa file conflict

Ví dụ:

```text
<<<<<<< HEAD
code hiện tại
=======
code từ stash
>>>>>>> ...
```

Chọn hoặc kết hợp code phù hợp, sau đó xóa conflict marker.

### Bước 5: Add

```bash
git add .
```

### Bước 6: Commit

```bash
git commit -m "Resolve stash conflict"
```

### Bước 7: Push nếu cần

```bash
git push
```

---

## 8. Conflict khi dùng `git apply`

`git apply` dùng để áp dụng một patch vào working tree.

Ví dụ:

```bash
git apply patch.diff
```

Nếu patch không áp dụng được hoàn toàn:

### Bước 1: Kiểm tra

```bash
git status
```

### Bước 2: Kiểm tra phần patch không áp dụng được

Git có thể tạo file `.rej`, ví dụ:

```text
file.tsx.rej
```

Mở file `.rej` để xem phần thay đổi nào không áp dụng được.

### Bước 3: Sửa thủ công

Mở file gốc và đưa phần thay đổi cần thiết vào đúng vị trí.

### Bước 4: Xóa file `.rej` nếu không còn cần

```cmd
del file.tsx.rej
```

### Bước 5: Kiểm tra thay đổi

```bash
git diff
```

### Bước 6: Add và commit

```bash
git add .
git commit -m "Apply patch"
```

---

# Bảng ghi nhớ nhanh

| Trường hợp         | Lệnh tiếp tục sau khi sửa conflict          |
| --------------------- | ----------------------------------------------- |
| `git stash pop`     | `git add .` → `git commit`                 |
| `git pull --rebase` | `git add .` → `git rebase --continue`      |
| `git merge`         | `git add .` → `git commit`                 |
| `git cherry-pick`   | `git add .` → `git cherry-pick --continue` |
| `git rebase`        | `git add .` → `git rebase --continue`      |
| `git revert`        | `git add .` → `git revert --continue`      |
| `git apply`         | Xử lý patch →`git add .` → `git commit` |

## Công thức chung

```text
Git command
    ↓
CONFLICT
    ↓
git status
    ↓
Mở file bị conflict
    ↓
Sửa / chọn / kết hợp code
    ↓
Xóa <<<<<<< ======= >>>>>>>
    ↓
git add .
    ↓
Lệnh tiếp tục tương ứng
    ↓
git push (nếu cần)
```

> Lưu ý: Không phải mọi conflict đều xuất hiện dưới dạng `<<<<<<< ======= >>>>>>>`. Một số thao tác như `git apply` có thể báo patch/hunk không áp dụng được thay vì tạo conflict marker.
