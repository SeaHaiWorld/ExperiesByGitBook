### Think.jsCRUD操作
#### 添加数据
+ 添加一条数据
<pre>
<code>
    const id = await model.add({name: 'xxx', id: 'yyy'});
</code>
</pre>
<pre>
<code>
    使用 add 方法可以添加一条数据，返回值为插入数据的 id。如：
    export default class extends think.controller.base {
      async addAction(){
        const model = this.model('user');
        const id = await model.add({name: 'xxx', id: 'yyy'});
      }
    }
</code>
</pre>
+ 添加多条数据
<pre>
<code>
   使用 addMany 方法可以添加多条数据，如：
   export default class extends think.controller.base {
     async addAction(){
       const model = this.model('user');
       const insertId = await model.addMany(
       [
         {name: 'xxx1', pwd: 'yyy1'},
         {name: 'xxx2', pwd: 'yyy2'}
       ]
     )}
   }
   
   注：addMany添加多条数据时应该插入一个数组
</code>
</pre>

#### 查询数据
模型中提供了多种方式来查询数据，如：查询单条数据，查询多条数据，读取字段值，读取最大值，读取总条数等。
+ 查询单条数据

    Think.js 提供 find 方法查询单条数据，返回值为对象。如：
<pre>
<code>
    const data = await model.where({key: value}).find();
    注: where应是对象key值
</code>
</pre>
<pre>
<code>
    [
    ...
    {name: 'thinkjs', email: 'admin@thinkjs.org', ...}
    ...
    ]
</code>
</pre>
<pre>
<code>
    export default class extends think.controller.base {
      async listAction(){
        const model = this.model('user');
        const data = await model.where({name: 'thinkjs'}).find();
        //data will returns {name: 'thinkjs', email: 'admin@thinkjs.org', ...}
      }
    }
    
    注：如果数据表没有对应的数据，那么返回值为空对象 {}，可以通过 think.isEmpty 方法来判断返回值是否为空。
</code>
</pre>
+ 查询多条数据

    Think.js 提供 select 方法查询多条数据( number )，返回值为数组。如：
<pre>
<code>
    const data = await model.limit(number).select();
</code>
</pre>
<pre>
<code>
    [
    {name: 'test', email: 'test@thinkjs.org', ...}
    {name: 'thinkjs', email: 'admin@thinkjs.org', ...}
    ...
    ]
</code>
</pre>
<pre>
<code>
    export default class extends think.controller.base {
      async listAction(){
        const model = this.model('user');
        const data = await model.limit(2).select();
        /*data will returns 
        [{name: 'test', email: 'test@thinkjs.org', ...}
        {name: 'thinkjs', email: 'admin@thinkjs.org', ...}]
        */
      }
    }
</code>
</pre>
+ 分页查询数据

    分页来展现某些数据，这种情况下就需要先查询总的条数，然后在查询当前分页下的数据。查询完数据后还要计算有多少页。
    Think.js 提供了 countSelect 方法来方便这一操作，会自动进行总条数的查询。
    可以借助table框架迅速分析数据
<pre>
<code>const data = await model.page(page:number, listrows:number).countSelect();
</code></pre>
<pre>
<code>
    export default class extends think.controller.base {
      async listAction(){
        const model = this.model('user');
        const data = await model.page(2, 10).countSelect();
      }
    }
</code>
</pre>
<pre>
<code>
    {
      count: number,  //总条数
      totalPages: number,  //总页数
      pageSize: number,  //每页显示的条数
      currentPage: number,  //当前页
      data: array //当前页下的数据列表
    }
</code>
</pre>
