**You:**

const searchResults = document.querySelector('#search'); //添加newDiv const newDiv = document.createElement('div'); newDiv.classList.add('newDiv'); newDiv.innerHTML = \`<h2>My Search Result: ${len}</h2>\`; searchResults.insertAdjacentElement('beforebegin', newDiv); //添加listDiv const listDiv = document.createElement('ol'); listDiv.classList.add('listDiv'); listDiv.style.display ="none" newDiv.appendChild(listDiv); //添加itemDiv if (len > 0) { for (const bookmark of bookmarks) { const itemDiv = document.createElement('li'); itemDiv.classList.add('itemDiv'); itemDiv.innerHTML = \`<a href="${bookmark.url}" target="\_blank">${bookmark.title}</a>\`; listDiv.appendChild(itemDiv); } } // 添加折叠和展开的功能 newDiv.addEventListener('click', function() { if (listDiv.style.display === "none") { listDiv.style.display = "block"; } else { listDiv.style.display = "none"; } }); 将点击事件的监听，从newDiv改为h2

* * *

**ChatGPT:**

可以将代码中的 `newDiv.addEventListener` 语句改为以下语句，将监听事件从 `newDiv` 元素改为 `h2` 元素：

```javascript
newDiv.querySelector('h2').addEventListener('click', function() {
    if (listDiv.style.display === "none") {
        listDiv.style.display = "block";
    } else {
        listDiv.style.display = "none";
    }
});
```

这样，当用户点击 `h2` 元素时，会触发事件，从而展开或折叠搜索结果列表。