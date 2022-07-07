# Các phiên bản Node.js được hỗ trợ

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/support-statement.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/support-statement.md)

AVA hỗ trợ bản release mới nhất của bất kỳ phiên bản chính nào [được hỗ trợ bởi chính Node.js](https://github.com/nodejs/Release#release-schedule).

*Hỗ trợ* ở đây có nghĩa là chúng tôi chạy toàn bộ bộ test của chúng tôi theo các phiên bản Node.js đã cho và sẽ chấp nhận các pull request để sửa bất kỳ lỗi nào (miễn là chúng không phải là các lỗi đã biết của Node.js và sẽ được sửa ngay lập tức). Do đó, *bỏ hỗ trợ* nghĩa là chúng tôi sẽ loại bỏ các phiên bản Node.js khỏi ma trận test của chúng tôi và sẽ không còn chấp nhận các pull request cụ thể để sửa các lỗi trong các phiên bản đó nữa.

Khi chúng tôi bỏ hỗ trợ cho phiên bản chính thức LTS-covered, chúng tôi sẽ tăng phiên bản chính của AVA (hoặc, khi chúng tôi đang ở giai đoạn `0`, phiên bản phụ).

Chúng tôi sẽ bỏ hỗ trợ cho các phiên bản Node.js được đánh số lẻ (ví dụ: `7` hoặc `9`) *mà không* tăng phiên bản chính của AVA.

Chúng tôi cố gắng tránh việc *vô tình* bỏ hỗ trợ cho các bản release không phải là mới nhất của Node.js. Nếu sự cố như vậy xảy ra, chúng tôi sẽ chấp nhận các pull request để khôi phục chức năng. Chúng tôi có thể quyết định bỏ qua bản release AVA lỗi và thay vào đó tăng phiên bản chính của AVA.

Chúng tôi có thể ngưng hỗ trợ một cách rõ ràng cho các bản release Node.js mới nhất. Nếu điều này xảy ra, chúng tôi sẽ tăng phiên bản chính của AVA. Điều này có thể là do việc chấp nhận các backported API hoặc tính khả dụng của các bản release V8 trong các phiên bản Node.js sau này, hoặc trong chính bản thân AVA hay là một trong số các dependencies của chúng tôi.

Chúng tôi có thể ngưng hỗ trợ cho một phiên bản nào đó của Node.js, trong một bản release của phiên bản release trước để tăng phiên bản, nếu phiên bản AVA mới được dự kiến là sẽ ổn định trong hoặc sau ngày hết hạn của phiên bản Node.js.
