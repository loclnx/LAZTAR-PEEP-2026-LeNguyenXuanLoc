
+++
title = "Ngày 01 - 15/09/2026 (ON-SITE) Báo cáo Git"
weight = 1
+++

### Báo cáo Ngày 01

### Nội dung đã học

Những lệnh hoạt động với Git:

- Khởi tạo repository bằng `git init` và kết nối với repository từ xa bằng `git remote`.
- Sao chép repository bằng `git clone`, đồng bộ thay đổi bằng `git fetch` và `git pull`.
- Kiểm tra trạng thái bằng `git status` và quản lý branch bằng `git branch`, `git switch` và `git checkout`.
- Chuẩn bị thay đổi bằng `git add`, lưu thay đổi bằng `git commit` và chỉnh sửa commit gần nhất bằng `git commit --amend`.
- Đẩy thay đổi lên repository từ xa bằng `git push`, hoàn tác hoặc sắp xếp lại commit bằng `git reset` và `git rebase`.
- Sử dụng `git rebase -i` để xem lại và làm gọn lịch sử commit.
- Tạm thời lưu thay đổi chưa hoàn thành bằng `git stash` và khôi phục bằng `git stash pop`.
- Kết hợp thay đổi từ các branch bằng `git merge` và áp dụng một commit cụ thể bằng `git cherry-pick`.

LƯU Ý: LUÔN PHẢI FETCH VÀ PULL CODE VỀ ĐỂ TRÁNH CONFLICT, VÀ CHECK BRANCH HIỆN TẠI

CÁCH GIẢI QUYẾT CONFLICT THƯỜNG GẶP:

Khi Git báo conflict, trước tiên kiểm tra trạng thái repository:

```bash
git status
```

Mở các file bị ảnh hưởng, xử lý phần code giữa các conflict marker, xóa marker, kiểm tra lại bằng `git diff` rồi chạy `git add`.

#### 1. Thay đổi chưa commit (`git stash`)

```bash
git stash
git pull
```

#### 2. Khôi phục thay đổi đã stash (`git stash pop`)

```bash
git stash pop
```

Nếu `git stash pop` bị conflict, sửa file rồi chạy `git add .`, `git commit -m "Resolve stash conflict"` và `git push`.

#### 3. Đã commit nhưng chưa push (`git pull --rebase`)

```bash
git pull --rebase
# sửa conflict
git add .
git rebase --continue
git push
```

Có thể hủy bằng `git rebase --abort`.

#### 4. Gộp branch (`git merge`)

```bash
git switch main
git merge <ten-branch>
# sửa conflict
git add .
git commit
git push
```

Hủy bằng `git merge --abort`.

#### 5. Áp dụng một commit (`git cherry-pick`)

```bash
git cherry-pick <commit-id>
# sửa conflict
git add .
git cherry-pick --continue
```

Hủy bằng `git cherry-pick --abort`.

#### 6. Rebase branch (`git rebase`)

```bash
git switch <ten-branch>
git rebase main
# sửa conflict
git add .
git rebase --continue
```

Lặp lại nếu có conflict tiếp theo. Hủy bằng `git rebase --abort`.

#### 7. Hoàn tác commit (`git revert`)

```bash
git revert <commit-id>
# sửa conflict
git add .
git revert --continue
git push
```

Hủy bằng `git revert --abort`.

#### 8. Áp dụng patch (`git apply`)

Nếu patch không áp dụng được hoàn toàn, Git có thể tạo file `.rej` thay vì conflict marker. Đọc phần hunk bị từ chối, đưa thay đổi cần thiết vào file gốc, xóa file `.rej` khi không còn cần rồi chạy:

```bash
git add .
git commit -m "Apply patch"
```

### Thực hành

#### 1. `git init`

![git init](/images/report/day-01/git_init_remote_branch_add.png)

#### 2. `git remote`

![git remote](/images/report/day-01/git_init_remote_branch_add.png)

#### 3. `git clone`

![git clone](/images/report/day-01/git_clone.png)

#### 4. `git fetch`

![git fetch](/images/report/day-01/git_fetch.png)

#### 5. `git pull`

![git pull](/images/report/day-01/git_pull.png)

#### 6. `git status`

![git status](/images/report/day-01/git_status_switch.png)

#### 7. `git branch`

![git branch](/images/report/day-01/git_status_switch.png)

#### 8. `git switch`

![git switch](/images/report/day-01/git_status_switch.png)

#### 9. `git checkout`

![git checkout](/images/report/day-01/git_push_checkout.png)

#### 10. `git add`

![git add](/images/report/day-01/git_init_remote_branch_add.png)

#### 11. `git commit`

![git commit](/images/report/day-01/git_commit.png)

#### 12. `git commit --amend`

![git commit --amend](/images/report/day-01/git_commit_--amend.png)

#### 13. `git push`

![git push](/images/report/day-01/git_push_checkout.png)

#### 14. `git reset`

![git reset](/images/report/day-01/git_reset.png)

#### 15. `git rebase`

![git rebase](/images/report/day-01/git_rebase.png)

#### 16. `git rebase -i`

![git rebase -i](/images/report/day-01/github_rebase-i.png)

#### 17. `git stash`

![git stash](/images/report/day-01/git_stash_stashpop.png)

#### 18. `git stash pop`

![git stash pop](/images/report/day-01/git_stash_stashpop.png)

#### 19. `git merge`

![git merge](/images/report/day-01/conflic.png)

#### 20. `git cherry-pick`

![git cherry-pick](/images/report/day-01/git_cherry.png)
