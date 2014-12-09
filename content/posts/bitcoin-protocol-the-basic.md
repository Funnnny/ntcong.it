+++
date = "2014-01-19T12:51:42+07:00"
title = "Bitcoin Protocol: The Basic"
author = "Cong Nguyen"
authorlink = "http://ntcong.it"
authorfacebook = "http://facebook.com/ntcong.it"
authorgoogleplus = "https://plus.google.com/+CongNguyen"
Categories = ["thought", "bitcoin"]
Tags = ["thought", "bitcoin"]
thumbnail = "img/bitcoin.png"

+++

Ở bài trước, các bạn đã biết thế nào là cryptocurrency nói chung, và cách hoạt động của nó. Tuy nhiên bài viết chỉ rờ đến một trong những điểm nổi nhất của hệ thống Bitcoin, nếu muốn tìm hiểu một cách chuyên sâu hơn 1 chút, thì Bitcoin hoạt động như thế nào?

Bài viết nhằm phân tích ý nghĩa và phương thích hoạt động của loại tiền mặt này. Trước tiên, nhắc lại một chút về Cryptocurrency

## 1. Cryptocurrency/Bitcoin?

Cryptocurrency được chỉ một loại tiền mặt thỏa mãn các điều kiện

* Peer-to-peer &amp; Decentralized: không phụ thuộc vào bất cứ một cá nhân, máy móc nào làm nhiệm vụ quản lí.
* Digital: tồn tại ở dạng số, không có một thể hiện cụ thể về mặt vật lí.

Bởi vì đồng tiền là không phụ thuộc vào bất cứ ai quản lí, vì vậy việc xác nhận/kiểm tra một giao dịch cần được thực hiện bởi các thành viên trong hệ thống (P2P). Quá trình này sử dụng các thuật toán mã hóa, do đó đồng tiền được gọi là cryptocurrency.

Bitcoin là đồng tiền đầu tiên áp dụng cryptocurrency.

## 2. Bitcoin hoạt động thế nào?

Bitcoin, cũng như cryptocurrency nói chung, sử dụng các thuật toán mã hóa để xác nhận giao dịch, trong quá trình xác nhận giao dịch này, người thực thi thành công việc xác nhận một nhóm giao dịch, được thưởng một số lượng tiền tương ứng, với Bitcoin(BTC) là 25 đồng. Số lượng tiền này được giảm đi 1 nửa sau mỗi 4 năm, nhằm mục đích hạn chế số lượng BTC tối đa là 21 triệu.

Giống như RFC của một protocol, Bitcoin được định nghĩa bởi các thành phần riêng lẻ, và các giao thức để kết nối các thành phần lại với nhau, vì vậy ta sẽ tìm hiểu Bitcoin bằng cách phân tích các thành phần này:

### a. Address

Address là một dãy số, dài từ&nbsp;27-34 kí tự, bắt đầu bằng 1 hoặc 3, ví dụ:&nbsp;**13q1kfWwR56AwTxeJoJV8bgRnQ2WYxWiY5**

**Address**&nbsp;được sinh ra rất đơn giản, enduser tạo ra 1 cặp[ private-public key](http://en.wikipedia.org/wiki/Public-key_cryptography), sau đó [hash public key này bằng một thuật toán có sẵn](https://en.bitcoin.it/wiki/Technical_background_of_Bitcoin_addresses) tạo ra Address.

### b. Transaction

Một transaction chứa các thông tin sau:

*   hash: hash của toàn bộ thông tin transaction này, được sử dụng như là đại diện của transaction
*   version number, number of inputs, number of outputs: tự cái tên nó đã giải thích
*   lock_time: thời gian mà transaction này thực hiện, thường là 0 tức là ngay lập tức
*   size
*   list of inputs and outputs, trong đó mỗi input và output có định dạng riêng biệt
*   inputs:
  *   previous tx: hash của transaction trước đó
  *   index: vị trí của transaction
  *   scriptSig: &lt;sig&gt; &lt;pubKey&gt;: public key, và signature được sign bởi private key và mã hash của transaction trước đó.&nbsp;
*   outputs:
  * value: số tiền tính bằng satoshi
  * scriptPubKey: đoạn script được dùng để redeem số tiền này

Vậy, để thực hiện 1 transaction, giả sử từ Alice đến Bob, thì 2 bên này phải làm những gì ?



1.  Alice có được address của Bob để gửi tới, đây chính là hash public key của Bob, Alice sinh ra transaction với các thông tin như trên. Trong đó, quan trọng nhất là đoạn scriptCheckSig kia, Alice đặt vào đó hash public key của Bob. Alice là người duy nhất cầm private key của mình, vì vậy &lt;sig&gt; và &lt;pubKey&gt; kia chỉ có Alice mới tạo ra được.
2.  Alice gửi gói tin này cho toàn mạng, toàn mạng biết được và verify Alice đã gửi cho Bob.
3.  Bob phải claim số tiền này tại thời điểm Bob muốn tiêu số tiền này, chính là hình thức Bob tạo ra 1 transaction mới. Tại thời điểm đó, Bob phải chứng minh rằng, Bob chính là Bob trong transaction Alice đã gửi.
4.  Tương tự Alice, Bob tạo transaction mới, đặt vào đó &lt;sig&gt; và &lt;pubKey&gt; của mình, và các miner sẽ verify cho Bob như sau:

<span id="docs-internal-guid-63637fce-a624-69aa-ca81-923515518b87"><span style="color: #434343; font-family: Arial; font-size: 15px; vertical-align: baseline; white-space: pre-wrap;"></span></span>
<div dir="ltr">
<table style="border-collapse: collapse; border: none;"><colgroup><col width="143"></col><col width="314"></col><col width="216"></col></colgroup><tbody>
<tr style="height: 0px;"><td style="background-color: #f2f2f2; border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt; text-align: center;">
<span style="background-color: #f2f2f2; color: #434343; font-family: Arial; font-size: 15px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">Stack</span>
</td><td style="background-color: #f2f2f2; border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt; text-align: center;">
<span style="background-color: #f2f2f2; color: #434343; font-family: Arial; font-size: 15px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">Script</span>
</td><td style="background-color: #f2f2f2; border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt; text-align: center;">
<span style="background-color: #f2f2f2; color: #434343; font-family: Arial; font-size: 15px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">Description</span>
</td></tr>
<tr style="height: 0px;"><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">Empty.</span>
</td><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">&lt;sig&gt; &lt;pubKey&gt; OP_DUP OP_HASH160 &lt;pubKeyHash&gt; OP_EQUALVERIFY OP_CHECKSIG</span>
</td><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">scriptSig và scriptPubKey được ghép lại</span>
</td></tr>
<tr style="height: 0px;"><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">&lt;sig&gt; &lt;pubKey&gt;</span>
</td><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">OP_DUP OP_HASH160 &lt;pubKeyHash&gt; OP_EQUALVERIFY OP_CHECKSIG</span>
</td><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">đẩy 2 biến const vào stack</span>
</td></tr>
<tr style="height: 0px;"><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">&lt;sig&gt; &lt;pubKey&gt; &lt;pubKey&gt;</span>
</td><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">OP_HASH160 &lt;pubKeyHash&gt; OP_EQUALVERIFY OP_CHECKSIG</span>
</td><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">OP_DUP: duplicate top stack</span>
</td></tr>
<tr style="height: 0px;"><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">&lt;sig&gt; &lt;pubKey&gt; &lt;pubHashA&gt;</span>
</td><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">&lt;pubKeyHash&gt; OP_EQUALVERIFY OP_CHECKSIG</span>
</td><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">OP_HASH160: hash top stack</span>
</td></tr>
<tr style="height: 0px;"><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">&lt;sig&gt; &lt;pubKey&gt; &lt;pubHashA&gt; &lt;pubKeyHash&gt;</span>
</td><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">OP_EQUALVERIFY OP_CHECKSIG</span>
</td><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1.15; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">đẩy biển const vào stack</span>
</td></tr>
<tr style="height: 0px;"><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">&lt;sig&gt; &lt;pubKey&gt;</span>
</td><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">OP_CHECKSIG</span>
</td><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">OP_EQUALVERIFY: check bằng nhau giữa &lt;pubHashA&gt; và &lt;pubKeyHash&gt;, chính là check address</span>
</td></tr>
<tr style="height: 0px;"><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">true</span>
</td><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">Empty.</span>
</td><td style="border: 1px solid #aaaaaa; padding: 3px 3px 3px 3px; vertical-align: top;"><div dir="ltr" style="line-height: 1; margin-bottom: 0pt; margin-top: 0pt;">
<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">OP_CHECKSIG: check &lt;sig&gt; bằng public_key</span>

<span style="background-color: transparent; color: #434343; font-family: Arial; font-size: 11px; font-style: normal; font-variant: normal; font-weight: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;"></span></td></tr>
</tbody></table>

Như vậy, chỉ có Alice mới tạo được gói tin tiêu số tiền trong address của mình, và cũng chỉ có Bob mới tạo được gói tin mới tiêu số tiền mà Bob nhận được.

### c. Blocks

Block là một tập hợp các transaction, thỏa mãn 1 số điều kiện nhất định. Thông tin trong 1 block gồm có:

*   magic no: luôn là&nbsp;0xD9B4BEF9
*   blocksize
*   header
  *   version
  *   hash previous block
  *   hash merkle block: hash của tất cả các transaction
  *   time: thời gian
  *   target: target hiện tại
  *   nonce: 1 số nguyên
*   các transactions

Một block thế nào là hợp lệ? Block là hợp lệ khi hash của version + hashPrevBlock + hashMerkleBlock + time + target + nonce nhỏ hơn target. Điều này trái với nhiều bài viết thường nói là các peer sẽ verify block, thực chất ra bất cứ ai cũng có thể verify block đó, và 1 block khi được announce cho toàn mạng thì nó đã hợp lệ.

Để nói về chuyện tìm block, thì cần phải nói đến các miners và mining

### d. Mining


Mining là quá trình miner đi tìm block, với 1 lần tìm thấy block, miner sẽ được hưởng 1 số tiền nhất định (hiện tại là 25BTC), và được nhận bởi transaction đầu tiên trong block: transaction không có người gửi và người nhận là miner. Transaction đầu tiên này gọi là [coinbase](https://en.bitcoin.it/wiki/Coinbase).

Vậy miner tìm ra 1 block như thế nào? Trong 1 block thì các thông tin&nbsp;version + hashPrevBlock + hashMerkleBlock + time + target&nbsp;là cố định (tạm gọi là hashBase) và giá trị nonce là thay đổi. Miner sẽ hash SHA256(SHA256(hashBase+0)), ra 1 giá trị, so sánh giá trị này với target, nếu nhỏ hơn thì dừng lại, thông báo block cho toàn mạng, nếu không, tăng nonce lên 1, tiếp tục hash&nbsp;SHA256(SHA256(hashBase+0))cho đến khi tìm ra block.




Để biết được độ khó của việc này, ta thử một phép tính như sau: giả sử hashBase là "hello" và ta chỉ phải hash 1 lần


*   sha256("hello0")=5a936ee19a0cf3c70d8cb0006111b7a52f45ec01703e0af8cdc8c6d81ac5850c

để có 2 số 0 ở đầu, ta phải duyệt nonce đến 227

*   sha256("hello227")=001b92541ed0a22b0cb89018b561d895503206c0082c0ecf2d0b7e5182191eed


để có 4 số 0 ở đầu, ta phải duyệt nonce đến 60067


*   sha256("hello60067")=0000e49eab06aa7a6b3aef7708991b91a7e01451fd67f520b832b89b18f4e7de
để có 6 số 0 ở đầu, ta phải duyệt đến 33290382


*   sha256("hello33290382")=000000865194de5c0744d72183dcce2eca2b69264b749a1798f1b6996baef7de

Số nonce này tăng lên rất nhanh, hiện tại để nhỏ hơn target ta cần ít nhất 15 chữ số 0 ở đầu


Vậy chốt lại, mining đơn giản là quá trình tính hash sha256, mỗi lần với 1 số nonce khác nhau, cho đến khi tìm ra dãy số hợp lệ. Quá trình tìm nonce này không thể tính toán bằng phương pháp số, mà bắt buộc phải tính sha256 từng lần một, vì vậy nó là proof-of-work.




Các miners thường tập hợp lại thành 1 Pool, trong đó pool sẽ phân chia các miner tính, ví dụ miner1 tính nonce từ 0-&gt;1 tỉ, miner2 tình từ 1 tỉ-&gt;2 tỉ, mỗi block từ 0-&gt;1 tỉ gọi là 1 share, khi tìm ra block, các miner sẽ được chia đều dựa trên số share mình submit được (càng nhiều share càng được chia nhiều). Thực tế, share được hình thành phức tạp hơn, dựa trên nguyên tắc sinh block nhưng với target thấp hơn, tuy nhiên về bản chất nó cũng chính là hình thức phân chia việc tính nonce này.

## 3. FAQ


Thực chất mạng lưới Bitcoin về mặt concept hoạt động rất đơn giản, đó là lí do tại sao các Altcoin ra đời liên tục. Tuy nhiên để hiểu được nó lại cần một số kiến thức về tin học nhất định, đó là lí do tại sao nhiều người còn chưa biết nó là gì. Qua đó nảy sinh 1 số câu hỏi:

**a. Bây giờ cắm máy Mining thì có lãi ko?**

- &nbsp;Mining đã từ lâu không còn là cuộc chơi của CPU/GPU nữa mà là của các máy chuyên dụng gọi là ASIC. Cắm máy mine Bitcoin thì chắc chắn ko bao giờ có lãi: giả sử bạn có 1 card ATI HD 7850, với độ khó hiện nay thì cần 850 năm để tìm ra 1 block, nếu join vào pool thì mỗi ngày bạn có&nbsp;0.0001 BTC là khoảng 0.06USD khoảng hơn 1000 không đủ để trả tiền điện.

**b. Vậy mua ASIC thì sao?**

- &nbsp;Câu trả lời vẫn là không, nếu ASIC là có lãi thì những người sản xuất ASIC họ bán ra làm gì? Sao họ không giữ máy lại và chạy? Đa phần các miner đã đầu tư ASIC từ lâu, và họ chạy ASIC ngoài nhiệm vụ mining kiếm tiền, họ còn góp 1 phần vào mạng lưới mining, để hệ thống Bitcoin ổn định lâu dài với khoản đầu tư từ lâu của họ.

**c. Lấy Bitcoin từ đâu?**

- Giống như vàng, có 2 cách để có vàng là đi đào và đi mua (hoặc là đi xin, coi như là mua với giá 0 đồng)

**d. Vậy sau khi đào hết Bitcoin, có ai đào nữa ko?**

- &nbsp;Khi tìm ra 1 block, ngoài tiền thưởng thì người đào còn được hưởng số tiền phí giao dịch (hiện nay phí giao dịch thường là 0.001 BTC mỗi giao dịch), và miner hoàn toàn có thể loại bỏ các transaction không kèm phí trong đó. Vì vậy khi khối lượng giao dịch nhiều lên thì phí này sẽ tăng lên tương ứng.

**e. Quá trình tính toán khi tìm ra Block có hữu ích gì ko?**

- Không giống như Folding@Home, quá trình tính toán block không có hữu ích gì cho xã hội, nó nhằm nhiệm vụ bảo vệ mạng lưới Bitcoin, vì vậy nó chỉ hữu ích cho Bitcoin mà thôi.

**f. Chấp nhận Bitcoin làm thế nào?**

- Một số third-party có cung cấp dịch vụ payment, ví dụ như Coinbase. Bạn chỉ cần làm theo hướng dẫn của Coinbase, Coinbase sẽ thực hiện việc thanh toán và trả về cho bạn tiền mặt (USD, EUR) mà không phải đụng đến Bitcoin.

*Kết thúc khoảng 3 tuần đọc và viết lung tung về Bitcoin*



