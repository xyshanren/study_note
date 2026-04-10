> 学习日期：2026-05-08（周四）

## 学习目标

1. 掌握网络请求
2. 理解数据存储
3. 能够实现网络交互和本地存储

---

## 1. 网络请求

### 1.1 HTTP 请求

```typescript
import http from '@ohos.net.http';

// 发起 GET 请求
async function fetchUser() {
    const httpRequest = http.createHttp();
    
    try {
        const response = await httpRequest.request(
            'https://api.example.com/user/123',
            {
                method: http.RequestMethod.GET,
                header: {
                    'Content-Type': 'application/json'
                }
            }
        );
        
        if (response.responseCode === 200) {
            const data = JSON.parse(response.result as string);
            console.log('用户数据:', data);
            return data;
        }
    } catch (error) {
        console.error('请求失败:', error);
    } finally {
        httpRequest.destroy();
    }
}

// 发起 POST 请求
async function login(username: string, password: string) {
    const httpRequest = http.createHttp();
    
    try {
        const response = await httpRequest.request(
            'https://api.example.com/login',
            {
                method: http.RequestMethod.POST,
                header: {
                    'Content-Type': 'application/json'
                },
                extraData: {
                    username,
                    password
                }
            }
        );
        
        if (response.responseCode === 200) {
            return JSON.parse(response.result as string);
        }
    } catch (error) {
        console.error('登录失败:', error);
    } finally {
        httpRequest.destroy();
    }
}
```

### 1.2 封装网络请求

```typescript
// utils/http.ts
import http from '@ohos.net.http';

interface RequestOptions {
    url: string;
    method?: http.RequestMethod;
    header?: Record<string, string>;
    extraData?: object | string;
}

interface Response<T> {
    code: number;
    data: T;
    message: string;
}

class HttpClient {
    private baseUrl: string;
    
    constructor(baseUrl: string) {
        this.baseUrl = baseUrl;
    }
    
    async request<T>(options: RequestOptions): Promise<T> {
        const httpRequest = http.createHttp();
        
        try {
            const response = await httpRequest.request(
                this.baseUrl + options.url,
                {
                    method: options.method || http.RequestMethod.GET,
                    header: options.header || { 'Content-Type': 'application/json' },
                    extraData: options.extraData
                }
            );
            
            if (response.responseCode === 200) {
                const result = response.result as string;
                return JSON.parse(result) as T;
            }
            
            throw new Error(`请求失败: ${response.responseCode}`);
        } finally {
            httpRequest.destroy();
        }
    }
    
    get<T>(url: string): Promise<T> {
        return this.request<T>({ url, method: http.RequestMethod.GET });
    }
    
    post<T>(url: string, data: object): Promise<T> {
        return this.request<T>({
            url,
            method: http.RequestMethod.POST,
            extraData: data
        });
    }
}

// 使用
const api = new HttpClient('https://api.example.com');

interface User {
    id: number;
    name: string;
}

const user = await api.get<User>('/user/123');
const loginResult = await api.post<{ token: string }>('/login', {
    username: 'test',
    password: '123456'
});
```

---

## 2. 数据存储

### 2.1 首选项存储

```typescript
import preferences from '@ohos.data.preferences';

// 获取首选项实例
async function getPreferences() {
    const context = getContext(this);
    return await preferences.getPreferences(context, 'myPreferences');
}

// 保存数据
async function saveData() {
    const prefs = await getPreferences();
    
    // 写入数据
    await prefs.put('username', '张三');
    await prefs.put('age', 25);
    await prefs.put('isLogin', true);
    
    // 提交保存
    await prefs.flush();
}

// 读取数据
async function readData() {
    const prefs = await getPreferences();
    
    const username = prefs.get('username', '默认值') as string;
    const age = prefs.get('age', 0) as number;
    const isLogin = prefs.get('isLogin', false) as boolean;
    
    console.log(`用户名: ${username}, 年龄: ${age}, 登录: ${isLogin}`);
}

// 删除数据
async function deleteData() {
    const prefs = await getPreferences();
    
    await prefs.delete('username');
    await prefs.flush();
}

// 清除所有数据
async function clearAll() {
    const prefs = await getPreferences();
    
    await prefs.clear();
    await prefs.flush();
}
```

### 2.2 键值型存储工具类

```typescript
// utils/storage.ts
import preferences from '@ohos.data.preferences';

const PREFERENCES_NAME = 'app_prefs';

class Storage {
    private prefs: preferences.Preferences | null = null;
    
    async init() {
        const context = getContext(this);
        this.prefs = await preferences.getPreferences(context, PREFERENCES_NAME);
    }
    
    async set<T>(key: string, value: T): Promise<void> {
        if (!this.prefs) await this.init();
        
        if (typeof value === 'string') {
            await this.prefs.put(key, value);
        } else if (typeof value === 'number') {
            await this.prefs.put(key, value);
        } else if (typeof value === 'boolean') {
            await this.prefs.put(key, value);
        } else {
            await this.prefs.put(key, JSON.stringify(value));
        }
        
        await this.prefs.flush();
    }
    
    async get<T>(key: string, defaultValue: T): Promise<T> {
        if (!this.prefs) await this.init();
        
        const value = this.prefs.get(key, defaultValue);
        if (typeof defaultValue === 'object') {
            return JSON.parse(value as string) as T;
        }
        return value as T;
    }
    
    async remove(key: string): Promise<void> {
        if (!this.prefs) await this.init();
        await this.prefs.delete(key);
        await this.prefs.flush();
    }
    
    async clear(): Promise<void> {
        if (!this.prefs) await this.init();
        await this.prefs.clear();
        await this.prefs.flush();
    }
}

export const storage = new Storage();
```

---

## 3. 文件存储

### 3.1 读取文件

```typescript
import fileio from '@ohos.fileio';

async function readTextFile(fileName: string): Promise<string> {
    const context = getContext(this);
    const filePath = context.filesDir + '/' + fileName;
    
    try {
        const data = await fileio.readText(filePath);
        return data;
    } catch (error) {
        console.error('读取文件失败:', error);
        return '';
    }
}
```

### 3.2 写入文件

```typescript
async function writeTextFile(fileName: string, content: string): Promise<boolean> {
    const context = getContext(this);
    const filePath = context.filesDir + '/' + fileName;
    
    try {
        await fileio.writeText(filePath, content);
        return true;
    } catch (error) {
        console.error('写入文件失败:', error);
        return false;
    }
}
```

### 3.3 JSON 文件存储

```typescript
class JsonStorage<T> {
    private fileName: string;
    
    constructor(fileName: string) {
        this.fileName = fileName;
    }
    
    async save(data: T): Promise<void> {
        const content = JSON.stringify(data);
        await writeTextFile(this.fileName, content);
    }
    
    async load(): Promise<T | null> {
        const content = await readTextFile(this.fileName);
        if (content) {
            try {
                return JSON.parse(content) as T;
            } catch {
                return null;
            }
        }
        return null;
    }
    
    async remove(): Promise<void> {
        const context = getContext(this);
        const filePath = context.filesDir + '/' + this.fileName;
        try {
            await fileio.unlink(filePath);
        } catch (e) {
            // 文件不存在
        }
    }
}

// 使用
interface User {
    name: string;
    email: string;
}

const userStorage = new JsonStorage<User>('user.json');

// 保存用户
await userStorage.save({
    name: '张三',
    email: 'zhangsan@example.com'
});

// 读取用户
const user = await userStorage.load();
```

---

## 4. 案例：用户登录

### 4.1 登录页面

```typescript
@Entry
@Component
struct LoginPage {
    @State username: string = '';
    @State password: string = '';
    @State isLoading: boolean = false;
    
    async doLogin() {
        if (!this.username || !this.password) {
            AlertDialog.show({
                message: '请输入用户名和密码'
            });
            return;
        }
        
        this.isLoading = true;
        
        try {
            const result = await login(this.username, this.password);
            
            // 保存登录状态
            await storage.set('token', result.token);
            await storage.set('user', result.user);
            
            // 跳转到首页
            router.pushUrl({ url: 'pages/Home' });
        } catch (error) {
            AlertDialog.show({
                message: '登录失败: ' + error.message
            });
        } finally {
            this.isLoading = false;
        }
    }
    
    build() {
        Column({ space: 20 }) {
            Text('登录')
                .fontSize(30)
                .fontWeight(FontWeight.Bold);
            
            TextInput({ placeholder: '请输入用户名' })
                .onChange((value: string) => {
                    this.username = value;
                });
            
            TextInput({ placeholder: '请输入密码' })
                .type(InputType.Password)
                .onChange((value: string) => {
                    this.password = value;
                });
            
            Button(this.isLoading ? '登录中...' : '登录')
                .width('100%')
                .enabled(!this.isLoading)
                .onClick(() => {
                    this.doLogin();
                });
        }
        .padding(30)
        .justifyContent(FlexAlign.Center)
        .height('100%');
    }
}
```

### 4.2 检查登录状态

```typescript
// 检查是否已登录
async function checkLogin(): Promise<boolean> {
    const token = await storage.get<string>('token', '');
    return !!token;
}

// 退出登录
async function logout() {
    await storage.remove('token');
    await storage.remove('user');
    router.pushUrl({ url: 'pages/Login' });
}
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：网络请求 ==========
// 1. 封装 HTTP 请求工具类
// 2. 实现用户列表 API 调用

// ========== 练习 2：本地存储 ==========
// 1. 实现首选项存储
// 2. 实现 JSON 文件存储

// ========== 练习 3：登录功能 ==========
// 1. 实现登录页面
// 2. 保存登录状态
// 3. 检查登录状态
```

---

## 🎯 今日自检清单

- [ ] HTTP 网络请求
- [ ] 首选项存储
- [ ] 文件存储
- [ ] JSON 数据处理

---

## 下一步

明天我们将学习：**ArkTS 实战项目 - 天气应用**
