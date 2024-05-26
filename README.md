# Build Attack-Defend


## 1. Giới thiệu chung

- Attack-Defend CTF là một dạng cuộc thi bảo mật thông tin, nơi các đội không chỉ phải bảo vệ hệ thống của mình mà còn phải tấn công hệ thống của các đội khác. ForcAD hỗ trợ quản lý và điều hành các cuộc thi dạng này một cách hiệu quả, cung cấp môi trường để người tham gia có thể học hỏi và thực hành các kỹ năng bảo mật.

- Cài đặt và Thiết lập.
  - Yêu cầu hệ thống:
    - Python 3.8 trở lên
    - Docker và Docker Compose 

- Mô hình ở đây chúng ta sẽ chia làm 2 phần:
  - Phần đầu sẽ là về "Services" có tác dụng đùng để build các challenges và checker.
  - Phần thứ hai sẽ là về "ForcAD" sử dụng để build ra bxh và hiển thị các team, các challenges cùng với ip các đội thi.
 
## 2. Build ForcAD

### 2.1.1. ForceAD
- Ở phần này chúng ta sẽ tiến hành phân tích code và thực hiện build ra bxh để sử dụng trong attack-defend. Follow theo đường link sau https://github.com/pomo-mondreganto/ForcAD, đây là template là chúng ta sẽ sử dụng trong hầu hết các cuộc thi Attack-Defend. Về cơ bản nó sẽ có các chức năng cho admin hoặc các user sử dụng.

  ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/4edd58b7-3abc-4d82-8dbf-fa4b18519644)

### 2.1.2. Build
#### Bước 1: Setup môi trường
- Chúng ta sẽ tiến hành build theo cách sau:
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
    
  - Cùng tiến hành phân tích nó. 
(KienViet)

- Tiến hành cài đặt các thư viện bằng dòng lệnh  `pip3 install -r cli/requirements.txt`.
- Các thư viện sau sẽ được cài đặt.
  ```
  click==8.1.7
  pydantic==2.4.2
  PyYAML==6.0.1
  ```
  
    ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/ffcdad3c-307f-4b83-82b9-5a03afb986d6)

- Docker
  - Tại đây chúng ta sẽ phải cài phiên bản docker sát với phiên bản của ForcAD.
    
      ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/54007db5-9e93-4c3a-8919-0af6f3fb2fb0)
    
  - Tham khảo tại các link sau để tải các phiên bản docker phù hợp với phiên bản ForcAD.
  - `https://docs.docker.com/engine/install/ubuntu/`
  - `https://docs.docker.com/compose/install/standalone/` 
  - `https://github.com/docker/compose/releases/tag/v2.2.3`
    
  #### Bước 2: Khởi chạy hệ thống.
- Chạy file `./control.py setup` để chuẩn bị cho ForcAD config. Tại đây nó sẽ tạo 1 mk và 1 tài khoản nếu mà mình không cung cấp tài khoản `admin` và mật khẩu trong file `config.yml` nó sẽ gen ra 1 tài khoản và mật khẩu cho admin để control các services hay các team cũng như là các ip của bxh.
- Phân tích fie `control.py`
  (KienViet)

- Sau khi đã chạy file `control.py` với setup được truyền vào thì ta sẽ tiếp tục chạy nó với start để tiến hành khởi động hệ thống của mình. Chờ cho đến khi nó được build thành công có thể mất nhiều thời gian 1 chút.
  
    ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/061922e0-5bdd-4775-a050-035464bd846e)

#### Bước 3: Setup hệ thống.
- Sau khi đã cài đặt thành công hệ thống của chúng ta đã được dựng lên.
- Các port đã được mở ra mặc định bxh là `http://103.197.185.208/` với admin nó sẽ ở `http://103.197.185.208/admin`
  
    ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/66f0547b-495b-464e-aa84-7de8475a7894)

- Admin đăng nhập vào sẽ phải nhập username và password. Nếu chưa có username và password chúng ta sẽ xem ở file `config.yml`.

    ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/14973006-dc37-4860-819a-0e4b91e5bc14)

- Tại đây admin có thể kiểm soát các challenges và các team nhưng hiện tại các challenges vẫn chưa thể start được services của chúng nên con bot checker đã checkfailed. Để tiếp tục chúng ta sẽ đi đến phần set up các Services.
    
    ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/e72f2acb-995b-4e71-8df1-de13a148ba0b)
  

  


