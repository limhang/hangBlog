---
title: photo接口
date: 2017-06-01 15:18:26
tags: interface
categories: interface
---

# photo接口

# 缘由：photo接口说明文档

<!--more-->

## 一、基础模块接口：
### 1-1、基础教程列表
```
http://39.108.82.4/v1_0/common/baseInfo/lists
```

```js
{
    "code": 200, 
    "info": "", 
    "datas": {
        "lists": [
            {
                "id": 10, 
                "status": 1, 
                "title": "日常保养", 
                "des": "用专业的清洁器材，配合特定手法，才能进行有效清洁", 
                "img": "http://www.nikon.com.cn/microsite/beginnertutor/common/images/base_knowledge/maintain_2/maintain_2_img_02.jpg", 
                "webUrl": "http://39.108.82.4/photo/dailyMaintenance.html"
            }, 
            {
                "id": 9, 
                "status": 1, 
                "title": "选择合适的对焦模式", 
                "des": "选择合适的对焦模式", 
                "img": "http://www.nikon.com.cn/microsite/beginnertutor/common/images/base_knowledge/focus_2/focus_2_img_01.jpg", 
                "webUrl": "http://39.108.82.4/photo/focusMode.html"
            }, 
            {
                "id": 8, 
                "status": 1, 
                "title": "曝光补偿", 
                "des": "曝光补偿的基础知识", 
                "img": "http://www.nikon.com.cn/microsite/beginnertutor/common/images/base_knowledge/exposure_2/exposure_2_img_01.jpg", 
                "webUrl": "http://39.108.82.4/photo/exposureCompensation.html"
            }, 
            {
                "id": 7, 
                "status": 1, 
                "title": "镜头的乐趣", 
                "des": "世界随镜头而变——来享受单反/无反相机更换镜头的乐趣吧", 
                "img": "http://www.nikon.com.cn/microsite/beginnertutor/common/images/base_knowledge/enjoy_nikkel/en_img_01.jpg", 
                "webUrl": "http://39.108.82.4/photo/sceneFun.html"
            }, 
            {
                "id": 6, 
                "status": 1, 
                "title": "PASM模式", 
                "des": "手动曝光模式——实现照片创意的基石", 
                "img": "http://www.nikon.com.cn/microsite/beginnertutor/common/images/base_knowledge/pasm/pasm_img_01.jpg", 
                "webUrl": "http://39.108.82.4/photo/pasmPattern.html"
            }, 
            {
                "id": 5, 
                "status": 1, 
                "title": "光圈快门感光度", 
                "des": "控制照片的亮度——光圈、快门速度、感光度", 
                "img": "http://www.nikon.com.cn/microsite/beginnertutor/common/images/base_knowledge/asiso/asiso_img_01.jpg", 
                "webUrl": "http://39.108.82.4/photo/apertureShutterSensitivity.html"
            }, 
            {
                "id": 4, 
                "status": 1, 
                "title": "焦点", 
                "des": "准确合焦，让照片清晰起来", 
                "img": "http://www.nikon.com.cn/microsite/beginnertutor/common/images/base_knowledge/focus/focus_img_01.jpg", 
                "webUrl": "http://39.108.82.4/photo/focus.html"
            }, 
            {
                "id": 3, 
                "status": 1, 
                "title": "白平衡", 
                "des": "白平衡——照片色调的奥秘", 
                "img": "http://www.nikon.com.cn/microsite/beginnertutor/common/images/base_knowledge/white_balance/wb_img_01.jpg", 
                "webUrl": "http://39.108.82.4/photo/colorBalance.html"
            }, 
            {
                "id": 2, 
                "status": 1, 
                "title": "相机的基本持握方式", 
                "des": "先来掌握相机的握持方法和拍摄姿势吧", 
                "img": "http://www.nikon.com.cn/microsite/beginnertutor/common/images/base_knowledge/hold_way/hold_way_img_01.jpg", 
                "webUrl": "http://39.108.82.4/photo/holdingCameraRightWay.html"
            }, 
            {
                "id": 1, 
                "status": 1, 
                "title": "构图", 
                "des": "视角和构图，好照片的基础", 
                "img": "http://www.nikon.com.cn/microsite/beginnertutor/common/images/base_knowledge/composition_pic/composition_pic_img_01.jpg", 
                "webUrl": "http://39.108.82.4/photo/composition.html"
            }
        ]
    }
}
```

## 二、答疑模块接口
### 2-1、获取指定模块的数据

```
http://39.108.82.4/v1_0/common/quoraInfo/lists/5
``` 

* 参数  后缀为5：夜景  注意：如果参数传10，则获取所有数据
* 参数取值 1-9，代表9中不同模块，分别是，器材基础知识.摄影基础知识.基础技巧.人像.夜景.风光.运动.微距.配件&闪光灯


```js
{
    "code": 200, 
    "info": "", 
    "datas": {
        "lists": [
            {
                "id": 34, 
                "status": 5, 
                "classify": "夜景", 
                "question": "怎样才能把水族馆里的鱼拍得很漂亮？", 
                "answer": "在水族馆里拍摄鱼类时要隔着玻璃，由于玻璃会反光，周围的景色也会投影在玻璃上。特别是水箱中光线较暗、周围比较明亮的时候，玻璃会像镜子一样反光。在这种条件下，如果镜头不紧贴住玻璃，由于玻璃上的投影，水槽中的画面就会显得不够鲜明。为了防止出现这种现象，拍摄时镜头的前端要与玻璃表面垂直紧贴。如果镜头与玻璃表面不成直角，就容易拍摄到投影。因为水箱中光线较暗，鱼的游动速度也很快，所以要将ISO感光度提高。另外，在很难自动对焦时，要使用手动对焦（MF）模式手动进行对焦。有些水族馆禁止在拍摄时使用闪光灯。即使并未禁止，由于玻璃会反射光线，还是不使用闪光灯为好。"
            }, 
            {
                "id": 33, 
                "status": 5, 
                "classify": "夜景", 
                "question": "在拍摄夜景时，为何迟迟无法再次按下快门？", 
                "answer": "因为夜景比白天光线弱，故快门速度也比白天慢很多。虽然快门速度与ISO感光度、夜景的亮度等都有关系，但一般情况下拍摄夜景的曝光时间会超过1秒。但是，如果数码相机长时间曝光，或者用超高感光度拍摄，照片就会有粗糙的感觉，容易出现噪点。因此，在将拍摄的照片存储到存储卡中时，相机就会启动“降噪功能”来减少噪点。降噪处理需要的时间和曝光的时间基本相同，因此造成迟迟无法再次按下快门。相机的初始设置里降噪功能是开启的，如果将其关闭，拍摄夜景时也可以实现连拍。"
            }, 
            {
                "id": 32, 
                "status": 5, 
                "classify": "夜景", 
                "question": "夜景拍摄得过于明亮时应该怎么办？", 
                "answer": "在拍摄夜景时，照片的亮度会根据亮部与暗部的比例不同而不同。如果照片中暗部（较黑的部分）较多，那么相机就会把夜景拍得更明亮；如果亮部（较白的部分）较多，那么相机就会把夜景拍得更暗一些。夜景被拍得过于明亮，就是由于相机判断画面中暗部的比例过大造成的。##如果感觉夜景照片过于明亮，拍摄时可以对曝光进行负补偿。进行负补偿后，照片整体会变暗。反之，如果画面上霓虹灯或灯光装饰等明亮的光线过多，拍摄时照片整体就会变暗。这种情况需要对曝光进行正补偿。##此外，这并不局限于夜景，在拍摄风景或生活照时这一规则也同样适用。比如，如果雪景被拍得过暗，则可以对曝光进行正补偿；如果阴暗处的照片过于明亮，则可以进行负补偿，最终拍出满意的照片。"
            }
        ]
    }
}
```

### 2-2、查询答疑模块数据
```
http://39.108.82.4/v1_0/common/quoraInfo/searchQuora/焦点
```

* 参数  焦点
* 查询答疑模块数据

```js
{
    "code": 200, 
    "info": "", 
    "datas": {
        "lists": [
            {
                "id": 40, 
                "status": 7, 
                "classify": "运动", 
                "question": "拍摄运动物体时，怎样才能不错过精彩瞬间？", 
                "answer": "在拍摄孩子、动物等运动毫无规律的对象时，连续AF模式的效果最好，它在半按快门时焦点能够一直追踪被摄体。但是，如果被摄体在自动对焦区域之外，就必须目不转睛地盯着取景框之中的被摄体，以防错失拍摄良机。##如果要拍摄交通工具等沿着固定轨道运行的物体或者是能够预测运动轨迹的物体，则可以将相机设定为手动对焦（MF）模式，在要拍摄的位置使用可以提前完成对焦的“预对焦”，这将会给拍摄带来很大的便利。“预对焦”完成后，拍摄者就可以在被摄体到达焦点位置的瞬间直接按下快门，捕捉到拍摄时机。"
            }, 
            {
                "id": 39, 
                "status": 7, 
                "classify": "运动", 
                "question": "拍摄运动中的物体时应该怎么对焦？", 
                "answer": "在相机的初始设定中，相机自动对焦模式（AF模式）为单次AF。在这种模式下，对焦后焦点的位置固定，在拍摄静物时可以对焦。##但是，在拍摄运动中的物体时，因为从对焦之后到按下快门的瞬间被摄体发生了移动，如果使用单次AF，焦点就会不对，因此，在拍摄运动的物体时要把自动对焦模式设定为连续AF。这样，只要被摄体在对焦框之内，在半按快门时焦点就会一直追踪被摄体，所以即使被摄体在运动中，也能够拍摄出对焦的照片。"
            }, 
            {
                "id": 38, 
                "status": 7, 
                "classify": "运动", 
                "question": "在拍摄运动的对象时，使用“运动”模式好吗？", 
                "answer": "运动模式在模式盘上用一个跑步者的图标来表示。选择这个模式时，相机会自动设置各种参数，以适合拍摄速度很快的运动场景，非常方便。在运动模式下，相机可以根据焦距、最大光圈、现场的亮度等因素自动设定ISO感光度，用尽可能高的快门速度进行曝光。而且，相机会将自动对焦切换为在半按快门期间可以一直追踪焦点的连续AF模式。使用运动模式拍摄好动的孩子或宠物也很方便。但上述设定全部由相机自动设置，拍摄者连曝光补偿等也无法调节。我们建议精通各种拍摄参数的摄影高手选择“S”模式，这样比较容易根据被摄体和现场情况来进行调整。"
            }, 
            {
                "id": 37, 
                "status": 6, 
                "classify": "风光", 
                "question": "在拍摄风景时，焦点在哪儿最好？", 
                "answer": "对焦的基本要领就是将焦点对准被摄体。即使是拍摄大范围的风景，风景之中也一定会有自己最为关注的部分。如果不把焦点对准那个部分，就无法传递出自己的意图，观众就会不明白照片到底想要表达些什么。##在拍摄时，建议拍摄者将对焦点设置为“中央”，并使想要拍摄的物体位于取景器的中心，然后进行对焦，在半按快门的状态下对构图进行再次调整。而且，拍摄者还可以先确定构图，再选择位于想要对焦的位置的对焦点。"
            }, 
            {
                "id": 16, 
                "status": 2, 
                "classify": "摄影基础知识", 
                "question": "什么是“景深”？", 
                "answer": "对焦后在焦点前后能够清晰成像的范围叫做“景深”。如果收缩镜头光圈（增大F值），对焦后清晰成像的范围就大，照片就不容易虚化，这种状态叫做“大景深”。反之，如果开放镜头光圈（减少F值），对焦范围就狭窄，背景就容易出现虚化，这种状态叫做“浅景深”。因为景深对于拍摄的影响很大，所以在拍摄时一定要认识到它的重要性。##景深根据镜头焦距及相机与被摄体之间距离的变化而变化。广角镜头的景深大，长焦镜头的景深浅。而且，相机与被摄体之间的距离越远，景深就越大；距离越近，景深就越浅。"
            }
        ]
    }
}
```

## 三、鉴赏模块接口
### 3-1、图片查询接口

```
http://39.108.82.4/v1_0/common/photoViewInfo/lists
```

* 注意：返回的url中后面有'\n'需要去掉

```js
{
    "code": 200, 
    "info": "", 
    "datas": {
        "lists": [
            {
                "id": 18, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/Sydney,%20Australia.jpg"
            }, 
            {
                "id": 17, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/Bu%CC%81%C3%B0ir-%20at%20the%20end%20of%20the%20road.jpg"
            }, 
            {
                "id": 16, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/Bamboo%20forest%20Kyoto%202.jpg
"
            }, 
            {
                "id": 15, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/Amsterdam%20II.jpg
"
            }, 
            {
                "id": 14, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/Algarve,%20Portugal.jpg
"
            }, 
            {
                "id": 13, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/Afternoon%20sunlight.jpg
"
            }, 
            {
                "id": 12, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/EMPIRE%20STATE.jpg
"
            }, 
            {
                "id": 11, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/Go%C5%82ucho%CC%81w%20Castle.jpg
"
            }, 
            {
                "id": 10, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/Half%20Tower.jpg
"
            }, 
            {
                "id": 9, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/Dresden,%20German.jpg
"
            }, 
            {
                "id": 8, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/Great%20Smoky%20Mountains,%20U.S..jpg
"
            }, 
            {
                "id": 7, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/fishing.jpg
"
            }, 
            {
                "id": 6, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/Hallstatt,%20Austria.jpg
"
            }, 
            {
                "id": 5, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/France,%20Europe,.jpg
"
            }, 
            {
                "id": 4, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/breidavik%20church...jpg
"
            }, 
            {
                "id": 3, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/Guardian%20of%20heaven.jpg
"
            }, 
            {
                "id": 2, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/Glass%20lake....jpg
"
            }, 
            {
                "id": 1, 
                "imgUrl": "http://oqv1anv8e.bkt.clouddn.com/Foggy%20Morning.jpg
"
            }
        ]
    }
}
```