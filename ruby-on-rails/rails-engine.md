---
description: 'Rails::Engine'
---

# 🐤 การใช้งาน Rails::Engine เบื้องต้น

[อ้างอิง Getting Started with Engines](https://guides.rubyonrails.org/engines.html)

ในบทความนี้ เป็นวิธีใช้ engines และ ฟังก์ชันเพิ่มเติม ให้กับ แอปพลิเคชันโฮสต์ ผ่านอินเทอร์เฟซที่สะอาดตา ยิ่งขึ้น ไม่มากก็น้อย

## engines คือ อะไร <a id="d9d6"></a>

ในที่นี้ เราจะหมายถึง [Rails::Engine](https://edgeapi.rubyonrails.org/classes/Rails/Engine.html) โดยปกติ เว็ป หรือ host applications ของ Rails ก็จะ สืบทอด หรือ inheriting มาจาก Rails::Application และ Rails::Application ก็ สืบทอด มาจาก [Rails::Engine](https://edgeapi.rubyonrails.org/classes/Rails/Engine.html)

## มาเริ่มวิธีใช้กัน <a id="2526"></a>

เริ่มเเรกสร้าง project มาเลยครับ

```text
$ rails new unicorn
```

![](https://miro.medium.com/max/2266/1*Dtf0Xs3oJy1FqrPfNlXlEg.png)

ต่อมา มาทำการสร้าง [engine](https://guides.rubyonrails.org/engines.html#generating-an-engine) กันครับ โดย ใช้ command

```text
$ rails plugin new blorgh --mountable
```

![](https://miro.medium.com/max/2684/1*iL04Oj_5LD1Otwngt_4EUg.png)

โดยจะมี options`--mountable` และ`--full`

โดย `--mountable`จะต่างกันที่

* สร้าง files \(`application.js` and `application.css`\) ให้
* A namespaced `ApplicationController` stub
* A namespaced `ApplicationHelper` stub
* A layout view template for the engine
* Namespace isolation to `config/routes.rb`:

เมื่อสร้างเสร็จ ใน Gemfile ของเราก็ทำการเพิ่ม gem มาให้ด้วย

![](https://miro.medium.com/max/1788/1*068OJkT4uJiszEBQVfOR9g.png)

ต่อมาเรามาลองสร้าง Model Article ใน blorgh ที่เป็น engine ด้วยคำต่อไปนี้นะครับ โดยต้องทำการเข้าไปใน engine เราก่อน

```text
$ cd blorgh
$ rails generate scaffold article title:string text:text
```

![](https://miro.medium.com/max/1134/1*vGOI59-PYpLHnpKWtSPrcQ.png)

เมื่อสร้างเสร็จ ก็เข้าไป กำหนด root path กันหน่อยใน unicorn/blorgh/config/routes.rb

```text
root to: "articles#index"
```

![](https://miro.medium.com/max/1828/1*QS4T_OGEiPnvv2Se1gQAMg.png)

เเค่นี้ตัว engine เราก็ใช้งานได้เเล้วครับ

นั้นมาต่อกันเลยในเมื่อ engine เราเส็จเเล้ว ลองนำเอามาใช้ใน project unicorn ของเรากันดู โดยเข้าไป add route ใน unicorn/config/routes.rb ด้วย

```text
mount Blorgh::Engine, at: "/blog"
```

![](https://miro.medium.com/max/1828/1*R_mknqihLv933WHDz8P42g.png)

เสร็จเเล้วเราก็จะมา install migarte กัน โดยใช้ command ดังต่อไปนี้ ที่ unicorn path

```text
$ rails railties:install:migrations
```

ถ้าเรามีหลาย engine หรือ lig ก็จะทำทั้งหมดในคราวเดียว

![](https://miro.medium.com/max/1482/1*E_Vc3NymyekjdGOiygJ4-w.png)

จากนั้นก็ สั้ง migrate ได้เลยครับ

```text
$ rails db:migrate
```

![](https://miro.medium.com/max/1524/1*oPkgbE_9uIYyyGPseUTKrw.png)

จากนั้น ก็ ลองมาดูผลลัพธ์ กัน สั่ง run ตัว unicorn ขึ้นมา

```text
$ rails s
```

![](https://miro.medium.com/max/1100/1*18TfyZTwh7Cr2Aa_UQtn7g.png)

เเล้วเข้าไป path ที่เรา mount Blorgh::Engine มาคือ [http://localhost:3000/blog](http://localhost:3000/blog)

![](https://miro.medium.com/max/1100/1*NZBwsJwFN8VSfn9Gn5lHVQ.png)

**เย้!! ใช้งานได้ เท่านี้ ถ้า project ใหน อยากใช้ articles ก็ mount Blorgh::Engine ได้เลย**

#### มีเพิ่มเติ่มอีกนิต ถ้าหา เราต้องการดัดแปลง Model ใน engine ก็มีด้วยกัน 2 วิธี ซึ่งอยู่ในเรื่อง [Improving engine functionality](https://guides.rubyonrails.org/engines.html#improving-engine-functionality)

ลองมาทำกันดู กันสักนิตเริ่มที่ไป config engine ให้ overide ใน `unicorn/blorgh/lib/blorgh/engine.rb path`

![](https://miro.medium.com/max/2200/1*Gb0ZBz0U9gUnefExx6RVgg.png)

ต่อมาเราก็ไปสร้าง override กัน ใน `unicorn/app/overrides/models/blorgh/article_override.rb โดยเพิ่ม function` time\_since\_created ขึ้นมา

![](https://miro.medium.com/max/2200/1*B1E6XG5RyUN090W_lpKPOw.png)

เรามาลองทดสอบใช้ function time\_since\_created ที่เราสร้างกัน ใช้

```text
$ rails c
```

![](https://miro.medium.com/max/1008/1*aMuph49ejgxKT-KJml6h8A.png)

จากนั้นของมาเรียกใช้ function ดู

```text
Blorgh::Article.first.time_since_created
```

![](https://miro.medium.com/max/2046/1*cbf_SHHM9ObYlUWoxGwNwg.png)

**เย้!! ใช้ได้ครับ**

เเล้วมีอีกวิธี คือการใช้ [ActiveSupport::Concern](https://guides.rubyonrails.org/engines.html#reopening-existing-classes-using-activesupport-concern) ซึ้งจะใช้งานได้เหมือนกันและใช้ในงานได้มากกว่า

