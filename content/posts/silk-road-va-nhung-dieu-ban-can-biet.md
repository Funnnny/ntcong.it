+++
date = "2013-10-09T21:06:00+07:00"
title = "Silk Road và những điều bạn cần biết"
author = "Cong Nguyen"
authorlink = "http://ntcong.it"
authorfacebook = "http://facebook.com/ntcong.it"
authorgoogleplus = "https://plus.google.com/+CongNguyen"
Categories = ["thought", "bitcoin"]
Tags = ["thought", "bitcoin"]
thumbnail = "img/silkroad.png"

+++

Nhân tiện một buổi họp công nghệ và bắt gặp 1 dòng chữ "Silk road bảo mật website như thế nào?", mình phát hiện ra 1 điều là mọi người đọc báo và biết về Silk Road, chứ ko hề hiểu nó là cái gì. Và vì thế, lại 1 bài chém gió ra đời.

**Vậy, câu hỏi đầu tiên, Silk Road là cái gì?**

- Silk Road là một chợ đen hoạt động trên mạng Tor.

Đến đây đủ để giải đáp cho câu hỏi "Silk road bảo mật website như thế nào?": câu trả lời là "Silk Road không hề bảo mật website"

Nếu vậy thì lí do gì để có Silk Road? Silk Road được sinh ra nhằm mục đích bảo vệ thông tin người sử dụng, vì vậy Silk Road đã sử dụng những thành phần sau:

*   Tor network: nói 1 cách đơn giản thì Tor giúp người sử dụng giấu đi tất cả mọi thứ: location, usage...nhằm mục đích ngăn chặn việc điều tra ra người dùng bằng hành vi trên mạng. Đây là bước thứ nhất: liên hệ
*   PGP: Mỗi dealer trên BitCoin đều sử dụng PGP để giao dịch mua bán, qua đó dealer sẽ cấp 1 public key, những người liên hệ với dealer sẽ encrypt địa chỉ, thông tin và message bằng public key đó, và chỉ dealer có private key mới giải mã để đọc được. Đây là bước thứ 2: giao dịch
*   BitCoin: BitCoin là một loại tiền được cho là&nbsp;pseudo-anonymous, các giao dịch trên BitCoin được mã hóa và không thông qua một máy chủ tập trung nào. Ngoài ra SilkRoad còn cung cấp 1 tumbler, được sử dụng như một trung gian thứ 3 nhận tiền và gửi tiền. Đây là bước thứ 3: trả tiền.

Như vậy, một giao dịch hoàn toàn được bảo mật và dealer lẫn buyer đều anonymous. Đó chính là lí do Silk Road tồn tại: tạo lập một sân chơi, chứ không phải là một shop buôn bán drug.

**Vậy, lí do tại sao tôi dám chém gió là Silk Road không hề bảo mật website?**

Nếu bạn lắm thời gian và thừa dở hơi như tôi thì có thể đọc [cái này](http://www1.icsi.berkeley.edu/~nweaver/UlbrichtCriminalComplaint.pdf). Một số điểm quan trọng:


*   23&#47;07/2013, FBI đã có một bản hoàn thiện website Silk Road, trong đó có user account và thông tin giao dịch
*   FBI đã nắm được&nbsp;1,217,218 message trao đổi trên mạng lưới Silk Road từ 24&#47;5-&gt;23&#47;07/2013.

Đa phần thông tin trong này không có ý nghĩa, vì những điều được nhắc tới ở trên: bảo mật giao dịch, không bảo mật website ở mức cao cấp (chỉ có dở hơi mới tin là nó không làm gì để bảo mật website cả). Tất nhiên một số người stupid không dùng PGP thì sẽ bị bắt.


**Lí do tại sao admin trang này, DPR bị bắt**


1.  Người đầu tiên quảng bá Silk Road vào năm 2011, được biết đến qua điều tra tên là "altoid"&nbsp;
2.  11&#47;2011, một người tên altoid post trên Bit Coin Talk, đăng tìm người và dẫn đến email tại gmail "rossulbricht"


Từ đây, FBI rất dễ dàng điều tra ra**&nbsp;Ross William Ulbricht**, người sáng lập trang web, bằng rất nhiều nguồn khác nhau:


1.  LinkedIn, Google+, Youtube đều từ cái tên mà bị truy ra
2.  Dùng VPN log-in vào server Silk Road, từ đó bị truy ra địa chỉ nhà
3.  Bị điều tra ra sử dụng rất nhiều ID giả
4.  Post trên Stack Overflow với tên thật và hỏi về Tor, sau đó sửa tên thành frosty, trùng tên với SSH public key được lưu trên server Silk Road (stupid)
5.  Cùng với nhiều biện pháp nghiệp vụ khác.

Vì vậy, sử dụng VPN chỉ là một trong những điều khiến cho DPR bị bắt, chứ không phải là nguyên nhân, bởi vì một khi đã bị bắt tên và cái ảnh, thì chắc chắn là không có lí do gì để thoát cả.





**Kết luận**


1.  Silk Road là mạng lưới chợ đen, thành công của nó nằm ở chỗ bảo vệ thông tin người mua bán, chứ không phải là bảo mật website
2.  DPR bị bắt vì những sai lầm rất căn bản và từ rất lâu, tất cả đều liên quan đến anonymity.

Bài học được rút ra là: đừng phạm tội, nếu có phạm tội thì cũng đừng ngu như thằng này.

Thông tin được tổng hợp bởi một redditor thỉnh thoảng vào /r/Bitcoin và /r/SilkRoad (tất nhiên là không hề có tham gia mua bán gì)

