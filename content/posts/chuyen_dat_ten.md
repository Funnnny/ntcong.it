+++
date = "2013-01-21T16:18:00+07:00"
title = "Phiếm bàn chuyện đặt tên"
author = "Cong Nguyen"
Tags = ["development", "thought"]
Categories = ["development", "thought"]

+++

Code mình viết ra cũng như con đẻ của mình vậy, chuyện đặt cho nó cái tên để gọi là chuyện vô cùng quan trọng. Đặt tên sao để cho tên nó hay, có ý nghĩa, lại còn phải để người khác dễ gọi, chưa kể là còn phải tránh tên dễ suy luận không là bạn bè của con nó chửi xéo.

Chắc 100% người đụng vào code đều biết mấy kiểu đặt tên họ:

*   viết_thường_kèm_gạch_dưới
*   ViếtHoaChữCáiĐầu
*   VIẾT_HOA_KÈM_GẠCH
*   một vài loại biến thể khác

Ngoài ra một số quy tắc khác như tên biến int thì bắt đầu bằng chữ i, class bắt đầu bằng cls, rồi thì là const thì viết hoa tất, quy tắc thì có lẽ chỉ dành cho project manager ngồi bịa ra viết tài liệu rồi cho các bạn đọc, đọc xong rồi nhớ áp dụng là xong.

Đã phiếm bàn thì bàn cái gì cho nó hay tí, gọi là **"đặt tên một cách thông minh".**

Đầu tiên có lẽ cần trích đến nguyên tắc đặt tên của Golang: short, concise, evocative; tạm dịch là ngắn gọn, súc tích và tạo ấn tượng.

Ngắn gọn nhưng súc tích là một điểm đến rất khó khi đặt tên, một vài ví dụ như:

**1.** **setup_and_run(new_work, job)** : ngắn gọn, đầy đủ, rõ nghĩa

do_setup_and_run: ai chả biết là làm ?

setup_new_and_run: đã setup...(new_work) rồi lại còn có new ở tên.

**2.** **io.NewIO(),** **Device.NewDevice():** trong package A có hàm NewA là một lỗi khá phổ biến, cách đặt thừa tên này dẫn đến việc đọc code rất là căng thẳng.

Tránh điều này rất đơn giản, ta có thể dùng io.New() nếu cần làm 1 số việc và trả về object IO, hoặc sử dụng trực tiếp io.IO()

**3.** **god.Do(action)** **và** **_god.DoOrWaitTilDone(action):** cách gọi thứ 2 tuy giải thích được việc hàm này sẽ thực hiện hoặc chờ nếu action đã đang thực hiện, tuy nhiên việc này nên đặt trong comment thì hơn là đặt vào tên. Nên nhớ nói dài là nói dại.

**4. from what import what_exception:** hay thường được biết đến với dòng from what.what_exception import WhatNotFound. Tại sao lại là what.what_exception ? Bỏ cái đoạn what_ có làm nó khó hiểu hơn ko ? Có lẽ tự ai cũng hiểu: from what.exception import WhatNotFound

**5. from what.exception import WhatNotFound_Exception_**: tại sao ??? WhatNotFound tốt, WhatError tốt, WhatYeahException tốt, NotFoundException vừa trùng lặp lại khó nhớ.

__6. User.username, User.user_email:__ khỏi nói chắc ai cũng biết là thừa, User.name, User.email là 2 cái tên ngắn gọn và tốt hơn rất nhiều

__7. from devices.device import Device:__ move Device import qua __init__ là cách tránh tình trạng này rất dễ dàng, nên để nó là from devices import Device

Vậy, tránh việc đặt tên xấu như thế nào?

Thực ra, tên xấu ngay bản thân nó đã thể hiện được cách tránh, đã là cái tên thì phải đọc, đọc lên thấy...xấu thì phải suy nghĩ ngay:

__1. Tên quá dài:__ đặt 1 cái tên xong thấy nó quá dài, hãy đọc lại thử 1 lần nữa xem nó có vấn đề gì không. Đừng quá ham nhét nhiều thông tin vào tên biến, tên hàm. Súc tích không có nghĩa là đầy đủ thông tin. Dành thời gian thừa khi nghĩ tên vào việc viết document hoặc comment cho rõ ràng.

__2. Tên lặp lại quá nhiều:__ khi gọi vào biến đó hoặc hàm đó, bạn thấy rằng có 1 từ đang lặp lại 2 lần hay thậm chí 3 lần, dừng lại và nghĩ xem tại sao nó lặp lại, và việc lặp lại đó có ý nghĩa gì không. Điều này đúng với cả từ trùng nghĩa lặp lại như NotFoundException.

__3. Tên đọc thấy...trẹo lưỡi:__ tên xấu, ví dụ như earth.create_a_new_god(), chưa đọc đã thấy nó trẹo cả lưỡi rồi, earth.new_god() có lẽ sẽ tốt hơn rất nhiều, chỉ cần chú ý một chút để sửa.

Cuối cùng, code mình code ra, tên mình đặt, con mình mình nuôi, thế nên chú ý cho nó cái tên hay một chút, sau này có chuyển cho ai nuôi thì người ta cũng thích. Tất nhiên cũng đừng có nghĩ nhiều quá, đôi khi cũng cần một _"ngoại lệ"_ nho nhỏ, bởi vì dù sao, mình đặt tên ra thì mình là người gọi nhiều nhất, đặt theo ý người khác cũng chả vui gì.

_*Bài viết bậy viết bạ đánh dấu 2 năm không blog*&nbsp;_
