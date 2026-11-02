# GitHub и Java — настройка компьютера

Настройте Git, GitHub CLI и Java, затем переходите к [заданию](github-java-m3-assignment.md). Для университетских компьютеров используйте раздел Windows 10; для личного ноутбука выберите свою систему.

## 1. Аккаунт и подготовка

1. Откройте https://github.com, создайте аккаунт, подтвердите почту. Если аккаунт уже есть, используйте его.
2. Включите двухфакторную защиту в Settings → Password and authentication.
3. Сообщите преподавателю **логин**, не пароль и не токен. ФИО не является логином GitHub.
4. Примите приглашение в свой именной репозиторий с правом Admin. Затем примите приглашения в репозитории, где вас выбрали разработчиком или ревьюером.
5. На университетском ПК найдите личный сохраняемый диск и проверьте предустановленные программы по разделу 2. На собственном ноутбуке установите необходимые программы. После установки заново откройте терминал.
6. Используйте редактор кода (например, уже установленный VS Code). Сохраняйте файлы UTF-8 с указанными расширениями, без дополнительного `.txt`.

Git — программа на компьютере, GitHub — сайт, commit — локальное сохранение изменений, push — отправка коммитов на сервер. Pull Request — запрос на включение ветки в main; Issue — описание задачи.

## 2. Git, gh и компилятор Java

### Университетский компьютер: личный диск и новая сессия

На компьютерах университета используйте предустановленные Git, GitHub CLI (`gh`) и IDE. Не выполняйте команды установки ниже: установка ПО и недостающего JDK/компилятора — задача преподавателя или администратора. IntelliJ IDEA может быть установлена отдельно от JDK; проверьте доступный Project SDK.

После входа под университетским логином часть настроек профиля может восстанавливаться. Точный механизм уточняется у администратора. Не рассчитывайте, что Рабочий стол, Downloads, папка пользователя или произвольная папка на C: сохранятся. Исходники, `.git` и проекты храните на своём сохраняемом диске.

Откройте «Этот компьютер», найдите именно личный диск с вашим именем. Буква может различаться: Z: ниже — только пример. Уточните у преподавателя, какой диск сохраняется после выхода; чужой или общий диск не выбирайте. Посмотреть подключённые диски:

PowerShell:

```powershell
Get-PSDrive -PSProvider FileSystem | Select-Object Name,Root,DisplayRoot
net use
```

CMD:

```bat
net use
echo %HOMEDRIVE%
echo %HOMESHARE%
```

После подтверждения диска создайте на нём папку `github`. В примерах замените Z: своей буквой. Выполняйте этот блок в каждом новом терминале после входа, до `git config --global` и clone:

PowerShell:

```powershell
$UniversityGithub = 'Z:\github'
if (-not (Test-Path 'Z:\')) { throw 'Личный диск не подключён: уточните букву' }
New-Item -ItemType Directory -Force $UniversityGithub | Out-Null
Set-Location $UniversityGithub
$env:GIT_CONFIG_GLOBAL = Join-Path $UniversityGithub '.gitconfig'
git --version
```

CMD (если диск недоступен, остановитесь и уточните букву):

```bat
dir Z:\
set "UniversityGithub=Z:\github"
if not exist "%UniversityGithub%" mkdir "%UniversityGithub%"
cd /d "%UniversityGithub%"
set "GIT_CONFIG_GLOBAL=%UniversityGithub%\.gitconfig"
git --version
```

`GIT_CONFIG_GLOBAL` указывает Git на ваш файл настроек на личном диске. Поэтому следующие команды `git config --global` сохранят имя, почту и учебный прокси там. Сама переменная действует только в текущем терминале: после нового входа или открытия другого терминала задайте её снова. В терминале IntelliJ выполните тот же блок. На личном ноутбуке этот блок не нужен: используйте обычный файл настроек Git.

Не записывайте пароль, токен или пароль прокси в `.gitconfig`, URL или файл на сетевом диске. Вход GitHub через браузер и хранилище учётных данных может потребоваться повторить после новой сессии. Откройте проект IDE из папки личного диска, при необходимости заново выберите SDK. Если Git, gh или компилятор не найден, сообщите преподавателю; не меняйте системные настройки и не устанавливайте ПО самостоятельно.

### Зачем нужен GitHub CLI и где его устанавливать

Для этого занятия установите **Git и GitHub CLI (`gh`) и на Windows, и на Linux**. Git хранит код и коммиты, gh помогает войти в GitHub через браузер и подключить этот вход к Git. GitHub CLI не заменяет Git и JDK.

На университетском ПК сначала проверьте `git --version`, `gh --version`, `javac -version`. Если программы нет или требуются права администратора, её устанавливает администратор/преподаватель. Команды установки ниже выполняйте на личном ноутбуке или с разрешения администратора.

### Установка на личном ноутбуке

Следующие команды относятся к собственному компьютеру. На университетском ПК переходите к проверке и данным автора.

### Windows: PowerShell

```powershell
winget install --id Git.Git -e
winget install --id GitHub.cli -e
winget install --id EclipseAdoptium.Temurin.21.JDK -e
```

### Windows: CMD

```bat
winget install --id Git.Git -e
winget install --id GitHub.cli -e
winget install --id EclipseAdoptium.Temurin.21.JDK -e
```

### Linux (Ubuntu/Debian)

```sh
sudo apt update
sudo apt install git gh openjdk-21-jdk
```

Если пакет `gh` не найден в подключённых репозиториях Ubuntu/Debian, используйте [официальную инструкцию GitHub CLI для Linux](https://github.com/cli/cli/blob/trunk/docs/install_linux.md#debian). Не подменяйте пакет случайной программой с похожим названием.

Для ALT Linux не выполняйте Ubuntu-команды вслепую: пакет и доступный репозиторий для gh уточните у администратора. GitHub CLI существует для Linux; конкретная установка зависит от дистрибутива.

Для других дистрибутивов используйте их пакетный менеджер.

### macOS

```sh
xcode-select --install
brew install --cask temurin@21
brew install gh
```

На macOS установка Command Line Tools даёт Git. Для команды brew нужен заранее установленный Homebrew; альтернативно скачайте Temurin JDK 21 с https://adoptium.net. Если winget недоступен, установите Git с https://git-scm.com/downloads, GitHub CLI через официальный Windows-установщик `.msi` на https://github.com/cli/cli/releases/latest и JDK с https://adoptium.net. Выберите MSI для архитектуры своего Windows-компьютера. Если установка запрещена, обратитесь к администратору. После установки заново откройте терминал и проверьте `git --version`, `gh --version`, `javac -version`.

### Проверка и данные автора

Команды одинаковы в PowerShell, CMD и Linux/macOS:

```sh
git --version
gh --version
javac -version
java -version
git config --global user.name "Фамилия Имя"
git config --global user.email "ВАША-ПОЧТА-GITHUB"
git config --global init.defaultBranch main
git config --global core.quotepath false
```

Указывайте свою почту GitHub или noreply-адрес из Settings → Emails. Имя автора коммита не выдаёт права в репозитории.

## 3. Если Git не подключается: Windows 10 и прокси

Этот раздел выполняйте **до clone**, если сеть университета требует прокси или появляется timeout. Открытие GitHub в браузере ещё не означает, что Git использует те же настройки. Прокси браузера/Windows, WinHTTP, переменные окружения и настройки Git могут различаться.

Сначала проверьте сеть без добавления новых настроек. Одинаково в PowerShell и CMD:

```sh
git ls-remote --heads https://github.com/git/git.git
```

Это публичный проект Git: проверка не требует доступа к учебному репозиторию. Если появились хеши веток, переходите к clone своего проекта; прокси без причины не добавляйте. Команда использует уже действующие настройки Git и окружения. При timeout или ошибке подключения выполняйте диагностику ниже. При `Repository not found` у приватного учебного проекта проверьте HTTPS-адрес, вход нужным аккаунтом и принятие приглашения; повторная настройка прокси сама по себе прав не выдаёт. Для проверки сети используйте именно публичный `git/git`, а не чужой приватный учебный репозиторий.

### Быстрое подключение в университете

Если проверка публичного репозитория ниже не работает напрямую, используйте проверенный прокси. Выполните в текущем терминале перед `gh auth login`, clone и push.

PowerShell:

```powershell
git -c http.proxy="http://proxy.kubsu.ru:8080" ls-remote --heads https://github.com/git/git.git
git config --global http.https://github.com.proxy "http://proxy.kubsu.ru:8080"
$env:HTTP_PROXY = 'http://proxy.kubsu.ru:8080'
$env:HTTPS_PROXY = 'http://proxy.kubsu.ru:8080'
```

CMD:

```bat
git -c http.proxy="http://proxy.kubsu.ru:8080" ls-remote --heads https://github.com/git/git.git
git config --global http.https://github.com.proxy "http://proxy.kubsu.ru:8080"
set "HTTP_PROXY=http://proxy.kubsu.ru:8080"
set "HTTPS_PROXY=http://proxy.kubsu.ru:8080"
```

Настройка `git config` относится к Git; gh не читает её как собственную настройку прокси. Поэтому для входа gh через университетский прокси задаются также `HTTP_PROXY` и `HTTPS_PROXY`. Переменные действуют только в этом терминале: после нового входа в Windows или нового терминала повторите их. На личном ноутбуке дома сначала проверьте прямое подключение, уберите добавленные университетские настройки, если сеть работает без них.

Ниже сохранены команды диагностики и отмены настроек. Если быстрый способ не сработал, используйте их и передайте преподавателю точную ошибку без секретов.

### 3.1. Получить текущие настройки

Выберите блок для своей оболочки. Команды ниже читают настройки; ничего не меняют. Вывод может содержать пароль, поэтому перед отправкой преподавателю замажьте учётные данные.

**PowerShell (Windows):**

```powershell
netsh winhttp show proxy
Get-ItemProperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings' | Select-Object ProxyEnable,ProxyServer,ProxyOverride,AutoConfigURL
Get-ChildItem Env: | Where-Object Name -Match '^(HTTP_PROXY|HTTPS_PROXY|ALL_PROXY|NO_PROXY)$'
git config --show-origin --get-regexp '.*proxy.*'
git config --get-urlmatch http.proxy https://github.com
Test-NetConnection github.com -Port 443
```

**CMD (Windows):**

```bat
netsh winhttp show proxy
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings" /v ProxyEnable
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings" /v ProxyServer
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings" /v ProxyOverride
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings" /v AutoConfigURL
set | findstr /I "HTTP_PROXY HTTPS_PROXY ALL_PROXY NO_PROXY"
git config --show-origin --get-regexp ".*proxy.*"
git config --get-urlmatch http.proxy https://github.com
curl.exe -I --connect-timeout 15 https://github.com
```

Как читать результат:

- `ProxyEnable=1` и `ProxyServer` вида `proxy.kubsu.ru:8080` дают адрес и порт. Значение `http=...;https=...` задаёт отдельные серверы для разных протоколов.
- `Direct access` у WinHTTP не доказывает отсутствие прокси в браузере.
- `AutoConfigURL`/PAC — **адрес файла правил**, а не прокси-сервера. Git не принимает `.pac` как значение `http.proxy`. Администратор должен сообщить сервер и порт, выбранные PAC для `github.com`; не угадывайте их.
- Отсутствие вывода у `git config --get-regexp` или `--get-urlmatch` с кодом 1 означает, что настройка не найдена.
- `NO_PROXY`/`no_proxy` может исключать GitHub из прокси. Проверьте его, если Git продолжает подключаться напрямую.

### 3.2. Проверить найденный прокси без постоянного изменения

В сети университета используйте прокси **`proxy.kubsu.ru:8080`**. На домашнем компьютере вне сети университета не включайте его без необходимости.

**PowerShell:**

```powershell
git -c http.proxy="http://proxy.kubsu.ru:8080" ls-remote --heads https://github.com/git/git.git
```

**CMD:**

```bat
git -c http.proxy="http://proxy.kubsu.ru:8080" ls-remote --heads https://github.com/git/git.git
```

В ответе должны быть хеши и `refs/heads/...`. Здесь используется публичный репозиторий проекта Git для проверки сети, а не ваш учебный проект. `http://` у прокси допустим и при обращении к `https://github.com`: через HTTP CONNECT устанавливается TLS-соединение к GitHub. Для SOCKS-прокси используйте, например, `socks5h://127.0.0.1:1080`, только если это реальный адрес вашего запущенного прокси; `socks5h` передаёт разрешение DNS прокси-серверу.

### 3.3. Сохранить прокси только для GitHub

Ниже команды для PowerShell и CMD. Прокси на Linux/macOS вынесен в раздел 6 в конце файла.

**PowerShell:**

```powershell
git config --global http.https://github.com.proxy "http://proxy.kubsu.ru:8080"
git config --get-urlmatch http.proxy https://github.com
```

**CMD:**

```bat
git config --global http.https://github.com.proxy "http://proxy.kubsu.ru:8080"
git config --get-urlmatch http.proxy https://github.com
```

Git использует параметр `http.proxy` и для HTTPS. Отдельный параметр Git `https.proxy` для этого создавать не нужно. Для всех HTTP(S)-репозиториев вместо настройки по URL существует `git config --global http.proxy "http://HOST:PORT"`, но на занятии достаточно настройки для GitHub.

Альтернатива — переменные **только текущей сессии**. Они действуют шире, чем настройка только GitHub, и могут использоваться другими программами. Не смешивайте способы без необходимости.

**PowerShell:**

```powershell
$env:HTTP_PROXY = 'http://proxy.kubsu.ru:8080'
$env:HTTPS_PROXY = 'http://proxy.kubsu.ru:8080'
```

**CMD:**

```bat
set "HTTP_PROXY=http://proxy.kubsu.ru:8080"
set "HTTPS_PROXY=http://proxy.kubsu.ru:8080"
```

### 3.4. Вернуться к прямому подключению

Удаляйте только настройки, которые добавили на занятии. Сначала сохраните старые значения, если они были. Чтобы убрать прокси только GitHub:

**PowerShell:**

```powershell
git config --global --unset-all http.https://github.com.proxy
Remove-Item Env:HTTP_PROXY,Env:HTTPS_PROXY -ErrorAction SilentlyContinue
```

**CMD:**

```bat
git config --global --unset-all http.https://github.com.proxy
set HTTP_PROXY=
set HTTPS_PROXY=
```

Если сами задавали общий `http.proxy`, уберите его через `git config --global --unset-all http.proxy`. Значения из системного или локального файла могут продолжить действовать: `--show-origin` показывает источник. Не удаляйте чужие настройки вслепую. `ALL_PROXY` и `NO_PROXY` проверяются отдельно.

### 3.5. Прокси и авторизация — разные проблемы

| Ошибка | Что делать |
|---|---|
| Could not resolve proxy | Проверить имя прокси, подключение к сети университета/VPN и DNS |
| Failed to connect / timeout | Проверить сервер и порт, работает ли прокси и разрешён ли GitHub |
| 407 Proxy Authentication Required | Прокси требует входа; уточнить у администратора механизм (NTLM, Negotiate и др.) и поддержку Git. Пароль не записывать в URL, историю команд или репозиторий |
| SSL certificate problem | Получить у администратора доверенный корневой сертификат и настроить доверие к нему. Не использовать `http.sslVerify=false` и `curl -k` |
| 403 / 404 при push | Проверить аккаунт, адрес репозитория, принятые приглашения и право Write; 404 также может скрывать отсутствие доступа |
| Authentication failed | Для GitHub HTTPS нужен credential manager/вход браузером или токен; пароль аккаунта GitHub не заменяет токен |

Настройка `http.proxy` не влияет на SSH-адрес `git@github.com:...`. Для этой инструкции используйте **HTTPS**. Если уже клонировали по SSH, внутри проекта замените адрес на HTTPS своего репозитория:

```sh
git remote -v
git remote set-url origin https://github.com/Flovwvers-students/ИМЯ-РЕПОЗИТОРИЯ.git
```

Эти две команды одинаковы в PowerShell, CMD и Linux/macOS. Не отключайте проверку сертификатов ради подключения.

## 4. Вход в GitHub через gh: Windows и Linux

После установки программ и проверки сети выполните одинаковые команды в PowerShell, CMD или терминале Linux/macOS:

```sh
gh --version
gh auth login --hostname github.com --git-protocol https --web
gh auth setup-git
gh auth status --hostname github.com
```

Следуйте подсказкам gh: откройте предложенную страницу в браузере, введите показанный одноразовый код и войдите **в свой GitHub-аккаунт**. Если спросит, использовать ли этот вход для Git, согласитесь. Проверьте, что `gh auth status` показывает ваш логин. Одноразовый код, пароль и токен никому не отправляйте.

Если браузер не открылся автоматически, откройте адрес, который вывел gh, вручную и продолжите вход. На университетском компьютере сначала задайте прокси в текущем терминале, если прямого подключения нет. Открытый в браузере GitHub сам по себе не авторизует Git.

GitHub не принимает пароль аккаунта вместо токена в запросе `Password for https://github.com`. Для занятия используем gh и вход через браузер, чтобы не копировать токены в команды. В приглашении `Username` нужен GitHub-логин, а не email, но после корректного `gh auth setup-git` повторный ручной ввод обычно не требуется. Если ошибка повторяется, проверьте `gh auth status` и аккаунт, под которым вошли.

Вход не выдаёт доступ к приватному репозиторию. Примите приглашение. Если клонируете чужой репозиторий как разработчик или ревьюер, его аналитик должен сначала выдать вам Write. Убедитесь, что открываете нужный URL. Если переменная `GIT_CONFIG_GLOBAL` используется для личного диска, выполняйте настройку и дальнейшую работу в одном терминале или повторяйте блок выбора диска в каждом новом.

После новой университетской сессии заново выберите личный диск и файл настроек Git, при необходимости задайте прокси, выполните `gh auth status`, при отсутствии входа повторите login и setup-git. Файлы проекта сохраняйте в `github` на личном сохраняемом диске, а не на Рабочем столе или в Downloads.

## 5. Готовность к заданию

- [ ] Есть подтверждённый личный GitHub-логин и принято нужное приглашение.
- [ ] Работают `git --version`, `gh --version`, `javac -version` и `java -version`.
- [ ] `gh auth status` показывает мой аккаунт; `gh auth setup-git` выполнен.
- [ ] Проверка `git ls-remote --heads https://github.com/git/git.git` выводит ветки.
- [ ] В университете выбран сохраняемый личный диск и папка `github`; буква диска подтверждена.

Теперь откройте [задание по трём ролям](github-java-m3-assignment.md). Установку и настройку повторять внутри задания не нужно.

## Источники

- [Установка GitHub CLI на Windows](https://github.com/cli/cli/blob/trunk/docs/install_windows.md).
- [Установка GitHub CLI на Linux](https://github.com/cli/cli/blob/trunk/docs/install_linux.md).
- [Вход gh auth login](https://cli.github.com/manual/gh_auth_login).
- [Настройки Git, http.proxy и GIT_CONFIG_GLOBAL](https://git-scm.com/docs/git-config).

## 6. Прокси на личном ноутбуке: Linux и macOS

На этих системах прокси нужен только если этого требует ваша сеть. Установка, сборка и обычная работа с Git описаны выше. Сначала `git ls-remote --heads https://github.com/git/git.git`; если сеть работает, пропустите настройку прокси. `proxy.kubsu.ru:8080` — университетский прокси; используйте его в сети университета, если требуется. Дома он обычно не нужен. PAC-файл не является адресом прокси. Пароли не сохраняйте; ошибки сертификатов не обходите отключением TLS.

### Получить текущие настройки

**Linux/macOS (bash/zsh):**

```sh
env | grep -iE '^(http_proxy|https_proxy|all_proxy|no_proxy)='
git config --show-origin --get-regexp '.*proxy.*'
git config --get-urlmatch http.proxy https://github.com
curl -I --connect-timeout 15 https://github.com
```

### Системные настройки macOS

**Дополнительно macOS:**

```sh
scutil --proxy
networksetup -listallnetworkservices
networksetup -getwebproxy "Wi-Fi"
networksetup -getsecurewebproxy "Wi-Fi"
networksetup -getautoproxyurl "Wi-Fi"
```

Подставьте точное имя активной службы из списка вместо `Wi-Fi`. В `scutil --proxy` ищите `HTTPEnable`, `HTTPProxy`, `HTTPPort`, `HTTPSProxy`, `HTTPSPort`, `ProxyAutoConfigURLString`.

### Системные настройки Linux GNOME

**Дополнительно Linux с GNOME (если установлен gsettings):**

```sh
gsettings get org.gnome.system.proxy mode
gsettings get org.gnome.system.proxy.http host
gsettings get org.gnome.system.proxy.http port
gsettings get org.gnome.system.proxy.https host
gsettings get org.gnome.system.proxy.https port
gsettings get org.gnome.system.proxy autoconfig-url
```

### Проверить прокси на один запуск

**Linux/macOS:**

```sh
git -c http.proxy="http://proxy.kubsu.ru:8080" ls-remote --heads https://github.com/git/git.git
```

### Сохранить прокси только GitHub

**Linux/macOS:**

```sh
git config --global http.https://github.com.proxy "http://proxy.kubsu.ru:8080"
git config --get-urlmatch http.proxy https://github.com
```

### Альтернатива: переменные текущей сессии

**Linux/macOS:**

```sh
export http_proxy='http://proxy.kubsu.ru:8080'
export https_proxy='http://proxy.kubsu.ru:8080'
export HTTP_PROXY='http://proxy.kubsu.ru:8080'
export HTTPS_PROXY='http://proxy.kubsu.ru:8080'
```

### Убрать добавленные настройки

**Linux/macOS:**

```sh
git config --global --unset-all http.https://github.com.proxy
unset http_proxy https_proxy HTTP_PROXY HTTPS_PROXY
```

В Linux нет единого системного прокси для всех окружений. На KDE и сервере уточните настройки сети у администратора.

Удаляйте только добавленные вами настройки. Общий `http.proxy`, `ALL_PROXY` и `NO_PROXY` проверяются отдельно; источник покажет `git config --show-origin --get-regexp ".*proxy.*"`. Диагностика 407, PAC и сертификатов — раздел 3.5.

Переменные окружения в этом приложении нужны также для gh. После их задания выполните вход из раздела 4 в том же терминале.
