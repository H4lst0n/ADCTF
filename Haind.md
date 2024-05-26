# 1. Control.py

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

## 1.1. init.py
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

## 1.2. models.py
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

## 1.3. options.py
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

## 1.4. constants.py
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

## 1.5. utils.py
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
