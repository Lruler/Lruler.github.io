---
title: HOC | render props | hook
date: 2021-09-14 20:08:34
tags: react
categories: 笔记
---

react的中 对于一些复用的节点，我们可以抽成一个组件，而对于一些复用的逻辑，我们也可以抽象化这个逻辑来优化代码，主要有三种方法，HOC，render props，和自定义hook

<!--more-->

# HOC
HOC 就是高阶组件吗，之前有博客介绍了的，虽然写的有点烂但我这里就不赘述了，这里用一个计数器的demo，直接上高阶组件实现计数器的代码
```jsx
import React from 'react'
import {View, Text,Button} from 'react-native'
 
function Count({count,add,minus}) {
    return (
        <View style={{flex:1,alignItems:'center',justifyContent:'center'}}>
            <Text>You clicked {count} times</Text>
            <Button onPress={add} title={'add'}/>
            <Button onPress={minus} title={'minus'}/>
            <Button onPress={changeTheme} title={'ChangeTheme'}/>
        </View>
    );
}
 
const countNumber=(initNumber)=> (WrappedComponent)=>
    class CountNumber extends React.Component {
        state = {count: initNumber};
        add = () => this.setState({count: this.state.count + 1});
        minus = () => this.setState({count: this.state.count - 1});
        render() {
            return <WrappedComponent
                {...this.props}
                count={this.state.count}
                add={this.add.bind(this)}
                minus={this.minus.bind(this)}
            />
        }
    };
export default countNumber(0)(Count);
```
优点:
可以看到，高阶组件是把逻辑和UI分开了，Count组件指负责渲染UI，被传入HOC中才有了计数的逻辑，这很好的维护了内层组件的状态，降低了耦合度
缺点:
1. 注意一下HOC的state是写死的，也就是说传入给Count组件的prop是写死的，这就有可能产生命名冲突，覆盖掉Count组件原有的props
2. HOC是链式调用，会导致错误难以定位

# render props
先直接上代码
```jsx
import React from 'react'
import {View, Text,Button} from 'react-native'
 
export default function RenderProps() {
    return (
        <View style={{flex:1,alignItems:'center',justifyContent:'center'}}>
            <CountNumber initNumber={0} render={
                    ({count,add,minus})=>
                    <>
                        <Text>You clicked {count} times</Text>
                        <Button onPress={add} title={'add'}/>
                        <Button onPress={minus} title={'minus'}/>
                        <Button onPress={changeTheme} title={'ChangeTheme'}/>
                    </>
                }>
            </CountNumber>
        </View>
    );
}
class CountNumber extends React.Component{
    state={count:this.props.initNumber};
    add=()=>this.setState({count:this.state.count+1});
    minus=()=>this.setState({count:this.state.count-1});
    render(){
        return this.props.render({
            count: this.state.count,
            add: this.add,
            minus:this.minus
        })
    }
}
```
这样也实现了逻辑复用，还留下了一个“插槽”，也就是预留位置，缺点就是不能在return语句外访问数据，同时用的过多的话会跟HOC差不多，HOC是过长的链式调用形成异常栈，而render props则会形成潜逃地狱

# Hook
最后看一看最新的自定义hook
```jsx
import React,{useState} from 'react'
import {View, Text,Button} from 'react-native'

export default function HookCount() {
    const [count,addCount,minusCount] = useCountNumber(0);
    return (
        <View style={{backgroundColor:theme,flex:1,alignItems:'center',justifyContent:'center'}}>
            <Text>You clicked {count} times</Text>
            <Button onPress={addCount} title={'add'}/>
            <Button onPress={minusCount} title={'minus'}/>
            <Button onPress={changeTheme} title={'ChangeTheme'}/>
        </View>
    );
}

function useCountNumber(initNumber) {
    const [count, setCount] = useState(initNumber);
    const addCount=()=> setCount(count + 1);
    const minusCount=()=>setCount(count -1);
    return [
        count,
        addCount,
        minusCount
    ]
}

```
反正 hook作为最新的语法，最好还是多用他，但也不能完全代替
+ Hooks：
    + 替代 Class 的大部分用例，除了 getSnapshotBeforeUpdate 和 componentDidCatch 还不支持。
    + 提取复用逻辑。除了有明确父子关系的，其他场景都可以使用 Hooks。
+ Render Props：在组件渲染上拥有更高的自由度，可以根据父组件提供的数据进行动态渲染。适合有明确父子关系的场景。
+ 高阶组件：适合用来做注入，并且生成一个新的可复用组件。适合用来写插件。