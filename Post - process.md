# POST PROCESS
`BRAND`
- Esperar a la brand si no buscarla en el texto
```javascript
async(brand,{input,config},{_,Errors}) =>{
    return await input.page.evaluate(async() => {
        if(brand.includes('Brand:') == false ){
            let url = window.location.href.replace(/\?variant=.*/g,'.json')
            return await fetch(url).then(res => res.json()).then(data => data.product.vendor)
        }else{
            brand = brand.substring(brand.indexOf('Brand:')+7,brand.length)
            brand = brand.substring(0,brand.indexOf('\n'))
            return brand
        }
        
    })
}
```
`IMAGES`
- Eliminar imagenes que salen como `Background - image : url():`
```javascript
images => {
    if (images[0] == null){
        return []
    } else {
        var i = 0
        images.forEach(x => {
            images[i] = images[i].match(/(//cdn)([\S]+)(.(jpg|png|gif|jpeg|JPG|heic))/g)[0]
            images[i] = "https:" + images[i]
            var resolution = images[i].match(/([0-9]+)x(?=.(jpg|png|gif|jpeg|JPG|heic))/g)
            if (resolution !==null){
              images[i] = images[i].replace(/([0-9]+)x(?=.(jpg|png|gif|jpeg|JPG|heic))/g, "")
            }
            i++
        })
        return images
    }
} 
```
- Sacar imagenes desde el json `si no te sirve elimina la configuracion``Se pega desde la configuracion`
```javascript
"perVariantFns": [
            {
                fnName: 'customFunction',
                fnParams: {
                    fnCode: async (fnParams, page, extractorsDataObj) => {
                        let data = extractorsDataObj.customData
                        extractorsDataObj.customData.customImages = Object.fromEntries(data.variants.map((variant) => {
                            return [variant.id, data.media.filter(media => media['media_type'] == "image" && media.alt.trim() === variant.featured_image.alt.trim()).map((media) => media.src)]
                        }))[data.variantData.id]
                    },
                },
            },
        ]
```
selector type: customImages <br>
Selector: variantData.featured_image.src

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
```javascript
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
- Eliminar tablas de forma global con `RegexMatch And Replace`
```
(?<=<table)([\s\S])+(?<=</table)
```

- Eliminar tablas de forma global
```javascript
data => {
  data[0].content = data[0].content.replace(/<table[.*|\s*|\S*]*<\/table>/g, '')
  return data
}
```
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
- Cuando hay una coma y la tool la pone como un punto
```javascript
real_price =>{

  try {
      let num = real_price.match(/[0-9.,]+/g)[0];
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
- Mientras la key tenga `size` como texto, te la devolvera como size
```javascript
options => {
    var keys = Object.keys(options)
    const new_options = {...options}
    if (keys[0] == null){
        return options
    } else {
        keys.map(key => {
            if(key.toLowerCase().includes("size")){
                new_options["size"] = options[key]
                delete new_options[key]
            }
        })
        return new_options
   
    }
    return options     
}
```
- Cuando viene `size - color` juntos como opcion `cambia el sentido y el divisor`
```javascript
options => {
    var keys = Object.keys(options)
    var newOptions = {}
    if(keys[0] === null){
        return options
    } else {
        keys.map(key => {
            if(key === "SIZE  COLOR"){
                var val_1 = options[key].split(' - ')
                newOptions["Size"] = val_1[0]
                newOptions["Color"] = val_1[1]
            } else {
                newOptions[key] = options[key]
            }
        })
        return newOptions
    }
}
```
- Cambiar Title por amount si el selector esta tomando ejemplo: `Title | $200
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
- Remover `:` en opcion de color
``` Javascript
(options, {input, config}, {_, Errors}) => {
  var keys = Object.keys(options)
  var newOptions = {}
  if (keys[0] == null){
    return options
  } else {
    keys.forEach(key => {
      if (!options[key].includes('Title') && !options[key].includes('Default')){
        if (key.includes(':')){
          let ky = key.replace(/\n|[\s]{2,}/gmi, '').toUpperCase()
          ky = ky.match(/([\s\S]+)(?=:)/gmi)[0]
          newOptions[ky] = options[key]
        } else {
          newOptions[key] = options[key]
        }
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







