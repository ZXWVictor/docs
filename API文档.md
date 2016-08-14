## 隧道APP 后端API需求

```javascript
// 基本所有请求都会使用的headers
// user登录除外
{
    "Content-Type": "application/json",
    "Authorization": "Bearer {token}"
}
```

---
### 1.用户
url-prefix: `/user`


#### 1.1 登录

url:  `POST /user/auth`

> Input:
```javascript
{
    "username": "xxx",
    "password" "faexaxa"
}
```
> Output:
```javascript
    {
        "userID": 123,
        "username": "xxx",
        "displayName": "林欣欣",
        "token": "xxasdfaefa"
    }
```
---

#### 1.2 验证token有效性
url: `POST /user/validate`

> Input:
```javascript
{
    "token": "Bearer {token}"
}
```

---

### 2. 巡检相关
url-prefix: `/inspections`

#### 2.1 获取巡检列表
url: `GET /inspections`

> Input
```json
N/A
```

> Output
```javascript
[
    {
        "id": 1234,
        "inspectionName": "东线巡检",
    },
    {
        "id": 1235,
        "inspectionName": "西线巡检"
    }
]
```

#### 2.2 获取巡检活动类型
URL: `GET /inspections/activityTypes`
> Input
```javascript
N/A
```
> Output
```javascript
[
    {
        "id": 123,
        "typeName": "巡检1"
    },
    {
        "id": 124,
        "typeName": "巡检2"
    }
]
```

#### 2.3 获取巡检活动列表
Url: `GET /inspections/:inspectionID/activities`

> Input
```javascript
N/A
```

> Output
```javascript
[
    {
        "id": 123,
        "activityName": "巡检活动1",
        "longitude": 15.3214,
        "latitude": 92.133,
        "locationName": "上海大学城中路20号"
        "activityTypeID": 123,
        "createdAt": ...,
        "status": 1,
        "description": "一些描述",
    },
    {
        "id": 124,
        "activityName": "巡检活动2",
        "longitude": 15.3214,
        "latitude": 92.133,
        "locationName": "上海大学城中路20号"
        "description": "",
        "activityTypeID": 123,
        "createdAt": ...,
        "status": 2
    },
]
```

#### 2.3 获取巡检活动
Url: `GET /inspections/all/activities/:activityID`
```javascript
{
    "id": 124,
    "activityName": "巡检活动2",
    "longitude": 15.3214,
    "latitude": 92.133,
    "locationName": "上海大学城中路20号"
    "description": "",
    "activityTypeID": 123,
    "createdAt": ...,
    "status": 2,
    "medias": [
        {
            "mediaID": 22,
            "mediaType": "image",
            "previewUrl": "",
            "url":""
        },
        {
            "mediaID": 23,
            "mediaType": "image",
            "previewUrl": "",
            "url":""
        },
    ]
}
```

#### 2.4 搜索巡检活动
Url: `POST /inspections/:inspectionID/activities/search`
> Input
```javascript
{
    "query": "上海大学",
    "from": 123124, //时间
    "to": 123145345, //时间
    "status": activityStatus, // enum: [1,2,3]
    "type": :activityTypeID
}
```
> Output
```javascript
[
    // 从数据库里出来的数据
    {
        "id": 123,
        "activityName": "巡检活动1",
        "longitude": 15.3214,
        "latitude": 92.133,
        "locationName": "上海大学城中路20号"
        "activityTypeID": 123,
        "createdAt": ...,
        "status": 1,
        "description": "一些描述"
    },
    {
        "id": 124,
        "activityName": "巡检活动2",
        "longitude": 15.3214,
        "latitude": 92.133,
        "locationName": "上海大学城中路20号"
        "description": "",
        "activityTypeID": 123,
        "createdAt": ...,
        "status": 2
    },
    //从百度API中取出的数据
    {
        "locationName": "上海大学城中路20号",
        "longitude": 15.3214,
        "latitude": 92.133
    }
]
```

#### 2.5 创建巡检活动
Url: `POST /inspections/:inspectionID/activities`
> Input
```
"Headers": {
    "Content-Type": "none"
},
"Body":{
    "id": 123,
    "activityName": "巡检活动1",
    "longitude": 15.3214,
    "latitude": 92.133,
    "locationName": "上海大学城中路20号"
    "activityTypeID": activityType.id,
    "createdAt": ...,
    "description": "一些描述"
}
```
> Output
```
{
    "id": 123,
    "createdAt": ...,
    "status": 0// default status
},
```
#### 2.6 删除巡检多媒体
Url: `DELETE /inspections/all/activities/:activityID/medias/:mediaID`
> Output
```
{
    id: mediaID
}
```

#### 2.7 更新巡检活动
Url: `PUT /inspections/:inspectionID/activities/:activityID`
> Input
```
"Headers": {
    "Content-Type": "none"
},
"Body":{
    "id": 123,
    "activityName": "巡检活动1",
    "longitude": 15.3214,
    "latitude": 92.133,
    "locationName": "上海大学城中路20号"
    "activityTypeID": activityType.id,
    "createdAt": ...,
    "description": "一些描述"
}
```
> Output
```
{
    "id": 123,
    "createdAt": ...,
    "status": 0// default status
},
```

#### 2.8 删除巡检活动
Url: `DELETE /inspections/:inspectionID/activities/:activityID`
> Output
```
{
    "id": activityID
},
```


### 3 地下设施
url-prefix: `/facilities`

#### 3.1 获取设施列表
Url: `GET /facilities`
> Output
```
[
    {
        "id": 231,
        "displayName": "东线安全通道"
    },
    {
        "id": 232,
        "displayName": "西线安全通道"
    }
]
```

#### 3.2 获取设施
Url: `GET /facilities/:facilityID`
> Output
```
{
    "id": 123
    "name": "设施名",
    "rings": [
        {"id": 1, mileage: 0},
        {"id": 2, mileage: 20}
    ],
    "diseases": [
        {
            "id": 123,
            "mileage": 133,
            "histories": [
                {
                    "recordID": 1,
                    "createdAt": 123133,
                    "imgUrl": "/img.jpeg", 
                    "diseases": [
                        {
                            "id": 123,
                            "diseaseType": 1,//病害大类
                            "detailType": 2,//病害小类
                            "depth": 30,//深度mm
                            "length": 123,//长度mm
                            "width": 121,//宽度mm
                            "area": 214,//mm²
                            "jointOpen": 100,//张开量mm
                            "dislocation": 100, //错台量mm
                            "imgUrl": "/img.jpeg", 
                            "createdBy": userID // 创建人ID
                        },
                        {
                            "id": 123,
                            "diseaseType": 1,//病害大类
                            "detailType": 2,//病害小类
                            "depth": 30,//深度mm
                            "length": 123,//长度mm
                            "width": 121,//宽度mm
                            "area": 214,//mm²
                            "jointOpen": 100,//张开量mm
                            "dislocation": 100, //错台量mm
                            "createdBy": userID // 创建人ID
                        }
                    ]
                },
                {
                    "recordID": 2,
                    "createdAt": 123133,
                    "diseases": [
                        {
                            "id": 123,
                            "diseaseType": 1,//病害大类
                            "detailType": 2,//病害小类
                            "depth": 30,//深度mm
                            "length": 123,//长度mm
                            "width": 121,//宽度mm
                            "area": 214,//mm²
                            "jointOpen": 100,//张开量mm
                            "dislocation": 100, //错台量mm
                            "imgUrl": "/img.jpeg", 
                            "createdBy": userID // 创建人ID
                        },
                        {
                            "id": 123,
                            "diseaseType": 1,//病害大类
                            "detailType": 2,//病害小类
                            "depth": 30,//深度mm
                            "length": 123,//长度mm
                            "width": 121,//宽度mm
                            "area": 214,//mm²
                            "jointOpen": 100,//张开量mm
                            "dislocation": 100, //错台量mm
                            "imgUrl": "/img.jpeg", 
                            "createdBy": userID // 创建人ID
                        }
                    ]
                }
            ]
        }
    ]
}

```

#### 3.3 获取灾害种类
Url: `GET /diseaseTypes`
```javascript
[
    {
        "diseaseTypeID": 123,
        "typeName": "漏水",
        "subTypes": [
            {
                "subTypeID": 1,
                "typeName": "水1"
            },
            {
                "subTypeID": 2,
                "typeName": "水2"
            }
        ]
    }
]
```

#### 3.4 上传灾害
```javascript
[
    {
        "mileage": 133,
        "imgUrl": "/img.jpeg", 
        "diseases": [
            {
                "id": 123,
                "diseaseType": 1,//病害大类
                "detailType": 2,//病害小类
                "depth": 30,//深度mm
                "length": 123,//长度mm
                "width": 121,//宽度mm
                "area": 214,//mm²
                "jointOpen": 100,//张开量mm
                "dislocation": 100, //错台量mm
                "imgUrl": "/img.jpeg", 
                "createdBy": userID // 创建人ID
            },
            {
                "id": 123,
                "diseaseType": 1,//病害大类
                "detailType": 2,//病害小类
                "depth": 30,//深度mm
                "length": 123,//长度mm
                "width": 121,//宽度mm
                "area": 214,//mm²
                "jointOpen": 100,//张开量mm
                "dislocation": 100, //错台量mm
                "createdBy": userID // 创建人ID
            }
        ]
    }
]

```