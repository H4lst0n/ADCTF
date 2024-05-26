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

### CLI

#### 1. CLI Control.py

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

#### 2. CLI init.py
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

#### 3. CLI models.py
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

#### 4. CLI options.py
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

#### 5. CLI constants.py
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

#### 6. CLI utils.py
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

### Checker Bot
#### 1. Cấu hình Checker Bot

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

#### 2 Thư mục Checkers
Thư mục checkers ở thư mục gốc của dự án (chứa tất cả các thư mục checker) được khuyến nghị có cấu trúc như sau:

    ```
    checkers:
      - requirements.txt  <-- yêu cầu tự động cài đặt (bằng pip) yêu cầu kết hợp của tất cả các checkers (phải có)
      - task1:
          - checker.py  <-- tệp thực thi (o+rx)
      - task2:
          - checker.py  <-- tệp thực thi (o+rx)
    ```


#### 3. Cấu trúc một Checker cơ bản ( dựa trên 1 challenges trong services để phân tích vì file checker này sẽ được sửa dụng ở cả 2 bên là services và forcad)
Cấu trúc thư mục Checker

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


## 3. Build Services

Đây là các services chúng ta sẽ build và khởi chạy nó trên server. Chúng ta sẽ dựa trên github `https://github.com/C4T-BuT-S4D/stay-home-ctf-2022/tree/master`.
<br>
<br>
<br>
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

`Hiện tại chúng em vẫn đang bị lỗi tại đây và không thể tiến hành xây dựng tiếp được.`

