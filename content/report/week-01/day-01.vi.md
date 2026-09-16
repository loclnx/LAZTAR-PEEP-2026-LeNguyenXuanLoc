+++
title = "Ngày 01 - Báo cáo Git"
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
1. ĐỐI VỚI CHƯA COMMIT: Dùng 'git stash' để cất tạm những thay đổi bạn chưa commit ra khỏi working directory, để thư mục code trở về trạng thái sạch. Sau đó pull code mới về, dùng 'git stash pop' để lấy lại những file đã cất tạm, rồi git add git commit push lên bình thường.
2. ĐỐI VỚI VIỆC ĐÃ COMMIT: Dùng 'git pull --rebase' để đưa commit lên remote mới nhất, sau đó sửa conflict thủ công sau đó 'git add .' và 'git rebase --continue'

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
