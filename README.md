
#### How  to use
`
npm i el-pagination
`

Import on your page

`
import Pagination from 'el-pagination/Pagination'
`
#### Attributes

<table>
  <tr>
    <th>Attributes</th>
    <th>Description</th>
    <th>Type</th>
    <th>Optional value</th>
    <th>Defaults</th>
  </tr>
  <tr>
    <td>upName</td>
    <td>Left button value</td>
    <td>String</td>
    <td>-</td>
    <td>上一页</td>
  </tr>
  <tr>
    <td>downName</td>
    <td>Right button value</td>
    <td>String</td>
    <td>-</td>
    <td>下一页</td>
  </tr>
  <tr>
    <td>pageCount</td>
    <td>Total number of pages</td>
    <td>number</td>
    <td>-</td>
    <td>-</td>
  </tr>
   <tr>
    <td>pageSize</td>
    <td>Page length per page</td>
    <td>number</td>
    <td>Recommended value greater than 5</td>
    <td>10</td>
  </tr>
  <tr>
    <td>currentPage</td>
    <td>Current page count, the default number of pages is started for the first time</td>
    <td>number</td>
    <td>-</td>
    <td>-</td>
  </tr>
</table>

#### Events

<table>
  <tr>
    <th>Event name</th>
    <th>Description</th>
    <th>Callback parameter</th>
  </tr>
  <tr>
    <td>click</td>
    <td>Clicking on the page number will trigger</td>
    <td>currentPage</td>
  </tr>
  <tr>
    <td>prev-click</td>
    <td>Clicking on the left button will trigger</td>
    <td>currentPage</td>
  </tr>
  <tr>
    <td>next-click</td>
    <td>Clicking on the right button will trigger</td>
    <td>currentPage</td>
  </tr>
</table>

#### Example

```javascript
<template>
  <div>
      <Pagination
      :upName="upName"
      :downName="downName"
      :pageCount ="pageCount"
      :currentPage ="currentPage"
      :pageSize="pageSize"
      @click="click"
      @prev-click="prevClick"
      @next-click="nextClick">
      </Pagination>   
    </div>
  </div>
</template>
<script>
import Pagination from 'el-pagination/Pagination'
export default {
  name: 'About',
  data () {
    return {
      upName: '上一页',
      downName: '下一页',
      pageCount: 20,
      currentPage: 2,
      pageSize: 10
    }
  },
  components: {
    Pagination
  },
  methods: {
    click(page) {
      console.log(page)
    },
    prevClick(page) {
      console.log(page)
    },
    nextClick(page) {
      console.log(page)
    }
  }
}
</script>
```
**if your currentPage is 2 and your pageCount is 20 pagesSize is 10, you will see this picture**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190615140832776.png)

**this dots is quick  skip many page, this skip number depending on your pageCount value, for example, If your pageCount is 20, the skip number is  pageCount/2 = 2**

**if you currentPage is 8, you will see this picture**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190615141433671.png)

**look the left dot, this is same as the right dot, the difference is skip left page**
