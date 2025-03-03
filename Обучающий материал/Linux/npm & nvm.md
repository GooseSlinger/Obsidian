### **Установка через nvm (Node Version Manager)**

`nvm` — это удобный инструмент для управления версиями Node.js и npm. Он позволяет устанавливать несколько версий Node.js и переключаться между ними.

#### 1. Установите `nvm`:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```
_(Замените `v0.39.5` на последнюю версию, если нужно.)_

#### 2. Перезапустите терминал или примените изменения:
```bash
source ~/.bashrc
```

#### 3. Установите последнюю стабильную версию Node.js:
```bash
nvm install --lts
```

#### 4. Установите её как активную:
```bash
nvm use --lts
```

#### 5. Проверьте версии:
```bash
node --version
npm --version
```