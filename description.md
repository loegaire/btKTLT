Dear all
Như thầy đã kể chuyện về cuộc thi League of Robot Runner 2024
Nhóm được giải nhất là một nhóm ở Nga: Team_No_Man's_Sky. Đây là nhóm đã giành được vài giải ở cuộc thi 2023 và đến cuộc thi 2024 là nhóm gần như giành được tất cả các giải nhất ở các hạng mục
Cuộc thi này có 2 phần mã nguồn:
1. Phần tìm đường cho <= 10.000 robot chạy trong 5000 giây. Ở: https://github.com/MAPF-Competition/Start-Kit/tree/main. Các nhóm được phép sửa mã nguồn src/MAPFPlanner.cpp và inc/MAPFPlanner.h (Nơi chứa logic tìm đường).
2. Phần hiển thị giao diện đồ họa trực quan: https://github.com/MAPF-Competition/PlanViz
Một phiên bản giải pháp của nhóm đạt giải nhất 2024:
https://github.com/MAPF-Competition/Code-Archive/tree/master/2024%20Competition/Team_No_Man's_Sky/faa15de96d1521a81d54bc4cb72a0fa6f2302a58
=========================
Với mong muốn học hỏi được cách làm của nhóm giải nhất, để một năm nào đó có nhóm Việt Nam được tham gia cuộc thi này (vốn do Amazon tài trợ); thầy có bài tập như sau:
chạy thử mã nguồn giải pháp của nhóm Team No Man's Sky, với số lượng robot 10, 20, 40, 80, 100. Sau đó thử hiển thị trực quan trên PlanViz
Tiếp theo sửa lại mã nguồn của Start-Kit để sao cho mỗi khi robot đến đích X (họ dùng số nguyên để đại diện cho điểm ở không gian 2D) thì nó dừng tại X trong khoảng thời gian (X% 100) giây.
Để biết các em làm được hay không thì bạn làm được phải quay video clip chạy với số robot bằng 10 và đẩy source code của bạn ấy lên github (cùng video)
bạn nào làm được sẽ được +10 nhé ^_^

chỉ cần chạy trên map là Warehouse thôi nhé
https://github.com/MAPF-Competition/Start-Kit/blob/main/example_problems/warehouse.domain/maps/warehouse_large.map
