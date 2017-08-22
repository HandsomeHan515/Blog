# 2级多选不限的实现
1. 以direction和card为例子，card是direction的子集，通过map操作将card放到parent=task id的direction下；
2. 若directions cards长度为0，则不限；
  
```
const results = [
    {
        "id": 12,
        "direction": [
            {
                "id": 1,
                "name": "服务业"
            },
            {
                "id": 2,
                "name": "IT业"
            }
        ],
        "card": [
            {
                "id": 1,
                "name": "餐饮业",
                "parent": 1
            },
            {
                "id": 2,
                "name": "家政业",
                "parent": 1
            }
        ],
        "date": [
            {
                "id": 1,
                "date": "周一",
                "duration": "上午"
            },
            {
                "id": 2,
                "date": "周一",
                "duration": "下午"
            },
            {
                "id": 3,
                "date": "周一",
                "duration": "晚上"
            },
            {
                "id": 4,
                "date": "周二",
                "duration": "上午"
            }
        ],
        "county": [
            {
                "id": 1,
                "name": "市南",
                "city": 204
            },
            {
                "id": 2,
                "name": "市北",
                "city": 204
            }
        ],
        "remark": "周末加班"
    }
];

directions = results[0].direction
cards = results[0].card

interest = directions.map(item => {
    item.cards = []
    cards.map(e => {
        if (e.parent === item.id) {
            item.cards.push(e)
        }
        return null
    })
    return item
})

console.log(interest)
```
