React.js教程: React State
====

----
和properties一样,state(状态)影响着一个组件的行为和渲染. 但与properties不同的是,不允许通过JSX来定义组件的state.

##Properties vs. State 属性 VS 状态


当组件被创建的时候properties通过JSX或者纯JS被定义. state, 只可以在组件内部被定义. 这是第一个也是最重要的区别.

想起properties应该想起组件初始化. 想起state,应该想起这是会影响组件渲染的内部数据集.

有几种方法去验证properties的有效性和类型, state却没有同样的方法或者机制. 作为组件的开发者, 你只唯一一个知道你的组件需要什么state, 和state正确的数据类型和怎么样的值会被接受或者提供.

state应该被当做私有数据.

##Setting Initial State 设置初始状态值

在使用state之前我们需要声明state的初始值, 可以通过getInitialState()函数返回一个对象.

	/* @jsx React.DOM /
	
	
	'use strict';
	
	
	
	var InterfaceComponent = React.createClass({
	  getInitialState: function () {
	    return {
	      name: 'xinran',
	      job: 'developer'
	    };
	  },
	  render: function () {
	    return <div> My name is {this.state.name} and I am a {this.state.job}. </div>;
	  }
	});
	
	
	
	React.render(
	  <InterfaceComponent />,
	  document.body
	);
	
getInitialState()这个函数告诉组件在第一轮渲染的时候什么值是可用的直到state的值被改变.
	
##Setting State 设置状态
	
只可以从组件的内部设置state的值.就像上面所说的, 我们应该看待state为私有数据, 但是你也可以去更新它.
	
	/* @jsx React.DOM /
	
	
	'use strict';
	
	
	
	var InterfaceComponent = React.createClass({
	  getInitialState: function () {
	    return {
	      name: 'xinran',
	    };
	  },
	  handleClick: function () {
	    this.setState({
	      name: 'little cat'
	    });
	  },
	  render: function () {
	    return <div onClick = {this.handleClick}> My name is {this.state.name} </div>;
	  }
	});
	
	
	
	React.render(
	  <InterfaceComponent />,
	  document.body
	);

直到组件被重新渲染你是看不到新的state的值的. 这种设置state的值的方法最好只用在组件的渲染和行为依赖于外部的因素.

##Replacing State 替换状态

使用replaceState()也是可以替换state的值的.

	/*
	 * @jsx React.DOM
	 /
	
	
	var InterfaceComponent = React.createClass({
	  getInitialState : function() {
	    return {
	      first : "chris",
	      last  : "pitt"
	    };
	  },
	  handleClick : function() {
	    this.replaceState({
	      first : "bob"
	    });
	  },
	  render : function() {
	    return <div onClick={this.handleClick}>
	      hello { this.state.first + " " + this.state.last }
	    </div>;
	  }
	});

当你想要清除一些已经在state里面存在的值并且追加一些新值的时候可以使用replaceState()

##Avoiding State 避免使用状态

尽可能减少使用state.当你使用state的时候, 你承担着组件的渲染和行为会出现错误的风险.

不要在静态的组件上使用state. 如果你的组件基于外部因素不需要改变那么就不要使用state. 

在render()里面计算渲染的值更好.
