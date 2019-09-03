```
fetch('/api/poverty-reduction/summary-data').then(res => {
        return res.json()
    }).catch(e => {
        console.log(e)
    }).then(json => {
        buildHtml(elements, json)
    })
```