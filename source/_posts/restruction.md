---
title: 记一次代码重构
date: 2020-7-29 14:08:15
category: 前端
tags: javascript
description: 上个月中途接手了个项目，主要功能是C端用户购房申请资料的表单提交，单纯就功能而言确实没什么可说的，但问题在于期极复杂繁琐多变的资格逻辑校验。由于政府的购房政策多变，几乎每隔一段时间都要调那么几次，而且动不动还要做新老数据兼容。我接手的源代码里面几乎全部都是`if else`判断，几个页面里差不多都有二三十个判断，代码的可读性维护性很差。可以想象一份很难维护的代码，又反复要求被修改，这对开发这而言是极其痛苦的事情。因此我重构的目就是尽量减少`if else`逻辑判断。
---
![](https://images.unsplash.com/photo-1544256718-3bcf237f3974?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1200&q=10)


上个月中途接手了个项目，主要功能是C端用户购房申请资料的表单提交，单纯就功能而言确实没什么可说的，但问题在于期极复杂繁琐多变的资格逻辑校验。由于政府的购房政策多变，几乎每隔一段时间都要调那么几次，而且动不动还要做新老数据兼容。我接手的源代码里面几乎全部都是`if else`判断，几个页面里差不多都有二三十个判断，代码的可读性维护性很差。可以想象一份很难维护的代码，又反复要求被修改，这对开发这而言是极其痛苦的事情。因此我重构的目就是尽量减少`if else`逻辑判断。
<!-- more -->
于是我想到用特定的数据结构来映射逻辑关系。以后要动里面的逻辑我简单改下数据结构就好了。虽然最后的数据结构很庞大，但是我发现维护起来确实很方便。就目前而言，代码的可维护性是我考虑的首选。

实现主要思路逻辑：
```javascript:;
//后台给你返回的数据结构大概如下
{
    household:{
        allowForbidden: false,
        caseDesc: "",
        caseImg: [],
        desc: "",
        fixed: 1,
        forceHidden: false,
        hidden: false,
        max: 1,
        min: 1,
        options: [
            {name: "杭州市户籍（9区）", key: "杭州市户籍（9区）"},
            {name: "非杭州市户籍", key: "非杭州市户籍"},
            {name: "落户满2年的临安、桐庐、建德、淳安户籍", key: "落户满2年的临安、桐庐、建德、淳安户籍"},
            {name: "落户未满2年的临安、桐庐、建德、淳安户籍", key: "落户未满2年的临安、桐庐、建德、淳安户籍"}
        ],
        qkey: "household",
        title: "户籍",
        type: "select"
    },
    settleMode:{
        allowForbidden: false,
        caseDesc: "",
        caseImg: [],
        desc: "",
        fixed: 1,
        forceHidden: false,
        hidden: true,
        max: 1,
        min: 1,
        options: [{name: "非以投靠成年子女方式", key: "非以投靠成年子女方式"}, {name: "以投靠成年子女方式，且落户已满3年", key: "以投靠成年子女方式，且落户已满3年"}, {name: "以投靠成年子女方式，且落户未满3年", key: "以投靠成年子女方式，且落户未满3年"}],
        qkey: "settleMode",
        title: "落户方式及年限",
        type: "select"
    },
    housingStockNumber:{
        allowForbidden: false,
        caseDesc: "",
        caseImg: [],
        desc: "",
        fixed: 1,
        forceHidden: false,
        hidden: false,
        max: 1,
        min: 1,
        options: [{name: "0套", key: "0套"}, {name: "1套", key: "1套"}, {name: "2套及以上", key: "2套及以上"}],
        qkey: "housingStockNumber",
        title: "在杭州市（9区）拥有住房",
        type: "select"
    },
    ltjcHousingStockNumber:{
        allowForbidden: false,
        caseDesc: "",
        caseImg: [],
        desc: "",
        fixed: 1,
        forceHidden: false,
        hidden: true,
        max: 1,
        min: 1,
        options: [
            {name: "0套", key: "0套"},
            {name: "1套", key: "1套"},
            {name: "2套及以上", key: "2套及以上"}
        ],
        qkey: "ltjcHousingStockNumber",
        title: "在户籍所在地拥有住房",
        type: "select"
    },
    maritalStatus:{
        allowForbidden: false,
        caseDesc: "",
        caseImg: [],
        desc: "",
        fixed: 1,
        forceHidden: false,
        hidden: false,
        max: 1,
        min: 1,
        options: [
            {name: "未婚单身", key: "未婚单身"},
            {name: "已婚", key: "已婚"},
            {name: "离异单身", key: "离异单身"},
            {name: "丧偶", key: "丧偶"}
        ],
        qkey: "maritalStatus",
        title: "婚姻情况",
        type: "select"
    },
    divorceTime:{
        allowForbidden: false,
        caseDesc: "",
        caseImg: [],
        desc: "",
        fixed: 1,
        forceHidden: false,
        hidden: true,
        max: 1,
        min: 1,
        options: [{name: "2018年4月4号前（含）", key: "2018年4月4号前（含）"}, {name: "2018年4月4号后", key: "2018年4月4号后"}],
        qkey: "divorceTime",
        title: "离异时间段",
        type: "select"
    },
    personalIncome:{
        allowForbidden: false,
        caseDesc: "",
        caseImg: [],
        desc: "",
        fixed: 1,
        forceHidden: false,
        hidden: true,
        max: 1,
        min: 1,
        options: [{name: "是", key: "是"}, {name: "否", key: "否"}],
        qkey: "personalIncome",
        title: "自购房之日起前3年内在杭州市连续缴纳2年及以上社保或者个税（如有新政策出台，以新政策为准）",
        type: "select"
    }
}

//我定义类似如下数据结构来映射逻辑关系
{
    household:{
        '杭州市户籍（9区':{
            maritalStatus:{
                "未婚单身":{
                    housingStockNumber:{
                        "0套":{
                            result:{
                                limit: true//是不是要限购
                            }
                        },
                        "1套":{
                            //...
                        },
                        "2套及以上":{
                            //...
                        }
                    }
                },
                "已婚":{
                    //...
                },
                "离异单身":{
                    //...
                },
                "丧偶":{
                    //...
                }
            }
        },
        '落户满2年的临安、桐庐、建德、淳安户籍':{
            //...
        },
        '落户未满2年的临安、桐庐、建德、淳安户籍':{
            //...
        },
        '非杭州市户籍':{
            //...
        },
        '非中华人民共和国国籍':{
            //...
        }
    }
}
```

每一个分支的逻辑关系都通过上述数据结构来映射，哪个环节的哪些字段要不要显示，以及最后的提交要不要限购，都可以通过上述结构定义。只有你选中一个选项才会出现其选项下对应的下一个待选项（这样就直接控制了显示隐藏的逻辑），而我就通过用户的具体选项来层层检索整个数据结构，拿到特定条件的判断逻辑。具体检索代码我就不放出来了。