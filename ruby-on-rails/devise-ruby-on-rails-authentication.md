---
description: 'ruby:2.6.3, rails: 6.0.2, devise: 4.7.1'
---

# 🐌 มาใช้ Devise ใน Ruby on Rails ทำ Authentication กันเถอะ

## Devise คืออะไร ?

ตามหัวข้อเลยครับ [Devise](https://github.com/plataformatec/devise#example-applications) คือ ตัวที่มาช่วยให้เรา ทำ Authentication ได้ง่ายขึ้น หรือถ้าไม่ต้องการ fix อะไรมาก ก็สามารถใช้งานได้เลย!! อาจยังไม่เห็นภาพ นั้นมาเริ่มกันเลยดีกว่า!!

เริ่มแรกเรามาสร้าง project rails กันก่อนนะครับ ในที่นี้ผมจะใช้ชื่อ rails-devise ด้วยคำสั่ง

```text
rails new rails-devise
```

เมื่อ run เส็จก็ได้ หน้าตาประมาญนี้!!

![](https://miro.medium.com/max/442/1*1qHEWnyP2lwK9Gpmcu2Apg.png)

จากนั้น เรา ก็ ลองสร้างเนื้อหาใน app เราก่อน โดยในที่นี้ จะสร้าง articles ขึ้นมาด้วยคำสั่ง

```text
rails generate scaffold article title:string content:string
```

เมื่อ run เส็จก็จะได้ files ประมาญนี้ครับ

![](https://miro.medium.com/max/928/1*-eiCDtXo79-VpSInqCXBQQ.png)

เเล้ว update ฐานข้อมูลก่อนหน่อย ด้วย

```text
rails db:migrate
```

![](https://miro.medium.com/max/1130/1*nHQCHRDU6YpHRl7lYUQ2cg.png)

เราลอง run project ขึ้นมาดู article เรากันหน่อย ด้วย

```text
rails s
```

![](https://miro.medium.com/max/922/1*NWkP7FWLRHJLJZFSqApMLA.png)

เเล้วลองไปดูที่ [http://localhost:3000/articles](http://localhost:3000/articles)![Image for post](https://miro.medium.com/max/60/1*cObKa_iUBQEII98087GcjA.png?q=20)

![](https://miro.medium.com/max/2308/1*cObKa_iUBQEII98087GcjA.png)

articles ของเรามาเเล้ว ต่อมา เรามาดู ตัว highlights ของเรา [Devise](https://github.com/plataformatec/devise#example-applications) เพื่อที่จะทำ authentication เริ่มแรก เรามาเพิ่ม [Devise](https://github.com/plataformatec/devise#example-applications) เข้ามาใน project ก่อน โดย ไปเพิ่มใน `Gemfile`ของ project เรา

```text
gem 'devise'
```

จากนั้น ก็ install มันสะ!!

```text
bundle install
```

หลังจาก install [Devise](https://github.com/plataformatec/devise#example-applications) เส็จเเล้ว ก็มาทำการ generator ตัว [Devise](https://github.com/plataformatec/devise#example-applications) ให้ project เราใช้งาน มันสะด้วยคำสั่ง

```text
rails generate devise:install
```

![](https://miro.medium.com/max/1212/1*KUOBmPPhWQrW-UbzeiwKZA.png)

เมื่อ เส็จ เเล้ว ก็จะได้ files มา 2 files นะครับ ตามภาพเลย เเถม ตัว [Devise](https://github.com/plataformatec/devise#example-applications) ก็มีข้อมาไกด์

ค่อยมาต่อ…..[Devise](https://github.com/plataformatec/devise#example-applications)

