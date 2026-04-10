> 学习日期：2026-05-09（周五）

## 学习目标

1. 综合运用所学知识
2. 完成天气应用实战项目

---

## 1. 项目概述

### 1.1 功能需求

- 首页显示当前天气
- 显示未来几天的天气预报
- 城市搜索功能
- 天气详情页面

### 1.2 项目结构

```
weather_app/
├── src/
│   └── main/
│       ├── ets/
│       │   ├── pages/
│       │   │   ├── index.ets          # 首页
│       │   │   ├── search.ets         # 搜索页
│       │   │   └── detail.ets         # 详情页
│       │   ├── components/
│       │   │   ├── WeatherCard.ets    # 天气卡片
│       │   │   ├── ForecastItem.ets   # 预报项
│       │   │   └── SearchItem.ets     # 搜索项
│       │   ├── services/
│       │   │   └── weather.ts         # 天气服务
│       │   ├── utils/
│       │   │   ├── http.ts            # 网络请求
│       │   │   └── storage.ts         # 本地存储
│       │   └── entryability/
│       │       └── EntryAbility.ts
│       └── resources/
└── module.json5
```

---

## 2. 数据模型

### 2.1 定义类型

```typescript
// types/weather.ts
interface WeatherData {
    location: Location;
    current: CurrentWeather;
    forecast: Forecast[];
}

interface Location {
    name: string;
    country: string;
    lat: number;
    lon: number;
}

interface CurrentWeather {
    temp: number;
    condition: string;
    icon: string;
    humidity: number;
    windSpeed: number;
    feelsLike: number;
}

interface Forecast {
    date: string;
    dayOfWeek: string;
    tempMax: number;
    tempMin: number;
    condition: string;
    icon: string;
}

interface City {
    id: string;
    name: string;
    country: string;
    lat: number;
    lon: number;
}
```

---

## 3. 网络服务

### 3.1 天气 API 服务

```typescript
// services/weather.ts
import http from '@ohos.net.http';

const API_KEY = 'your_api_key';
const BASE_URL = 'https://api.weather.com';

interface WeatherResponse {
    location: {
        name: string;
        country: string;
        lat: number;
        lon: number;
    };
    current: {
        temp_c: number;
        condition: {
            text: string;
            icon: string;
        };
        humidity: number;
        wind_kph: number;
        feelslike_c: number;
    };
    forecast: {
        forecastday: Array<{
            date: string;
            day: {
                maxtemp_c: number;
                mintemp_c: number;
                condition: {
                    text: string;
                    icon: string;
                };
            };
            date_epoch: number;
        }>;
    };
}

class WeatherService {
    async getWeather(city: string): Promise<WeatherData> {
        const httpRequest = http.createHttp();
        
        try {
            const response = await httpRequest.request(
                `${BASE_URL}/forecast.json?key=${API_KEY}&q=${city}&days=7`,
                {
                    method: http.RequestMethod.GET
                }
            );
            
            const data = JSON.parse(response.result as string) as WeatherResponse;
            
            return this.transformData(data);
        } finally {
            httpRequest.destroy();
        }
    }
    
    private transformData(data: WeatherResponse): WeatherData {
        const forecast: Forecast[] = data.forecast.forecastday.map(day => ({
            date: day.date,
            dayOfWeek: this.getDayOfWeek(day.date_epoch * 1000),
            tempMax: Math.round(day.day.maxtemp_c),
            tempMin: Math.round(day.day.mintemp_c),
            condition: day.day.condition.text,
            icon: 'https:' + day.day.condition.icon
        }));
        
        return {
            location: {
                name: data.location.name,
                country: data.location.country,
                lat: data.location.lat,
                lon: data.location.lon
            },
            current: {
                temp: Math.round(data.current.temp_c),
                condition: data.current.condition.text,
                icon: 'https:' + data.current.condition.icon,
                humidity: data.current.humidity,
                windSpeed: Math.round(data.current.wind_kph),
                feelsLike: Math.round(data.current.feelslike_c)
            },
            forecast
        };
    }
    
    private getDayOfWeek(timestamp: number): string {
        const days = ['周日', '周一', '周二', '周三', '周四', '周五', '周六'];
        return days[new Date(timestamp).getDay()];
    }
}

export const weatherService = new WeatherService();
```

---

## 4. 首页开发

### 4.1 主页面

```typescript
// pages/index.ets
import router from '@ohos.router';
import weatherService from '../services/weather';
import WeatherCard from '../components/WeatherCard';
import ForecastItem from '../components/ForecastItem';

@Entry
@Component
struct Index {
    @State weatherData: WeatherData | null = null;
    @State isLoading: boolean = false;
    @State errorMessage: string = '';
    @State currentCity: string = '北京';

    async aboutToAppear() {
        await this.loadWeather();
    }

    async loadWeather() {
        this.isLoading = true;
        this.errorMessage = '';

        try {
            this.weatherData = await weatherService.getWeather(this.currentCity);
        } catch (error) {
            this.errorMessage = '加载天气失败，请稍后重试';
        } finally {
            this.isLoading = false;
        }
    }

    build() {
        Column() {
            if (this.isLoading) {
                LoadingProgress()
                    .width(60)
                    .height(60);
            } else if (this.errorMessage) {
                Column({ space: 20 }) {
                    Text(this.errorMessage)
                        .fontSize(16)
                        .fontColor('#666666');
                    Button('重试')
                        .onClick(() => this.loadWeather());
                }
            } else if (this.weatherData) {
                // 当前位置
                Row() {
                    Text(this.weatherData.location.name)
                        .fontSize(24)
                        .fontWeight(FontWeight.Bold);
                    Text(', ' + this.weatherData.location.country)
                        .fontSize(16)
                        .fontColor('#666666');
                }
                .margin({ top: 20 });

                // 天气卡片
                WeatherCard({ weather: this.weatherData.current })
                    .margin({ top: 30 });

                // 天气预报标题
                Text('7天预报')
                    .fontSize(18)
                    .fontWeight(FontWeight.Medium)
                    .margin({ top: 30, left: 20 })
                    .alignSelf(ItemAlign.Start);

                // 预报列表
                Column({ space: 10 }) {
                    ForEach(this.weatherData.forecast, (day: Forecast) => {
                        ForecastItem({ forecast: day });
                    });
                }
                .margin({ top: 10, left: 20, right: 20 });

                // 搜索按钮
                Button('切换城市')
                    .margin({ top: 30 })
                    .onClick(() => {
                        router.pushUrl({ url: 'pages/search' });
                    });
            }
        }
        .width('100%')
        .height('100%')
        .backgroundColor('#f5f5f5');
    }
}
```

### 4.2 天气卡片组件

```typescript
// components/WeatherCard.ets
@Component
struct WeatherCard {
    @Prop weather: CurrentWeather;

    build() {
        Column() {
            // 温度
            Text(`${this.weather.temp}°`)
                .fontSize(80)
                .fontWeight(FontWeight.Light);

            // 天气状况
            Text(this.weather.condition)
                .fontSize(20)
                .margin({ top: 10 });

            // 详细信息
            Row({ space: 30 }) {
                Column() {
                    Text('体感')
                    Text(`${this.weather.feelsLike}°`)
                        .fontSize(16)
                        .fontWeight(FontWeight.Medium);
                }

                Column() {
                    Text('湿度')
                    Text(`${this.weather.humidity}%`)
                        .fontSize(16)
                        .fontWeight(FontWeight.Medium);
                }

                Column() {
                    Text('风速')
                    Text(`${this.weather.windSpeed} km/h`)
                        .fontSize(16)
                        .fontWeight(FontWeight.Medium);
                }
            }
            .margin({ top: 30 });
        }
        .width('90%')
        .padding(30)
        .backgroundColor('#ffffff')
        .borderRadius(20)
        .shadow({
            radius: 10,
            color: '#dddddd',
            offsetX: 0,
            offsetY: 5
        });
    }
}
```

### 4.3 预报项组件

```typescript
// components/ForecastItem.ets
@Component
struct ForecastItem {
    @Prop forecast: Forecast;

    build() {
        Row() {
            // 日期
            Text(this.forecast.dayOfWeek)
                .fontSize(16)
                .width(60);

            // 天气图标
            Image(this.forecast.icon)
                .width(30)
                .height(30);

            // 温度
            Row({ space: 10 }) {
                Text(`${this.forecast.tempMax}°`)
                    .fontSize(16)
                    .fontWeight(FontWeight.Medium);

                Text(`${this.forecast.tempMin}°`)
                    .fontSize(16)
                    .fontColor('#999999');
            }
            .layoutWeight(1)
            .justifyContent(FlexAlign.End);
        }
        .width('100%')
        .padding({ top: 15, bottom: 15, left: 20, right: 20 })
        .backgroundColor('#ffffff')
        .borderRadius(10);
    }
}
```

---

## 5. 搜索页面

### 5.1 搜索页面

```typescript
// pages/search.ets
import router from '@ohos.router';

@Entry
@Component
struct Search {
    @State searchText: string = '';
    @State historyCities: string[] = [];

    async aboutToAppear() {
        // 加载历史搜索
    }

    build() {
        Column() {
            // 搜索框
            Row() {
                TextInput({ placeholder: '搜索城市' })
                    .width('80%')
                    .onChange((value: string) => {
                        this.searchText = value;
                    });

                Button('搜索')
                    .width('18%')
                    .onClick(() => {
                        if (this.searchText) {
                            router.back({
                                params: { city: this.searchText }
                            });
                        }
                    });
            }
            .padding(20);

            // 历史搜索
            if (this.historyCities.length > 0) {
                Text('历史搜索')
                    .fontSize(16)
                    .fontColor('#666666')
                    .margin({ left: 20, top: 20 })
                    .alignSelf(ItemAlign.Start);

                Column({ space: 10 }) {
                    ForEach(this.historyCities, (city: string) => {
                        Text(city)
                            .fontSize(16)
                            .padding(15)
                            .backgroundColor('#f5f5f5')
                            .borderRadius(8)
                            .onClick(() => {
                                router.back({ params: { city } });
                            });
                    });
                }
                .margin({ left: 20, right: 20, top: 10 });
            }
        }
        .width('100%')
        .height('100%')
        .backgroundColor('#ffffff');
    }
}
```

---

## 6. 路由配置

### 6.1 配置路由

```json
// module.json5
{
    "module": {
        "pages": {
            "src": [
                "pages/index",
                "pages/search"
            ]
        }
    }
}
```

### 6.2 首页接收参数

```typescript
// index.ets 添加
onPageShow() {
    const params = router.getParams() as Record<string, string>;
    if (params && params.city) {
        this.currentCity = params.city;
        this.loadWeather();
    }
}
```

---

## 📝 课后练习

```typescript
// ========== 练习 1：完善功能 ==========
// 1. 添加城市搜索功能
// 2. 保存搜索历史

// ========== 练习 2：界面优化 ==========
// 1. 添加动画效果
// 2. 优化加载状态

// ========== 练习 3：数据持久化 ==========
// 1. 保存常用城市
// 2. 缓存天气数据
```

---

## 🎯 今日自检清单

- [ ] 项目结构规划
- [ ] 数据模型定义
- [ ] 网络服务封装
- [ ] 首页开发
- [ ] 组件拆分
- [ ] 搜索功能

---

## HarmonyOS 阶段总结

这周我们学习了鸿蒙开发：
1. ✅ Day 1: 开发环境搭建
2. ✅ Day 2: ArkTS 语法基础
3. ✅ Day 3: 组件与布局
4. ✅ Day 4: 网络与存储
5. ✅ Day 5: 实战项目

---

## 🎉 全栈学习路径阶段性完成！

到目前为止，我们已经完成了：
- ✅ TypeScript (8天)
- ✅ Vue 3 + TypeScript (7天)
- ✅ Python FastAPI (5天)
- ✅ 鸿蒙 ArkTS (5天)

接下来按计划应该是：
- 前后端项目实战
- 部署与运维（Docker, CI/CD）
- 最终项目

需要继续往下吗？
