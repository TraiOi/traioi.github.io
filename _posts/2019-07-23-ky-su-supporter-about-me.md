---
layout: post
title:  "Ký sự Supporter: About Me"
date:   2019-07-23 11:12:08 +0700
categories: cty1
description: Giới thiệu sơ về bản thân và 1 số điều tự kỉ sắp tới
---
<link rel="stylesheet" href="{{ "/assets/posts.css" | relative_url }}">

### 1. Giới thiệu bản thân

Tại thời điểm hiện tại viết bài này thì tôi đang là 1 Supporter cho 1 công ty nhỏ chuyên về Proxy chống DDoS. Mặc dù công việc của tôi là hỗ trợ khách hàng khi nhưng lượng kiến thức mà tôi thu nhận được rất nhiều.  

Tính tôi thì không thích bị nghe chữi cho lắm, nên khi làm cũng đôi khi thấy khó chịu nhưng môi trường làm việc giữa mọi người trong team và công ty cũng khá nhẹ nhàng, thân thiện nên tôi cũng cảm thấy đỡ được phần nào, ngoài ra còn được tiếp thu thêm 1 lượng lớn kiến thức khi debug 1 số vấn đề mà khách hàng cố gắng làm khó tôi ấy. Debug và giải quyết xong 1 vấn đề giống như cách bạn hoàn thành 1 project code vậy, cảm giác thấy khá thoải mái.  

### 2. Tự kỉ

Mục đích ban đầu của tôi khi viết về series này là để lưu lại những gì tôi đã học được khi còn làm Supporter ở đây như 1 quyển nhật ký.  

Ngoài ra, tôi cũng muốn chia sẻ cho mọi người kinh nghiệm và kiến thức mà tôi đã từng trải qua và tiếp thu được. Cũng như một số lời khuyên và bài học cho các bạn mới ra trường hay thực tập, các bạn không có định hướng sau khi ra trường.  

Ở thời điểm viết bài này thì tôi cũng đã làm được 2 năm ở đây rồi, năm 4 ĐH đã bắt đầu đi làm và cũng như các bạn, mù mờ định hướng cho tương lai. Ở công ty tôi được coi là thằng bị ở lại thực tập lâu nhất, tận 6 tháng, trong khi mấy đồng nghiệp vào cùng thì chỉ 2, 3 tháng là lên chính thức luôn rồi.  

Tôi lúc nộp CV lúc nào cũng tự ti vào kiến thức của mình nên chỉ dám nộp vào công ty nhỏ thôi (mà thật ra công ty do thằng bạn giới thiệu :))). Lúc đó cũng không hy vọng nhiều vì nghe nói công ty chuyên về Linux nên chỉ muốn vào để tiếp thu thêm kiến thức về Linux và nhảy việc. Nhưng mà ở đây có môi trường, và lượng kiến thức tôi nhận vào khá nhiều nên cũng thấy thích thú trong khoảng thời gian dài.  

Ngoài ra, quyền lợi khi được làm ở công ty nhỏ là tôi được thể hiện mình, bất kỳ ai cũng có thể được nêu ý tưởng của mình và dễ dàng được để ý nếu bạn đóng góp gì đó có ích, dù chỉ là nhỏ nhoi. Sếp cũng vui tính nữa :D.  

Vì vậy, tôi hiện đang là 1 Supporter nhưng vô tình mang trong mình 1 lượng kiến thức của 1 System Admin và cũng có thể là SysOps, DevOps, IT Security, .. vì bên này hỗ trợ khách hàng thì bị sai làm culi đủ thứ tạp nham, hehe.

### 3. Mục lục

Đây là danh sách các bài viết trong series mà tôi lưu lại trong quá trình học hỏi ở đây.  
<p class="index">
{% for post in site.categories['cty1'] reversed %}
  <a href="{{ site.baseurl }}{{ post.url }}">{{ forloop.index0 }}. {{post.title}}</a> <br />
{% endfor %}
</p>
