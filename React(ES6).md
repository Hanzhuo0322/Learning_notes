React （常用ES6写法）
===

## 一、模块

**1.引入模块引入模块以便使用**

`import '模块文件地址'`

`import 组件 from '模块文件地址'`

**2.导出模块**

	export default class MyComponent extends Component{
    ...
	}
引用：

`import MyComponent from './MyComponent';`

## 二、组件

**1.定义组件**

通过定义一个继承自React.Component的class来定义一个组件类：

	class Photo extends React.Component {
    	render() {
       		...
    	}
	}

**2.定义组件方法**

直接用名字(){},很像java定义类方法的写法：

	class Photo extends React.Component {
    	componentWillMount() {

    	}
    	render() {
        	return (
            	<Image source={this.props.source} />
        	);
    	}
	}


**3.定义组件的属性类型和默认属性**

统一使用static成员来实现：

	class Video extends React.Component {
    	static defaultProps = {
	        autoPlay: false,
	        maxLoops: 10,
    	};  // 注意这里有分号
    	static propTypes = {
	        autoPlay: React.PropTypes.bool.isRequired,
	        maxLoops: React.PropTypes.number.isRequired,
	        posterFrameSrc: React.PropTypes.string.isRequired,
	        videoSrc: React.PropTypes.string.isRequired,
	    };  // 注意这里有分号
	    render() {
	        return (
	            <View />
	        );
	    } // 注意这里既没有分号也没有逗号
	}

*注意: 对React而言，static成员在IE10及之前版本不能被继承，而在IE11和其它浏览器上可以，有时会带来一些问题。React Native则不用担心这个问题。*

**4.初始化STATE**

在构造函数中初始化（这样可以根据需要做一些计算）：

	class Video extends React.Component {
	    constructor(props){
	        super(props);
	        this.state = {
	            loopsRemaining: this.props.maxLoops,
	        };
	    }
	}

**5.把方法作为回调提供并使用**

ES6下，需要通过bind来绑定this引用，或者使用箭头函数（它会绑定当前scope的this引用）来调用：

	//ES6
	class PostInfo extends React.Component{
	    handleOptionsButtonClick(e){
	        this.setState({showOptionsModal: true});
	    }
	    render(){
	        return (
	            <TouchableHighlight 
	                onPress={this.handleOptionsButtonClick.bind(this)}
	                onPress={e=>this.handleOptionsButtonClick(e)}
	                >
	                <Text>{this.props.label}</Text>
	            </TouchableHighlight>
	        )
	    },
	}


箭头函数是在这里定义了一个临时的函数，箭头函数的箭头=>之前是一个空括号、单个的参数名、或用括号括起的多个参数名，而箭头之后可以是一个表达式（作为函数的返回值），或者是用花括号括起的函数体（需要自行通过return来返回值，否则返回的是undefined）。

即：箭头函数箭头前是参数，箭头后是函数体或返回值。

注意： 
不论是bind还是箭头函数，每次被执行都返回的是一个新的函数引用，因此如果你还需要函数的引用去做一些别的事情（譬如卸载监听器），那么必须自己保存这个引用：

	// 错误的做法
	class PauseMenu extends React.Component{
	    componentWillMount(){
	        AppStateIOS.addEventListener('change', this.onAppPaused.bind(this));
	    }
	    componentDidUnmount(){
	        AppStateIOS.removeEventListener('change', this.onAppPaused.bind(this));
	    }
	    onAppPaused(event){
	    }
	}

	// 正确的做法
	class PauseMenu extends React.Component{
	    constructor(props){
	        super(props);
	        this._onAppPaused = this.onAppPaused.bind(this);//注意这里
	    }
	    componentWillMount(){
	        AppStateIOS.addEventListener('change', this._onAppPaused); //还有这里
	    }
	    componentDidUnmount(){
	        AppStateIOS.removeEventListener('change', this._onAppPaused);
	    }
	    onAppPaused(event){
	    }
	}
