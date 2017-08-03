```
let cardMap = new Map(), cardsData = []
  this.cardsId.map(id => {
    let direction = cardTypes.filter(item => item.id == id)[0].direction
    let card = cardTypes.filter(item => item.id == id)[0]

    let cards = []

    if (cardMap.has(direction)) {
      cards = cardMap.get(direction)
      cards.push(card)
    } else {
      cards.push(card)
    }

    cardMap.set(direction, cards)
  })

  for (let [key, value] of cardMap.entries()) {
    cardsData.push({ direction: key, cards: value })
  }
  cardsData.map(item => {
    if (item.cards.length > 1) {
      let limit = item.cards.filter(e => e.name == '不限')
      console.log('limit: %o', limit[0].id)
      if (limit.length) {
        this.cardsId = this.cardsId.filter(e => e !== limit[0].id)
        console.log('han: %o', this.cardsId)
      }
    }
  })
  console.log('card map: %o', cardMap, cardsData, this.cardsId)
})
```
