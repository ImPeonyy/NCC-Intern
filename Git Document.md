Git
- Git là một hệ thống quản lý phiên bản phân tán (Distributed Version Control System – DVCS), được sử dụng rộng rãi trong quản lý mã nguồn và dự án phần mềm.

- Điểm chính:
	+ Quản lý phiên bản: Giúp theo dõi sự thay đổi trong mã nguồn của dự án. Bạn có thể xem lịch sử các sự thay đổi, so sánh các phiên bản khác nhau và quay lại bất kỳ phiên bản cụ thể nào.

	+ Hệ thống phân phối: Cho phép làm việc với nhiều bản sao của dự án trên nhiều máy tính khác nhau.

	+ Nhánh và hợp nhất: Cho phép bạn tạo và quản lý nhiều nhánh của dự án, giúp phát triển độc lập các tính năng và sửa lỗi mà không ảnh hưởng đến phiên bản chính. Sau đó, bạn có thể hợp nhất (merge) các nhánh này lại với nhau.

- Git Convention:
	+ Đặt tên branch:
		> Sử dụng tiền tố: như feature/, bugfix/ ,release/,...
		> Sử dụng dấu gạch ngang: dùng để phân tách các từ dễ đọc( như: feature/login-authenticate).
		> Đặt tên ngắn gọn nhưng có ý nghĩa.
		> Có thể bao gồm số của issuse(nếu có).

	+ Format commit:
		> Bắt đầu message title ngắn gọn nhằm tóm tắt mục đích, sau message title là động từ như: Add, Update, Delete, Drop, Fix, Optimize,...
		> Sử dụng tối đa 50 kí tự khi mô tả commit message, cách 1 khoảnh trắng giữa message title với commit message.

	+ Cấu trúc:

		<type>[optional scope]: <description>

		> Type: từ khoá để phân loại commit: feature, fixbug, optimize, refactor,..
		> Scope: vùng mà commit ảnh hưởng tới( ví dụ: feat(authentication), fix(search)
		> Description: mô tả ngắn về sửa đổi trong commit đấy( ví dụ: feat(search): add more options for search

	+ Các loại convention commit:
		> Feat: thêm 1 feature.
		> Fix: fix bug của hệ thống, vá lỗi.
		> Refactor: sửa code nhưng không fix bug cũng không thêm feature hoặc đôi lúc fix bug từ việc này.
		> Docs: thêm/thay đổi document.
		> Chore: những sửa đổi nhỏ nhặt không liên quan tới code.
		> Style: những thay đổi không làm thay đổi ý nghĩa của code như css, ui.
		> Perf: code cải tiến hiệu suất xử lý.
		> Vendor: cập nhật version cho các module, packages, dependencies,..

		> Ví dụ: feat: add feature A for login
			 Fix: fix cannot search product
			 feat(feature_A): add feature A1

	

- Git Commands:
	+ Lệnh cơ bản: 
		> git init: Khởi tạo một kho Git mới ở local.
		> git clone [url]: Tạo bản sao của kho Git hiện có từ một URL.
		> git status: Hiển thị trạng thái của các thay đổi trong thư mục làm việc.
		> git add [file]: Đưa tệp (hoặc tệp) vào staging.
		> git add .: Đưa tất cả tệp vào staging 
		> git commit -m "message": Commit các thay đổi ở staging với message.
		> git push: Đẩy các commit từ local lên remote.
		> git pull: Tải về các thay đổi từ remote và gộp chúng vào.
		> git fetch: Tải về các thay đổi từ remote nhưng không gộp vào.
		> git merge [branch]: Gộp nhánh được chỉ định vào nhánh hiện tại.
		> git branch: Liệt kê tất cả các nhánh trong repo.
		> git branch [branch-name]: Tạo một nhánh mới.
		> git checkout [branch-name]: Chuyển sang nhánh được chỉ định.
		> git checkout -b [branch-name]: Tạo và chuyển sang một nhánh mới.
		> git log: Hiển thị lịch sử commit.
		> git diff: Hiển thị sự khác biệt giữa các commit, thư mục làm việc, v.v.

	+ Lệnh Remote:
		> git remote -v: Liệt kê các remote repo liên kết với local repo.
		> git remote add [name] [url]: Thêm một remote repo mới.
		> git remote remove [name]: Xóa một remote repo.
		> git push [remote] [branch]: Đẩy nhánh hiện tại lên remote repo.
		> git pull [remote] [branch]: Tải về các thay đổi từ nhánh remote và gộp chúng vào nhánh hiện tại.
	
	+ Lệnh Branches và Merge:
		> git branch -d [branch-name]: Xóa một nhánh (local).
		> git branch -D [branch-name]: Xóa một nhánh sạch sẽ.
		> git merge --no-ff [branch-name]: Gộp nhánh nhưng đảm bảo tạo một commit gộp.
		> git rebase [branch]: Áp dụng lại các commit trên một nhánh khác (thường dùng để tạo lịch sử tuyến tính).

	+ Lệnh Stash:
		> git stash: Lưu trữ tạm thời các thay đổi chưa commit.
		> git stash pop: Áp dụng thay đổi từ stash mới nhất và xóa nó khỏi danh sách stash.
		> git stash list: Liệt kê tất cả các stash.
		> git stash drop: Xóa một stash cụ thể.

	+ Lệnh Convert:
		> git reset [file]: Hủy bỏ tệp đã được đưa vào khu vực chờ.
		> git reset --hard: Đặt lại thư mục làm việc và khu vực chờ về commit cuối cùng (cảnh báo: thao tác này sẽ xóa dữ liệu).
		> git revert [commit]: Hoàn tác thay đổi của một commit cụ thể bằng cách tạo ra một commit mới.
		> git clean -f: Xóa các tệp không được theo dõi trong thư mục làm việc.

- Advanced commands: 
	+ git cherry-pick [commit]: Áp dụng thay đổi từ một commit cụ thể lên nhánh hiện tại.
	+ git reflog: Hiển thị lịch sử thay đổi của các tham chiếu (dùng để phục hồi commit bị mất).
	+ git bisect: Giúp tìm ra commit gây lỗi bằng cách thực hiện tìm kiếm nhị phân.
	+ git submodule: Quản lý các submodule Git trong kho.

- Git Flow:
	+ Create a branch: Tạo 1 nhánh mới cho 1 chức năng mới hoặc để cho các mục đích khác
	+ Makes changes and commits: Lưu sự thay đổi code và tạo commit
	+ Deploy: Deploy 1 nhánh có thể là từ local lên remote.
	+ Open Pull Request: Tạo PR để thông báo cho sự thay đổi của code mình vừa tạo để review
	+ Review: Xem xét và thảo luận PR nhằm cải thiện hoặc fix nếu có
	+ Merge: Sau khi review PR và được approve, thì sẽ thực hiện merge vào main branch.

- Merge - Rebase:
	+ Merge: 
		> Tạo commit gộp mới
		> Giữ nguyên lịch sử commit của cả 2 nhánh
		> Giải quyết xung đột khi merge
		> Có commit gộp
		> Lịch sử phức tạp với nhiều merge commit
	+ Rebase: 
		> Di chuyển các commit lên nhánh đích
		> Lịch sử tuyến tính, các commit được thay thế
		> Giải quyết xung đột cho từng commit
		> Không có commit gộp (merge commit)
		> Lịch sử sạch sẽ và tuyến tính

	+ Khi nào sử dụng Merge:
		> Khi bạn muốn giữ nguyên lịch sử của cả hai nhánh.
		> Khi làm việc với các nhóm và không muốn thay đổi lịch sử commit của người khác.
		> Khi bạn cần ghi lại một dấu mốc rõ ràng về việc gộp nhánh.

	+ Khi nào sử dụng Rebase:
		> Khi bạn muốn có lịch sử commit sạch sẽ và tuyến tính, đặc biệt khi làm việc một mình hoặc trên các nhánh riêng.
		> Khi bạn muốn đưa nhánh của mình lên "phía trước" nhánh đích mà không tạo ra commit gộp.
		> Khi bạn muốn chỉnh sửa lịch sử commit trước khi gộp nhánh vào nhánh chính (thường là trước khi push vào kho từ xa).