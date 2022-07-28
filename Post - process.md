# POST PROCESS
`PARAGRAPH`

- Arreglar formatos de lista
```javascript
adjacents=>{
    let i=0;
    adjacents.forEach(adjacent=>{
        if(adjacent.content.includes("<ul>")){
            adjacents[i].content = adjacents[i].content.replace(/<ul>/g, "<ul><br>")
        }
        i++;
    })
    return adjacents
}
```
- Arreglar formato de lista `funciona mas`
```
adjacents=>{
    let i=0;
    adjacents.forEach(adjacent=>{
        if(adjacent.content.includes("<li")){
            adjacents[i].content = adjacents[i].content.replace(/<li/g, "<br> <li")
        }
        if(adjacent.content.includes("<ul")){
            adjacents[i].content = adjacents[i].content.replace(/<ul/g, "\n <ul")
        }
        i++;
    })
    return adjacents
}
```
`DESCRIPTIONS`

- If description is empty
```javascript
(data, {input, config}, {_, Errors}) => {
  let countString = data[0].content.replace(/\s+/g, '').length;
  if (countString >= 87){
      data[0].content = "<b></b>"
      data[0].description_placement = DESCRIPTION_PLACEMENT.DISTANT
      return data
  }else{
  return data
  }
}
```
`TABLES`<br>

- Dar formato de tabla a span con saltos de linea
```javascript
des => {
    if (des[0] == null){
        return []
    } else {
        var i = 0
        des.forEach(x => {
            if (des[i].content.includes("<table")){
                des[i].content = des[i].content.replace(/<tr/g, '<span')
                des[i].content = des[i].content.replace(/<td/g, '<span')
                des[i].content = des[i].content.replace(/<\/tr>/g, '<br> </span>')
                des[i].content = des[i].content.replace(/<\/td>/g, '</span>')
                des[i].content = des[i].content.replace(/<\/table>/g, '</span>')
                des[i].content = des[i].content.replace(/<\/tbody>/g, '</span>')
                des[i].content = des[i].content.replace(/<table/g, '<span')
                des[i].content = des[i].content.replace(/<tbody/g, '<span')
            }
            if (des[i].content.includes("<ul")){
                des[i].content = des[i].content.replace(/<ul>/g, "<ul><br>")
            }
            i++
        })
        return des
    }
}
```

- Dar formato a una tabla que no tiene su formato tabla por defecto
```javascript
des => {
    if (des[0] == null){
        return []
    } else {
        var i = 0
        des.forEach(x => {
            if (des[i].content.includes("<table")){
                des[i].content = des[i].content.replace(/<tr/g, '<span')
                des[i].content = des[i].content.replace(/<td/g, '<br> <span')
                des[i].content = des[i].content.replace(/<\/tr>/g, '</span>')
                des[i].content = des[i].content.replace(/<\/td>/g, '</span>')
                des[i].content = des[i].content.replace(/<\/table>/g, '</span>')
                des[i].content = des[i].content.replace(/<\/tbody>/g, '</span>')
                des[i].content = des[i].content.replace(/<table/g, '<span')
                des[i].content = des[i].content.replace(/<tbody/g, '<span')
            }
            i++
        })
        return des
    }
}
```
- Dar formato de tabla a una tabla en las descriptions:
```javascript
 adjacents=>{
     let i=0;
     adjacents.forEach(adjacent=>{
         if(adjacent.content.includes("<tbody")){
             adjacents[i].content = adjacents[i].content.replace(/<p/g, "<span")
             adjacents[i].content = adjacents[i].content.replace(/<\/p>/g, "<\/span>")
             adjacents[i].content = adjacents[i].content.replace(/<table/g, "<div")
             adjacents[i].content = adjacents[i].content.replace(/<\/table>/g, "<\/div>")
             adjacents[i].content = adjacents[i].content.replace(/<tr/g, "<p")
             adjacents[i].content = adjacents[i].content.replace(/<th/g, "<span")
             adjacents[i].content = adjacents[i].content.replace(/<td/g, "<span")
             adjacents[i].content = adjacents[i].content.replace(/<\/tr>/g, "<\/p>")
             adjacents[i].content = adjacents[i].content.replace(/<\/th>/g, "<\/span>")
             adjacents[i].content = adjacents[i].content.replace(/<\/td>/g, "<\/span>")
         }
         if(adjacent.content.includes("<li")){
             adjacents[i].content = adjacents[i].content.replace(/<li/g, "<br> <li")
         }
         i++;
     })
     return adjacents
 }
```

`PRICE OR H PRICE`<br>
- Para cuando el higher sale como `0`
```javascript
higher_price =>{
    if(higher_price==0 || higher_price == null){
        return undefined;
}
    else{
        return higher_price
}
return higher_price
}
```
- Cambiar de letra a numero el precio
```javascript
real_price =>{
    
  try {
      let num = real_price.match(/[0-9\.,]+/g)[0];
      if (num.includes(",")){
          num = num.replace(/,/g, "")
      }
      num = Number(num)
      if (num == 0 || isNaN(num)){
          return undefined
      } else{
          return num
       }
    
  } catch (error){
        return undefined
    }
}
```
`OPTIONS`
- Cambiar size por amount si el selector esta tomando ejemplo: `size | $200
```javascript
options => {
    var keys = Object.keys(options)
    var newOptions = {}
    if (keys[0] == null){
        return options
    } else {
        keys.forEach(key => {
            if (options[key].includes('Title') || options[key].includes('Default')){
                options[key] = options[key]
            } else if (options[key].includes("$")) {
                newOptions["Amount"] = options[key]
            }else {
                newOptions[key] = options[key]
            }
        })
        return newOptions
    }
}
```
`USER AGENT USED:`
```javascript
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.71 Safari/537.36 (compatible: Remo/0.1; +https://www.remotasks.com/en/info.txt)
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.131 Safari/537.36 (compatible: Remo/0.1; +https://www.remotasks.com/en/info.txt)
Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:90.0) Gecko/20100101 Firefox/90.0 (compatible: Remo/0.1; +https://www.remotasks.com/en/info.txt)
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.164 Safari/537.36 (compatible: Remo/0.1; +https://www.remotasks.com/en/info.txt)
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.131 Safari/537.36 (compatible: Remo/0.1; +https://www.remotasks.com/en/info.txt)
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.107 Safari/537.36 (compatible: Remo/0.1; +https://www.remotasks.com/en/info.txt)
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36 (compatible: Remo/0.1; +https://www.remotasks.com/en/info.txt)
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.1.2 Safari/605.1.15 (compatible: Remo/0.1; +https://www.remotasks.com/en/info.txt)
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.1.1 Safari/605.1.15 (compatible: Remo/0.1; +https://www.remotasks.com/en/info.txt)
Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:91.0) Gecko/20100101 Firefox/91.0 (compatible: Remo/0.1; +https://www.remotasks.com/en/info.txt)
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.77 Safari/537.36 (compatible: Remo/0.1; +https://www.remotasks.com/en/info.txt)
Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:89.0) Gecko/20100101 Firefox/89.0 (compatible: Remo/0.1; +https://www.remotasks.com/en/info.txt)
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.77 Safari/537.36 (compatible: Remo/0.1; +https://www.remotasks.com/en/info.txt)
```







