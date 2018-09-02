

```js
/*
<div class="xxx">xxx</div>

要在div.xxx的外边包裹一层div.wrap，使其变成
<div class="wrap">
	<div class="xxx">xxx</div>
</div>
*/
let divWrap = document.createElement('div');
divWrap.setAttribute('class','wrap');

let xxx = document.querySelector('.xxx');
//记下现在xxx的元素父节点
let parent = xxx.parentElement;
//将xxx作为divWrap的最后一个字节点插入，此时xxx从DOM中移除
divWrap.appendChild(xxx);

parent.appendChild(divWrap);

/*---------------------------------------*/

/*
<div class="xxx">xxx</div>
<div class="xxx">xxx</div>
<div class="xxx">xxx</div>

要在每个div.xxx的外边包裹一层div.wrap，使其变成
<div class="wrap">
	<div class="xxx">xxx</div>
</div>
<div class="wrap">
	<div class="xxx">xxx</div>
</div>
<div class="wrap">
	<div class="xxx">xxx</div>
</div>
*/
let xxx = document.querySelectorAll('.xxx');

for (var i = 0; i < xxx.length; i++) {
    let divWrap = document.createElement('div');
	divWrap.setAttribute('class','wrap');
    
    let parent = xxx[i].parentElement;
    
    divWrap.appendChild(xxx[i]);
    
    parent.appendChild(divWrap);
}

/*---------------------------------------*/

/*
<div class="xxx">xxx</div>
<p>this is a pppp</p>
<div class="xxx">xxx</div>
<p>this is a pppp</p>
<div class="xxx">xxx</div>
<p>this is a pppp</p>

要在每个div.xxx的外边包裹一层div.wrap，使其变成
<div class="wrap">
	<div class="xxx">xxx</div>
</div>
<p>this is a pppp</p>
<div class="wrap">
	<div class="xxx">xxx</div>
</div>
<p>this is a pppp</p>
<div class="wrap">
	<div class="xxx">xxx</div>
</div>
<p>this is a pppp</p>
*/
let xxx = document.querySelectorAll('.xxx');

for (var i = 0; i < xxx.length; i++) {
  let divWrap = document.createElement('div');
  divWrap.setAttribute('class','wrap');
    
  let parent = xxx[i].parentElement;
  let sibling = xxx[i].nextElementSibling;
    
  divWrap.appendChild(xxx[i]);
    
  if (sibling) {
    parent.insertBefore(divWrap,sibling)
  } else {
    parent.appendChild(divWrap);
  }
}

/*---------------------------------------*/

function wrap(wrapper,elements) {
    /*
     document.querySelector()取得的domElement没有length属性，则将其放入数组
     document.querySelectorAll()取得的domElement是一个NodeList类数组对象，有length属性
    */
    if (!elements.length) {
        elements = [elements];
    }
    
    for (var i = 0; i < elements.length; i++) {
        //记录下此时elements[i]的元素父节点和紧跟其后的同级元素节点
        let parent = elements[i].parentElement;
        let sibling = elements[i].nextElementSibling;
        
        //将elements[i]作为wrapper的最后一个子节点插入，此时elements[i]从DOM中移除
        wrapper.appendChild(elements[i]);
        
	    //如果elements[i]有nextElementSibling，则将elements[i]放在其nextElementSibling的前面来保持HTML结构
        if (sibling) {
            parent.insertBefore(wrapper,sibling);
        } else {
            parent.appendChild(wrapper);
        }
    } 
 }

/*---------------------------------------*/

function wrapAll(wrapper,elements) {
    //取elements中的第一个或者其本身，此时都只有一个element
    let element = elements.length ? elements[0] : elements;
    
    //记录下此时element的元素父节点和紧跟element后的同级元素节点
    let parent = element.parentElement;
    let sibling = element.nextElementSibling;
    
    //将elements作为wrapper的最后一个子节点插入，此时elements从DOM中移除
    wrapper.appendChild(element);
    
    //如果还有其它的element，也将它们放入wrapper
    //有一个bug，这里的elements应该是动态的元素节点集合，比如HTMLCollection的实例
    //如果是通过querySelectorAll()得到的元素则不行，因为其是静态的节点集合
    while (elements.length) {
    	wrapper.appendChild(elements[0]);
    }
    
    if (sibling) {
        parent.insertBefore(wrapper,sibling);
    } else {
        parent.appendChild(wrapper);
    }
}
```



