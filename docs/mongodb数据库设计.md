## Mongodb 数据库设计

#### 版本历史
版本/状态 | 作者 | 参与者 | 指定日期 | 备注
---|--- | --- | --- | ---
v0.1 | 复旦-陈识 |   | 2017/9/1 | 
v0.2 | 复旦-陈识 |   | 2017/9/10| 
v0.3 | 复旦-陈识 |   | 2017/9/14| 
v0.4 | 复旦-陈识 |   | 2017/9/18| 



#### 文章数据表 article 

字段 | 描述 | 类型 | 备注
---|--- | --- | ---
_id | mongodb自动id | 数字 |
articleID | 文章id | 字符串 | uuid
types | 文章类型 |字符串数组| 
source | 文章来源 |字符串|
channel | 文章频道（粗粒度）|字符串|
category | 文章类别（细粒度）| 对象数组 | 文章导航信息（id + 内容）
tags | 文章标签 |字符串数组| 带图文章数据提供
keywords | 文章关键词 |字符串数组| 算法抽取
url | 文章原始地址 |字符串|
title | 文章标题 | 字符串|
content | 文章内容（纯文字）|字符串|
imgID | 图片id |字符串数组 | 图片uuid

- 举例
```
{
    "articleID": "",
    "types": ["pics",""],
    "source": "",
    "channel": "",
    "category" : [
        [
            {
                "_id" : NumberInt(51928), 
                "name" : "综合", 
                "parent_id" : NumberInt(200856)
            }, 
            {
                "_id" : NumberInt(200856), 
                "name" : "资讯", 
                "parent_id" : NumberInt(51895)
            }, 
            ...
        ], 
        [
            {
                "_id" : NumberInt(38790), 
                "name" : "手机新闻频道", 
                "parent_id" : NumberInt(258)
            }, 
            {
                "_id" : NumberInt(258), 
                "name" : "手机新浪网", 
                "parent_id" : NumberInt(0)
            },
            ...
        ]
    ]，
    "tags": ["","",""],
    "keywords": ["","",""],
    "url": "",
    "title": "",
    "content": "",
    "code": "",
    "imgID": ["","",""]
   
}
```

#### 图片标签表 image

字段 | 描述 | 类型 | 备注
---|--- | --- | ---
_id | mongodb自动id | 数字 |
imgID | 图片id |字符串|  同图片名字
imgURL  | 图片url | 字符串|
imgOriginalURL | 图片本地路径 | 字符串 |
imgDescription | 图片描述 | 字符串 |即图片标题 | 
imgCategory | 图片类别 | 数组 | 文章web端及app端category_id
imgTagList | 图片标签列表 | 对象数组 | TagInfo对象
imgVisualTagList| 图片标签列表 | 对象数组 | TagInfo对象| 存放从图像识别中抽取的标签
tagID | 标签id | 字符串 |
tagName | 标签名 | 字符串 |
tagSource | 标签来源 | 字符串 | 文章、图像、社交、用户捐献等
tagScore | 标签分数 | 数字 | 由标签抽取算法计算得出，可优化


- 示例
```
{
   "imgID": "",
   "imgURL": "",
   "imgOriginalURL": "",
   "imgDescription": "",
   "imgCategory" []，
   "imgVisualTagList":  [
        {
            "tagID": "",
            "tagName": "",
            "tagScore": 10
        },
        ...
   ],
   "imgTagList": [
        {
            "tagID": "",
            "tagName": "",
            "tagSource": "文章",
            "tagScore": 10
        },
        ...
   ]
}
```

#### 编辑反馈行为表 feedback
字段 | 描述 | 类型 | 备注
---|--- | --- | ---
sessionID | 推荐会话id | 字符串 |
feedbackDate | 反馈时间 | 时间 | 
feekbackIPAddr | 反馈ip地址 | 字符串
adoptedImgs | 编辑采纳图片imgID列表 | 字符串数组 |



- 数据模型
    ```
    {
        "sessionID": "59b79f00891e483ba46f6050",    //推荐会话id
        "feebackDate": "2016-05-18T16:00:00Z",     //反馈时间
        "feekbackIPAddr": "10.132.141.120",     //反馈ip地址
        "adoptedImgs":  ["img001", "img004", "img005", ...]   //编辑采纳图片列表

    }

   ```
   
#### 编辑增加标签记录表
   
字段 | 描述 | 类型 | 备注
---|--- | --- | ---
sessionID | 推荐会话id | 字符串 |
addTagDate | 新增标签时间 | 时间 | 
addTagIPAddr | 新增标签ip地址 | 字符串
addTags | 新增标签列表 | 对象数组 | 
imgID | 图片ID | 字符串 |  
newTagName | 新增标签名 | 字符串 | 

- 数据模型
    ```
    {
        "sessionID": "59b79f00891e483ba46f6050",    //推荐会话id
        "addTagDate": "2016-05-18T16:00:00Z",     //新增标签时间
        "addTagIPAddr": "10.132.141.120",     //新增标签ip地址
        "addTags": [        //新增标签集合
            {
                "imgID": "img099",      //图片id
                "newTagName": "火锅"    //标签名
            }，
            {
                "imgID": "img101",
                "newTagName": "牛排"
            },
            ...
        ]
    }

   ```
#### 编辑增加标签记录表
   
字段 | 描述 | 类型 | 备注
---|--- | --- | ---
sessionID | 推荐会话id | 字符串 |
deleteTagDate | 删除标签时间 | 时间 | 
deleteTagIPAddr | 删除标签ip地址 | 字符串
deleteTags | 删除标签列表 | 对象数组 | 
imgID | 图片ID | 字符串 |  
newTagName | 新增标签名 | 字符串 | 
tagID | 标签ID列表 | 字符串数组 | 

- 数据模型
    ```
    {
        "sessionID": "59b79f00891e483ba46f6050",    //推荐会话id
        "deleteTagDate": "2016-05-18T16:00:00Z",     //删除标签时间
        "deleteTagIPAddr": "10.132.141.120",     //删除标签ip地址
        "deleteTags": [        //删除标签集合
            {
                "imgID": "img400",  //图片id
                "tagID": ["tag1200", "tag1201"]    //标签id
            },
            {
                "imgID": "img405",
                "tagID": ["tag1205", "tag1208"]
            },
            ...
        ]
    }

   ```
#### 文章训练集表



   