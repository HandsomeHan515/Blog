# ES6 New Map,以对象为key的方式
```
const checkins = [
      {
          "id": 1,
          "owner": {
              "id": 21,
              "avatar": "http://127.0.0.1:10017/media/user10_aZhySMu.jpg",
              "my_cards": [
                  {
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
                          },
                          {
                              "id": 3,
                              "name": "法律咨询",
                              "parent": 1
                          }
                      ]
                  }
              ],
              "achieves": 0,
              "username": "18753377130",
              "nickname": "韩帅",
              "address": "北京昌平",
              "email": "han@123.com",
              "sex": "male",
              "is_vip": false,
              "level": 0
          },
          "status": 1,
          "start": 1500001234,
          "end": null,
          "position": "北京海淀",
          "job": 1
      },
      {
          "id": 3,
          "owner": {
              "id": 8,
              "avatar": "http://127.0.0.1:10017/media/a20114fd-97c.jpg",
              "my_cards": [
                  {
                      "card": []
                  },
                  {
                      "card": []
                  }
              ],
              "achieves": 0,
              "username": "13126724584",
              "nickname": "Lala",
              "address": "北京市朝阳区",
              "email": null,
              "sex": "female",
              "is_vip": false,
              "level": 0
          },
          "status": 0,
          "start": 1501950600,
          "end": 1501993,
          "position": "北京市昌平区",
          "job": 1
      },
      {
          "id": 4,
          "owner": {
              "id": 21,
              "avatar": "http://127.0.0.1:10017/media/user10_aZhySMu.jpg",
              "my_cards": [
                  {
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
                          },
                          {
                              "id": 3,
                              "name": "法律咨询",
                              "parent": 1
                          }
                      ]
                  }
              ],
              "achieves": 0,
              "username": "18753377130",
              "nickname": "韩帅",
              "address": "北京昌平",
              "email": "han@123.com",
              "sex": "male",
              "is_vip": false,
              "level": 0
          },
          "status": 0,
          "start": 1501950630,
          "end": null,
          "position": null,
          "job": 1
      },
      {
          "id": 5,
          "owner": {
              "id": 21,
              "avatar": "http://127.0.0.1:10017/media/user10_aZhySMu.jpg",
              "my_cards": [
                  {
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
                          },
                          {
                              "id": 3,
                              "name": "法律咨询",
                              "parent": 1
                          }
                      ]
                  }
              ],
              "achieves": 0,
              "username": "18753377130",
              "nickname": "韩帅",
              "address": "北京昌平",
              "email": "han@123.com",
              "sex": "male",
              "is_vip": false,
              "level": 0
          },
          "status": 0,
          "start": 1501950540,
          "end": null,
          "position": null,
          "job": 1
      },
      {
          "id": 6,
          "owner": {
              "id": 21,
              "avatar": "http://127.0.0.1:10017/media/user10_aZhySMu.jpg",
              "my_cards": [
                  {
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
                          },
                          {
                              "id": 3,
                              "name": "法律咨询",
                              "parent": 1
                          }
                      ]
                  }
              ],
              "achieves": 0,
              "username": "18753377130",
              "nickname": "韩帅",
              "address": "北京昌平",
              "email": "han@123.com",
              "sex": "male",
              "is_vip": false,
              "level": 0
          },
          "status": 1,
          "start": 1500000123,
          "end": null,
          "position": null,
          "job": 1
      },
      {
          "id": 7,
          "owner": {
              "id": 21,
              "avatar": "http://127.0.0.1:10017/media/user10_aZhySMu.jpg",
              "my_cards": [
                  {
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
                          },
                          {
                              "id": 3,
                              "name": "法律咨询",
                              "parent": 1
                          }
                      ]
                  }
              ],
              "achieves": 0,
              "username": "18753377130",
              "nickname": "韩帅",
              "address": "北京昌平",
              "email": "han@123.com",
              "sex": "male",
              "is_vip": false,
              "level": 0
          },
          "status": 1,
          "start": 1500000123,
          "end": null,
          "position": null,
          "job": 1
      }
  ]
var userMap = new Map()

checkins.map(item=> {
  owner = JSON.stringify(item.owner)
  delete item.owner

  checks = []

  if (userMap.has(owner)) {
    checks = userMap.get(owner)
    checks.push(item)
  } else {
    checks.push(item)
  }
  userMap.set(owner, checks)
})

let newCheckins = []
for (let [key, value] of userMap.entries()) {
  newCheckins.push({owner: JSON.parse(key), checks: value})
}

console.log('user map: %o checkins: %o', userMap, newCheckins)
```
