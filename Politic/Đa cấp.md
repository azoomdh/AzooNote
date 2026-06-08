> (đa cấp) = Người tham gia được hưởng hoa hồng từ doanh số bán hàng của chính họ và của những người trong mạng lưới do họ tuyển dụng

<br/>

mọi đại lý đa cấp mới đều phải khai báo công an, nếu không là bị phạt

<br/>

công ty đa cấp tăng trưởng A% -> chỉ được mở rộng đại lý (A-2)%

mở rộng đại lý không được quá 7%

<br/>

công ty đa cấp giảm A% -> trong vòng 1 tháng phải sa thải A% đại lý 

cách sa thải = 

1. // cần sa thải N người, cây có M tầng

2. Chọn tầng M của cây

3. nếu tầng M có nhiều hơn N -> sa thải ngẫu nhiên; kết thúc

4. nếu tầng M có đủ N, -> sa thải toàn bộ, kết thúc

5. nếu tầng M có ít hơn N -> 
   
   1. soNguoi = số người tại tầng M
   
   2. sa thải toàn bộ tầng M
   
   3. M = M-1
   
   4. N = N-soNguoi
   
   5. goto 2; (không phải 5.2)
