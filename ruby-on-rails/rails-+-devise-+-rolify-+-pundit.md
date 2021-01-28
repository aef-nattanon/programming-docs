# 🐝 มาใช้ Rails + Devise + Rolify + Pundit แบบขอสั้นๆกัน

![](https://cdn-images-1.medium.com/max/1600/1*US12UCC_3AwoIDBn7MFv7Q.jpeg)

ผู้ที่หลงเข้ามา ก็คงต้องมีพื่นฐาน Rails อยู่ไม่มากก็น้อย

* [Devise](https://github.com/heartcombo/devise) ใช้ในการทำเรื่อง authentication หรือ user นั้นเอง
* [Rolify](https://github.com/RolifyCommunity/rolify) ใช้ทำ Roles ในการ authentication นั้นเอง
* [Pandit](https://github.com/varvet/pundit) ใช้ทำ authorization การเข้าถึงหรือใช้งานส่วนต่าง

เพื่อไม่ให้เสียเวลาเราก็มาเริ่มเลยดีกว่า

### **มาสร้าง Project กัน 🌱**

```bash
rails new post-example
```

มาเพิ่ม Gems ที่ต้องใช้งานกันต่อคับ ที่ `./Gemfile`

{% code title="Gemfile" %}
```ruby
gem 'devise'
gem 'pundit'
gem 'rolify'
```
{% endcode %}

เมื่อเพิ่มเสร็จเเล้วก็ สั่ง install gems ที่เราเพิ่มมาด้วย

```bash
bundle install
```

### **Authentication 👨‍✈️**

ต่อมาเรามาสร้าง User Model ด้วย [Devise](https://github.com/heartcombo/devise) ไว้เพื่อ authentication กัน โดยอันดับเเรกเราต้อง install เจ้าตัว [Devise](https://github.com/heartcombo/devise) มาก่อนด้วย

```bash
rails generate devise:install
```

![&#xE15;&#xE31;&#xE27; Devise &#xE08;&#xE30;&#xE2A;&#xE23;&#xE49;&#xE32;&#xE07; config &#xE21;&#xE32;&#xE43;&#xE2B;&#xE49;&#xE40;&#xE23;&#xE32; 2 files &#x1F44F;](https://cdn-images-1.medium.com/max/1600/1*PUDoJI6FL93n6kntpgv0_Q.png)

ต่อมาก็มาสร้าง User Model ของเรากัน

```bash
rails generate devise User
```

![&#xE15;&#xE31;&#xE27; devise &#xE2A;&#xE23;&#xE49;&#xE32;&#xE07; user model &#xE21;&#xE32;&#xE43;&#xE2B;&#xE49;&#xE40;&#xE23;&#xE32;&#xE40;&#xE40;&#xE25;&#xE49;&#xE27; &#x1F44F;](https://cdn-images-1.medium.com/max/1600/1*vxmPKB6F_0I0jxXLKSKYAQ.png)

ต่อมาก็เพื่อ Code ให้ authenticate ใน web เรากันโดยใส่ไว้ที่ file path `./app/controllers/application_controller.rb`

{% code title="application\_controller.rb" %}
```ruby
class ApplicationController < ActionController::Base
  before_action :authenticate_user!
end
```
{% endcode %}

มี User Model เเล้ว เราก็มาต่อในการสร้าง Roles ในการ authentication ด้วย [Rolify](https://github.com/RolifyCommunity/rolify) ของเรานั้นเอง ด้วยระบุให้มัน ชี้ไปที่ User Model ของเรา และ สร้าง Role Model ขึ้นมา

```bash
rails g rolify Role User
```

![Rolify &#xE2A;&#xE23;&#xE49;&#xE32;&#xE07; Role Modle &#xE21;&#xE32;&#xE43;&#xE2B;&#xE49;&#xE40;&#xE23;&#xE32;&#xE40;&#xE40;&#xE25;&#xE49;&#xE27; &#x1F44F;](https://cdn-images-1.medium.com/max/1600/1*MLDyDiLQBK6HB75cHE0IUw.png)

จากนั้นก็ของ สั่ง Migrate สักรอบก็เเล้วกันว่ามันใช้งานได้มั้ย

```text
rails db:migrate
```

![&#xE23;&#xE2D;&#xE14;&#xE44;&#xE1B;&#xE22;&#xE31;&#xE07;&#xE43;&#xE0A;&#xE49;&#xE07;&#xE32;&#xE19;&#xE44;&#xE14;&#xE49;&#xE2D;&#xE22;&#xE39;&#xE48; &#x1F923;](https://cdn-images-1.medium.com/max/1600/1*A_n2PnRGt2Ju0tly953Aqg.png)

### **Post 📃**

เมื่อ ทำเรื่อง authentication เสร็จเราก็มาต่อกันที่ตัว Post Model ก่อนต่อครับ โดยทำการสร้าง เเล้ว `references` ไปยังตัว User Model ของเราด้วย

```bash
rails generate scaffold Post title:string content:string user:references
```

![&#xE27;&#xE49;&#xE32;&#xE27;&#xE43;&#xE0A;&#xE49; rails generate scaffold &#xE21;&#xE32;&#xE17;&#xE31;&#xE49;&#xE07; views &#xE40;&#xE40;&#xE25;&#xE49;&#xE27; model &#x1F929;](https://cdn-images-1.medium.com/max/1600/1*4XwNgwYdfGJr_VTgF53Gqw.png)

จากนั้นก็ของ สั่ง Migrate กันอีกสักรอบ

```bash
rails db:migrate
```

เเล้ว ระบุ root route ให้ไปที่ post กัน

{% code title="routes.rb" %}
```ruby
Rails.application.routes.draw do
  root 'posts#index'
  resources :posts
  devise_for :users
end
```
{% endcode %}

### **สร้าง User กัน👨‍🏭**

ด้วยเราจะสร้าง 2 Role คือ user และ admin โดยไปที่ `./db/seeds.rb`เล้วใส่คำสั่งต่อไปนี้เพื่อสร้าง user ทั้ง 2

{% code title="./db/seeds.rb" %}
```ruby
unless User.find_by(email: 'admin@post.com').present?
  user = User.create(email: 'admin@post.com', password: 'post123')
  user.add_role :admin
end

unless User.find_by(email: 'user@post.com').present?
  user = User.create(email: 'user@post.com', password: 'post123')
  user.add_role :user
end

```
{% endcode %}

เเล้วสั่ง run seeds ของเราเพื่อสร้าง user ทั้ง 2

```bash
rails db:seed
```

### **ลอง Test Login 🔌**

เริ่มเเรกเรามา สั่ง run web เราก่อน

```bash
rails s
```

เเล้วไปที่ url

```bash
open http://localhost:3000/
```

![&#xE43;&#xE0A;&#xE49;&#xE07;&#xE32;&#xE19;&#xE44;&#xE14;&#xE49;&#xE04;&#xE23;&#xE31;&#xE1A; &#x1F44D;](https://cdn-images-1.medium.com/max/1600/1*YDbysELnDuMmeYOI873WFw.png)

เเล้วลอง login ด้วย

* **Email:** admin@post.com **Pass:** post123 **Role:** Admin
* **Email:** user@post.com **Pass:** post123 **Role:** User

![&#xE40;&#xE02;&#xE49;&#xE32;&#xE44;&#xE14;&#xE49;&#xE17;&#xE31;&#xE49;&#xE07; 2 User &#xE17;&#xE31;&#xE49;&#xE07; admin &#xE41;&#xE25;&#xE30; user](https://cdn-images-1.medium.com/max/1600/1*S9DgP0A-V0LAOP07kqbUcg.png)

### **Authorization ⚙️**

มาต่อที่ [Pandit](https://github.com/varvet/pundit) ใช้ทำ authorization การเข้าถึงหรือใช้งานส่วนต่างกันเรื่อง

```bash
rails g pundit:install
```

![&#xE40;&#xE1B;&#xE47;&#xE19;&#xE2D;&#xE31;&#xE19;&#xE40;&#xE2A;&#xE23;&#xE47;&#xE08;](https://cdn-images-1.medium.com/max/1600/1*rS4xs9WdIaKl6uR95wy4vw.png)

ต่อมามาสร้าง Post Policy กัน

```bash
rails g pundit:policy post
```

![&#xE44;&#xE1B;&#xE01;&#xE31;&#xE19;&#xE15;&#xE48;&#xE2D;](https://cdn-images-1.medium.com/max/1600/1*ct0h9cxU7hQ-TeX7f90gMA.png)

เรามากำหนด Post Policy กันหน่อย คือระบุให้

* admin เห็นทุก posts แต่ user เห็นเเค่ของตัวเอง
* admin เเก้ใขหรือลบได้ทุก posts แต่ user เเค่ของตัวเอง

เพิ่ม include Pundit ใน ApplicationController

![./app/controllers/application\_controller.rb](https://cdn-images-1.medium.com/max/1600/1*dg46LcRKmhAxxhmatnFk6w.png)

ต่อมาเพิ่ม authorize ที่ posts\_controller.rb ใน set\_post

![./app/controllers/posts\_controller.rb](https://cdn-images-1.medium.com/max/1600/1*XSkBdzW9h1PNCVIFCkBxfA.png)

เเล้ว ใช้ policy\_scope\(\) เเละ policy\(\) กันหน่อย ที่ index.html ของ post

![./app/views/posts/index.html.erb](https://cdn-images-1.medium.com/max/1600/1*G-4ewQ8Z8t4l3njz7WFL-g.png)

เป็นอันเสร็จสิ้น

{% tabs %}
{% tab title="./app/policies/post\_policy.rb" %}
```ruby
# frozen_string_literal: false

# Class PostPolicy
class PostPolicy < ApplicationPolicy
  # Class Scope
  class Scope < Scope
    def resolve
      if user.has_role?(:admin)
        scope.all
      else
        scope.where(user: user)
      end
    end
  end

  def update?
    user.present? and user.has_role?(:admin) or user == post.user
  end

  def destroy?
    user.present? and user.has_role?(:admin) or user == post.user
  end

  private

  def post
    record
  end
end

```
{% endtab %}

{% tab title="./app/controllers/application\_controller.rb" %}
```ruby
# frozen_string_literal: false

# Class ApplicationController
class ApplicationController < ActionController::Base
  include Pundit
  before_action :authenticate_user!
  rescue_from Pundit::NotAuthorizedError, with: :user_not_authorized

  private

  def user_not_authorized(exception)
    policy_name = exception.policy.class.to_s.underscore
    flash[:warning] = t("#{policy_name}.#{exception.query}", scope: 'pundit')
    redirect_back fallback_location: root_path
  end
end

```
{% endtab %}

{% tab title="./app/controllers/posts\_controller.rb" %}
```ruby
class PostsController < ApplicationController
  before_action :set_post, only: %i[show edit update destroy]

  # GET /posts or /posts.json
  def index
    @posts = Post.all
  end

  # GET /posts/1 or /posts/1.json
  def show; end

  # GET /posts/new
  def new
    @post = Post.new
  end

  # GET /posts/1/edit
  def edit; end

  # POST /posts or /posts.json
  def create
    @post = Post.new(post_params)
    @post.user = current_user
    respond_to do |format|
      if @post.save
        format.html { redirect_to @post, notice: 'Post was successfully created.' }
        format.json { render :show, status: :created, location: @post }
      else
        format.html { render :new, status: :unprocessable_entity }
        format.json { render json: @post.errors, status: :unprocessable_entity }
      end
    end
  end

  # PATCH/PUT /posts/1 or /posts/1.json
  def update
    respond_to do |format|
      if @post.update(post_params)
        format.html { redirect_to @post, notice: 'Post was successfully updated.' }
        format.json { render :show, status: :ok, location: @post }
      else
        format.html { render :edit, status: :unprocessable_entity }
        format.json { render json: @post.errors, status: :unprocessable_entity }
      end
    end
  end

  # DELETE /posts/1 or /posts/1.json
  def destroy
    @post.destroy
    respond_to do |format|
      format.html { redirect_to posts_url, notice: 'Post was successfully destroyed.' }
      format.json { head :no_content }
    end
  end

  private

  # Use callbacks to share common setup or constraints between actions.
  def set_post
    @post = authorize Post.find(params[:id])
  end

  # Only allow a list of trusted parameters through.
  def post_params
    params.require(:post).permit(:title, :content, :user_id)
  end
end

```
{% endtab %}

{% tab title="./app/views/posts/index.html.erb" %}
```ruby
<p id="notice"><%= notice %></p>

<h1>Posts</h1>

<table>
  <thead>
    <tr>
      <th>Title</th>
      <th>Content</th>
      <th>User</th>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <% policy_scope(Post).each do |post| %>
      <tr>
        <td><%= post.title %></td>
        <td><%= post.content %></td>
        <td><%= post.user.email %></td>
        <td><%= link_to 'Show', post %></td>
        <td><%= link_to 'Edit', edit_post_path(post) %></td>
        <td><%= link_to 'Destroy', post, method: :delete, data: { confirm: 'Are you sure?' } if policy(post).destroy? %></td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>

<%= link_to 'New Post', new_post_path %>

```
{% endtab %}
{% endtabs %}

ลอง ให้ user และ admin สร้าง post คนละ 1 post 

![admin &#xE40;&#xE2B;&#xE47;&#xE19;&#xE17;&#xE31;&#xE49;&#xE07;&#xE2B;&#xE21;&#xE14; &#xE40;&#xE40;&#xE15;&#xE48; user &#xE40;&#xE2B;&#xE47;&#xE19;&#xE40;&#xE40;&#xE04;&#xE48;&#xE02;&#xE2D;&#xE07;&#xE15;&#xE31;&#xE27;&#xE40;&#xE2D;&#xE07;](https://cdn-images-1.medium.com/max/1600/1*zfN1YDvXfE-Xok9fTzvYkQ.png)

ลองเข้าไปแก้ใข post ของ admin โดย url ตรง [http://localhost:3000/posts/1/edit](http://localhost:3000/posts/1/edit)

![admin &#xE40;&#xE40;&#xE01;&#xE49; post &#xE17;&#xE35;&#xE48;&#xE15;&#xE31;&#xE27;&#xE40;&#xE2D;&#xE07;&#xE2A;&#xE23;&#xE49;&#xE32;&#xE07;&#xE44;&#xE14;&#xE49; &#xE40;&#xE40;&#xE15;&#xE48; user &#xE44;&#xE21;&#xE48;&#xE2A;&#xE32;&#xE21;&#xE32;&#xE23;&#xE16;&#xE40;&#xE40;&#xE01;&#xE49;&#xE43;&#xE02;&#xE44;&#xE14;&#xE49; &#xE21;&#xE35;&#xE02;&#xE49;&#xE2D;&#xE04;&#xE27;&#xE32;&#xE21;&#xE40;&#xE40;&#xE08;&#xE49;&#xE07;&#xE40;&#xE15;&#xE37;&#xE48;&#xE2D;&#xE19; translation missing: en.pundit.post\_policy.edit?](https://cdn-images-1.medium.com/max/1600/1*2CwOWf7aAen2vRHLdFsHlw.png)

ลองเข้าไปแก้ใข post ของ userโดย url ตรง [http://localhost:3000/posts/2/edit](http://localhost:3000/posts/1/edit)

![&#xE17;&#xE31;&#xE49;&#xE07; user &#xE41;&#xE25;&#xE30; admin &#xE2A;&#xE32;&#xE21;&#xE32;&#xE23;&#xE16;&#xE40;&#xE40;&#xE01;&#xE49;&#xE43;&#xE02;&#xE44;&#xE14;&#xE49;](https://cdn-images-1.medium.com/max/1600/1*zClB-5w4OOuZ16--nzXLew.png)

🏍 Code ตัวอย่างข้างต้น: [https://github.com/aef-nattanon/post-example](https://github.com/aef-nattanon/post-example)

