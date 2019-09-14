### 使用React进行父子组件传值

在React项目里，父子组件传值是比较常见的，今天学习了一下React的父子组件传值。

#### 父组件向子组件传值：

+ 父组件向子组件传值：父通过初始state，子组件通过this.props进行接收就可以了；
+ 子组件向父组件传值：需要绑定一个事件，然后事件是父组件传递过来的this.props.event来进行值的更替。


##### 父向子传值：
+ 父组件Comment.js,子组件ComentList.js
+ 父通过初始state，子组件通过this.props进行接收就可以了；


        Comment.js
        
        import React from "react"
        import ComentList from "./ComentList"
         
        class Comment extends React.Component {
            constructor(props) {
                super(props);
                this.state = {
                    arr:["123",]
            }
            render() {
                return (
                    <div>
                        <ComentList arr={this.state.arr}> //这里把state里面的arr传递到子组件
                        </ComentList>
                    </div>
                )
            }
        }
         
        export default Comment;
        
        --------------------------------------------------------------
        
        ComentList.js
        
        import React from "react"
         
        class ComentList extends React.Component {
            constructor(props) {
                super(props);
            }
            render() {
                return (
                    <div className="list">
                        <ul>
                            {
                                this.props.arr.map(item => { //这个地方通过this.props.arr接收到父组件传过来的arr，然后在{}里面进行js的循环
                                    return (
                                        <li key={item}>
                                        评论是:{item}
                                        </li>
                                    )
                                })
                            }
                        </ul>
                    </div>
                )
            }
        }
         
        export default ComentList;

##### 子向父传值
+ 需要绑定一个事件，然后事件是父组件传递过来的this.props.event来进行值的更替。


        Comment.js
        
        import React from "react"
        import ComentList from "./ComentList"
         
        class Comment extends React.Component {
            constructor(props) {
                super(props);
                this.state = {
                    parentText: "this is parent text",
           
                    arr: [{
                        "userName": "fangMing", "text": "123", "result": true
                    }, {
                        "userName": "zhangSan", "text": "345", "result": false
                    }, {
                        "userName": "liSi", "text": "567", "result": true
                    }, {
                        "userName": "wangWu", "text": "789", "result": true
                    },]
                }
            }
         
            fn(data) {
                this.setState({
                    parentText: data //把父组件中的parentText替换为子组件传递的值
                },() =>{
                   console.log(this.state.parentText);
                });
            }
            render() {
                return (
                    <div>
                        //通过绑定事件进行值的运算，这个地方一定要记得.bind(this)，
                        <ComentList arr={this.state.arr} pfn={this.fn.bind(this)}>
                        </ComentList>
                        <p>
                            text is {this.state.parentText}
                        </p>
                
                    </div>
         
                )
            }
        }
         
        export default Comment;  
        
        --------------------------------------------------------------
        
        ComentList.js
        import React from "react"
         
        class ComentList extends React.Component {
            constructor(props) {
                super(props);
                this.state = ({
                    childText: "this is child text"
                })
         
            }
            clickFun(text) {
                this.props.pfn(text)//这个地方把值传递给了props的事件当中
            }
            render() {
                return (
                    <div className="list">
                        <ul>
                            {
                                this.props.arr.map(item => {
                                    return (
                                        <li key={item.userName}>
                                            {item.userName} 评论是:{item.text}
                                        </li>
                                    )
                                })
                            }
                        </ul>
                        这个地方还是要强调，this，this，this
                        <button onClick={this.clickFun.bind(this, this.state.childText)}>
                            click me
                        </button>
                    </div>
                )
            }
        }
         
        export default ComentList;
