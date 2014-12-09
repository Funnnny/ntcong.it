+++
date = "2013-09-12T16:06:44+07:00"
title = "Bitcoin - Người ta đã làm ra nó như thế nào"
author = "Cong Nguyen"
authorlink = "http://ntcong.it"
authorfacebook = "http://facebook.com/ntcong.it"
authorgoogleplus = "https://plus.google.com/+CongNguyen"
Categories = ["thought", "bitcoin"]
Tags = ["thought", "bitcoin"]
thumbnail = "img/bitcoin.png"

+++

Bitcoin không còn là mới, nó đã xuất hiện được vài năm nay. Nhưng chỉ trong thời gian gần đây nó mới bùng nổ thành một hình thức thanh toán cực kì tiện lợi.

Bitcoin là một cryptocurrency, để hiểu được một cách chính xác Bitcoin là gì cần một lượng kiến thức về software, crypto, network nhất định. Tuy nhiên chúng ta có thể tiếp cận Bitcoin để hiểu nó như một người sử dụng "có hiểu biết".

Như vậy, Bitcoin được hình thành trên những concept gì?

## 1. Mô hình không tập trung

Tiền mặt thông thường được lưu qua các tài khoản. Việc bạn gửi tiền từ tài khoản của mình sang tài khoản người khác được thực hiện thông qua việc gặp trực tiếp, hoặc qua ngân hàng.

Ngân hàng sẽ làm nhiệm vụ kiểm tra tiền của bạn có thật không, có hợp pháp không; đảm bảo việc chuyển tiền giữa bạn và người kia là duy nhất (không có chuyện gửi 2 lần); đảm bảo rằng bạn không thể chuyển tiền cho người kia rồi sau đó lấy lại chuyển cho người khác mà không có lí do. Ngoài ra tiền của bạn cũng không tự nhiên mà có, cũng phải có một ai đó in nó ra, cấp cho bạn qua một hình thức nào đó.

Bitcoin được sinh ra với mục tiêu không có ai là đơn vị quản lí tập trung nó cả, tức là sẽ không có ai kiểm tra tiền đó có thật không, không ai đảm bảo rằng bạn không chuyển tiền 2 lần cả. Vì vậy nó phải sinh ra một số phương pháp để phục vụ mục đích này.

## 2. Định danh người - Chữ kí điện tử

Việc đầu tiên trong quá trình chuyển một cục tiền điện tử, là xác định xem người gửi và người nhận là ai. Đơn cử như dùng Facebook thì phải login, dùng ngân hàng thì phải xuất thẻ CMND. Giả sử, bạn A muốn gửi cho bạn B một đồng tiền, bạn ấy sẽ viết "Tôi, tên là A, gửi cho B 1 đồng". Làm thế nào để bạn B biết chắc chắn rằng đó là từ bạn A gửi?

Điều này được thực hiện bằng một thuật ngữ gọi là **public-key cryptography**, tương tự như việc đóng dấu. Bạn có một con dấu và cung cấp cái hình con dấu cho mọi người biết, một khi mọi người nhìn thấy văn bản được đóng dấu, tức là tài liệu đó đã được tin tưởng rằng do bạn viết ra (coi như là con dấu không thể làm giả). Tương tự như vậy, đối với public-key crypto, bạn có một private-key (con dấu) và một public-key (hình). Bạn sẽ kí bằng private-key vào dãy "Tôi, tên là A, gửi cho B 1 đồng" ra một văn bản khác. Người nhận khi nhận được văn bản đó, sẽ dùng public-key để giải mã ra được "Tôi, tên là A, gửi cho B 1 đồng". Như vậy, thông tin giao dịch chắc chắn đã được đảm bảo, bất cứ ai cũng có thể biết A gửi cho B 1 đồng, nhưng không thể có con dấu để giả thông báo đó được.

## 3. Định danh tiền

Liệu, khi bạn A gửi "Tôi, tên là A, gửi cho B 1 đồng" cho B, sau đó B gửi liên tục thông báo này đi, thì A có bị mất tiền nhiều lần không? Điều này được giải quyết một cách rất đơn giản, mỗi đồng tiền sẽ có một cái tên nhất định: "Tôi, tên là A, gửi cho B 1 đồng với mã số 007". Tất nhiên B có gửi thông báo này đi bao nhiêu lần thì cũng chỉ có đồng 007 này được gửi cho B mà thôi.

Tuy nhiên, làm thế nào để B biết được đồng tiền 007 này là thật? A hoàn toàn có thể gửi cho B một đồng tiền giả, thông thường B có ngân hàng để hỏi, trong trường hợp này thì B phải làm thế nào?

Bitcoin sử dụng một hệ thống cây giao dịch gọi là "block chain", nhìn một cách đơn giản thì việc kiểm tra này được thực hiện bằng cách: B sẽ tra ngược về đầu, giả sử có một bạn X gửi cho A đồng 006, sau đó A đổi là 007 gửi cho B, thì B sẽ kiểm tra xem đồng 006 có phải là thật không. Tương tự thế B check đồng 005 là bố của 006, rồi 004 là bố 005, đến khi nào B tra được đến gốc của nó để đảm bảo nó là thật.

Tuy nhiên điều này không ngăn được việc A từ đồng 006 đặt ra 2 mã số cho đồng tiền đó là 007 và 008, sau đó đồng thời gửi cho B và C. Cả B và C sẽ cùng kiểm tra block chain, và đều tìm về đồng 005 rồi 004 và tưởng nó là thật. Khái niệm này gọi là "double-spending".

Để chắc chắn rằng A không "double-spending", hệ thống cần đảm bảo được cả B và C khi tra định danh đồng tiền đó đều tra từ một điểm chung duy nhất. Việc đảm bảo này được thực hiện thông qua cơ chế "proof-of-work"

## 4. Proof of work

Proof of work, nôm na gọi là "bằng chứng về công việc", là một mô hình được đặt ra nhằm đảm bảo rằng, để A sinh ra được mã 007 và 008, A hoặc một người nào đó phải bỏ một công sức nhất định mà rất khó làm được 2 lần (ví dụ bắt A chạy 100km rồi mới cho A lấy tiền). Tất nhiên chuyện A chạy 100km là khó, hoặc A cũng có thể chạy 200km rồi sinh ra 2 mã kiếm tiền, đó cũng là một vấn đề mà ta sẽ nhắc đến sau.

Quá trình này gọi là quá trình kiểm định, qua đó sẽ có một số người kiểm định nhất định (bao gồm cả A và B), những người này sẽ thực hiện việc chạy 100km này, sau đó kí vào và xác nhận việc X gửi cho A, hay A gửi cho B là tin được tại thời điểm đó.

Như vậy, khi ta thấy một đồng tiền, ta đảm bảo rằng bố của nó đã phải bỏ một số công sức có được nó, một cách nào đó ta tin rằng việc chuyển từ 006-&gt;007 là **tạm tin được.**

Quay lại ví dụ trên, giả sử ông sở hữu đồng 006 là ông X có đồng 005 chạy 200km và in ra 2 đồng là 006 gửi cho A và 006' gửi cho Y. Sau đó có 10 người kí rằng tin là X chuyển sang A là hợp lệ, và 3 người kí vào là X gửi cho Y là hợp lệ. Như vậy đồng tiền này đã được **tạm tin**&nbsp;bởi X và A, và như thế việc gửi cho A là **tạm hợp lệ**, còn tiền sở hữu của Y là không hợp lệ. Như vậy Y hay B có thể đơn giản tránh việc bị lừa bằng cách đợi một số người kí vào giao dịch đó rồi mới tin là mình được nhận tiền. &nbsp;Người kiểm định thường ghép nhiều giao dịch vào kiểm định một thể, và sau khi hoàn thành việc kiểm định, người đó được thưởng một số tiền nhất định cùng với phí giao dịch.

Bitcoin sử dụng cơ chế crypto, nhìn một cách đơn giản, thì Bitcoin yêu cầu người muốn kiểm chứng giao dịch phải tìm một dãy sha256: sha256(data+number), trong đó data là thông tin giao dịch: "Tôi, tên là A, gửi cho B 1 đồng với mã số 007" và number là một số bất kì, sao cho sha256(data+number) phải bắt đầu bằng một số lượng số 0 nhất định. Việc này là rất mất công và yêu cầu người kiểm định phải tìm ra số đó, sau khi tìm ra một dãy số thỏa mãn, gọi là một **block**, thì người này được hưởng 50 đồng Bitcoin (giảm xuống theo thời gian, hiện nay là 25 đồng).

Như vậy, đồng thời với việc kiểm tra giao dịch, ta cũng giải quyết được vấn đề **in và cấp tiền**

## 5. Bitcoin

Như vậy, Bitcoin được tổ chức như thế nào?

Mỗi người sẽ có một cái ví điện tử, gọi là **wallet**, &nbsp;mỗi ví này sẽ có nhiều địa chỉ (như một ngăn ví), mỗi địa chỉ này chứa 1 số tiền nhất định, gọi là **address.**

Việc giao dịch tiền từ một address của người này sang address người khác gọi là một **transaction.**

Các giao dịch này được ghép lại và kiểm định, kết hợp với số tìm được thỏa mãn điều kiện, thành một **block.**

Những người kiểm định, tìm block với mục tiêu được thưởng tiền, gọi là **miners.**

Những block được kết nối với nhau thành một cây, block sau chứa dãy serial của block trước, cây này gọi là **block chain.**

## 6. Kết luận

Tuy đã giải quyết được phần lớn vấn đề của một đồng tiền điện tử. Bitcoin vẫn có rất nhiều vấn đề còn tồn tại:

*   Nếu như A mua toàn bộ người kiểm định (thực ra chỉ cần mua trên 50%), thì A hoàn toàn có thể sinh ra 2 đồng tiền từ một đồng với số chữ kí là như nhau, người nhận sau đó là B không thể kiểm tra được đó có phải là tiền hợp lệ hay không.
*   Trong quá trình hình thành đồng tiền, sẽ có rất ít người thực hiện việc kiểm định, vì vậy hệ thống cần đặt khả năng tìm được **block** là dễ dàng, việc này rất dễ gây ra chuyện tìm ra 2 block một lúc và việc chọn 1 trong 2 block sẽ gây ra block còn lại cùng với những người công nhận nó bị mất công sức (đi kèm với việc sẽ không kiểm chứng được giao dịch trong thời gian ngắn)
*   Địa chì là public, toàn bộ giao dịch cũng được theo dõi một cách thỏa mái. Vì vậy việc bị theo dõi đánh cắp là hoàn toàn có thể. Wallet lại chỉ là một dãy số, rất nguy hiểm khi bị đánh mất.
*   Toàn bộ giao dịch đều thông qua Network, rất dễ bị tấn công bởi DDOS, hoặc bị lọc các gói tin khiến thông tin về block bị thiếu sót, dễ bị tấn công double-spending
*   Việc tạo ra tiền: block là do các máy ngang hàng thực hiện, vì vậy hacker có thể dễ dàng thay đổi các thông số khiến cho block tạo ra bởi các máy này là không sử dụng được (ví dụ thay đổi gói tin ntp khiến các node nhận sai giờ, khiến cho block được tạo ra với thời gian chậm hơn thời gian thật)
*   Blockchain chứa thông tin của toàn bộ các block liên kết với nhau, vì vậy client cần phải lưu trữ với dung lượng lơn (đối với Bitcoin hiện nay là khoảng 15GB).

Mong rằng bài viết sau sẽ làm rõ hơn về giao thức của Bitcoin về mặt technical một cách nâng cao hơn.
