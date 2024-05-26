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
#### YML file
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
#### Control.py
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
  
## 3. Phân tích Source Code ForcAD

### Source code chia thành 5 phần chính

- Thư mục `cli`: Nhiệm vụ thực hiện các câu lệnh để deploy chương trình.
- Thư mục `Backend`: Nhiệm vụ thực hiện xử lý các api được gửi đến server.
- Thư mục `checker`: Nhiệm vụ bổ trợ server để thực hiện check flag, sự hoạt động và thông tin challenge của mỗi team.
- Thư mục `docker-config`: Nhiệm vụ chứa những docker file cần cài đặt cho dịch vụ.
- Thư mục `front`: Nhiệm vụ render giao diện bảng thông số. Giao diện của trang admin.
  
  ![image](https://github.com/H4lst0n/ADCTF/assets/97662987/f87e8e1e-22de-4a25-862c-c5627fda0af4)
  
### 3.1. Phân tích thư mục CLI

#### 3.1.1. CLI Control.py

File control.py với nội dung như sau:

```
#!/usr/bin/env python3

from cli import cli

if __name__ == '__main__':
    cli()
```

File này sẽ gọi vào thư mục cli và thực thi các file có trong cli. Ở đây gồm các file python. Chúng ta sẽ chỉ đi phân tích và đọc qua các file .py nằm trong thư mục cli để hiểu rõ hơn về chương trình của mình. 

  ![image](https://github.com/H4lst0n/ADCTF/assets/91616280/6d5a7cd4-830e-470f-9b61-27d07a013d0f)

Các tệp này cùng nhau xác định cấu trúc và cách hoạt động của ứng dụng:
Khởi Tạo và Quản Lý CLI: init.py tạo ra nhóm lệnh chính cli và đăng ký các lệnh từ module base, chuẩn bị sẵn sàng để thêm các lệnh và nhóm lệnh con khác.
- `models.py` xác định các mô hình dữ liệu.
- `options.py` chứa các tùy chọn cấu hình và các thông số có thể điều chỉnh.
- `requirements.txt` liệt kê các thư viện phụ thuộc cần thiết để cài đặt và chạy ứng dụng.
- `constants.py` cung cấp các đường dẫn và hằng số quan trọng cho cấu hình Docker, Terraform, và các bí mật, đảm bảo các tệp và thư mục cần thiết luôn sẵn sàng và dễ dàng truy cập.
- `utils.py` chứa các hàm để tải, kiểm tra, ghi đè, và sao lưu cấu hình, cũng như các hàm để quản lý các lệnh hệ thống, in thông báo, và quản lý tệp/thư mục. Những hàm này giúp tối ưu hóa và tự động hóa các tác vụ quản trị hệ thống.
- `init.py` tạo ra nhóm lệnh chính cli và đăng ký các lệnh từ module base, chuẩn bị sẵn sàng để thêm các lệnh và nhóm lệnh con khác.
  
Bây giờ chúng ta sẽ tiến hành phân tích chi tiết các tệp `init.py`, `constants.py`, `models.py`, `options.py`, và `utils.py`. Trước tiên, tôi sẽ mở và đọc nội dung của từng tệp này và sẽ phân tích chi tiết trực tiếp vào bên trong phần code của nó qua các dòng comment.

#### 3.1.2. CLI init.py
Tệp này thiết lập điểm vào chính cho ứng dụng CLI của bạn, sử dụng Click để quản lý các lệnh và nhóm lệnh.

  ```
  import click
  from . import base
  
  # Định nghĩa nhóm lệnh chính 'cli'
  @click.group()
  def cli():
      pass
  
  # Đăng ký các lệnh từ module 'base'
  base.register(cli)
  
  __all__ = ('cli',)
  ```

- `cli`: Được định nghĩa như một nhóm lệnh Click.
- `base.register(cli)`: Đăng ký các lệnh từ module base vào nhóm lệnh cli.
- `__all__`: Định nghĩa các biểu tượng sẽ được xuất khi sử dụng from module import *.

#### 3.1.3. CLI models.py
Tệp này định nghĩa các mô hình dữ liệu (data models) sử dụng Pydantic để xác thực và quản lý dữ liệu. Các mô hình này có thể đại diện cho cấu hình của trò chơi, thông tin về các đội, và các nhiệm vụ trong Attack-Defend.

  ```
  from datetime import datetime
  from typing import List, Optional
  from pydantic import BaseModel
  
  # Định nghĩa cấu hình của admin
  class AdminConfig(BaseModel):
      username: str
      password: str
  
  # Định nghĩa cấu hình của cơ sở dữ liệu
  class DatabaseConfig(BaseModel):
      user: str
      password: str
      host: str = 'postgres'
      port: int = 5432
      dbname: str = 'forcad'
  
  # Định nghĩa cấu hình của RabbitMQ
  class RabbitMQConfig(BaseModel):
      user: str
      password: str
      host: str = 'rabbitmq'
      port: int = 5672
      vhost: str = 'forcad'
  
  # Định nghĩa cấu hình của Redis
  class RedisConfig(BaseModel):
      password: str
      host: str = 'redis'
      port: int = 6379
      db: int = 0
  
  # Định nghĩa cấu hình lưu trữ
  class StoragesConfig(BaseModel):
      db: DatabaseConfig
      rabbitmq: RabbitMQConfig
      redis: RedisConfig
  
  # Định nghĩa cấu hình của trò chơi
  class GameConfig(BaseModel):
      flag_lifetime: int
      round_time: int
      start_time: datetime
      timezone: str = 'UTC'
      default_score: float = 2500
      game_hardness: float = 10
      mode: str = 'classic'
      get_period: Optional[int] = None
      inflation: bool = True
      volga_attacks_mode: bool = False
      checkers_path: str = '/checkers/'
      env_path: str = ''
  
  # Định nghĩa một nhiệm vụ trong trò chơi
  class Task(BaseModel):
      name: str
      checker: str
      gets: int = 1
      puts: int = 1
      places: int = 1
      checker_timeout: int = 10
      checker_type: str = 'hackerdom'
      env_path: Optional[str] = None
      default_score: Optional[float] = None
      get_period: Optional[int] = None
  
  # Định nghĩa một đội trong trò chơi
  class Team(BaseModel):
      ip: str
      name: str
      highlighted: bool = False
  
  # Định nghĩa cấu hình cơ bản
  class BasicConfig(BaseModel):
      admin: Optional[AdminConfig] = None
      game: GameConfig
      tasks: List[Task]
      teams: List[Team]
  
  # Định nghĩa cấu hình chính, mở rộng từ cấu hình cơ bản
  class Config(BasicConfig):
      admin: AdminConfig
      storages: StoragesConfig
  
  ```

#### 3.1.4. CLI options.py
Tệp này chứa các hàm để xử lý các tùy chọn dòng lệnh bằng thư viện Click. Nó định nghĩa các tùy chọn như --fast và --workers, giúp người dùng tùy chỉnh các thông số khi chạy ứng dụng.

  ```
  import click
  
  # Định nghĩa tùy chọn 'fast'
  def with_fast_option(func):
      return click.option(
          '--fast',
          is_flag=True,
          help='Use faster build with prebuilt images',
      )(func)
  
  # Định nghĩa tùy chọn 'workers'
  def with_workers_option(func):
      return click.option(
          '-w', '--workers',
          type=int,
          default=1,
          show_default=True,
          help='Number of workers to run',
      )(func)
  
  # Ví dụ áp dụng tùy chọn 'fast' và 'workers' vào một hàm
  @click.command()
  @with_fast_option
  @with_workers_option
  def build(fast, workers):
      click.echo(f"Fast: {fast}")
      click.echo(f"Workers: {workers}")
  
  if __name__ == '__main__':
      build()
  ```

#### 3.1.5. CLI constants.py
Tệp này định nghĩa các hằng số và đường dẫn quan trọng cho ứng dụng của bạn.

  ```
  from pathlib import Path
  
  # Các hằng số liên quan đến thông tin admin và cấu trúc thư mục
  ADMIN_USER = 'forcad'
  
  BASE_DIR = Path(__file__).absolute().resolve().parents[1]
  BASE_COMPOSE_FILE = 'docker-compose-base.yml'
  FAST_COMPOSE_FILE = 'docker-compose-fast.yml'
  TESTS_COMPOSE_FILE = 'docker-compose-tests.yml'
  
  DEPLOY_DIR = BASE_DIR / 'deploy'
  SECRETS_DIR = DEPLOY_DIR / 'secrets'
  KUSTOMIZATION_BASE_PATH = DEPLOY_DIR / 'kustomization.base.yml'
  KUSTOMIZATION_PATH = DEPLOY_DIR / 'kustomization.yml'
  
  TERRAFORM_DIR = DEPLOY_DIR / 'terraform'
  TF_CREDENTIALS_PATH = TERRAFORM_DIR / 'credentials.auto.tfvars.json'
  
  FULL_COMPOSE_PATH = BASE_DIR / 'docker-compose.yml'
  CONFIG_PATH = BASE_DIR / 'config.yml'
  VERSION_PATH = BASE_DIR / '.version'
  
  DOCKER_CONFIG_DIR = BASE_DIR / 'docker_config'
  DOCKER_VOLUMES_DIR = BASE_DIR / 'docker_volumes'
  
  ADMIN_ENV_PATH = DOCKER_CONFIG_DIR / 'services/' / 'admin.env'
  POSTGRES_ENV_PATH = DOCKER_CONFIG_DIR / 'postgres_environment.env'
  RABBITMQ_ENV_PATH = DOCKER_CONFIG_DIR / 'rabbitmq_environment.env'
  REDIS_ENV_PATH = DOCKER_CONFIG_DIR / 'redis_environment.env'
  
  ADMIN_SECRET_PATH = SECRETS_DIR / 'admin.yml'
  POSTGRES_SECRET_PATH = SECRETS_DIR / 'postgres.yml'
  RABBITMQ_SECRET_PATH = SECRETS_DIR / 'rabbitmq.yml'
  REDIS_SECRET_PATH = SECRETS_DIR / 'redis.yml'
  CONFIG_SECRET_PATH = SECRETS_DIR / 'config.yml'
  
  # Đọc phiên bản từ tệp .version, nếu không tồn tại thì gán là 'latest'
  try:
      VERSION = VERSION_PATH.read_text().strip()
  except FileNotFoundError:
      VERSION = 'latest'
  ```

- Đường dẫn và hằng số: Xác định các đường dẫn cần thiết cho cấu hình Docker, Terraform, và các bí mật.
- Phiên bản: Đọc phiên bản của ứng dụng từ tệp .version, nếu không tồn tại thì mặc định là 'latest'.

#### 3.1.6. CLI utils.py
Tệp này chứa các hàm tiện ích để hỗ trợ các thao tác hệ thống, quản lý tệp và in thông báo.
  ```
  import os
  import secrets
  import shutil
  import subprocess
  import sys
  import time
  from pathlib import Path
  from typing import List, Optional, Tuple
  
  import click
  import yaml
  from pydantic import ValidationError
  
  from . import constants, models
  
  # Hàm load cấu hình thô từ tệp YAML
  def load_raw_config(path: Path) -> dict:
      if not path.is_file():
          print_error(f'Config file missing at {path}')
          sys.exit(1)
  
      with path.open(mode='r') as f:
          raw = yaml.safe_load(f)
  
      # Điều chỉnh cấu hình cũ
      if 'global' in raw:
          raw['game'] = raw.pop('global')
  
      return raw
  
  # Hàm load cấu hình cơ bản
  def load_basic_config() -> models.BasicConfig:
      print_bold(f'Loading basic configuration from {constants.CONFIG_PATH}')
      raw = load_raw_config(constants.CONFIG_PATH)
  
      try:
          config = models.BasicConfig.model_validate(raw, strict=True)
      except ValidationError as e:
          print_error(f'Invalid configuration file: {e}')
          sys.exit(1)
  
      return config
  
  # Hàm load cấu hình đầy đủ
  def load_config() -> models.Config:
      print_bold(f'Loading full configuration from {constants.CONFIG_PATH}')
      raw = load_raw_config(constants.CONFIG_PATH)
  
      try:
          config = models.Config.parse_obj(raw)
      except ValidationError as e:
          print_error(f'Invalid configuration file: {e}')
          sys.exit(1)
  
      return config
  
  # Hàm backup cấu hình
  def backup_config():
      backup_path = constants.BASE_DIR / f'config_backup_{int(time.time())}.yml'
      print_bold(f'Creating config backup at {backup_path}')
      shutil.copy2(constants.CONFIG_PATH, backup_path)
  
  # Hàm ghi cấu hình mới vào tệp
  def dump_config(config: models.Config):
      print_bold(f'Writing new configuration to {constants.CONFIG_PATH}')
      with constants.CONFIG_PATH.open(mode='w') as f:
          yaml.safe_dump(config.model_dump(by_alias=True, exclude_none=True), f)
  
  # Hàm ghi đè cấu hình
  def override_config(
          config: models.Config, *,
          redis: Optional[str] = None,
          database: Optional[str] = None,
          rabbitmq: Optional[str] = None):
      if redis:
          host, port = parse_host_data(redis, 6379)
          config.storages.redis.host = host
          config.storages.redis.port = port
  
      if database:
          host, port = parse_host_data(database, 5432)
          config.storages.db.host = host
          config.storages.db.port = port
  
      if rabbitmq:
          host, port = parse_host_data(rabbitmq, 5672)
          config.storages.rabbitmq.host = host
          config.storages.rabbitmq.port = port
  
  # Hàm thiết lập cấu trúc phụ trợ
  def setup_auxiliary_structure(config: models.BasicConfig) -> models.Config:
      if not config.admin:
          new_username = constants.ADMIN_USER
          new_password = secrets.token_hex(8)
          config.admin = models.AdminConfig(
              username=new_username,
              password=new_password,
          )
  
          print_bold(f'Created new admin credentials: {new_username}:{new_password}')
      else:
          print_bold('Using existing credentials specified in admin section')
  
      username = config.admin.username
      password = config.admin.password
  
      storages = models.StoragesConfig(
          db=models.DatabaseConfig(user=username, password=password),
          rabbitmq=models.RabbitMQConfig(user=username, password=password),
          redis=models.RedisConfig(password=password),
      )
  
      return models.Config(
          admin=config.admin,
          game=config.game,
          storages=storages,
          tasks=config.tasks,
          teams=config.teams,
      )
  
  # Hàm chạy lệnh hệ thống
  def run_command(command: List[str], cwd=None, env=None):
      print_bold(f'Running command {command}')
      p = subprocess.run(command, cwd=cwd, env=env)
      if p.returncode != 0:
          print_error(f'Command {command} failed!')
          sys.exit(1)
  
  # Hàm lấy kết quả từ lệnh hệ thống
  def get_output(command: List[str], cwd=None, env=None) -> str:
      print_bold(f'Running command {command}')
      return subprocess.check_output(command, cwd=cwd, env=env).decode()
  
  # Hàm chạy Docker với các tham số
  def run_docker(args: List[str]):
      base = [
          'docker', 'compose',
          '-f', constants.BASE_COMPOSE_FILE,
      ]
  
      ctx = click.get_current_context()
      if ctx.params.get('fast'):
          base += ['-f', constants.FAST_COMPOSE_FILE]
      elif os.getenv('TEST'):
          base += ['-f', constants.TESTS_COMPOSE_FILE]
  
      env = os.environ.copy()
      env['FORCAD_VERSION'] = constants.VERSION
      run_command(
          base + args,
          cwd=constants.BASE_DIR,
          env=env,
      )
  
  # Hàm phân tích địa chỉ host
  def parse_host_data(value: str, default_port: int) -> Tuple[str, int]:
      if ':' in value:
          host, port = value.split(':', 1)
          port = int(port)
          return host, port
      return value, default_port
  
  # Các hàm in thông báo
  def print_file_exception_info(_func, path, _exc_info):
      print_bold(f'File {path} not found')
  
  def print_error(message: str):
      click.secho(message, fg='red', err=True)
  
  def print_success(message: str):
      click.secho(message, fg='green', err=True)
  
  def print_bold(message: str):
      click.secho(message, bold=True, err=True)
  
  # Hàm xóa tệp
  def remove_file(path: Path):
      if not path.exists():
          return
  
      print_bold(f'Removing file {path}')
      if not path.is_file():
          print_error(f'Not a file: {path}')
          return
  
      try:
          path.unlink()
      except FileNotFoundError:
          pass
  
  # Hàm xóa thư mục
  def remove_dir(path: Path):
      if not path.exists():
          return
  
      print_bold(f'Removing directory {path}')
      if not path.is_dir():
          print_error(f'Not a directory: {path}')
          return
  
      shutil.rmtree(path, ignore_errors=True)
  ```


Kết Luận
- models.py: Định nghĩa cấu trúc dữ liệu sử dụng Pydantic để xác thực và quản lý cấu hình của trò chơi, các đội và nhiệm vụ.
- options.py: Cung cấp các tùy chọn dòng lệnh để cấu hình và chạy ứng dụng thông qua Click.
- utils.py: Chứa các hàm tiện ích để hỗ trợ các thao tác hệ thống, quản lý tệp và in thông báo.
- Những tệp này cùng nhau tạo nên các thành phần quan trọng của ứng dụng, từ cấu hình và xác thực dữ liệu đến thao tác hệ thống và quản lý tệp
- Khởi Tạo và Quản Lý CLI: init.py tạo ra nhóm lệnh chính cli và đăng ký các lệnh từ module base, chuẩn bị sẵn sàng để thêm các lệnh và nhóm lệnh con khác.
- Cấu Hình và Quản Lý Hằng Số: constants.py cung cấp các đường dẫn và hằng số quan trọng cho cấu hình Docker, Terraform, và các bí mật, đảm bảo các tệp và thư mục cần thiết luôn sẵn sàng và dễ dàng truy cập.
- Các Hàm Tiện Ích: utils.py chứa các hàm để tải, kiểm tra, ghi đè, và sao lưu cấu hình, cũng như các hàm để quản lý các lệnh hệ thống, in thông báo, và quản lý tệp/thư mục. Những hàm này giúp tối ưu hóa và tự động hóa các tác vụ quản trị hệ thống.


Các tệp này kết hợp với nhau để tạo ra một hệ thống quản lý cấu hình mạnh mẽ và dễ dàng sử dụng thông qua CLI. Sự phân tách rõ ràng giữa các thành phần giúp mã dễ bảo trì và mở rộng, đồng thời giúp các thao tác hệ thống trở nên an toàn và hiệu quả hơn.

### 3.2. Checker Bot
#### 3.2.1. Cấu hình Checker Bot

Checksystem hoàn toàn tương thích với các checkers của Hackerdom, nhưng đã bổ sung một số cải tiến ở mức cấu hình. Các checkers được cấu hình riêng cho từng task. Nên đặt mỗi checker vào một thư mục riêng trong thư mục checkers ở thư mục gốc của dự án. Checker được xem là bao gồm tệp thực thi chính và một số tệp phụ trợ trong cùng một thư mục.

Các tùy chọn sau đây được hỗ trợ:

- name (bắt buộc): tên của dịch vụ hiển thị trên bảng điểm.
- checker (bắt buộc): đường dẫn tới tệp thực thi checker (tương đối với thư mục checkers), cần phải có quyền đọc và thực thi cho mọi người (chạy chmod o+rx checker_executable), vì checkers được chạy với người dùng nobody. Thông thường là <service_name>/checker.py.
- checker_timeout (bắt buộc): thời gian chờ (timeout) tính bằng giây cho mỗi hành động của checker. Vì ít nhất có 3 hành động được chạy (tùy thuộc vào puts và gets), nên khuyến nghị đặt round_time ít nhất gấp 4 lần giá trị checker_timeout lớn nhất nếu có thể.
- puts (tùy chọn, mặc định 1): số lượng cờ cần đặt cho mỗi đội trong mỗi vòng.
- gets (tùy chọn, mặc định 1): số lượng cờ cần kiểm tra từ các vòng có thời gian sống của cờ (flag_lifetime) cuối cùng.
- places (tùy chọn, mặc định 1): các nhiệm vụ lớn có thể chứa nhiều vị trí có thể đặt cờ, đó là số lượng. Nó được ngẫu nhiên hóa cho mỗi lần đặt trong khoảng [1, places] và được truyền vào các hành động PUT và GET của checker.
- checker_type (tùy chọn, mặc định hackerdom): một tùy chọn chứa các tag ngăn cách bởi dấu gạch dưới (các tag thiếu sẽ bị bỏ qua). Các ví dụ: hackerdom (tag hackerdom bị bỏ qua, vì vậy không có tag sửa đổi nào được áp dụng), pfr (checker với dữ liệu cờ công khai được trả về). Các tag hiện được hỗ trợ là:
    - pfr: checker trả về dữ liệu cờ công khai (ví dụ: tên người dùng của cờ) từ hành động PUT như một thông báo công khai, dữ liệu cờ riêng (flag_id) như một thông báo riêng tư, và thông báo công khai được hiển thị trên /api/client/attack_data cho người tham gia. Nếu checker không có tag này, không có dữ liệu tấn công nào được hiển thị cho nhiệm vụ.
    - nfr: flag_id được truyền vào PUT cũng được truyền vào GET cùng cờ đó. Bằng cách đó, flag_id được sử dụng để gieo hạt cho bộ sinh số ngẫu nhiên trong các checkers để nó trả về cùng các giá trị cho GET và PUT. Các checkers hỗ trợ tùy chọn này khá hiếm (và cũ), vì vậy không sử dụng nó trừ khi bạn chắc chắn.
   
Giải thích chi tiết về các tag của checker có thể được tìm thấy trong vấn đề này.
- env_path (tùy chọn): đường dẫn hoặc kết hợp các đường dẫn được thêm vào biến môi trường PATH (ví dụ: đường dẫn tới chromedriver).

#### 3.2.2. Thư mục Checkers
Thư mục checkers ở thư mục gốc của dự án (chứa tất cả các thư mục checker) được khuyến nghị có cấu trúc như sau:

    ```
    checkers:
      - requirements.txt  <-- yêu cầu tự động cài đặt (bằng pip) yêu cầu kết hợp của tất cả các checkers (phải có)
      - task1:
          - checker.py  <-- tệp thực thi (o+rx)
      - task2:
          - checker.py  <-- tệp thực thi (o+rx)
    ```


#### 3.2.3. Cấu trúc một Checker cơ bản 
Cấu trúc thư mục Checker dựa trên 1 challenges trong services để phân tích vì file checker này sẽ được sửa dụng ở cả 2 bên là services và forcad.

Thư mục checkers trong thư mục gốc của dự án chứa các thư mục riêng lẻ cho mỗi dịch vụ và tệp requirements.txt chung:
```
checkers/
  ├── requirements.txt  <-- yêu cầu tự động cài đặt (bằng pip) yêu cầu kết hợp của tất cả các checkers (phải có)
  ├── example_service/
  │   └── checker.py    <-- tệp thực thi (o+rx)
```

Đây là nội dung của checker.py trong challenges chúng ta sẽ sử dụng để phân tích `https://github.com/C4T-BuT-S4D/stay-home-ctf-2022/blob/master/checkers/5Go/checker.py`.
```
#!/usr/bin/env python3

import random
import re
import string
from collections import defaultdict

import google.protobuf.message
import grpc
import sys
from checklib import *

from client import Daeh5

PORT = 5005


class Checker(BaseChecker):
    vulns: int = 1
    timeout: int = 20
    uses_attack_data: bool = True

    def __init__(self, *args, **kwargs):
        super(Checker, self).__init__(*args, **kwargs)
        self.d = Daeh5(f'{self.host}:{PORT}')

    def action(self, action, *args, **kwargs):
        try:
            super(Checker, self).action(action, *args, **kwargs)
        except grpc.RpcError as e:
            if e.code() == grpc.StatusCode.UNAVAILABLE:
                self.cquit(Status.DOWN, 'Connection error', f'Got grpc error {e.code()}: {e.details()}')
            else:
                self.cquit(Status.MUMBLE, 'Unexpected grpc error', f'Got grpc error {e.code()}: {e.details()}')
        except google.protobuf.message.DecodeError as e:
            self.cquit(Status.MUMBLE, 'Protobuf parsing error', f'Got error parsing protobuf: {e}')

    def check(self):
        self.d.ping()
        with self.d.session() as session1:
            created = defaultdict(list)
            users = [rnd_username(32) for _ in range(random.randint(2, 3))]
            users += [rnd_string(i) for i in range(5, 10, 2)]
            users = random.sample(users, 5)
            for _ in range(random.randint(10, 20)):
                user = random.choice(users)
                content = rnd_string(random.randint(24, 348), alphabet=string.printable)
                name = rnd_string(random.choice((7, 8, 10, 11, 13, 14)))
                doc = session1.add_document(user, content, name)
                self.check_doc_id(doc.id, name)
                real_doc = session1.get_document(doc.id)
                self.assert_docs_equal(doc, real_doc)
                created[user].append(doc)

            with self.d.session() as session2:
                check_session = random.choice([session1, session2])
                random.shuffle(users)
                for user in users:
                    lst = check_session.list_documents(user)
                    self.assert_gte(len(lst), len(created[user]), 'Invalid number of documents for user')
                    for x, y in zip(lst, created[user]):
                        self.assert_docs_equal(x, y)

            with self.d.session() as session3:
                check_session = random.choice([session1, session3])
                random.shuffle(users)
                for user in users:
                    lst = check_session.list_documents(user)
                    self.assert_gte(len(lst), len(created[user]), 'Invalid number of documents for user')
                    for x, y in zip(lst, created[user]):
                        self.assert_docs_equal(x, y)
                random.shuffle(users)
                for user, docs in created.items():
                    random.shuffle(docs)
                    for doc in docs:
                        if random.randint(0, 2) == 0:
                            real_doc = check_session.get_document(doc.id)
                            self.assert_docs_equal(doc, real_doc)

        self.cquit(Status.OK)

    def put(self, flag_id, flag, vuln):
        user = rnd_username(32)
        part1 = rnd_string(random.randint(12, 150), alphabet=string.printable)
        part2 = rnd_string(random.randint(12, 150), alphabet=string.printable)
        content = f'{part1} {flag} {part2}'
        name = rnd_string(10)
        with self.d.session() as session:
            doc = session.add_document(user, content, name)
        self.check_doc_id(doc.id, name)
        self.cquit(Status.OK, name, f'{user}:{doc.id}')

    def get(self, flag_id, flag, vuln):
        user, doc_id = flag_id.split(':')
        with self.d.session() as session:
            doc = session.get_document(doc_id)
            self.assert_eq(doc.user, user, 'Invalid document returned', status=Status.CORRUPT)
            self.assert_eq(doc.id, doc_id, 'Invalid document returned', status=Status.CORRUPT)
            self.assert_in(flag, doc.content, 'Invalid document returned', status=Status.CORRUPT)

        with self.d.session() as session:
            docs = session.list_documents(user)
            self.assert_gte(len(docs), 1, 'Invalid document listing', status=Status.CORRUPT)

            doc = docs[0]
            self.assert_eq(doc.user, user, 'Invalid document returned', status=Status.CORRUPT)
            self.assert_eq(doc.id, doc_id, 'Invalid document returned', status=Status.CORRUPT)
            self.assert_in(flag, doc.content, 'Invalid document returned', status=Status.CORRUPT)

        self.cquit(Status.OK)

    def check_doc_id(self, doc_id, name, st=Status.MUMBLE):
        pattern = re.compile(rf"^{name}-[0-9a-f]{{8}}-[0-9a-f]{{4}}-[0-9a-f]{{4}}-[0-9a-f]{{4}}-[0-9a-f]{{12}}$")
        self.assert_neq(pattern.match(doc_id), None, 'Invalid document ID', status=st)

    def assert_docs_equal(self, doc1, doc2, st=Status.MUMBLE):
        self.assert_eq(doc1.id, doc2.id, 'Invalid document returned', status=st)
        self.assert_eq(doc1.user, doc2.user, 'Invalid document returned', status=st)
        self.assert_eq(doc1.content, doc2.content, 'Invalid document returned', status=st)
        self.assert_eq(
            doc1.created_at.ToMicroseconds(),
            doc2.created_at.ToMicroseconds(),
            'Invalid document returned',
            status=st,
        )


if __name__ == '__main__':
    c = Checker(sys.argv[2])
    try:
        c.action(sys.argv[1], *sys.argv[3:])
    except c.get_check_finished_exception():
        cquit(Status(c.status), c.public, c.private)
```

Ta sẽ phải tải và import các thư viện cần thiết là các thư viện được sử dụng trong quá trình thực hiện các tác vụ của checker.
```
random, re, string, defaultdict từ collections, google.protobuf.message, grpc, sys và checklib
```

Khai báo các biến và hàm của lớp Checker:
- PORT: Cổng dịch vụ được sử dụng.
- Checker: Lớp kế thừa từ BaseChecker của checklib.
- vulns, timeout, uses_attack_data: Các thuộc tính cấu hình của checker.
- __init__: Hàm khởi tạo kết nối tới dịch vụ thông qua Daeh5.

```
Hàm action sẽ thực hiện các hành động dựa trên tham số truyền vào. Xử lý các lỗi liên quan đến kết nối gRPC và lỗi phân tích protobuf.
Hàm check sẽ kiểm tra tính khả dụng của dịch vụ bằng cách tạo và xác thực các tài liệu ngẫu nhiên.
Hàm put sẽ thực hiện việc đặt cờ vào dịch vụ bằng cách tạo tài liệu chứa cờ.
Hàm get sẽ thực hiện việc kiểm tra cờ từ dịch vụ bằng cách lấy tài liệu chứa cờ và xác thực.
```

Các hàm hỗ trợ:
- check_doc_id: Kiểm tra định dạng của ID tài liệu.
- assert_docs_equal: So sánh hai tài liệu để đảm bảo chúng giống nhau.
Phần main sẽ khởi tạo checker và thực hiện hành động dựa trên tham số dòng lệnh.

File checker.py này được viết để kiểm tra dịch vụ thông qua các hành động đặt và lấy Flag, đồng thời kiểm tra tính toàn vẹn của dữ liệu. Việc tổ chức và cấu hình checker theo hướng dẫn sẽ đảm bảo dịch vụ hoạt động đúng và phát hiện sớm các lỗi tiềm ẩn.

### 3.3. Tìm hiểu Backend

<br>
Để hiểu sâu hơn về phần backend chúng ta sẽ đi đến file `requirements.txt` đây là nơi chứa tất cả module cần thiết trong dự án. Để thực hiện cài đặt module ta thực hiện câu lệnh `pip install -r requirements.txt`:
![image](https://github.com/H4lst0n/ADCTF/assets/97662987/9d9a4d57-9e37-42b4-b391-f6e519222d39)

**Chúng ta sẽ đi tìm hiểu thư mục `scripts`, thư mục sẽ bao gồm những file sau**:

#### 3.3.1 File `create_functions.sql`:
```
CREATE OR REPLACE FUNCTION recalculate_rating(_attacker_id INTEGER, _victim_id INTEGER, _task_id INTEGER,
                                              _flag_id INTEGER)
    RETURNS TABLE
            (
                attacker_delta FLOAT,
                victim_delta   FLOAT
            )
AS
$$
DECLARE
    _round          INTEGER;
    hardness        FLOAT;
    inflate         BOOLEAN;
    scale           FLOAT;
    norm            FLOAT;
    attacker_score  FLOAT;
    victim_score    FLOAT;
    _attacker_delta FLOAT;
    _victim_delta   FLOAT;
BEGIN
    SELECT real_round, game_hardness, inflation FROM GameConfig WHERE id = 1 INTO _round, hardness, inflate;

--     avoid deadlocks by locking min(attacker, victim), then max(attacker, victim)
    if _attacker_id < _victim_id THEN
        SELECT score
        FROM TeamTasks
        WHERE team_id = _attacker_id
          AND task_id = _task_id FOR NO KEY UPDATE
        INTO attacker_score;

        SELECT score
        FROM TeamTasks
        WHERE team_id = _victim_id
          AND task_id = _task_id FOR NO KEY UPDATE
        INTO victim_score;
    ELSE
        SELECT score
        FROM TeamTasks
        WHERE team_id = _victim_id
          AND task_id = _task_id FOR NO KEY UPDATE
        INTO victim_score;

        SELECT score
        FROM TeamTasks
        WHERE team_id = _attacker_id
          AND task_id = _task_id FOR NO KEY UPDATE
        INTO attacker_score;
    END IF;

    scale = 50 * sqrt(hardness);
    norm = ln(ln(hardness)) / 12;
    _attacker_delta = scale / (1 + exp((sqrt(attacker_score) - sqrt(victim_score)) * norm));
    _victim_delta = -least(victim_score, _attacker_delta);

    IF NOT inflate THEN
        _attacker_delta = least(_attacker_delta, -_victim_delta);
    END IF;

    INSERT INTO StolenFlags (attacker_id, flag_id) VALUES (_attacker_id, _flag_id);

    UPDATE TeamTasks
    SET stolen = stolen + 1,
        score  = score + _attacker_delta
    WHERE team_id = _attacker_id
      AND task_id = _task_id;

    UPDATE TeamTasks
    SET lost  = lost + 1,
        score = score + _victim_delta
    WHERE team_id = _victim_id
      AND task_id = _task_id;

    attacker_delta := _attacker_delta;
    victim_delta := _victim_delta;
    RETURN NEXT;
END;
$$ LANGUAGE plpgsql ROWS 1;


CREATE OR REPLACE FUNCTION get_first_bloods()
    RETURNS TABLE
            (
                submit_time   TIMESTAMP WITH TIME ZONE,
                attacker_name VARCHAR(255),
                task_name     VARCHAR(255),
                attacker_id   INTEGER,
                victim_id     INTEGER,
                task_id       INTEGER,
                vuln_number   INTEGER
            )
AS
$$
BEGIN
    RETURN QUERY WITH preprocess AS (SELECT DISTINCT ON (f.task_id, f.vuln_number) sf.submit_time AS submit_time,
                                                                                   sf.attacker_id AS attacker_id,
                                                                                   f.team_id      AS victim_id,
                                                                                   f.task_id      AS task_id,
                                                                                   f.vuln_number  as vuln_number
                                     FROM StolenFlags sf
                                              JOIN Flags f ON f.id = sf.flag_id
                                     ORDER BY f.task_id, f.vuln_number, sf.submit_time)
                 SELECT preprocess.submit_time AS submit_time,
                        tm.name                AS attacker_name,
                        tk.name                AS task_name,
                        preprocess.victim_id   AS victim_id,
                        tm.id                  AS attacker_id,
                        tk.id                  AS task_id,
                        preprocess.vuln_number AS vuln_number
                 FROM preprocess
                          JOIN Teams tm ON tm.id = preprocess.attacker_id
                          JOIN Tasks tk ON tk.id = preprocess.task_id
                 ORDER BY submit_time;
END;
$$ LANGUAGE plpgsql;


CREATE OR REPLACE FUNCTION fix_teamtasks()
    RETURNS VOID
AS
$$
BEGIN
    INSERT INTO TeamTasks (task_id, team_id, status, score)
    WITH product AS (
        SELECT teams.id as team_id, tasks.id as task_id, tasks.default_score as default_score
        FROM teams
                 CROSS JOIN tasks
    )
    SELECT task_id, team_id, -1, default_score
    FROM product
    ON CONFLICT (task_id, team_id) DO NOTHING;

END;
$$ LANGUAGE plpgsql;

```
Đây là ba hàm SQL được viết bằng ngôn ngữ lập trình PostgreSQL (plpgsql).

- `recalculate_rating`: Hàm này nhận vào 4 tham số: _attacker_id, _victim_id, _task_id, _flag_id. Hàm này trả về một bảng với hai cột: attacker_delta và victim_delta. Hàm này tính toán sự thay đổi điểm số của hai đội _attacker_id và _victim_id sau một cuộc tấn công thành công từ _attacker_id đến _victim_id với cờ _flag_id trong nhiệm vụ _task_id. Hàm này cũng cập nhật số cờ bị đánh cắp và số điểm của mỗi đội trong bảng TeamTasks.

- `get_first_bloods`: Hàm này không nhận tham số nào và trả về một bảng với các cột: submit_time, attacker_name, task_name, attacker_id, victim_id, task_id, vuln_number. Hàm này trả về thông tin về những lần đánh cắp cờ đầu tiên (first bloods) trong mỗi nhiệm vụ và mỗi lỗ hổng.

- `fix_teamtasks`: Hàm này không nhận tham số nào và không trả về giá trị nào. Hàm này thêm các bản ghi vào bảng TeamTasks cho mỗi cặp đội và nhiệm vụ, với trạng thái là -1 và điểm số mặc định là default_score từ bảng tasks. Nếu một bản ghi với cùng task_id và team_id đã tồn tại, hàm này sẽ không làm gì cả (DO NOTHING).
Giải thích chi tiết những câu lệnh trong file:

- `DECLARE`: Khai báo các biến sẽ được sử dụng trong hàm.

- `SELECT real_round, game_hardness, inflation FROM GameConfig WHERE id = 1 INTO _round, hardness, inflate;`: Lấy các thông số cấu hình trò chơi từ bảng GameConfig.

- `if _attacker_id < _victim_id THEN`: Để tránh deadlock, hàm này sẽ khóa đội có ID nhỏ hơn trước.

- `SELECT score FROM TeamTasks WHERE team_id = _attacker_id AND task_id = _task_id FOR NO KEY UPDATE INTO attacker_score;`: Lấy điểm số hiện tại của đội tấn công (_attacker_id) từ bảng TeamTasks.

- `SELECT score FROM TeamTasks WHERE team_id = _victim_id AND task_id = _task_id FOR NO KEY UPDATE INTO victim_score;`: Lấy điểm số hiện tại của đội bị tấn công (_victim_id) từ bảng TeamTasks.

- `scale = 50 * sqrt(hardness); norm = ln(ln(hardness)) / 12; _attacker_delta = scale / (1 + exp((sqrt(attacker_score) - sqrt(victim_score)) * norm)); _victim_delta = -least(victim_score, _attacker_delta);`: Tính toán sự thay đổi điểm số cho cả hai đội dựa trên công thức đã cho.

- `IF NOT inflate THEN _attacker_delta = least(_attacker_delta, -_victim_delta); END IF;`: Nếu inflate không được bật, _attacker_delta sẽ được giới hạn để không vượt quá _victim_delta.

- `INSERT INTO StolenFlags (attacker_id, flag_id) VALUES (_attacker_id, _flag_id);`: Cập nhật bảng StolenFlags để ghi nhận việc đội tấn công đã đánh cắp cờ.

- `UPDATE TeamTasks SET stolen = stolen + 1, score = score + _attacker_delta WHERE team_id = _attacker_id AND task_id = _task_id;`: Cập nhật điểm số và số cờ bị đánh cắp cho đội tấn công trong bảng TeamTasks.

- `UPDATE TeamTasks SET lost = lost + 1, score = score + _victim_delta WHERE team_id = _victim_id AND task_id = _task_id;`: Cập nhật điểm số và số cờ bị mất cho đội bị tấn công trong bảng TeamTasks.

- `attacker_delta := _attacker_delta; victim_delta := _victim_delta; RETURN NEXT;`: Trả về sự thay đổi điểm số cho cả hai đội (attacker_delta và victim_delta).

#### 3.3.2 File `create_tables.sql`

Có tác dụng tạo bảng trong database.

![image](https://github.com/H4lst0n/ADCTF/assets/97662987/bc1250ca-d579-44fa-844a-0bae07a943f6)
#### 3.3.3 File `drop_query.sql`

Có tác dụng xóa bảng và những function đã tồn tại trong database.

#### 3.3.4 File `init_db.py`

```
#!/usr/bin/env python3

import os
from pathlib import Path

import pytz
import yaml

from lib import models
from lib import storage

BACKEND_BASE = Path(__file__).resolve().absolute().parents[1]
SCRIPTS_DIR = BACKEND_BASE / 'scripts'
CONFIG_PATH = os.getenv('CONFIG_PATH')


def init_schema(curs):
    create_tables_path = SCRIPTS_DIR / 'create_tables.sql'
    create_tables_query = create_tables_path.read_text()
    curs.execute(create_tables_query)

    create_functions_path = SCRIPTS_DIR / 'create_functions.sql'
    create_functions_query = create_functions_path.read_text()
    curs.execute(create_functions_query)


def init_teams(config, curs):
    teams = []

    for team_conf in config:
        team_token = models.Team.generate_token()
        team = models.Team(id=None, **team_conf, token=team_token)
        team.insert(curs)
        teams.append(team)

    return teams


def init_tasks(config, game_config, curs):
    task_defaults = {
        'env_path': game_config['env_path'],
        'default_score': game_config['default_score'],
        'get_period': game_config.get('get_period', game_config['round_time']),
        'checker_type': 'hackerdom',
    }

    tasks = []

    for task_conf in config:
        for k, v in task_defaults.items():
            if k not in task_conf:
                task_conf[k] = v

        task_conf['checker'] = os.path.join(
            game_config['checkers_path'],
            task_conf['checker'],
        )

        task = models.Task(id=None, **task_conf)
        task.insert(curs)
        tasks.append(task)

    return tasks


def init_game_config(game_config, curs):
    game_config.pop('env_path', None)
    game_config.pop('default_score', None)
    game_config.pop('checkers_path', None)
    game_config.pop('get_period', None)

    tz = pytz.timezone(game_config['timezone'])
    game_config['start_time'] = tz.localize(game_config['start_time'])

    game_config['real_round'] = 0
    game_config['game_running'] = False

    # noinspection PyArgumentList
    game_config['mode'] = models.GameMode(game_config['mode'])

    game_config = models.GameConfig(id=None, **game_config)
    game_config.insert(curs)


def run():
    with open(CONFIG_PATH, 'r') as f:
        file_config = yaml.safe_load(f)

    with storage.utils.db_cursor() as (conn, curs):
        print('Initializing schema')
        init_schema(curs)

        print('Initializing teams')
        teams = init_teams(file_config['teams'], curs)

        game_defaults = {
            'checkers_path': '/checkers/',
            'env_path': '/checkers/bin/',
            'default_score': 2000.0,
            'game_hardness': 10.0,
            'inflation': True,
            'flag_lifetime': 5,
            'round_time': 60,
            'timezone': 'UTC',
            'mode': 'classic',
        }

        game_config = file_config['game']
        for k, v in game_defaults.items():
            if k not in game_config:
                game_defaults[k] = v

        print('Initializing tasks')
        tasks = init_tasks(file_config['tasks'], game_config, curs)

        data = [
            {
                'task_id': task.id,
                'team_id': team.id,
                'score': task.default_score,
                'status': -1,
            }
            for team in teams
            for task in tasks
        ]
        curs.executemany(storage.tasks.TEAMTASK_INSERT_QUERY, data)

        print('Initializing game config')
        init_game_config(game_config, curs)

        conn.commit()

    print('Initializing game state')
    storage.game.update_game_state(for_round=0)


if __name__ == '__main__':
    run()
```
File init_db.py là một script Python được sử dụng để khởi tạo cơ sở dữ liệu cho một ứng dụng game. Dưới đây là mô tả chi tiết về các hàm trong file này:

1. `init_schema(curs)`: Hàm này đọc và thực thi các câu lệnh SQL từ hai file create_tables.sql và create_functions.sql để tạo schema cho cơ sở dữ liệu.

2. `init_teams(config, curs)`: Hàm này tạo các đội chơi dựa trên cấu hình được truyền vào. Mỗi đội sẽ được tạo token và lưu vào cơ sở dữ liệu.

3. `init_tasks(config, game_config, curs)`: Hàm này tạo các nhiệm vụ dựa trên cấu hình được truyền vào. Mỗi nhiệm vụ sẽ được lưu vào cơ sở dữ liệu.

4. `init_game_config(game_config, curs)`: Hàm này tạo cấu hình game dựa trên cấu hình được truyền vào. Cấu hình game sau đó sẽ được lưu vào cơ sở dữ liệu.

run(): Hàm chính của script. Hàm này mở file cấu hình, khởi tạo schema, khởi tạo các đội, khởi tạo các nhiệm vụ, và khởi tạo cấu hình game. Sau cùng, hàm này cập nhật trạng thái game.

#### 3.3.5 File `print_tokens.py`

Đây là một script Python đơn giản được sử dụng để lấy và in ra tên và token của tất cả các đội từ cơ sở dữ liệu.
![image](https://github.com/H4lst0n/ADCTF/assets/97662987/66621f5d-e127-459b-97e5-2b3b40a5f303)

> File `reset_db.py`

```
#!/usr/bin/env python3

import time
from pathlib import Path

import psycopg2
import redis

from lib import storage

BASE_DIR = Path(__file__).absolute().resolve().parents[1]
SCRIPTS_DIR = BASE_DIR / 'scripts'


def run():
    conn = storage.utils.DBPool.get().getconn()
    curs = conn.cursor()

    create_query_path = SCRIPTS_DIR / 'drop_query.sql'
    create_query = create_query_path.read_text()
    # noinspection PyUnresolvedReferences
    try:
        curs.execute(create_query)
    except psycopg2.errors.UndefinedTable:
        pass
    else:
        conn.commit()
    finally:
        curs.close()

    while True:
        try:
            storage.utils.RedisStorage.get().flushall()
        except (redis.exceptions.ConnectionError, redis.exceptions.BusyLoadingError):
            print('[*] Redis isn\'t running, waiting...')
            time.sleep(5)
        else:
            break


if __name__ == '__main__':
    run()

```
Đây là một script Python được sử dụng để reset cơ sở dữ liệu của một ứng dụng. Dưới đây là mô tả chi tiết về các phần chính của script:

1. `import time, psycopg2, redis, storage`: Import các thư viện cần thiết.

2. `BASE_DIR = Path(__file__).absolute().resolve().parents[1]`: Định nghĩa đường dẫn tuyệt đối đến thư mục chứa script.

3. `SCRIPTS_DIR = BASE_DIR / 'scripts'`: Định nghĩa đường dẫn đến thư mục 'scripts' nằm trong thư mục chứa script.

4. `def run()`: Định nghĩa hàm run để thực hiện công việc chính của script.

5. `conn = storage.utils.DBPool.get().getconn()`: Lấy một kết nối đến cơ sở dữ liệu từ pool.

6. `curs = conn.cursor()`: Tạo một cursor từ kết nối đến cơ sở dữ liệu.

7. `create_query_path = SCRIPTS_DIR / 'drop_query.sql'`: Định nghĩa đường dẫn đến file 'drop_query.sql' nằm trong thư mục 'scripts'.

8. `create_query = create_query_path.read_text()`: Đọc nội dung của file 'drop_query.sql' và lưu vào biến create_query.

9. `curs.execute(create_query)`: Thực hiện câu truy vấn SQL từ file 'drop_query.sql'. Nếu bảng không tồn tại, một ngoại lệ psycopg2.errors.UndefinedTable sẽ được ném ra.

10. `conn.commit()`: Nếu không có ngoại lệ nào được ném ra, thì commit transaction.

11. `storage.utils.RedisStorage.get().flushall()`: Xóa tất cả dữ liệu từ Redis. Nếu Redis không chạy, một ngoại lệ sẽ được ném ra và script sẽ chờ 5 giây trước khi thử lại.

**Chúng ta sẽ đi tìm hiểu thư mục `lib`, thư mục sẽ bao gồm những file sau**:
![image](https://github.com/H4lst0n/ADCTF/assets/97662987/2223c73c-316a-4be5-8041-e4e709653d4d)

#### 3.3.6 File `/lib/config/models.py`

```
from typing import List

from pydantic import BaseModel, Field
from pydantic_settings import BaseSettings, SettingsConfigDict


class Redis(BaseSettings):
    model_config = SettingsConfigDict(env_prefix='redis_')

    host: str
    port: int
    password: str
    db: int = 0

    @property
    def url(self) -> str:
        return f'redis://:{self.password}@{self.host}:{self.port}/{self.db}'


class WebCredentials(BaseSettings):
    model_config = SettingsConfigDict(env_prefix='admin_')

    username: str
    password: str


class Database(BaseSettings):
    model_config = SettingsConfigDict(env_prefix='postgres_')

    host: str
    port: int
    user: str
    password: str
    dbname: str = Field(validation_alias='postgres_db')


class Celery(BaseModel):
    broker_url: str
    result_backend: str
    timezone: str

    worker_prefetch_multiplier: int = 1

    result_expires: int = 15 * 60
    redis_socket_timeout: int = 10
    redis_socket_keepalive: bool = True
    redis_retry_on_timeout: bool = True

    accept_content: List[str] = ['pickle', 'json']
    result_serializer: str = 'pickle'
    task_serializer: str = 'pickle'

```
Đây là một file Python có tên models.py, chứa các lớp mô hình Pydantic để định nghĩa cấu hình cho các thành phần khác nhau của ứng dụng.

1. `class Redis(BaseSettings)`: Định nghĩa cấu hình cho Redis. Các thuộc tính bao gồm host, port, password, và db. Có một phương thức url trả về URL kết nối đến Redis.

2. `class WebCredentials(BaseSettings)`: Định nghĩa cấu hình cho tài khoản quản trị web. Các thuộc tính bao gồm username và password.

3. `class Database(BaseSettings)`: Định nghĩa cấu hình cho cơ sở dữ liệu PostgreSQL. Các thuộc tính bao gồm host, port, user, password, và dbname.

4. `class Celery(BaseModel)`: Định nghĩa cấu hình cho Celery, một hệ thống xử lý tác vụ bất đồng bộ. Các thuộc tính bao gồm broker_url, result_backend, timezone, worker_prefetch_multiplier, result_expires, redis_socket_timeout, redis_socket_keepalive, redis_retry_on_timeout, accept_content, result_serializer, và task_serializer.

Các lớp mô hình này sử dụng Pydantic, một thư viện Python cho phép xác thực dữ liệu và phân tích cú pháp dữ liệu phức tạp. BaseSettings là một lớp cơ sở trong Pydantic cho phép đọc cấu hình từ biến môi trường hoặc từ file cấu hình. SettingsConfigDict cho phép tùy chỉnh cách đọc cấu hình, như tiền tố cho biến môi trường.

#### 3.3.7 File `/lib/config/getters.py`
```
import os

from lib import storage
from . import models


def get_web_credentials() -> models.WebCredentials:
    return models.WebCredentials()


def get_redis_config() -> models.Redis:
    return models.Redis()


def get_db_config() -> models.Database:
    return models.Database()


def get_broker_url() -> str:
    """Get broker url for RabbitMQ from config."""
    host = os.environ['RABBITMQ_HOST']
    user = os.environ['RABBITMQ_DEFAULT_USER']
    port = os.environ['RABBITMQ_PORT']
    password = os.environ['RABBITMQ_DEFAULT_PASS']
    vhost = os.environ['RABBITMQ_DEFAULT_VHOST']

    broker_url = f'amqp://{user}:{password}@{host}:{port}/{vhost}'
    return broker_url


def get_celery_config() -> models.Celery:
    redis_config = get_redis_config()
    redis_config.db = 1
    return models.Celery(
        broker_url=get_broker_url(),
        result_backend=redis_config.url,
        timezone=storage.game.get_current_game_config().timezone,
    )

```
- `get_web_credentials() -> models.WebCredentials`: Hàm này trả về một đối tượng WebCredentials mới. Các thông tin như tên người dùng, mật khẩu, vv. có thể được lấy từ biến môi trường hoặc từ một file cấu hình.

- `get_redis_config() -> models.Redis`: Hàm này trả về một đối tượng Redis mới. Các thông tin như host, port, vv. có thể được lấy từ biến môi trường hoặc từ một file cấu hình.

- `get_db_config() -> models.Database`: Hàm này trả về một đối tượng Database mới. Các thông tin như host, port, tên cơ sở dữ liệu, vv. có thể được lấy từ biến môi trường hoặc từ một file cấu hình.

- `get_broker_url() -> str`: Hàm này trả về URL của RabbitMQ Broker. URL này được tạo từ các biến môi trường RABBITMQ_HOST, RABBITMQ_DEFAULT_USER, RABBITMQ_PORT, RABBITMQ_DEFAULT_PASS, và RABBITMQ_DEFAULT_VHOST.

- `get_celery_config() -> models.Celery`: Hàm này trả về một đối tượng Celery mới. Cấu hình này bao gồm URL của RabbitMQ Broker, URL của backend kết quả (ở đây là Redis), và múi giờ hiện tại của game.

#### 3.3.8 File `/lib/flags/judge.py`

```
from typing import List

import eventlet

from lib import storage
from lib.models import AttackResult
from .notifier import Notifier
from .submit_monitor import SubmitMonitor


class Judge:
    def __init__(self, monitor: SubmitMonitor, logger):
        self._monitor = monitor
        self._notifier = Notifier(logger=logger)
        eventlet.spawn_n(self._monitor)
        eventlet.spawn_n(self._notifier)

    def _process_attack(self, team_id: int, flag: str) -> AttackResult:
        current_round = storage.game.get_real_round()
        ar = storage.attacks.handle_attack(
            attacker_id=team_id,
            flag_str=flag,
            current_round=current_round,
        )

        if ar.submit_ok:
            self._notifier.add(ar)
            self._monitor.inc_ok()
        else:
            self._monitor.inc_bad()

        return ar

    def process(self, team_id: int, flag: str) -> AttackResult:
        return self._process_attack(team_id, flag)

    def process_many(self, team_id: int, flags: List[str]) -> List[AttackResult]:
        return [self._process_attack(team_id, flag) for flag in flags]

```
- `class Judge`: Đây là lớp chính trong module, đại diện cho một "trọng tài" trong trò chơi CTF.

- `def __init__(self, monitor: SubmitMonitor, logger)`: Phương thức khởi tạo của lớp. Nó nhận vào một đối tượng SubmitMonitor và một logger. Nó cũng khởi tạo một đối tượng Notifier và khởi chạy SubmitMonitor và Notifier trong các green thread riêng biệt.

- `def _process_attack(self, team_id: int, flag: str) -> AttackResult`: Phương thức riêng để xử lý một cuộc tấn công từ một đội. Nó nhận vào ID của đội và một cờ, sau đó gọi storage.attacks.handle_attack để xử lý cuộc tấn công. Nếu cuộc tấn công thành công, nó sẽ thêm kết quả vào Notifier và tăng số lượng cuộc tấn công thành công trong SubmitMonitor. Nếu không, nó sẽ tăng số lượng cuộc tấn công không thành công trong SubmitMonitor.

- `def process(self, team_id: int, flag: str) -> AttackResult`: Phương thức công khai để xử lý một cuộc tấn công. Nó chỉ đơn giản gọi _process_attack.

- `def process_many(self, team_id: int, flags: List[str]) -> List[AttackResult]`: Phương thức công khai để xử lý nhiều cuộc tấn công cùng một lúc. Nó nhận vào ID của đội và một danh sách các cờ, sau đó gọi _process_attack cho mỗi cờ và trả về danh sách các kết quả.

#### 3.3.9 File `/lib/flags/notifier.py`

```
from logging import Logger

import eventlet
from eventlet.queue import LightQueue, Empty, Full

from lib import storage
from lib.models import AttackResult


class Notifier:
    def __init__(self, logger: Logger):
        self._logger = logger
        self._q = LightQueue(maxsize=1000)

        # ensure no one is writing to the same broker connection concurrently
        self._sio = storage.utils.SIOManager.create(write_only=True)

    def _process(self, ar: AttackResult):
        flag_data = ar.get_flag_notification()
        self._logger.debug('Sending notification with %s', flag_data)
        self._sio.emit(
            event='flag_stolen',
            data={'data': flag_data},
            namespace='/live_events',
        )

    def add(self, ar: AttackResult) -> bool:
        try:
            self._q.put_nowait(ar)
            return True
        except Full:
            return False

    def __call__(self) -> None:
        while True:
            try:
                ar = self._q.get(block=True, timeout=3)
            except Empty:
                eventlet.sleep(0.5)
            else:
                self._process(ar)

```
Notifier để gửi thông báo khi một cờ bị đánh cắp trong một trò chơi Capture The Flag (CTF).

- `class Notifier`: Đây là lớp chính trong module, đại diện cho một "thông báo" trong trò chơi CTF.

- `def __init__(self, logger: Logger)`: Phương thức khởi tạo của lớp. Nó nhận vào một logger và khởi tạo một hàng đợi nhẹ (LightQueue) với kích thước tối đa là 1000. Nó cũng khởi tạo một đối tượng SIOManager chỉ cho phép ghi.

- `def _process(self, ar: AttackResult)`: Phương thức riêng để xử lý một kết quả tấn công. Nó nhận vào một đối tượng AttackResult, lấy thông tin cờ từ đó, và gửi thông báo với thông tin cờ đó qua SIOManager.

- `def add(self, ar: AttackResult) -> bool`: Phương thức công khai để thêm một kết quả tấn công vào hàng đợi. Nếu hàng đợi đầy, nó sẽ trả về False. Nếu không, nó sẽ thêm kết quả tấn công vào hàng đợi và trả về True.

- `def __call__(self) -> None`: Phương thức này được gọi khi một đối tượng Notifier được gọi như một hàm. Nó sẽ lấy một kết quả tấn công từ hàng đợi (hoặc chờ nếu hàng đợi trống) và xử lý kết quả tấn công đó.

#### 3.3.10 File `/lib/helpers/checkers.py`

```
from logging import Logger
from typing import Optional

from lib import models
from lib.helpers.commands import run_generic_command
from lib.models import Action


class CheckerRunner:
    """Helper class """

    team: models.Team
    task: models.Task
    flag: Optional[models.Flag]

    def __init__(
            self,
            team: models.Team,
            task: models.Task,
            logger: Logger,
            flag: Optional[models.Flag] = None,
    ):
        self.team = team
        self.task = task
        self.logger = logger
        self.flag = flag

    def check(self) -> models.CheckerVerdict:
        return self._check_as_process()

    def put(self) -> models.CheckerVerdict:
        return self._put_as_process()

    def get(self) -> models.CheckerVerdict:
        return self._get_as_process()

    def _check_as_process(self) -> models.CheckerVerdict:
        """Check implementation using subprocess calling"""
        check_command = [
            self.task.checker,
            'check',
            self.team.ip,
        ]

        return run_generic_command(
            command=check_command,
            action=Action.CHECK,
            task=self.task,
            team=self.team,
            logger=self.logger,
        )

    def _put_as_process(self) -> models.CheckerVerdict:
        """Put implementation using subprocess calling"""
        assert self.flag is not None, 'Can only be called when flag is passed'

        put_command = [
            self.task.checker,
            'put',
            self.team.ip,
            self.flag.private_flag_data,
            self.flag.flag,
            str(self.flag.vuln_number),
        ]

        return run_generic_command(
            command=put_command,
            action=Action.PUT,
            task=self.task,
            team=self.team,
            logger=self.logger,
        )

    def _get_as_process(self) -> models.CheckerVerdict:
        """Get implementation using subprocess calling"""
        assert self.flag is not None, 'Can only be called when flag is passed'

        get_command = [
            self.task.checker,
            'get',
            self.team.ip,
            self.flag.private_flag_data,
            self.flag.flag,
            str(self.flag.vuln_number),
        ]

        return run_generic_command(
            command=get_command,
            action=Action.GET,
            task=self.task,
            team=self.team,
            logger=self.logger,
        )

```
Lớp CheckerRunner để thực hiện các hành động kiểm tra, đặt và lấy cờ trong một trò chơi Capture The Flag (CTF).

- `class CheckerRunner`: Đây là lớp chính trong module, đại diện cho một "runner" để thực hiện các hành động kiểm tra, đặt và lấy cờ.

- `def __init__(self, team: models.Team, task: models.Task, logger: Logger, flag: Optional[models.Flag] = None)`: Phương thức khởi tạo của lớp. Nó nhận vào một đội, một nhiệm vụ, một logger, và một cờ (có thể là None).

- `def check(self) -> models.CheckerVerdict`: Phương thức này thực hiện hành động kiểm tra bằng cách gọi _check_as_process.

- `def put(self) -> models.CheckerVerdict`: Phương thức này thực hiện hành động đặt cờ bằng cách gọi _put_as_process.

- `def get(self) -> models.CheckerVerdict`: Phương thức này thực hiện hành động lấy cờ bằng cách gọi _get_as_process.

- `def _check_as_process(self) -> models.CheckerVerdict`: Phương thức này tạo ra một lệnh kiểm tra và chạy nó bằng cách sử dụng run_generic_command.

- `def _put_as_process(self) -> models.CheckerVerdict`: Phương thức này tạo ra một lệnh đặt cờ và chạy nó bằng cách sử dụng run_generic_command. Nó yêu cầu một cờ phải được truyền vào khi khởi tạo CheckerRunner.

- `def _get_as_process(self) -> models.CheckerVerdict`: Phương thức này tạo ra một lệnh lấy cờ và chạy nó bằng cách sử dụng run_generic_command. Nó cũng yêu cầu một cờ phải được truyền vào khi khởi tạo CheckerRunner.

#### 3.3.11 File `/lib/storage/attacks.py`

```
from lib import models, storage
from lib.helpers import exceptions
from lib.helpers.exceptions import FlagExceptionEnum
from lib.models.types import TaskStatus
from lib.storage import utils, game
from lib.storage.keys import CacheKeys


def get_attack_data() -> str:
    """Get public flag ids for tasks that provide them, as json string."""
    with utils.redis_pipeline(transaction=False) as pipe:
        attack_data, = pipe.get(CacheKeys.attack_data()).execute()
    return attack_data or 'null'


def handle_attack(
        attacker_id: int, flag_str: str, current_round: int
) -> models.AttackResult:
    """
    Main routine for attack validation & state change.

    Checks flag, locks team for update, calls rating recalculation,
    then publishes redis message about stolen flag.

    :param attacker_id: id of the attacking team
    :param flag_str: flag to be checked
    :param current_round: round of the attack

    :return: attacker rating change
    """

    result = models.AttackResult(attacker_id=attacker_id)

    try:
        if current_round == -1:
            raise FlagExceptionEnum.GAME_NOT_AVAILABLE

        flag = storage.flags.get_flag_by_str(
            flag_str=flag_str,
            current_round=current_round,
        )
        if flag is None:
            raise FlagExceptionEnum.FLAG_INVALID
        if flag.team_id == attacker_id:
            raise FlagExceptionEnum.FLAG_YOUR_OWN

        game_config = game.get_current_game_config()
        if current_round - flag.round > game_config.flag_lifetime:
            raise FlagExceptionEnum.FLAG_TOO_OLD

        if game_config.volga_attacks_mode:
            teamtask = storage.tasks.get_latest_teamtask(
                team_id=attacker_id,
                task_id=flag.task_id,
            )
            # Status is a string from redis stream.
            if not teamtask or teamtask['status'] != str(TaskStatus.UP.value):
                raise FlagExceptionEnum.SERVICE_IS_DOWN

        result.victim_id = flag.team_id
        result.task_id = flag.task_id
        success = storage.flags.try_add_stolen_flag(
            flag=flag,
            attacker=attacker_id,
            current_round=current_round,
        )
        if not success:
            raise FlagExceptionEnum.FLAG_ALREADY_STOLEN

    except exceptions.FlagSubmitException as e:
        result.submit_ok = False
        result.message = str(e)

    else:
        result.submit_ok = True

        with utils.db_cursor() as (conn, curs):
            curs.callproc(
                "recalculate_rating",
                (
                    attacker_id,
                    flag.team_id,
                    flag.task_id,
                    flag.id,
                ),
            )
            attacker_delta, victim_delta = curs.fetchone()
            conn.commit()

        result.attacker_delta = attacker_delta
        result.victim_delta = victim_delta
        result.message = f'Flag accepted! Earned {attacker_delta} flag points!'

    return result

```
Đây là một module Python có tên attacks.py, chứa hai hàm chính: get_attack_data và handle_attack.

1. get_attack_data() -> str: Hàm này trả về các ID cờ công khai cho các nhiệm vụ cung cấp chúng dưới dạng chuỗi JSON. Nó sử dụng một pipeline Redis để lấy dữ liệu từ khóa CacheKeys.attack_data().

2. handle_attack(attacker_id: int, flag_str: str, current_round: int) -> models.AttackResult: Hàm này là trình tự chính để xác thực cuộc tấn công và thay đổi trạng thái. Nó kiểm tra cờ, khóa đội để cập nhật, gọi việc tính toán lại điểm số, sau đó xuất thông báo Redis về việc cờ đã bị đánh cắp.
  - Nếu vòng hiện tại là -1, nó sẽ ném ra một ngoại lệ FlagExceptionEnum.GAME_NOT_AVAILABLE.
  - Nó lấy cờ bằng cách sử dụng chuỗi cờ và vòng hiện tại. Nếu cờ không tồn tại, nó ném ra một ngoại lệ FlagExceptionEnum.FLAG_INVALID. Nếu cờ thuộc về đội tấn công, nó ném ra một ngoại lệ FlagExceptionEnum.FLAG_YOUR_OWN.
  - Nó kiểm tra xem cờ có quá cũ không dựa trên thời gian sống của cờ từ cấu hình trò chơi hiện tại.
  - Nếu trò chơi đang ở chế độ tấn công Volga, nó kiểm tra xem nhiệm vụ của đội tấn công có đang hoạt động không. Nếu không, nó ném ra một ngoại lệ FlagExceptionEnum.SERVICE_IS_DOWN.
  - Nó cố gắng thêm cờ đã bị đánh cắp vào cơ sở dữ liệu. Nếu không thành công, nó ném ra một ngoại lệ FlagExceptionEnum.FLAG_ALREADY_STOLEN.
  - Nếu không có ngoại lệ nào được ném ra, nó sẽ tính toán lại điểm số cho đội tấn công và nạn nhân, sau đó cập nhật kết quả tấn công.
  - Hàm handle_attack trả về một đối tượng models.AttackResult mô tả kết quả của cuộc tấn công.

#### 3.3.12 File `/lib/storage/flags.py`
```
from collections import defaultdict
from typing import Optional, List, Dict, DefaultDict, Union

from lib import models
from lib.helpers.cache import cache_helper
from lib.storage import caching, game, utils
from lib.storage.keys import CacheKeys

_GET_UNEXPIRED_FLAGS_QUERY = """
SELECT t.ip, f.task_id, f.public_flag_data FROM Flags f
INNER JOIN Teams t on f.team_id = t.id
WHERE f.round >= %s AND f.task_id IN %s
"""

_GET_RANDOM_ROUND_FLAG_QUERY = """
SELECT id FROM Flags
WHERE round = %(round)s AND team_id = %(team_id)s AND task_id = %(task_id)s
ORDER BY RANDOM()
LIMIT 1
"""


def try_add_stolen_flag(flag: models.Flag, attacker: int, current_round: int) -> bool:
    """
    Flag validation function.

    Checks that flag is valid for current round, adds it to cache for team,
    atomically checking if it's already submitted by the attacker.

    :param flag: Flag model instance
    :param attacker: attacker team id
    :param current_round: current round
    """
    stolen_key = CacheKeys.team_stolen_flags(attacker)
    with utils.redis_pipeline(transaction=True) as pipe:
        # optimization of redis request count
        cached_stolen = pipe.exists(stolen_key).execute()

        if not cached_stolen:
            cache_helper(
                pipeline=pipe,
                cache_key=stolen_key,
                cache_func=caching.cache_last_stolen,
                cache_args=(attacker, current_round, pipe),
            )

        is_new, = pipe.sadd(stolen_key, flag.id).execute()
    return bool(is_new)


def add_flag(flag: models.Flag) -> models.Flag:
    """
    Inserts a newly generated flag into the database and cache.

    :param flag: Flag model instance to be inserted
    :returns: flag with set "id" field
    """

    with utils.db_cursor() as (conn, curs):
        flag.insert(curs)
        conn.commit()

    game_config = game.get_current_game_config()
    expires = game_config.flag_lifetime * game_config.round_time * 2

    with utils.redis_pipeline(transaction=True) as pipe:
        pipe.set(CacheKeys.flag_by_id(flag.id), flag.to_json(), ex=expires)
        pipe.set(CacheKeys.flag_by_str(flag.flag), flag.to_json(), ex=expires)
        pipe.execute()

    return flag


def get_flag_by_field(
        name: str,
        value: Union[str, int],
        current_round: int,
) -> Optional[models.Flag]:
    """
    Get flag by generic field.

    :param name: field name to ask cache for
    :param value: value of the field "field_name" to filter on
    :param current_round: current round
    :returns: Flag model instance with flag.field_name == field_value or None
    """
    cached_key = CacheKeys.flags_cached()
    with utils.redis_pipeline(transaction=True) as pipe:
        cached, = pipe.exists(cached_key).execute()
        if not cached:
            cache_helper(
                pipeline=pipe,
                cache_key=cached_key,
                cache_func=caching.cache_last_flags,
                cache_args=(current_round, pipe),
            )

        flag_json, = pipe.get(CacheKeys.flag_by_field(name, value)).execute()

    if not flag_json:
        return None

    flag = models.Flag.from_json(flag_json)

    return flag


def get_flag_by_str(flag_str: str, current_round: int) -> Optional[models.Flag]:
    """
    Get flag by its string value.

    :param flag_str: flag value
    :param current_round: current round
    :returns: Flag model instance or None
    """
    return get_flag_by_field(
        name='str', value=flag_str, current_round=current_round,
    )


def get_flag_by_id(flag_id: int, current_round: int) -> Optional[models.Flag]:
    """
    Get flag by its id value.

    :param flag_id: flag id
    :param current_round: current round
    :return: Flag model instance or None
    """
    return get_flag_by_field(
        name='id', value=flag_id, current_round=current_round,
    )


def get_random_round_flag(
        team_id: int,
        task_id: int,
        from_round: int,
        current_round: int) -> Optional[models.Flag]:
    """
    Get random flag for team generated for specified round and task.

    :param team_id: team id
    :param task_id: task id
    :param from_round: round to fetch flag for
    :param current_round: current round
    :returns: Flag mode instance or None if no flag from rounds exist
    """
    with utils.db_cursor() as (_, curs):
        curs.execute(
            _GET_RANDOM_ROUND_FLAG_QUERY,
            {
                'round': from_round,
                'team_id': team_id,
                'task_id': task_id,
            }
        )
        result = curs.fetchone()

    if not result:
        return None
    return get_flag_by_id(result[0], current_round)


def get_attack_data(
        current_round: int,
        tasks: List[models.Task],
) -> Dict[str, DefaultDict[int, List[str]]]:
    """
    Get unexpired flags for round.

    :returns: flags in format {task.name: {team.ip: [flag.public_data]}}
    """
    task_ids = tuple(task.id for task in tasks)
    task_names = {task.id: task.name for task in tasks}

    config = game.get_current_game_config()
    need_round = current_round - config.flag_lifetime

    if task_ids:
        with utils.db_cursor() as (_, curs):
            curs.execute(_GET_UNEXPIRED_FLAGS_QUERY, (need_round, task_ids))
            flags = curs.fetchall()
    else:
        flags = []

    data: Dict[str, DefaultDict[int, List[str]]] = {
        task_names[task_id]: defaultdict(list) for task_id in task_ids
    }
    for flag in flags:
        ip, task_id, flag_data = flag
        data[task_names[task_id]][ip].append(flag_data)

    return data

```
`flags.py`, chứa một số hàm liên quan đến việc xử lý và quản lý các cờ trong một trò chơi.

- `try_add_stolen_flag(flag: models.Flag, attacker: int, current_round: int) -> bool`: Hàm này kiểm tra xem cờ có hợp lệ cho vòng hiện tại hay không, thêm nó vào bộ nhớ cache cho đội, kiểm tra xem nó đã được nộp bởi kẻ tấn công hay chưa.

- `add_flag(flag: models.Flag) -> models.Flag`: Hàm này chèn một cờ mới được tạo vào cơ sở dữ liệu và bộ nhớ cache.

- `get_flag_by_field(name: str, value: Union[str, int], current_round: int) -> Optional[models.Flag]`: Hàm này lấy cờ bằng cách sử dụng tên trường và giá trị tương ứng.

- `get_flag_by_str(flag_str: str, current_round: int) -> Optional[models.Flag]`: Hàm này lấy cờ bằng cách sử dụng giá trị chuỗi của cờ.

- `get_flag_by_id(flag_id: int, current_round: int) -> Optional[models.Flag]`: Hàm này lấy cờ bằng cách sử dụng ID của cờ.

- `get_random_round_flag(team_id: int, task_id: int, from_round: int, current_round: int) -> Optional[models.Flag]`: Hàm này lấy một cờ ngẫu nhiên cho đội được tạo cho vòng và nhiệm vụ cụ thể.

- `get_attack_data(current_round: int, tasks: List[models.Task]) -> Dict[str, DefaultDict[int, List[str]]]`: Hàm này lấy các cờ chưa hết hạn cho vòng. Nó trả về các cờ theo định dạng {task.name: {team.ip: [flag.public_data]}}.

Tất cả các hàm này đều sử dụng các hàm trợ giúp từ các module khác như models, cache_helper, caching, game, và utils để thực hiện các tác vụ cụ thể như truy vấn cơ sở dữ liệu, làm việc với bộ nhớ cache, và xử lý các mô hình dữ liệu.

**Chúng ta sẽ đi tìm hiểu thư mục `services`, thư mục sẽ bao gồm những file sau**:

#### 3.3.13 Với thư mục `/services/admin/`

Source code sẽ có những chức năng chủ yếu như sau:
- Thực hiện đăng nhập:
  ![image](https://github.com/H4lst0n/ADCTF/assets/97662987/bcb18bcd-cee5-42b9-9bb4-822b97832acb)
- Thực hiện thay đổi và tạo, xóa Team và Task:
  ![image](https://github.com/H4lst0n/ADCTF/assets/97662987/4c1ed390-29e4-48d1-94d3-e0fe70853010)

#### 3.3.14 Với thư mục `/services/api/views.py`

```
from flask import Blueprint
from flask import jsonify, make_response

from lib import storage

client_bp = Blueprint('client_api', __name__)


@client_bp.route('/teams/')
def get_teams():
    teams = [team.to_dict_for_participants() for team in storage.teams.get_teams()]
    return jsonify(teams)


@client_bp.route('/tasks/')
def get_tasks():
    tasks = [task.to_dict_for_participants() for task in storage.tasks.get_tasks()]

    return jsonify(tasks)


@client_bp.route('/config/')
def get_game_config():
    cfg = storage.game.get_current_game_config().to_dict()
    return jsonify(cfg)


@client_bp.route('/attack_data/')
def serve_attack_data():
    attack_data = storage.attacks.get_attack_data()
    response = make_response(attack_data)
    response.headers['Content-Type'] = 'application/json'
    return response


@client_bp.route('/teams/<int:team_id>/')
def get_team_history(team_id):
    teamtasks = storage.tasks.get_teamtasks_for_team(team_id)
    teamtasks = storage.tasks.filter_teamtasks_for_participants(teamtasks)
    return jsonify(teamtasks)


@client_bp.route('/ctftime/')
def get_ctftime_scoreboard():
    standings = storage.game.construct_ctftime_scoreboard()
    if not standings:
        return jsonify({'error': 'not available'}), 400
    return jsonify(standings)


@client_bp.route('/health/')
def health_check():
    return jsonify({'status': 'ok'})

```
Đây là một tệp Python chứa các tuyến đường API cho một ứng dụng Flask. Mỗi hàm được trang trí bởi @client_bp.route() tương ứng với một tuyến đường API khác nhau.

- `get_teams()`: Trả về danh sách các đội trong trò chơi dưới dạng JSON. Mỗi đội được biểu diễn dưới dạng từ điển.

- `get_tasks()`: Trả về danh sách các nhiệm vụ trong trò chơi dưới dạng JSON. Mỗi nhiệm vụ được biểu diễn dưới dạng từ điển.

- `get_game_config()`: Trả về cấu hình hiện tại của trò chơi dưới dạng JSON.

- `serve_attack_data()`: Trả về dữ liệu tấn công dưới dạng JSON. Dữ liệu này có thể bao gồm thông tin về các cuộc tấn công gần đây hoặc các cờ đã bị đánh cắp.

- `get_team_history(team_id)`: Trả về lịch sử của một đội cụ thể dưới dạng JSON. Lịch sử này có thể bao gồm các nhiệm vụ mà đội đã hoàn thành hoặc các cờ mà họ đã đánh cắp.

- `get_ctftime_scoreboard()`: Trả về bảng xếp hạng CTFtime dưới dạng JSON. Nếu bảng xếp hạng không khả dụng, nó sẽ trả về một lỗi.

- `health_check()`: Trả về trạng thái sức khỏe của ứng dụng. Nếu ứng dụng đang hoạt động, nó sẽ trả về {'status': 'ok'}.

Mỗi tuyến đường API này có thể được gọi bằng một yêu cầu HTTP GET đến URL tương ứng.


## 4. Build Services

Đây là các services chúng ta sẽ build và khởi chạy nó trên server. Chúng ta sẽ dựa trên github `https://github.com/C4T-BuT-S4D/stay-home-ctf-2022/tree/master`.
<br>
<br>
<br>
<br>
| Service | Language | Checker | Sploits | Authors |
|---------|----------|---------|---------|---------|
| **[5Go](services/5Go/)** | Rust & Go | [Checker](checkers/5Go/) | [Sploits](sploits/5Go/) | [@pomo_mondreganto](https://github.com/pomo-mondreganto) |
| **[kuar](services/kuar/)** | C++ | [Checker](checkers/kuar/) | [Sploits](sploits/kuar/) | [@revker](https://github.com/revervand) |
| **[modelrna](services/modelrna/)** | Python | [Checker](checkers/modelrna/) | [Sploits](sploits/modelrna/) | [@jnovikov](https://github.com/jnovikov) |
| **[sputnik_v8](services/sputnik_v8/)** | NodeJS | [Checker](checkers/sputnik_v8/) | [Sploits](sploits/sputnik_v8/) | [@iwalainz](https://github.com/iwalainz) |
| **[vacc_ex](services/vacc_ex/)** | Java | [Checker](checkers/vacc_ex/) | [Sploits](sploits/vacc_ex/) | [@jnovikov](https://github.com/jnovikov) & [@iwalainz](https://github.com/iwalainz) |
| **[virush](services/virush/)** | Bash | [Checker](checkers/virush/) | [Sploits](sploits/virush/) | [@keltecc](https://github.com/keltecc) |



