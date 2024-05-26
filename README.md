# Build Attack-Defend


## 1. Giới thiệu chung

Attack-Defend CTF là một dạng cuộc thi bảo mật thông tin, nơi các đội không chỉ phải bảo vệ hệ thống của mình mà còn phải tấn công hệ thống của các đội khác. ForcAD hỗ trợ quản lý và điều hành các cuộc thi dạng này một cách hiệu quả, cung cấp môi trường để người tham gia có thể học hỏi và thực hành các kỹ năng bảo mật.

Cài đặt và Thiết lập.
- Yêu cầu hệ thống:
  - Python 3.8 trở lên
  - Docker và Docker Compose 

Mô hình ở đây chúng ta sẽ chia làm 2 phần:
  - Phần đầu sẽ là về "Services" có tác dụng đùng để build các challenges và checker.
  - Phần thứ hai sẽ là về "ForcAD" sử dụng để build ra bxh và hiển thị các team, các challenges cùng với ip các đội thi.
 
## 2. Build ForcAD

### 2.1.1. ForceAD
- Ở phần này chúng ta sẽ tiến hành phân tích code và thực hiện build ra bxh để sử dụng trong attack-defend. Follow theo đường link sau https://github.com/pomo-mondreganto/ForcAD, đây là template là chúng ta sẽ sử dụng trong hầu hết các cuộc thi Attack-Defend. Về cơ bản nó sẽ có các chức năng cho admin hoặc các user sử dụng.

  ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/4edd58b7-3abc-4d82-8dbf-fa4b18519644)

### 2.1.2. Build
#### Bước 1: Setup môi trường
##### YML file
Chúng ta sẽ tiến hành build theo cách sau:
  - Đầu tiên mở file `config.yml` và sửa nó theo nội dung các services chúng ta muốn khởi chạy. Viết lại và thay thế file `config.yml.example` tác giả đã cho. Đây là nội dung của phần code.
    
    ```
    game:
      mode: classic
      round_time: 60
      start_time: 2023-12-08 10:00:00
      timezone: Europe/Moscow
      default_score: 2500
      flag_lifetime: 10
      game_hardness: 15.0
      inflation: true
    
    tasks:
      - checker: 5Go/checker.py
        checker_timeout: 15
        checker_type: gevent_pfr
        gets: 1
        name: 5Go
        places: 1
        puts: 1
      - checker: kuar/checker.py
        checker_timeout: 10
        checker_type: gevent
        gets: 1
        name: kuar
        places: 1
        puts: 1
      - checker: modelrna/checker.py
        checker_timeout: 15
        checker_type: gevent_pfr
        gets: 1
        name: modelrna
        places: 1
        puts: 1
      - checker: sputnik_v8/checker.py
        checker_timeout: 15
        checker_type: gevent_pfr
        gets: 1
        name: sputnik_v8
        places: 1
        puts: 1
      - checker: vacc_ex/checker.py
        checker_timeout: 10
        checker_type: gevent
        gets: 1
        name: vacc_ex
        places: 1
        puts: 1
      - checker: virush/checker.py
        checker_timeout: 15
        checker_type: gevent_pfr
        gets: 1
        name: virush
        places: 1
        puts: 1
    
    teams:
      - ip: 10.80.0.2
        name: Team1
        highlighted: false
      - ip: 10.80.1.2
        name: Team2
        highlighted: false
      - ip: 10.80.2.2
        name: Team3
        highlighted: false
      - ip: 10.80.3.2
        name: Team4
        highlighted: false
      - ip: 10.80.4.2
        name: Team5
        highlighted: false
      - ip: 10.80.5.2
        name: Team6
        highlighted: false
      - ip: 10.80.6.2
        name: Team7
        highlighted: false
    ```
    
Cùng tiến hành phân tích để hiểu rõ về file `yml` này.
  
Phần `Game`.

  ```
  game:
  mode: classic
  round_time: 60
  start_time: 2023-12-08 10:00:00
  timezone: Europe/Moscow
  default_score: 2500
  flag_lifetime: 10
  game_hardness: 15.0
  inflation: true
  ```
Các Tham số:
  - mode: Kiểu cuộc thi, ở đây là classic.
  - round_time: Thời gian mỗi vòng (round) tính bằng phút, ở đây là 60 phút.
  - start_time: Thời gian bắt đầu cuộc thi, định dạng YYYY-MM-DD HH:MM:SS.
  - timezone: Múi giờ của thời gian bắt đầu, ở đây là Europe/Moscow.
  - default_score: Điểm khởi đầu mặc định cho mỗi đội, ở đây là 2500 điểm.
  - flag_lifetime: Thời gian tồn tại của flag (cờ) tính bằng phút, ở đây là 10 phút.
  - game_hardness: Độ khó của trò chơi, giá trị ở đây là 15.0.
  - inflation: Tùy chọn để kích hoạt lạm phát điểm số, ở đây là true.

Phần Tasks (Nhiệm vụ hoặc Dịch vụ)

  ```
  tasks:
  - checker: 5Go/checker.py
    checker_timeout: 15
    checker_type: gevent_pfr
    gets: 1
    name: 5Go
    places: 1
    puts: 1
  - checker: kuar/checker.py
    checker_timeout: 10
    checker_type: gevent
    gets: 1
    name: kuar
    places: 1
    puts: 1
  - checker: modelrna/checker.py
    checker_timeout: 15
    checker_type: gevent_pfr
    gets: 1
    name: modelrna
    places: 1
    puts: 1
  - checker: sputnik_v8/checker.py
    checker_timeout: 15
    checker_type: gevent_pfr
    gets: 1
    name: sputnik_v8
    places: 1
    puts: 1
  - checker: vacc_ex/checker.py
    checker_timeout: 10
    checker_type: gevent
    gets: 1
    name: vacc_ex
    places: 1
    puts: 1
  - checker: virush/checker.py
    checker_timeout: 15
    checker_type: gevent_pfr
    gets: 1
    name: virush
    places: 1
    puts: 1
  ```
Các Tham số:
  - checker: Đường dẫn tới script kiểm tra (checker script).
  - checker_timeout: Thời gian chờ tối đa cho mỗi lần kiểm tra (tính bằng giây).
  - checker_type: Loại checker sử dụng (gevent hoặc gevent_pfr).
  - gets: Số lần GET yêu cầu (yêu cầu lấy cờ) mà checker thực hiện.
  - name: Tên của nhiệm vụ hoặc dịch vụ.
  - places: Số lần PLACE yêu cầu (yêu cầu đặt cờ) mà checker thực hiện.
  - puts: Số lần PUT yêu cầu (yêu cầu đưa cờ) mà checker thực hiện.
Mỗi task đại diện cho một dịch vụ mà các đội phải bảo vệ và tấn công.


Phần Teams (Các đội tham gia)

  ```
  teams:
    - ip: 10.80.0.2
      name: Team1
      highlighted: false
    - ip: 10.80.1.2
      name: Team2
      highlighted: false
    - ip: 10.80.2.2
      name: Team3
      highlighted: false
    - ip: 10.80.3.2
      name: Team4
      highlighted: false
    - ip: 10.80.4.2
      name: Team5
      highlighted: false
    - ip: 10.80.5.2
      name: Team6
      highlighted: false
    - ip: 10.80.6.2
      name: Team7
      highlighted: false
  
  ```

Các Tham số:
  - ip: Địa chỉ IP của đội.
  - name: Tên của đội.
  - highlighted: Tham số này xác định liệu đội có được làm nổi bật trong giao diện người dùng hay không. Giá trị ở đây là false cho tất cả các đội.

```
Phần game cấu hình các thông số chung cho cuộc thi như thời gian bắt đầu, thời gian mỗi vòng, và điểm số mặc định.
Phần tasks định nghĩa các dịch vụ và kiểm tra liên quan mà các đội sẽ tương tác trong suốt cuộc thi. Mỗi dịch vụ có một script kiểm tra cụ thể với các thông số như thời gian chờ và loại kiểm tra.
Phần teams liệt kê các đội tham gia, mỗi đội có một địa chỉ IP riêng và tên đội.
File YML này cung cấp tất cả các thông tin cần thiết để cấu hình và chạy một cuộc thi Attack-Defend CTF sử dụng ForcAD.
```

##### Docker and library

Tiến hành cài đặt các thư viện bằng dòng lệnh  `pip3 install -r cli/requirements.txt`.
- Các thư viện sau sẽ được cài đặt.
  ```
  click==8.1.7
  pydantic==2.4.2
  PyYAML==6.0.1
  ```
  
    ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/ffcdad3c-307f-4b83-82b9-5a03afb986d6)

Docker
  - Tại đây chúng ta sẽ phải cài phiên bản docker sát với phiên bản của ForcAD.
    
      ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/54007db5-9e93-4c3a-8919-0af6f3fb2fb0)
    
Tham khảo tại các link sau để tải các phiên bản docker phù hợp với phiên bản ForcAD.
  - `https://docs.docker.com/engine/install/ubuntu/`
  - `https://docs.docker.com/compose/install/standalone/` 
  - `https://github.com/docker/compose/releases/tag/v2.2.3`
    
#### Bước 2: Khởi chạy hệ thống.
##### Control.py
Chạy file `./control.py setup` để chuẩn bị cho ForcAD config. Tại đây nó sẽ tạo 1 mk và 1 tài khoản nếu mà mình không cung cấp tài khoản `admin` và mật khẩu trong file `config.yml` nó sẽ gen ra 1 tài khoản và mật khẩu cho admin để control các services hay các team cũng như là các ip của bxh.
- Phân tích fie `control.py` chúng ta sẽ tìm hiểu rõ trong phần sau.
- Sau khi đã chạy file `control.py` với setup được truyền vào thì ta sẽ tiếp tục chạy nó với start để tiến hành khởi động hệ thống của mình. Chờ cho đến khi nó được build thành công có thể mất nhiều thời gian 1 chút.
  
    ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/061922e0-5bdd-4775-a050-035464bd846e)

#### Bước 3: Setup hệ thống.
Sau khi đã cài đặt thành công hệ thống của chúng ta đã được dựng lên.
- Các port đã được mở ra mặc định bxh là `http://103.197.185.208/` với admin nó sẽ ở `http://103.197.185.208/admin`
  
    ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/66f0547b-495b-464e-aa84-7de8475a7894)

- Admin đăng nhập vào sẽ phải nhập username và password. Nếu chưa có username và password chúng ta sẽ xem ở file `config.yml`.

    ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/14973006-dc37-4860-819a-0e4b91e5bc14)

- Tại đây admin có thể kiểm soát các challenges và các team nhưng hiện tại các challenges vẫn chưa thể start được services của chúng nên con bot checker đã checkfailed. Để tiếp tục chúng ta sẽ đi đến phần set up các Services.
    
    ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/e72f2acb-995b-4e71-8df1-de13a148ba0b)
  
### 2.1.3. Phân tích Source Code


## 3. Build Services


Đây là các services chúng ta sẽ build và khởi chạy nó trên server.


| Service | Language | Checker | Sploits | Authors |
|---------|----------|---------|---------|---------|
| **[5Go](services/5Go/)** | Rust & Go | [Checker](checkers/5Go/) | [Sploits](sploits/5Go/) | [@pomo_mondreganto](https://github.com/pomo-mondreganto) |
| **[kuar](services/kuar/)** | C++ | [Checker](checkers/kuar/) | [Sploits](sploits/kuar/) | [@revker](https://github.com/revervand) |
| **[modelrna](services/modelrna/)** | Python | [Checker](checkers/modelrna/) | [Sploits](sploits/modelrna/) | [@jnovikov](https://github.com/jnovikov) |
| **[sputnik_v8](services/sputnik_v8/)** | NodeJS | [Checker](checkers/sputnik_v8/) | [Sploits](sploits/sputnik_v8/) | [@iwalainz](https://github.com/iwalainz) |
| **[vacc_ex](services/vacc_ex/)** | Java | [Checker](checkers/vacc_ex/) | [Sploits](sploits/vacc_ex/) | [@jnovikov](https://github.com/jnovikov) & [@iwalainz](https://github.com/iwalainz) |
| **[virush](services/virush/)** | Bash | [Checker](checkers/virush/) | [Sploits](sploits/virush/) | [@keltecc](https://github.com/keltecc) |


